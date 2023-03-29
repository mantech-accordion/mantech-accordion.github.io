---
layout: default
title: 2.3 Accordion 메뉴
nav_order: 3
parent: 2. Accordion v2
---
# 2.3 Accordion 메뉴
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

아코디언v2은 멀티클러스터 구조를 위한 글로벌, 클러스터, 네임스페이스 각 스코프를 통합한 메뉴가 존재합니다.

---
## 🧐 스코프

아코디언 메뉴는 3가지 영역으로 구분하고 있습니다.

- 글로벌
- 클러스터
- 네임스페이스

![acc-navi-1.png](/assets/images/accordion/acc-navi-1.png)

클러스터 메뉴와 네임스페이스 메뉴 선택에 따라 세부 메뉴 구성이 변경됩니다.

![acc-navi-2.png](/assets/images/accordion/acc-navi-2.png)


---

## 글로벌 메뉴

아코디언으로 구성된 호스트/멤버 클러스터를 관리하기 위한 메뉴입니다.

![acc-navi-3.png](/assets/images/accordion/acc-navi-3.png)

- 글로벌 대시보드

- 클러스터

- 헬름

- 계정

  | 항목              | 설명                                                         |
  | :---------------- | ------------------------------------------------------------ |
  | 사용자            | 아코디언 웹 콘솔에 접근하기 위한 계정 정보(ID/PW)를 생성하는 메뉴 |
  | 그룹              | 특정 용도(dev, stg, prd)에 맞게 사용자을 통합할 때 사용하는 메뉴 |
  | 글로벌 권한       | 글로벌 메뉴 및 클러스터, 네임스페이스 메뉴에 대한 권한 지정 가능 |
  | 클러스터 권한     | 클러스터/네임스페이스 메뉴에 대한 권한 지정 가능             |
  | 네임스페이스 권한 | 네임스페이스 메뉴에 대한 권한 지정 가능                      |
  | 글로벌 멤버       | 생성한 글로벌 권한에 대해 사용자 및 그룹 지정 가능           |
  | 사용자 접속 로그  | 로그인한 사용자에 대한 로그 확인 가능                        |

- 글로벌설정

  | 항목      | 설명                                                     |
  | --------- | -------------------------------------------------------- |
  | 활성화 키 | 아코디언에 사용된 활성화 키 확인(Subscription/Perpetual) |
  | 메일 서버 | 알람에 사용 될 메일 서버 등록 가능                       |

  

---

## 클러스터 메뉴

클러스터 범위에서 접근 가능한 리소스를 관리하기 위한 메뉴입니다.

![acc-navi-4.png](/assets/images/accordion/acc-navi-4.png)

- 클러스터 대시보드
- 네임스페이스
- 노드
- 애플리케이션

| 항목                     | 설명                                                         |
| ------------------------ | ------------------------------------------------------------ |
| 헬름                     | 헬름 저장소 등록 후 해당 저장소에 있는 앱들을 배포하는 메뉴  |
| 클러스터 카탈로그 템플릿 | 클러스터 레벨에서 공통으로 사용할 수 있는 카탈로그를 생성할 때 사용 |

- 빌드

| 항목                       | 설명                                           |
| -------------------------- | ---------------------------------------------- |
| 클러스터 테스크 템플릿     | 클러스터에서 공통으로 사용할 템플릿을 관리     |
| 클러스터 파이프라인 템플릿 | 클러스터에서 공통으로 사용할 파이프라인을 관리 |

- 워크로드

| 항목              | 설명                                        |
| ----------------- | ------------------------------------------- |
| 워크로드 대시보드 | 클러스터에 배포된 워크로드 상태 정보를 제공 |
| 파드              | 배포된 파드 정보를 제공                     |
| 디플로이먼트      | 배포된 디플로이먼트 정보를 제공             |
| 스테이트풀셋      | 배포된 스테이트풀셋 정보를 제공             |
| 데몬셋            | 배포된 데몬셋 정보를 제공                   |
| 잡                | 배포된 잡 정보를 제공                       |
| 크론잡            | 배포된 크론잡 정보를 제공                   |

- 구성

| 항목       | 설명                                                         |
| ---------- | ------------------------------------------------------------ |
| 컨피그맵   | 애플리케이션에 적용 할 구성요소 정보를 저장(기밀이 아닌 평문 데이터를 저장) |
| 시크릿     | 비밀번호, OAuth 토큰, Docker Registry, SSH키와 같은 민감 정보를 저장(base64로 인코딩하여 저장) |
| HPA        | 파드를 수평적으로 확장/관리하는 쿠버네티스 리소스            |
| 리밋레인지 | 네임스페이스에 대한 리소스 제한을 설정                       |

- 네트워크

