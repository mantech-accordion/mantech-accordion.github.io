---
layout: default
title: 4. ImagePullBackOff & ErrImagePull
parent: 부록. 트러블슈팅 가이드
nav_order: 4
---

# ImagePullError & ErrImagePull
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

## ImagePullBackOff & ErrImagePull 상태란?

레지스트리에서 컨테이너 이미지를 가져오려고 시도했지만, 실패했기 때문에 pod를 실행할 수 없는 경우에 발생하게 됩니다.
Pod는 매니페스트에 정의된 하나 이상의 컨테이너를 생성할 수 없기 때문에 시작을 거부하게 됩니다.

---

## 일반적인 장애 증상

-	이미지가 없거나, 이미지가 문제가 있어 이미지 저장소로 부터 이미지를 가져오지 못하는 경우발생
-	이미지 이름 또는 태그가 pod 매니페스트에 잘못 입력되었을 경우
-	이미지를 검색하기 위해 레지스트를 인증할 수 없는 경우

---

## 문제확인 방법

- Pod 상태 확인

`메뉴 → 워크로드 → Pod → Overview → container`

```
catalog  war-2pod-8579c8ccfc-9ftjn   2/2  Running            0      26d
catalog  wildlfy-ha-5867c485f7-kk5zs 0/1  ImagePullBackOff   0      22d    
catalog  wildlfy-ha-77ddd8cc44-plqts 1/1  Running            0      26d
```

- Pod 이벤트 확인

`메뉴 → 워크로드 → Pod → Overview → 이벤트`

```
[root@jwkim-master ~]# k describe pod wildlfy-ha-5867c485f7-kk5zs -n catalog
Name:         wildlfy-ha-5867c485f7-kk5zs
Namespace:    catalog
Priority:     0
Node:         jwkim-master/10.60.100.5
Start Time:   Thu, 09 Jun 2022 22:06:05 +0900
Labels:       app=wildlfy-ha
              monitoring.accordions.co.kr/scouter.collector-namespace=catalog
              monitoring.accordions.co.kr/scouter.is-agent-injected=true
              pod-template-hash=5867c485f7
Annotations:  cni.projectcalico.org/podIP: 172.32.203.48/32
              cni.projectcalico.org/podIPs: 172.32.203.48/32

…
Events:
  Type    Reason   Age                    From     Message
  ----    ------   ----                   ----     -------
  Normal  BackOff  4s (x208224 over 32d)  kubelet  Back-off pulling image "10.60.100.90:30001/jungwon/wildlfy-ha-7940bcd0-640610f8:53"
```

---

## 발생할 수 있는 케이스 및 조치 방법

- 이미지가 pull에 대한 권한이 없음 → `이미지 저장소에 Pull권한이 있는 spec.imagePullSecrets을 설정`

```
Events:
  Type     Reason     Age                     From               Message
  ----     ------     ----                    ----               -------
  Normal   Scheduled  8m56s                   default-scheduler  Successfully assigned gbkim/adfe-6849cbccdd-lgpj9 to acc-node2
  Normal   Pulling    7m23s (x4 over 8m55s)   kubelet            Pulling image "10.20.200.241:30001/accregistry/adfe-877ca4fa-aa89e392:3"
  Warning  Failed     7m23s (x4 over 8m55s)   kubelet            Failed to pull image "10.20.200.241:30001/accregistry/adfe-877ca4fa-aa89e392:3": rpc error: code = Unknown desc = failed to pull and unpack image "10.20.200.241:30001/accregistry/adfe-877ca4fa-aa89e392:3": failed to resolve reference "10.20.200.241:30001/accregistry/adfe-877ca4fa-aa89e392:3": unexpected status code [manifests 3]: 401 Unauthorized
```

---

- 이미지가 존재하지 않음 → `이미지 저장소에이미지 Push`

