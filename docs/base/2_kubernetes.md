---
layout: default
title: 1-2. Kubernetes
nav_order: 2
parent: 1. Container & Kubernetes
---

# 1-2. Kubernetes
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 컨테이너 오케스트레이션이란?

엔터프라이즈 환경에서 오케스트레이터를 사용하면 컨테이너를 적절하게 관리할 수 있으며 결과적 으로 애플리케이션 배포 및 리소스 관리와 관련된 활동 을 자동화 하여 IT 팀의 생산성을 향상시킬 수 있습니다. 예를 들어, 자동 크기 조정 및 로드 밸런싱 서비스는 리소스 사용을 최적화하여 필요한 곳에 할당합니다.         마찬가지로 오케스트레이터는 기술적인 문제로 인해 작업이 실패한 애플리케이션 컨테이너의 재시작을 자동화하여 비즈니스 연속성을 제공하는 프로세스의 속도를 보장할 수 있습니다. IT 팀은 중앙 집중식 콘솔을 통해 서비스 상태와 리소스 사용에 대한 즉각적인 가시성을 얻을 수 있습니다.

리소스 프로비저닝 을 표준화하고 자동화 하는 기능은 개발 팀의 작업을 효율적 으로 만들고 가장 적절한 리소스로 애플리케이션 소프트웨어의 발전을 동반할 수 있도록 합니다. 비즈니스 측면에서 Kubernetes를 사용하면 새로운 소프트웨어 프로젝트의 출시 시간을 단축 할 수 있을 뿐만 아니라 사이버 공격과 같은 경우 피해를 최소화 할 수 있습니다. 이러한 이점은 클라우드 네이티브 애플리케이션 뿐만 아니라 모놀리식 레거시 소프트웨어에서 마이크로서비스로 의 프로젝트 마이그레이션 컨텍스트에서도 찾을 수 있습니다.


## 컨테이너 오케스트레이션(Container Orchestration)

- 컨테이너 자동 배치 및 복제
- 컨테이너 그룹에 대한 로드 밸런싱
- 컨테이너 장애 복구
- 클러스터 외부에 서비스 노출
- 컨테이너 추가 또는 제거로 확장 및 축소
- 컨테이너 서비스간의 인터페이스를 통한 연결 및 네트워크 포트 노출 제어

![base-2.png](/assets/images/base/base-2.png)

---


모듈식의 확장 가능한 데이터 센터가 클라우드 서비스 와 공존 하여 필요한 성능과 가용성을 보장할 수 있는 현대 기업 IT에서 가장 자주 반복되는 단어 중 하나는 Kubernetes입니다. 그것은 무엇에 관한 것입니까? Kubernetes는 애플리케이션을 실행하는 데 사용되는 컨테이너의 배포, 관리, 위치 지정, 크기 조정 및 라우팅과 같은 작업을 자동화하기 위한 오픈 소스 오케스트레이션 시스템입니다 .

컨테이너 는 다른 컨테이너 및 운영 체제 자체에서 격리된 응용 프로그램 및 해당 종속성의 집합입니다 . 컨테이너는 온프레미스와 클라우드 시스템 간에 한 서버에서 다른 서버로 이동할 수 있는 논리적 구조입니다. 물리적 화물 컨테이너가 크레인으로 트럭에서 항공 또는 해상 화물로 이동할 수 있는 것과 정확히 같습니다. Kubernetes는 가장 유용하거나 편리한 위치로 워크로드를 쉽게 이동할 수 있도록 하는 오케스트레이터(즉, 크레인)입니다.

 ---

## Kubernetes의 탄생과 기능
오케스트레이터 Kubernetes는 처음 에 컨테이너 기술을 활용하는 관리자의 능력을 향상시키기 위한 목적으로 Google 엔지니어가 설계했습니다. 검색 엔진의 거인 은 매주 20억 개의 컨테이너 배포를 관리해야 했습니다. 이전에는 Kubernetes의 기반이 된 기술인 Borg 를 사용하여 자동화된 작업 이었습니다.

" k8s " 및 " kube " 라고도 알려진 Kubernetes는 초기 Google 코드에 Red Hat 의 다른 기여를 추가했습니다 . 현재 Cloud Native Computing Foundation에서 관리하는 오픈 소스 프로젝트입니다. 사실 Kubernetes는 컨테이너 작업 자동화를 위한 가장 일반적인 오픈 소스 플랫폼입니다.

