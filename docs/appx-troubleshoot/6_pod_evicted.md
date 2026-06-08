---
layout: default
title: 6. PodEvicted
parent: 부록. 트러블슈팅 가이드
nav_order: 6
---

# PodEvicted
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## PodEvicted 상태란?

Node의 자원부족 등 기타문제로 인하여  NotReady상태가 되면 노드에 호스팅된 Pod들이 축출(eviction)되기 시작합니다.
이러한 경우 어떻게 확인하고 조치하는지 확인합니다.

![6_pod_evicted_diagram.jpg](/assets/images/appx/troubleshoot/6_pod_evicted_diagram.jpg){: width="800" }

---

## 일반적인 장애 증상

-	kubelet이 사용할 수 있는 가용 메모리가 설정값보다 부족하여 pod evicted가 발생하는 경우
-	kubelet이 사용하는 파일 시스템이 부족하여 pod evicted가 발생하는 경우
-	image 파일시스템이 저장되는 공간에 disk가 부족하여 공간 확보를 위해 pod evicted가 발생하는 경우



---

## 문제확인 방법

- 노드 상태 확인

  - `Case 1 : Disk 자원이 부족한 경우`

  - `Case 2 : Memory 자원이 부족할 경우`

  ```
  acc-global    keycloak-db-79cc499cf7-rk9xj                    0/1     Error                         0              34d     172.32.193.106   p-iskim-worker2   <none>           <none>
  acc-system    auth-manager-67bd9f95c6-xlvlm                   0/1     Evicted                       0              34d     <none>           p-iskim-worker2   <none>           <none>
  acc-system    cicd-manager-686c77f645-chvnr                   0/1     Error                         3              34d     172.32.193.104   p-iskim-worker2   <none>           <none>
  acc-system    log-server-69cd8fb57d-nxtdl                     0/1     Error                         0              34d     172.32.193.107   p-iskim-worker2   <none>           <none>
  iskim         hostname-deployment-7dfd748479-wblpz            0/1     Error                         0              7d6h    172.32.193.65    p-iskim-worker2   <none>           <none>
  iskim         test-quota-65bcb465f-f8ph6                      0/1     Init:ContainerStatusUnknown   0              12m     172.32.193.79    p-iskim-worker2   <none>           <none>
  iskim         test-quota-65bcb465f-jc8bh                      0/1     Error                         0              12m     172.32.193.70    p-iskim-worker2   <none>           <none>
  iskim         test-quota-65bcb465f-n85cv                      0/1     Error                         0              12m     172.32.193.72    p-iskim-worker2   <none>           <none>
  iskim         test-quota-65bcb465f-tcztt                      0/1     Error                         0              12m     172.32.193.73    p-iskim-worker2   <none>           <none>
  iskim         test-quota-65bcb465f-z4h78                      0/1     Error                         0              12m     172.32.193.82    p-iskim-worker2   <none>           <none>
  ```

  ```
  Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                         Message
  ----                 ------  -----------------                 ------------------                ------                         -------
  NetworkUnavailable   False   Fri, 03 Jun 2022 15:58:22 +0900   Fri, 03 Jun 2022 15:58:22 +0900   CalicoIsUp                     Calico is running on this node
  MemoryPressure       True    Wed, 13 Jul 2022 22:57:08 +0900   Wed, 13 Jul 2022 22:56:58 +0900   KubeletHasInsufficientMemory   kubelet has insufficient memory available
  DiskPressure         False   Wed, 13 Jul 2022 22:57:08 +0900   Wed, 13 Jul 2022 22:51:43 +0900   KubeletHasNoDiskPressure       kubelet has no disk pressure
  PIDPressure          False   Wed, 13 Jul 2022 22:57:08 +0900   Wed, 13 Jul 2022 22:51:43 +0900   KubeletHasSufficientPID        kubelet has sufficient PID available
  Ready                True    Wed, 13 Jul 2022 22:57:08 +0900   Wed, 13 Jul 2022 22:56:49 +0900   KubeletReady                   kubelet is posting ready status

  Warning  SystemOOM                  21m                   kubelet  System OOM encountered, victim process: stress, pid: 14281
  Warning  SystemOOM                  5m30s                 kubelet  System OOM encountered, victim process: stress, pid: 32120
  Normal   NodeHasSufficientMemory    5m27s (x24 over 40d)  kubelet  Node p-iskim-worker2 status is now: NodeHasSufficientMemory
  Normal   NodeHasNoDiskPressure      5m27s (x24 over 40d)  kubelet  Node p-iskim-worker2 status is now: NodeHasNoDiskPressure
  Normal   NodeHasSufficientPID       5m27s (x24 over 40d)  kubelet  Node p-iskim-worker2 status is now: NodeHasSufficientPID
  Normal   NodeNotReady               5m27s                 kubelet  Node p-iskim-worker2 status is now: NodeNotReady
  Normal   NodeReady                  5m17s (x2 over 40d)   kubelet  Node p-iskim-worker2 status is now: NodeReady
  Warning  SystemOOM                  4m16s                 kubelet  System OOM encountered, victim process: stress, pid: 1614
  Warning  SystemOOM                  3m54s                 kubelet  System OOM
  .. 
  ```

  ```
  NAME              CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
  p-iskim-master    920m         11%    11482Mi         72%
  p-iskim-worker1   90m          2%     2957Mi          38%
  p-iskim-worker2   368m         9%     5148Mi          108%
  ```


- 조치 방법
    - 모두 노드의 자원이 부족해 발생하는 케이스이므로, 노드의 자원을 증설한다.