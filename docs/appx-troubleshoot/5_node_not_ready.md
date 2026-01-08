---
layout: default
title: 5. NodeNotReady
parent: 부록. 트러블슈팅 가이드
nav_order: 5
---

# Node Not Ready
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

<div class="code-example" markdown="1">
Cluster
{: .label .label-green }
</div>

## Node Not Ready 상태란?

Worker Node가 종료되거나 충돌이 나면 해당 노드에 있는 모든 상태저장 pod를 사용할 수 없게 되며 노드 상태가 NotReady 상태로 나타납니다.
노드의 NotReady 상태가 기본적으로 5분이상 지속될 경우 kubernetes는 예약된 pod의 상태를 Unknown 으로 변경하고 다른 노드에서 ContainerCreating을 시도합니다.

---

## 일반적인 장애 증상

-	API 서버 VM 종료 또는 apiserver 충돌
    -	포드, 서비스를 생성 및 업데이트 중지를 실행할 수 없다.
    -	Kubernetes API에 의존하지 않는 한 기존 포드 및 서비스는 계속 정상적으로 작동해야 한다.
-	etcd 손실
    -	kube-apiserver 구성 요소가 성공적으로 시작되지 않고 정상 상태가 되지 않는다.
    -	kubelets을 통해 기존 포드, 서비스를 변경할 수 없다..
-	kube-controller, replication controller manager, scheduler 노드 종료 또는 충돌
    -	apiserver 가 종료된 것과 동일한 현상 발생
    -	새로운 Pod 스케쥴 배치가 불가능
    -	Pod 복제 상태를 지속 불가능
-	노드(VM 또는 베어메탈) 셧다운
    -	해당 노드의 포드가 실행을 중지


---

## 문제확인 방법

- 노드 상태 확인

`메뉴 → 노드`

![5_nodenotready_node_status.jpg](/assets/images/troubleshoot/5_nodenotready_node_status.jpg){: width="800" }

- 노드의 상세 상태 확인

```
[root@tmp-master ~]# kubectl describe node dev-accordion2
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason
        Message
  ----                 ------  -----------------                 ------------------                ------
        -------
  NetworkUnavailable   False   Fri, 27 May 2022 14:06:26 +0900   Fri, 27 May 2022 14:06:26 +0900   CalicoIsUp
        Calico is running on this node
  MemoryPressure       False   Wed, 13 Jul 2022 17:52:31 +0900   Fri, 27 May 2022 14:05:11 +0900   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Wed, 13 Jul 2022 17:52:31 +0900   Fri, 27 May 2022 14:05:11 +0900   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Wed, 13 Jul 2022 17:52:31 +0900   Fri, 27 May 2022 14:05:11 +0900   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Wed, 13 Jul 2022 17:52:31 +0900   Fri, 27 May 2022 14:05:21 +0900   KubeletReady
        kubelet is posting ready status
```

- 노드의 이벤트 로그 확인

`메뉴 → 하단 이벤트 검색`

![5_nodenotready_node_event.jpg](/assets/images/troubleshoot/5_nodenotready_node_event.jpg){: width="800" }

---

## 발생할 수 있는 케이스 및 조치 방법

- 노드 NotReady상태의 원인을 파악한다. NotReady상태라도 복구 및 서비스유지를 위해서 etcd볼륨을 주기적으로 백업하며, 마스터를 3노드, 워커노드는 2노드 이상 구성하여 고가용성을 유지한다. 
    -	노드의 kubelet 상태확인 후 서비스 재기동
    -	노드의  container runtime(containerd or cri-o) 상태확인 후 서비스 재기동
    -	노드의 CNI확인(calico or weave)
    -	노드 주요 리소스 확인(CPU, 메모리, Disk)
    -	NotReady상태 노드의 서버 재기동 


--- 