오픈 소스 커뮤니티에서 개발한 확장 기능과 다른 프로젝트의 기여 덕분에 Kubernetes는 이제 네트워크, 스토리지, 보안, 원격 측정 및 기타 서비스와 통합할 수 있습니다. 애플리케이션 확장성을 배포하고 보장하는 데 필요한 대부분의 수동 프로세스를 제거하는 완전한 컨테이너 인프라를 지원합니다.

 
## Kubernetes란 무엇이며 구성 요소가 작동하는 방식
Kubernetes는 애플리케이션 소프트웨어가 호스팅되는 컨테이너를 관리하기 위한 도구 인 Docker 컨테이너 오케스트레이터입니다. 이를 통해 IT 팀 은 운영을 표준화하고 소프트웨어 관리에서 추상화의 기준을 높일 수 있습니다 . Kubernetes는 애플리케이션의 연속성과 성능을 보장합니다.

Kubernetes의 기본 구성 요소를 살펴보겠습니다.

컨테이너 . 응용 프로그램을 포함하는 소프트웨어 단위입니다. 모든 코드와 해당 종속성을 패킹함으로써 애플리케이션은 운영 체제 및 기타 소프트웨어 구성 요소에서 독립하게 됩니다.

도커 . 가상 컨테이너에서 애플리케이션 과 해당 종속성 을 격리 하는 가장 널리 사용되는 방법 중 하나입니다 . 가상 머신에서 얻은 격리와 달리 Docker 컨테이너는 운영 체제의 동일한 인스턴스를 공유할 수 있으며 사용 가능한 메모리 및 처리 리소스를 보다 효율적으로 활용할 수 있습니다.

포드 . Pod는 가장 복잡한 소프트웨어 환경에서 컨테이너 사용을 용이하게 하는 상부 구조입니다. 서비스가 작동하는 데 필요한 스토리지, 네트워크 사양 및 기타 정보와 함께 여러 컨테이너를 그룹화할 수 있습니다. 이러한 방식으로 포드는 전체 정보 시스템을 캡슐화할 수 있으며 필요에 따라 생성, 수정 및 파괴할 수 있습니다.

네임스페이스 . 오케스트레이터 가 물리적 클러스터 내에서 호스팅되는 가상 클러스터 , 즉 데이터 센터의 상호 연결된 서버 풀을 식별하는 데 사용하는 이름입니다. 네임스페이스 생성은 물리적 클러스터에 있는 리소스의 일부를 제어하고 개별 부서, 팀 또는 프로젝트에 할당해야 하는 대규모 기업 애플리케이션 환경에서 유용합니다.


## Kubernetes 운영자
Kubernetes를 사용하면 컨테이너가 사용되는 대규모 서버 클러스터 를 쉽고 효율적으로 관리할 수 있습니다 . 이러한 클러스터에는 온프레미스 서버, 퍼블릭, 프라이빗 또는 하이브리드 클라우드가 포함될 수 있으며 최종 고객 서비스 또는 실시간 데이터 스트리밍과 같이 높은 확장성을 요구하는 클라우드 네이티브 애플리케이션이 실행되어야 합니다.

회사에서 Kubernetes는 여러 컨테이너를 활용 하고 다양한 보안 요구 사항을 요구하는 여러 호스트 서버 에 배포해야 하는 프로덕션 애플리케이션을 관리하는 데 도움이 됩니다.
Pod 구조는 컨테이너를 실행하는 방법에 대한 사양으로 스토리지 및 네트워크 정보를 모두 공유합니다. 따라서 Kubernetes는 IT 팀이 추가 소프트웨어를 단일 인스턴스 컨테이너로 사용하고 공유 리소스가 필요한 복잡한 애플리케이션을 관리할 수 있도록 하는 추상화 수준을 추가합니다.
Kubernetes를 사용하면 다른 애플리케이션 플랫폼 또는 기타 관리 시스템에서 호스팅되는 활동이 간소화 됩니다. Kubernetes는 클라우드 네이티브 애플리케이션을 통해 어려운 프로덕션 환경에서 컨테이너를 안정적으로 만듭니다. 이러한 컨테이너는 물리적 및 가상 머신의 클러스터를 활용합니다.

 
---

우리가 본 모든 활동 은 더 나은 성능과 상당한 비용 절감 측면에서 비즈니스 이점 을 보장 합니다. 사실 Kubernetes를 사용하면 새 버전의 애플리케이션을 지속적으로 통합 및 릴리스하고 롤아웃 및 롤백을 자동화하고 제품 제공 성능을 개선할 수 있습니다. 또한 로드 밸런싱과 메모리, 스토리지 및 네트워크 대역폭을 포함한 하드웨어 리소스 할당을 관리하여 결과적으로 비용을 절감할 수 있습니다. 마지막 으로 수요 증가에 따라 컨테이너에서 Pod를 수평으로 확장 할 수 있습니다. 이러한 모든 이유로 Kubernetes 기반 개발 솔루션은 확실히 최신 애플리케이션 구현을 위한 최고의 솔루션입니다.