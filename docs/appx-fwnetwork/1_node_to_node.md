---
layout: default
title: 1. 노드 간 방화벽 설정
parent: 부록. 방화벽 및 네트워크 설정 가이드
nav_order: 1
---

# **노드 간 방화벽 설정**
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

### **안내 사항**
* **프로토콜 번호 (괄호)**: RFC에 정의된 번호이며, 특정 포트 번호를 의미하는 것이 아닙니다. (예: TCP=6, UDP=17, ICMP=1, IPIP=4)
* **빨간색 설명**: 외부 로드밸런서(LB)가 없을 경우에만 선택적으로 사용합니다.
* **SSH 포트**: 보안 정책에 따라 다른 포트로 변경하여 사용할 수 있습니다.
* 노드(VM)간 내부 방화벽이 차단되지 않은 환경일 경우 본 설정은 생략 가능합니다. (단, IPIP 프로토콜 제외)
* **관리용 LB**: Master 노드 관리용 로드밸런서 / **서비스용 LB**: Client 트래픽 수신용 로드밸런서
* Infra 노드가 별도로 구성되어 있을 경우, Worker 노드와 동일하게 방화벽 오픈이 필요합니다.
* 아래 규칙은 아코디언 v2, v3 공통이지만, 멤버 클러스터 관련 포트 차이가 있으니 아래 표에서 확인을 당부드립니다. (30443, 31000 포트)
<br>
![nodetonode.png](/assets\images\appx\fwnetwork\nodetonode.png){: width="500" }

---

### **공통 기본 방화벽 규칙**
<div class="tech-table-wrapper">
  <table class="tech-table">
    <thead>
      <tr>
        <th>출발지</th>
        <th>목적지</th>
        <th>포트</th>
        <th>프로토콜</th>
        <th>목적 및 설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>모든 노드 (all)</td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>6443</td>
        <td>TCP (6)</td>
        <td>쿠버네티스 API 서버</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>8443</td>
        <td>TCP (6)</td>
        <td><span class="badge-red">쿠버네티스 API 서버</span> <span class="note-text">(Master 3중화, 외부 LB 없을 경우 사용)</span></td>
      </tr>
      <tr>
        <td><span class="badge-orange">Master 노드</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>2379</td>
        <td>TCP (6)</td>
        <td>ETCD Server Client API</td>
      </tr>
      <tr>
        <td><span class="badge-orange">Master 노드</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>2380</td>
        <td>TCP (6)</td>
        <td>ETCD Cluster Peer</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>10250</td>
        <td>TCP (6)</td>
        <td>kubelet API</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>10255</td>
        <td>TCP (6)</td>
        <td>kubelet 헬스체크 (read only)</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>10256</td>
        <td>TCP (6)</td>
        <td>kube-proxy 헬스체크</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>10257</td>
        <td>TCP (6)</td>
        <td>kube-controller-manager</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>10259</td>
        <td>TCP (6)</td>
        <td>kube-scheduler</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30000</td>
        <td>TCP (6)</td>
        <td>아코디언 관리 콘솔</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>30001</td>
        <td>TCP (6)</td>
        <td>아코디언 유저 레지스트리</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>30002</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (thanos-store-gateway)</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>30003</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (thanos-sidecar)</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>30443</td>
        <td>TCP (6)</td>
        <td>아코디언 멤버 에이전트 <span class="note-text">(아코디언 v2 한정)</span></td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>31000</td>
        <td>TCP (6)</td>
        <td>아코디언 멤버 클러스터 관리 콘솔 (clusterinfo 확인) <span class="note-text">(아코디언 v3 한정)</span></td>
      </tr>
      <tr>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>모든 노드 (all)</td>
        <td>22</td>
        <td>TCP (6)</td>
        <td>SSH (ansible 통신) <span class="note-text">(정책 따라 다른 포트로 변경 가능)</span></td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>9100</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (Prometheus node-exporter)</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>5000</td>
        <td>TCP (6)</td>
        <td>아코디언 베이스 레지스트리</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>5001</td>
        <td>TCP (6)</td>
        <td><span class="badge-red">아코디언 베이스 레지스트리</span> <span class="note-text">(Master 3중화, 외부 LB 없을 경우 사용)</span></td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>-</td>
        <td>ICMP (1)</td>
        <td>노드 간 Ping 통신</td>
      </tr>
    </tbody>
  </table>
</div>

---

### **쿠버네티스 CNI 유형 별 추가 방화벽 규칙**

사용하고자 하는 CNI 유형(Calico 또는 Cilium)에 따라 아래의 포트를 바인딩하여 오픈해야 합니다.

- Calico 선택 시
<div class="tech-table-wrapper">
  <table class="tech-table">
    <thead>
      <tr>
        <th>출발지</th>
        <th>목적지</th>
        <th>포트</th>
        <th>프로토콜</th>
        <th>목적 및 설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>179</td>
        <td>TCP (6)</td>
        <td>Calico BGP 라우팅</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>4789</td>
        <td>UDP (17)</td>
        <td>Calico VXLAN 오버레이 네트워킹</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>-</td>
        <td>IPIP (4)</td>
        <td>Calico IPIP 캡슐화 네트워킹</td>
      </tr>
    </tbody>
  </table>
</div>

- Cilium 선택 시

<div class="tech-table-wrapper">
  <table class="tech-table">
    <thead>
      <tr>
        <th>출발지</th>
        <th>목적지</th>
        <th>포트</th>
        <th>프로토콜</th>
        <th>목적 및 설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>8472</td>
        <td>UDP (17)</td>
        <td>Cilium VXLAN 오버레이 네트워킹</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>6081</td>
        <td>UDP (17)</td>
        <td>Cilium Geneve 오버레이 네트워킹 <span class="note-text">(VXLAN 대신 Geneve 설정 시 필요)</span></td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>51871</td>
        <td>UDP (17)</td>
        <td>Cilium Wireguard 데이터 암호화 터널링 포트</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>4240</td>
        <td>TCP (6)</td>
        <td>Cilium Agent 헬스체크</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>4244</td>
        <td>TCP (6)</td>
        <td>Cilium Hubble 서버 포트 <span class="note-text">(gRPC 기반 흐름 관측)</span></td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>4245</td>
        <td>TCP (6)</td>
        <td>Cilium Hubble Relay 서비스 통신 포트</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>4250</td>
        <td>TCP (6)</td>
        <td>Cilium Agent 원격 호출 및 동기화 용 gRPC API 내부 포트</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>9962</td>
        <td>TCP (6)</td>
        <td>Cilium Agent Prometheus 메트릭 수집 포트</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>9963</td>
        <td>TCP (6)</td>
        <td>Cilium Operator Prometheus 메트릭 수집 포트</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>9964</td>
        <td>TCP (6)</td>
        <td>Cilium Envoy proxy 메트릭 수집 포트</td>
      </tr>
      <tr>
        <td>모든 노드 (all)</td>
        <td>모든 노드 (all)</td>
        <td>9965</td>
        <td>TCP (6)</td>
        <td>Cilium Hubble Prometheus 메트릭 수집 포트</td>
      </tr>
    </tbody>
  </table>
</div>