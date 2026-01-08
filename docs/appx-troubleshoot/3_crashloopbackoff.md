---
layout: default
title: 3. CrashLoopBackOff
parent: 부록. 트러블슈팅 가이드
nav_order: 3
---

# CrashLoopBackOff
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

<div class="code-example" markdown="1">
Cluster
{: .label .label-blue }
</div>

## CrashLoopBackOff 상태란?

Pod가 기동 중에 컨테이너 실행 오류가 발생하면  crashloCrashLoopBackOff 오류가 발생합니다.
이러한 상황이 발생할 경우 조치 방법을 알아봅니다.

---

## 일반적인 장애증상 

-	응용 프로그램을 기동 중에 오류가 발생하여 종료되는 경우
-	응용 프로그램을 시작하지 못하게 하는 응용 프로그램에 버그
-	컨테이너가 올바르게 구성되지 않음
-	활성 프로브가 너무 많이 실패

---

## 문제확인 방법

- Pod 상태 확인

![3_crashloopbackoff_pod_status.png](/assets/images/troubleshoot/3_crashloopbackoff_pod_status.png){: width="800" }


- Pod 이벤트 확인

![3_crashloopbackoff_event.png](/assets/images/troubleshoot/3_crashloopbackoff_event.png){: width="800" }


- pod 다운 전/후 로그 확인 : 컨테이너의 이미지 종류와 기동 환경에 따라 매우 다양하게 발생될 수 있으므로, 애플리케이션 로그를 확인하여 조치

  - `Case 1 : DB 연결되지 않았을 경우`

```
04:30:41,657 INFO  [org.keycloak.connections.infinispan.DefaultInfinispanConnectionProviderFactory] (ServerService Thread Pool -- 61) Node name: acc-keycloak-0, Site name: null
04:30:42,098 WARN  [org.jboss.jca.core.connectionmanager.pool.strategy.OnePool] (ServerService Thread Pool -- 61) IJ000604: Throwable while attempting to get a new connection: null: javax.resource.ResourceException: IJ031084: Unable to create connection
..
Caused by: java.lang.RuntimeException: Failed to connect to database
        at org.keycloak.keycloak-model-jpa@8.0.1//org.keycloak.connections.jpa.DefaultJpaConnectionProviderFactory.getConnection(DefaultJpaConnectionProviderFactory.java:372)
        at org.keycloak.keycloak-model-jpa@8.0.1//org.keycloak.connections.jpa.updater.liquibase.lock.LiquibaseDBLockProvider.lazyInit(LiquibaseDBLockProvider.java:65)
```

  - `Case 2 : acc-prometheus-operator-prometheus.monitoring 호스트를 찾지 못함 → 네임서버문제로 확인 → 조치 후 정상동작`

```
[root@master /var]# k logs pod/accordion-router-757c7cf689-qxs5l -n accordion
2020/05/12 10:13:12 [warn] 1#1: the "ssl" directive is deprecated, use the "listen ... ssl" directive instead in /nginx.conf:38
nginx: [warn] the "ssl" directive is deprecated, use the "listen ... ssl" directive instead in /nginx.conf:38
2020/05/12 10:13:12 [warn] 1#1: duplicate MIME type "text/html" in /nginx.conf:106
nginx: [warn] duplicate MIME type "text/html" in /nginx.conf:106
2020/05/12 10:13:12 [warn] 1#1: duplicate MIME type "text/html" in /nginx.conf:132
nginx: [warn] duplicate MIME type "text/html" in /nginx.conf:132
2020/05/12 10:13:12 [emerg] 1#1: host not found in upstream "acc-prometheus-operator-prometheus.monitoring" in /nginx.conf:98
nginx: [emerg] host not found in upstream "acc-prometheus-operator-prometheus.monitoring" in /nginx.conf:98
```

  - `Case 3 : lock.dat파일이 존재하여 애플리케이션 기동 시 오류 발생 → 볼륨마운트되어 있는 lock.dat파일 제거 후 정상동작`

```
[root@jwkim-master ~]#kubectl -n apm  logs -p scouter-server-7579c6f47b-sqbpl  server
...
 20:06:13.303 [main]  INFO  org.eclipse.jetty.server.session[369] - No SessionScavenger set, using defaults
 20:06:13.304 [main]  INFO  org.eclipse.jetty.server.session[149] - Scavenging every 600000ms
Can't lock the database
Please remove the lock : /opt/server/./database/lock.dat
java -cp ./boot.jar scouter.boot.Boot [./lib]
```