- **docker 데몬 환경설정이 잘못되어 문제가 발생하는 케이스** : 환경설정 수정 후 → kubelet 정상기동

  - 노드 상태확인

  ```
  [root@acc-master1 accordion-installer]# k get node -o wide
  NAME          STATUS     ROLES    AGE   VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION           CONTAINER-RUNTIME
  acc-master1   Ready      master   37d   v1.16.9   10.150.100.60   <none>        CentOS Linux 7 (Core)   3.10.0-1127.el7.x86_64   docker://19.3.8
  acc-master2   Ready      master   37d   v1.16.9   10.150.100.62   <none>        CentOS Linux 7 (Core)   3.10.0-1127.el7.x86_64   docker://19.3.8
  acc-master3   Ready      master   37d   v1.16.9   10.150.100.63   <none>        CentOS Linux 7 (Core)   3.10.0-1127.el7.x86_64   docker://19.3.8
  acc-node1     Ready      <none>   37d   v1.16.9   10.150.100.64   <none>        CentOS Linux 7 (Core)   3.10.0-1127.el7.x86_64   docker://19.3.8
  acc-node2     Ready      <none>   37d   v1.16.9   10.150.100.65   <none>        CentOS Linux 7 (Core)   3.10.0-1127.el7.x86_64   docker://19.3.8
  acc-node3     Ready      <none>   28d   v1.16.9   10.150.100.68   <none>        CentOS Linux 7 (Core)   3.10.0-1127.el7.x86_64   docker://19.3.8
  acc-node4     NotReady   <none>   23m   v1.16.9   10.150.100.69   <none>        CentOS Linux 7 (Core)   3.10.0-1127.el7.x86_64   docker://19.3.8
  ```

  - kubelet 상태 확인

  ```
  [root@acc-node4 log]# journalctl -fu kubelet
  -- Logs begin at 수 2020-08-12 15:54:55 KST. --
   8월 13 16:28:18 acc-node4 kubelet[48130]: I0813 16:28:18.479712   48130 docker_service.go:240] Hairpin mode set to "hairpin-veth"
  …
  Expected:dc9208a3303feef5b3839f4323d9beb36df0a9dd} InitCommit:{ID:fec3683 Expected:fec3683} SecurityOptions:[name=seccomp,profile=default] ProductLicense: Warnings:[]}
   8월 13 16:28:28 acc-node4 kubelet[48229]: F0813 16:28:28.982974   48229 server.go:271] failed to run Kubelet: failed to create kubelet: misconfiguration: kubelet cgroup driver: "systemd" is different from docker cgroup driver: "cgroupfs"
   8월 13 16:28:28 acc-node4 systemd[1]: kubelet.service: main process exited, code=exited,
  ```

---

- **노드 자원 부족 케이스** : 누수가 되는 프로세스가 없는지 확인하고, 최신 패치를 유지하는 것을 권장.

```
root@acc-master2:~/jungwon/nfs/jungwon-war-exploded-deploy2-f8348c26-b43ce0c9-pvc-61c5d6d1-f925-4e99-a485-149821f54f23# k get nodes
NAME          STATUS     ROLES                  AGE   VERSION
acc-master    Ready      control-plane,master   23d   v1.20.8
acc-master2   Ready      control-plane,master   23d   v1.20.8
acc-master3   Ready      control-plane,master   23d   v1.20.8
acc-node1     NotReady   worker                 23d   v1.20.8
acc-node2     Ready      worker                 23d   v1.20.8
```

```
root@]kubectl describe node acc-node1
Lease:
  HolderIdentity:  acc-node1
  AcquireTime:     <unset>
  RenewTime:       Fri, 05 Nov 2021 13:28:10 +0900
Conditions:
  Type                 Status    LastHeartbeatTime                 LastTransitionTime                Reason              Message
  ----                 ------    -----------------                 ------------------                ------              -------
  NetworkUnavailable   False     Mon, 01 Nov 2021 23:01:43 +0900   Mon, 01 Nov 2021 23:01:43 +0900   CalicoIsUp          Calico is running on this node
  MemoryPressure       Unknown   Fri, 05 Nov 2021 13:28:10 +0900   Fri, 05 Nov 2021 13:29:33 +0900   NodeStatusUnknown   Kubelet stopped posting node status.
  DiskPressure         Unknown   Fri, 05 Nov 2021 13:28:10 +0900   Fri, 05 Nov 2021 13:29:33 +0900   NodeStatusUnknown   Kubelet stopped posting node status.
  PIDPressure          Unknown   Fri, 05 Nov 2021 13:28:10 +0900   Fri, 05 Nov 2021 13:29:33 +0900   NodeStatusUnknown   Kubelet stopped posting node status.
  Ready                Unknown   Fri, 05 Nov 2021 13:28:10 +0900   Fri, 05 Nov 2021 13:29:33 +0900   NodeStatusUnknown   Kubelet stopped posting node status.
```

![5_nodenotready_node_high_memory_usage.jpg](/assets/images/troubleshoot/5_nodenotready_node_high_memory_usage.jpg){: width="800" }

---

- **노드 재기동**

`노드 이벤트확인`

```
oot@master ~/war_issue_docker/mantech-accordion-dockerfile-ce6f44fc6c68/tomcat]# k get event
LAST SEEN   TYPE      REASON                    OBJECT       MESSAGE
9m15s       Normal    NodeHasSufficientMemory   node/node3   Node node3 status is now: NodeHasSufficientMemory
9m15s       Normal    NodeHasNoDiskPressure     node/node3   Node node3 status is now: NodeHasNoDiskPressure
9m15s       Normal    NodeHasSufficientPID      node/node3   Node node3 status is now: NodeHasSufficientPID
8m41s       Normal    NodeReady                 node/node3   Node node3 status is now: NodeReady
9m15s       Normal    NodeNotReady              node/node3   Node node3 status is now: NodeNotReady
16m         Normal    NodeNotReady              node/node3   Node node3 status is now: NodeNotReady
8m42s       Warning   SystemOOM                 node/node3   System OOM encountered, victim process: java, pid: 22970
```