---
layout: default
title: 2.2 Accordion ์ํคํ์ณ
nav_order: 2
parent: 2. Accordion v2
---

# 2.2 ๐ ์์ฝ๋์ธv2 ์ํคํ์ณ
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


## ๐ ์์ฝ๋์ธv2 ์ํคํ์ณ

์์ฝ๋์ธ์ ๋ง์คํฐ ๋ธ๋ 3๋, ์ธํ๋ผ ๋ธ๋(๊ถ๊ณ ), ์์ปค๋ธ๋๋ก ๊ตฌ์ฑ๋์ด ์์ต๋๋ค.

![acc-5.png](/assets/images/accordion/acc-5.png)

Master Node : Kuberentes ๋ฅผ ์ ์ดํ๋ ๋ธ๋
Infra Node : ํด๋ฌ์คํฐ๋ฅผ ๊ด๋ฆฌ๋ฅผ ์ํ CI/CD, Logging, Monitoring, Alarm ๊ธฐ๋ฅ์ ์ํ ๋ธ๋
Worker Node : ์ ํ๋ฆฌ์ผ์ด์ ์๋น์ค ๋ธ๋


---

## ๋ฉํฐํด๋ฌ์คํฐ ํตํฉ ๊ด๋ฆฌ
๊ฐ๋ฐ, ํ์คํธ, ์ด์, ์ธ๋ถ ํด๋ผ์ฐ๋, ๋ถ๋ฆฌ๋ ๋คํธ์ํฌ ๋ง ๋ฑ์ ๋ถ์ฐ ๊ตฌ์ฑ๋ ํด๋ฌ์คํฐ๋ฅผ ๋จ์ผ ์ฝ์๋ก ๊ด๋ฆฌํ  ์ ์๊ณ  Host Cluster๋ฅผ ํตํด K8S ํด๋ฌ์คํฐ๋ฅผ ๋งค์ฐ ์ฝ๊ฒ ๋ฐฐํฌํ  ์ ์์ต๋๋ค.

![acc-7.png](/assets/images/accordion/acc-7.png)


**ํด๋ฌ์คํฐ๋ณ ์ญํ **

- Host Cluster : ๋ฉํฐํด๋ฌ์คํฐ๋ฅผ ๊ด๋ฆฌ์ฉ ํด๋ฌ์คํฐ๋ก Cluster๊ฐ ์ฑ ๋ฐฐํฌ, ํตํฉ ๊ถํ ์ค์  ๋ฐ ๋ชจ๋ํฐ๋ง ๊ธฐ๋ฅ์ ์ ๊ณตํฉ๋๋ค.
- Member Cluster : ์๋น์ค ํด๋ฌ์คํฐ๋ก ํด๋ฌ์คํฐ๋ง๋ค ์ค์น๋ ์์ด์ ํธ๋ฅผ ํตํด Host Cluster์ ๋ช๋ น์ ๋ฐ๊ฒ ๋ฉ๋๋ค.


---

## CI/CD pipeline
๋ด์ฅ๋ CI/CD pipeline์ ํตํด ๋ฉํฐ ํด๋ผ์ฐ๋ ํ๊ฒฝ์์ ์ปจํ์ด๋๋ฅผ ์ฝ๊ฒ ๋ฐฐํฌํ  ์ ์์ต๋๋ค.

![acc-10.png](/assets/images/accordion/acc-10.png)

---

## ์์ฝ๋์ธ ์๋ฃจ์ ๊ตฌ์ฑ

์์ฝ๋์ธ์ ์ธ์ฆ, CI/CD, ์ปจํ์ด๋ ๊ด๋ฆฌ, ๋ชจ๋ํฐ๋ง ๋ชจ๋๋ก ๊ตฌ์ฑ๋์ด ์์ต๋๋ค.

![acc-6.png](/assets/images/accordion/acc-6.png)

**Identity Module**

์ฌ์ฉ์ ๊ด๋ฆฌ ์ธ์ฆ/์ธ๊ฐ ์ ๊ทผ์ ์ด SSO

- Keycloak
- Accordion Gateway

**CI/CD Automation Module**

- Argo CI/CD
- Docker Registry / Harbor

**Container Orchestration**

- Kubernetes
- Calcio/Weave
- Accordion console
- Catalog & Helm
- Istio & Kiali

**Observability Module**

- Thanos & Prometheus
- FileBeat & Opensearch
- Scouter APM