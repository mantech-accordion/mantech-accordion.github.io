---
layout: default
title: 2. CreateContainerConfigError
parent: 부록. 트러블슈팅 가이드
nav_order: 2
---

# Pending
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## CreateContainerError 상태란?

일반적으로 Secret 또는 ConfigMap이 누락된 경우 발생합니다.
- Secret은 데이터베이스 자격증명과 같은 민감한 정보를 저장하는데 사용되는 Kubernetes 개체입니다.
- ConfigMap은 데이터를 키-값 쌍으로 저장하며 일반적으로 여러 포드에서 사용하는 구성 정보를 보관하는데 사용됩니다.

---

## 문제 확인
CLI 상에서 Kubectl get pods 명령어를 실행하여 pod 상태 정보 확인
```
$ kubectl get pods 
NAME                 READY   STATUS                       RESTARTS   AGE
pod-missing-config   0/1  CreateContainerConfigError   0        1m23s
```

---

## 자세한 정보 얻기 및 문제 해결
문제에 대한 자세한 정보를 얻기 위해서, 하기 명령어를 실행해서 누락된 ConfigMap을 확인합니다.

- `kubectl describe [pod name]`

```
$ kubectl describe pod pod-missing-config 
Warning Failed 34s (x6 over 1m45s) kubelet 
Error: configmap "configmap-3" not found
```

- `kubectl get configmap [configmap name]` 명령어를 수행해서, 해당 ConfigMap이 존재하는지 확인합니다. 

```
$ kubectl get configmap configmap-3
Error from server (NotFound): configmaps "configmap-3" not found 
```

- ConfigMap이 없을 경우, 아코디언 메뉴의 [구성]->[컨피그맵]에서 생성 가능합니다. ConfigMap 관련한 상세 정보는 아래 설명서를 참고하시기 바랍니다.
(https://kubernetes.io/docs/concepts/configuration/configmap)

- ConfigMap 생성 후 Pod가 정상적으로 동작하는지 하기 명령어를 통해서 확인하면 됩니다.

```
$ kubectl get pods
NAME                 READY   STATUS    RESTARTS   AGE
pod-missing-config   0/1     Running   0          1m23s
```