| 항목            | 설명                                                         |
| --------------- | ------------------------------------------------------------ |
| 서비스          | 애플리케이션 및 워크로드의 파드 집합에 단일 DNS를 부여하고 로드밸런싱을 수행 |
| 인그레스        | 클러스터 외부에서 클러스터 내부 서비스로 HTTP와 HTTPS 경로 노출 및 외부에서 서비스 접속이 가능한 URL, <br />로드 밸런스 트래픽, /SSL/TLS 종료 그리고 이름-기반의 가상 호스팅을 제공하도록 구성이 가능 |
| 네트워크 폴리시 | 네트워크 트래픽 제어를 위해 트래픽에 대한 규칙을 정의        |

- 스토리지

| 항목                 | 설명                                                         |
| -------------------- | ------------------------------------------------------------ |
| 퍼시스턴트볼륨       | 프로비저닝 드라이버 또는 스토리지 클래스를 사용하여 동적으로 생성한 클러스터의 스토리지 |
| 퍼시스턴트볼륨클레임 | 사용자 스토리지에 대한 요청으로  퍼시스턴트볼륨 리소스를 사용하며 특정 크기 및 접근 모드를 요청 가능 |
| 스토리지클래스       | 스토리지클래스에 속하는 퍼시스턴트볼륨을 동적으로 생성       |

- 커스텀 리소스
- 접근제어

| 항목                 | 설명                                                         |
| -------------------- | ------------------------------------------------------------ |
| 롤                   | 특정 네임스페이스의 API나 리소스에 대한 권한을 명시          |
| 롤바인딩             | 특정 네임스페이스에 롤/클러스터롤과 서비스어카운트를 연결해주며 지정된 서비스어카운트들이 명시된 롤을 사용할 수 있도록 함 |
| 클러스터 롤          | 클러스터의 API나 리소스에 대한 권한을 명시(네임스페이스 보다 상위 권한) |
| 클러스터 롤 바인딩   | 클러스터롤과 서비스어카운트를 연결해주며 지정된 서비스어카운트들이 명시된 클러스터롤을 사용할 수 있도록 함 |
| 서비스어카운트       | 쿠버네티스 API 접근 시 파드의 권한을 식별하기 위한 리소스    |
| 파드 시큐리티 폴리시 | 파드 명세의 보안 관련 측면을 제어하는 클러스터-레벨의 리소스이다. 파드 생성 및 업데이트에 대한 세분화된 권한을 부여 |

- 모니터링

| 항목          | 설명                                                         |
| ------------- | ------------------------------------------------------------ |
| 시스템        | 전체 네임스페이스에 대해 CPU, MEMORY, 네트워크 사용량 확인 가능 |
| 이벤트 로그   | 클러스터에서 발생하는 쿠버네티스 이벤트 로그를 제공          |
| 컨테이너 로그 | 클러스터에 배포된 컨테이너에서 발생한 로그를 제공            |
| 감사 로그     | 클러스터에 배포된 쿠버네티스 리소스에 대한 접근 감사 로그를 제공 |
| 서비스 메시   | Kubernetes 클러스터 내의 서비스에 대한 트래픽 관리, 로드 밸런싱, 서비스 검색, 관찰 및 보안과 같은 기능을 제공 |
| 알림          | 클러스터/네임스페이스 알림 정책의 총 알림 발생 횟수 정보를 차트로 제공 |

- 설정

| 항목                       | 설명                                                         |
| -------------------------- | ------------------------------------------------------------ |
| 클러스터/네임스페이스 멤버 | 생성한 클러스터/네임스페이스 권한에 대해 사용자 및 그룹 지정 가능 |
| 알림 정책                  | Node, Node Selector, Expression을 통해 임계치 설정 후 등록한 메일 서버, SMS, 슬랙을 통해 알림 전송 |
| 레지스트리                 | 클러스터에 생성된 레지스트리 조회 가능                       |
| 슬랙                       | 슬랙을 통해 알림을 받을 수 있도록 슬랙 정보를 설정           |

---

## 네임스페이스 메뉴

특정 클러스터의 네임스페이스 범위에서 접근 가능한 리소스를 관리하기 위한 메뉴입니다.

![acc-navi-5.png](/assets/images/accordion/acc-navi-5.png)

- 네임스페이스 대시보드
- 애플리케이션

| 항목     | 설명                                                         |
| -------- | ------------------------------------------------------------ |
| 헬름     | 헬름 저장소 등록 후 해당 저장소에 있는 앱들을 배포하는 메뉴  |
| 카탈로그 | 클러스터 레벨에서 공통으로 사용할 수 있는 카탈로그를 생성할 때 사용 |

- 빌드

| 항목          | 설명                                                         |
| ------------- | ------------------------------------------------------------ |
| 파이프라인    | 생성한 테스크를 조합하여 조건에 맞게 순차적으로 빌드를 수행  |
| 승인          | 등록한 승인자에 대해 승인/거절을 통해 파이프라인 진행 및 중지 가능 |
| 테스크 템플릿 | 파이프라인 작성시 사용하는 테스크에 대한 템플릿을 관리       |

- 워크로드
- 구성
- 네트워크
- 스토리지
- 접근제어
- 모니터링
- 설정
