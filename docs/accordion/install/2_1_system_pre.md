---
layout: default
title: 2.4.1 π»μμ€ν μκ΅¬μ¬ν­
nav_order: 4
parent: 2.4 Accordion μ€μΉ
grand_parent: 2. Accordion v2
---

# 2-4-1 μμ€ν μκ΅¬μ¬ν­
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## π»μμ€ν μκ΅¬μ¬ν­
- Accordion μλ²λ κ°μ μμ€ν λλ λ¬Όλ¦¬ μμ€ν μ€λΉ
- Accordion μλ²λ₯Ό μ€μΉνκΈ° μν΄μλ Master μλ² 3λμ μλΉμ€λ₯Ό μν ν  Node μλ² 2λ μ΄μ μμ΄μΌ ν©λλ€.
- Master, Node μλ² κ°μλ λ€νΈμν¬ μ¬μ μμ μ€λΉ
- Master μλ²μ Node μλ² κ°μ ssh μ κ·Όμ΄ κ°λ₯ν΄μΌ ν©λλ€. (root μ μνμ)
- Master/Node μλ² μκ°μ λμΌν΄μΌ ν©λλ€.


**OS μ΄μμ²΄μ **
- CentOS : 7.4 μ΄μ (CentOS 8 λ―Έμ§μ)
- RHEL : 7.4 λλ 8.4 μ΄μ
- Ubuntu: 18.04 μ΄μ
- OracleLinux: 7.4 λλ 8.4 μ΄μ
- RHCK μ»€λ μ§μ, UEK μ»€λ λ―Έμ§μ

**λΈλλ³ μμ€ν μκ΅¬μ¬ν­**

| κ΅¬λΆ        | CPU | MEMORY | DISK |
|:-------------|:------------------|:------|:------|:------|
| Master       | μ΅μ 8core | μ΅μ 16GB  | μ΅μ 500GB |
| Node(s)      | μ΅μ 8core   | μ΅μ 16GB | μ΅μ 300GB |

---

## π‘ μμ½λμΈ λ°©νλ²½(calico v2)

**λ§μ€ν°λΈλ**

| Protocol	| Direction | 	Port Range |	Source	| Destination | Purpose |
|:----------|:----------|:-------------|:-----------|:------------|:--------|
| TCP	| Inbound	| 6443	| μμ»€λΈλ	| λ§μ€ν°λΈλ	| Kubernetes API server |
| TCP	| Inbound	| 2379-2380	| μμ»€λΈλ	| λ§μ€ν°λΈλ	| etcd server client API |
| TCP	| Inbound	| 10250	| μμ»€λΈλ	| λ§μ€ν°λΈλ	| Kubelet API |
| TCP	| Inbound	| 10251	| μμ»€λΈλ	| λ§μ€ν°λΈλ	| kube-scheduler |
| TCP	| Inbound	| 10252	| μμ»€λΈλ	| λ§μ€ν°λΈλ	| kube-controller-manager |
| TCP	| Inbound	| 10255	| μμ»€λΈλ	| λ§μ€ν°λΈλ	| worker node kubelet healthcheck read only |
| TCP	| Inbound	| 30000-32767	| ν΄λΌμ΄μΈνΈ	| λ§μ€ν°λΈλ	| NodePort Services |
| TCP	| Inbound	| 179	| λͺ¨λ μλ²	| λͺ¨λ μλ²	| Calico networking (BGP) |
| UDP	| Inbound	| 4789	| λͺ¨λ μλ²	| λͺ¨λ μλ²	| Calico networking with VXLAN enabled |
| Protocol	| Inbound	| 4	| λͺ¨λ μλ²	| λͺ¨λ μλ²	| Calico networking IP in IP (encapsulation) |
| TCP	| Inbound	| 5473	| λͺ¨λ μλ²	| λͺ¨λ μλ²	| Calico networking with Typha enabled |
| TCP	| Inbound	| 9100	| λͺ¨λ μλ²	| λͺ¨λ μλ²	| node exporter |
| TCP	| Inbound	| 8443	| μμ»€λΈλ	| λ§μ€ν°λΈλ	| Kubernetes API server(λ§μ€ν°λΈλ 3μ€ν) |
| TCP	| Inbound	| 5000	| μμ»€λΈλ	| λ§μ€ν°λΈλ	| Accordion local Infra-registry |
					
**μμ»€λΈλ**

| Protocol	| Direction | 	Port Range |	Source	| Destination | Purpose |
|:----------|:----------|:-------------|:-----------|:------------|:--------|
| TCP	| Inbound	| 10250	| λ§μ€ν°λΈλ	| μμ»€λΈλ | Kubelet API | 
| TCP	| Inbound	| 10255	| λ§μ€ν°λΈλ	| μμ»€λΈλ	| worker node kubelet healthcheck read only | 
| TCP	| Inbound	| 30000-32767	| ν΄λΌμ΄μΈνΈ	| μμ»€λΈλ	| NodePort Services | 
| TCP	| Inbound	| 179	| λͺ¨λ μλ²	| λͺ¨λ μλ²	| Calico networking (BGP) | 
| Protocol	| Inbound	| 4	| λͺ¨λ μλ²	| λͺ¨λ μλ²	| Calico networking IP in IP (encapsulation) | 
| UDP	| Inbound	| 4789	| λͺ¨λ μλ²	| λͺ¨λ μλ²	| Calico networking with VXLAN enabled | 
| TCP	| Inbound	| 5473	| λͺ¨λ μλ²	| λͺ¨λ μλ²	| Calico networking with Typha enabled | 
| TCP	| Inbound	| 9100	| λͺ¨λ μλ²	| λͺ¨λ μλ²	| node exporter | 
| TCP	| Inbound	| 80	| ν΄λΌμ΄μΈνΈ	| μμ»€λΈλ	| App service(nginx-ingress Hostport) | 
| TCP	| Inbound	| 443	| ν΄λΌμ΄μΈνΈ	| μμ»€λΈλ	| App service(nginx-ingress Hostport ) | 
					
**μμ½λμΈμμ μ¬μ©νλ NodePort λ¦¬μ€νΈ**					

| Protocol	| Direction | 	Port Range | Purpose |
|:----------|:----------|:-------------|:--------|
| TCP	| Inbound	| 30000			| Accordion console |
| TCP	| Inbound	| 30001			| user-registry |
| TCP	| Inbound	| 30002			| thanos-store-gateway | 
| TCP	| Inbound	| 30003			| thanos-sidecar | 
| TCP	| Inbound	| 30443			| member-agent | 

