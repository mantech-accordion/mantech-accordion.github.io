---
layout: default
title: 1. Pending
parent: 부록. 트러블슈팅 가이드
nav_order: 1
---

# Pending
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Pending 상태란?

Pod가 생성되지 못하고 항상 Pending 상태입니다

---

## 일반적인 장애증상

-	클러스터에 Pod를 실행하기에 충분한 리소스(예: CPU 및 메모리) 부족
-	현재 네임스페이스에는 ResourceQuota 객체가 있으며 Pod를 생성하면 네임스페이스가 할당량을 초과
-	Pod는 Pending 상태의 Persistent Volume Claim에 바인딩

---

## 문제확인 방법

- Pod 상태 확인

```
메뉴 → 워크로드 → Pod → Overview
```

- Pod 이벤트 확인

```
메뉴 → 워크로드 → Pod → Overview → 이벤트
```

-	PVC 확인

```
$ kubectl get pvc -A | grep -i <PVC_NAME>
```

---

## 조치 방법

- Resource의 단위를 잘못 표기, 리소스가 부족한 경우 단위 및 값을 수정한다.
- Pod에 설정된 볼륨이 존재하지 않는 경우 PVC를 생성한다.
-	Pod가 배포된 노드가 없는 경우 FailedScheduling발생한다. 이러한 경우 위에 설명한 FailedScheduling오류 조치사항을 참고한다. 

---

- **Case 1: Resource의 단위를 잘못 표기한 경우**
  - Pod 상태 확인

  ```
  kubectl get pod 
  test-quota-65588cb487-4gvbr      0/1     Init:0/1   0          39s
  test-quota-687b8999cf-4ndp2      1/1     Running    0          5m29s
  ```

  - Pod 이벤트 로그 확인

  ```
  Events:
    Type     Reason   	Age   From           	Message
    ----     ------   	----  ----           	-------
    Normal   Scheduled	72s   default-scheduler  Successfully assigned iskim/test-quota-65588cb487-4gvbr to p-iskim-worker2
    Warning  FailedMount  72s   kubelet        	MountVolume.SetUp failed for volume "kube-api-access-965gv" : write /var/lib/kubelet/pods/5bf6a0b0-6198-4c35-9c1a-66c8edecb31b/volumes/kubernetes.io~projected/kube-api-access-965gv/..2022_07_14_01_42_04.048041162/ca.crt: no space left on device
    Warning  FailedMount  72s   kubelet        	MountVolume.SetUp failed for volume "kube-api-access-965gv" : write /var/lib/kubelet/pods/5bf6a0b0-6198-4c35-9c1a-66c8edecb31b/volumes/kubernetes.io~projected/kube-api-access-965gv/..2022_07_14_01_42_04.027965345/token: no space left on device
    Warning  FailedMount  71s   kubelet        	MountVolume.SetUp failed for volume "kube-api-access-965gv" : write /var/lib/kubelet/pods/5bf6a0b0-6198-4c35-9c1a-66c8edecb31b/volumes/kubernetes.io~projected/kube-api-access-965gv/..2022_07_14_01_42_05.382770828/token: no space left on device
    Warning  FailedMount  68s   kubelet        	MountVolume.SetUp failed for volume "kube-api-access-965gv" : write /var/lib/kubelet/pods/5bf6a0b0-6198-4c35-9c1a-66c8edecb31b/volumes/kubernetes.io~projected/kube-api-access-965gv/..2022_07_14_01_42_08.666824315/ca.crt: no space left on device
    Warning  FailedMount  64s   kubelet        	MountVolume.SetUp failed for volume
  ```

  - Pod Resource 설정(resource quota) 확인

  ```
  kubectl edit pod test-quota-65588cb487-4gvbr

  Limits:
    cpu:     0
    memory:  500
  Requests:
    cpu:     0
    memory:  500
  ```

  - Pod의 resource quota 단위 및 값 수정
  - Pod 정상배포 확인

  ```
  $ kubectl get pod 
    test-quota-867df7fdfc-99bt5        	1/1 	Running   	0      	13s
    test-quota-867df7fdfc-9mmsx         	1/1 	Running   	0      	4s
  ```

---

- **Case 2 : FailedScheduling**

  default-scheduler는 Pod가 적절한 Node에 스케줄링 되도록 관리해 준다. 하지만 노드에 스케줄링 하지 못하는 경우 FailedScheduling가 발생하게 된다. 이러한 경우 이벤트 로그를 통해 최초 확인하며, 로그 메시지를 보고 노드에 스케줄링 할 수 있도록 적절하게 조치해준다. 

  - Pod 상태 확인

  ```
  [root@jwkim-master ~]# k get pod 
  NAME                                            READY   STATUS    RESTARTS        AGE
  dev-tech-mattermost-day-alarm-bdfd9cf4c-b588s   1/1     Running   1 (57d ago)     84d
  redmine-5d6f6c668f-4v44l                        1/1     Running   171 (26d ago)   69d
  redmine-mariadb-0                               1/1     Running   0               26d
  redmine-postgresql-0                            1/1     Running   71 (12d ago)    26d
  tmp-shell                                       1/1     Running   2 (57d ago)     96d
  tmp-shell-host                                  1/1     pending  8 (57d ago)     151d

  ```

  - `에러 발생 원인 1 : 한 노드에 기본 200개의 Pod가 스케쥴 될 수 있으나, 그 이상 Pod가 배포되려고 하면 Pending 발생`

  ```
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  128m  default-scheduler  0/3 nodes are available: 1 node(s) had taint {node-role.kubernetes.io/master: }, that the pod didn't tolerate, 2 Too many pods.
  ```

  - `에러 발생 원인 2 : 노드에 taint가 설정되어 있는 경우`

  ```
  Type     Reason            Age    From               Message
  ----     ------           ----    ----               -------
  Warning  FailedScheduling 41m (x4 over 42m) default-scheduler  0/2 nodes are available: 2 node(s) had taint {eks.amazonaws.com/compute-type: fargate}, that the pod didn't tolerate.
  ```

  - `에러 발생 원인 3 : 선택된 노드가 일치하지 않는 경우(NodeSelector, NodeAffinity)`

  ```
  Type     Reason            Age    From               Message
  ----     ------           ----    ----               -------
  Warning FailedScheduling 1m (x34 over 11m) default-scheduler 0/6 nodes are available: 6 node(s) didn't match node selector
  ```

  - `에러 발생 원인 4 : 연관된 볼륨이 활성화 되지 않은 경우`

  ```
  Events:
  Type     Reason            Age    From               Message
  ----     ------            ----   ----               -------
  Warning  FailedScheduling  8m26s  default-scheduler  running PreBind plugin "VolumeBinding": binding volumes: timed out waiting for the condition
  Warning  FailedScheduling  58s (x20 over 26m)  default-scheduler  persistentvolumeclaim "create-dir-pvc" not found
  Warning  FailedScheduling  33s (x2 over 94s)  default-scheduler  0/4 nodes are available: 4 node(s) didn't find available persistent volumes to bind.
  ```

  - 조치 방법
    - 불필요한 Pod 삭제
    - pod에 toleration spec을 추가한다.
    - spec.nodeSelector의 key, value가 정확하게 설정되어 있는지 확인한다.
    - pvc상태를 확인하여 정상적으로 볼륨이 바운드 되는지 확인한다.