```
Warning  Failed     83s (x3 over 2m1s)  kubelet            Failed to pull image "10.20.200.160:5000/cicd-artifact:2.0.0": rpc error: code = NotFound desc = failed to pull and unpack image "10.20.200.160:5000/cicd-artifact:2.0.0": failed to resolve reference "10.20.200.160:5000/cicd-artifact:2.0.0": 10.20.200.160:5000/cicd-artifact:2.0.0: not found

Warning  Failed     24s (x2 over 41s)  kubelet, node1     Failed to pull image "127.0.0.1:30001/accordion:1.6.1.3": rpc error: code = Unknown desc = Error response from daemon: manifest for 127.0.0.1:30001/accordion:1.6.1.3 not found: manifest unknown: manifest unknown
```

---

- 이미지 저장소로의 커넥션이 맺어지지 않음 

```
Events:
  Type     Reason     Age                     From               Message
  ----     ------     ----                    ----               -------
 Warning  Failed   36m (x7 over 59m)     kubelet, master  Failed to pull image "127.0.0.1:30001/postgres": rpc error: code = Unknown desc = Error response from daemon: Get http://127.0.0.1:30001/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
  Warning  Failed   31m (x89 over 61m)    kubelet, master  Error: ImagePullBackOff
  Normal   BackOff  115s (x208 over 61m)  kubelet, master  Back-off pulling image "127.0.0.1:30001/postgres"
```

  - telnet, netstat 등 네트워크 분석 도구를 통해서 이미지 저장소 상태확인

    - telnet 확인

    ```
    [root@master /var/log]# telnet 127.0.0.1 30001  
    Trying 127.0.0.1...
    ```

    - netstat 확인

    ```
    [root@master ~]# netstat -nao | grep 30001
    tcp        0      1 127.0.0.1:62542         127.0.0.1:30001         SYN_SENT    on (0.79/2/0)
    tcp6       1      0 :::30001                :::*                    LISTEN      off (0.00/0/0)
    tcp6      50      0 ::1:30001               ::1:4522                CLOSE_WAIT  off (0.00/0/0)
    ```

    - kube-proxy 확인

    ```
    I0521 07:00:55.180954       1 graceful_termination.go:161] Trying to delete rs: 10.96.1.11:8086/TCP/192.168.0.231:8086
    I0521 07:00:55.180983       1 graceful_termination.go:175] Deleting rs: 10.96.1.11:8086/TCP/192.168.0.231:8086
    I0521 07:00:58.827261       1 graceful_termination.go:161] Trying to delete rs: 10.104.151.183:80/TCP/192.168.1.99:3000
    I0521 07:00:58.827296       1 graceful_termination.go:175] Deleting rs: 10.104.151.183:80/TCP/192.168.1.99:3000
    ```

---

- 이미지를 OCI이미지가 아닌 docker타입 이미지로 변경하여 이미지 서버에 push하여 조치

```
Warning  Failed     48m (x4 over 49m)     kubelet            Error: ImagePullBackOff
  Normal   Pulling    47m (x4 over 49m)     kubelet            Pulling image "10.60.100.5:30001/accregistry/wildlfy-ha-7940bcd0-640610f8:44"
  Warning  Failed     47m (x4 over 49m)     kubelet            Failed to pull image "10.60.100.5:30001/accregistry/wildlfy-ha-7940bcd0-640610f8:44": rpc error: code = Unknown desc = failed to pull and unpack image "10.60.100.5:30001/accregistry/wildlfy-ha-7940bcd0-640610f8:44": failed to extract layer sha256:869989761eb2743b74259c959501a857c9a3f9a97aaea261f99eb08b427099d7: archive/tar: invalid tar header: unknown
```

  - podman push -f v2s2 수행

  ```
  [root@jwkim-master /data/kaniko]# podman push -f v2s2 10.60.100.5:5000/accordion/wildfly-jdk11:26.0.1
  Getting image source signatures
  Copying blob 3fbe1e874b0d done  
  Copying blob 869989761eb2 done  
  Copying blob 115463be137a done  
  Copying blob 613be09ab3c0 done  
  Copying blob 81779498fa28 done  
  Copying config f6318faddb [======================================] 10.9KiB / 10.9KiB
  Writing manifest to image destination
  Storing signatures
  ```
