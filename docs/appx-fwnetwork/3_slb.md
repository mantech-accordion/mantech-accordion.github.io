---
layout: default
title: 3. LB 설정
parent: 부록. 방화벽 및 네트워크 설정 가이드
nav_order: 3
---

# **LB 설정**
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

### **안내 사항**
* **관리용 LB**: Master 노드 관리용 로드밸런서 / **서비스용 LB**: Client 트래픽 수신용 로드밸런서
* **클라이언트 PC**: 아코디언 접근 및 관리 권한이 부여된 관리자, 개발자, 엔지니어의 PC
* Infra 노드가 별도로 구성되어 있을 경우, Worker 노드와 동일하게 방화벽 오픈이 필요합니다.
<br>
![lb.png](/assets\images\appx\fwnetwork\lb.png){: width="500" }
![client.png](/assets\images\appx\fwnetwork\client.png){: width="500" }


---

### **LB 방화벽 규칙**
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
        <td><span class="badge-orange">Master 노드</span></td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>6443</td>
        <td>TCP (6)</td>
        <td>쿠버네티스 API 서버</td>
      </tr>
      <tr>
        <td><span class="badge-orange">Master 노드</span></td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>5000</td>
        <td>TCP (6)</td>
        <td>아코디언 베이스 레지스트리</td>
      </tr>
      <tr>
        <td><span class="badge-orange">Master 노드</span></td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>30001</td>
        <td>TCP (6)</td>
        <td>아코디언 유저 레지스트리</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>6443</td>
        <td>TCP (6)</td>
        <td>쿠버네티스 API 서버</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>5000</td>
        <td>TCP (6)</td>
        <td>아코디언 베이스 레지스트리</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30000</td>
        <td>TCP (6)</td>
        <td>아코디언 관리 콘솔</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30001</td>
        <td>TCP (6)</td>
        <td>아코디언 유저 레지스트리</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30002</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (thanos-sidecar)</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30003</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (thanos-gateway)</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30443</td>
        <td>TCP (6)</td>
        <td>아코디언 멤버 에이전트 <span class="note-text">(아코디언 v2 한정)</span></td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>31000</td>
        <td>TCP (6)</td>
        <td>아코디언 멤버 클러스터 관리 콘솔 (clusterinfo 확인) <span class="note-text">(아코디언 v3 한정)</span></td>
      </tr>
      <tr>
        <td>Worker 노드</td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>30001</td>
        <td>TCP (6)</td>
        <td>아코디언 유저 레지스트리</td>
      </tr>
      <tr>
        <td>Worker 노드</td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>30002</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (thanos-sidecar)</td>
      </tr>
      <tr>
        <td>Worker 노드</td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>30003</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (thanos-gateway)</td>
      </tr>
      <tr>
        <td>클라이언트 PC</td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>30000</td>
        <td>TCP (6)</td>
        <td>아코디언 관리 콘솔</td>
      </tr>
      <tr>
        <td>클라이언트 PC</td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>30001</td>
        <td>TCP (6)</td>
        <td>아코디언 유저 레지스트리</td>
      </tr>
      <tr>
        <td>클라이언트 PC</td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>31000</td>
        <td>TCP (6)</td>
        <td>아코디언 멤버 클러스터 관리 콘솔 (clusterinfo 확인) <span class="note-text">(아코디언 v3 한정, v2는 해당 없음)</span></td>
      </tr>
      <tr>
        <td>클라이언트 PC</td>
        <td><span class="badge-green">서비스용 LB VIP</span></td>
        <td>80</td>
        <td>TCP (6)</td>
        <td>인그레스 HTTP</td>
      </tr>
      <tr>
        <td>클라이언트 PC</td>
        <td><span class="badge-green">서비스용 LB VIP</span></td>
        <td>443</td>
        <td>TCP (6)</td>
        <td>인그레스 HTTPS</td>
      </tr>
    </tbody>
  </table>
</div>

---

### **SLB 설정 리스트**
<br><br>
**대상**
- **관리용 LB** = Master 노드 VIP
- **서비스용 LB** = Worker 노드 VIP
<br><br>


**LB 알고리즘**
- **관리용 LB** = **IP Hash 방식 (중요)**
  - Hash 방식이 아닐 경우, 사이즈가 큰 컨테이너 이미지 push 하는 과정에서 에러가 발생할 수 있습니다.
- **서비스용 LB** = RoundRobin, LeastConnection, Weight RoundRobin 등 서비스에 맞게 사용

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
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>6443</td>
        <td>TCP (6)</td>
        <td>쿠버네티스 API 서버</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>5000</td>
        <td>TCP (6)</td>
        <td>아코디언 베이스 레지스트리</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30000</td>
        <td>TCP (6)</td>
        <td>아코디언 관리 콘솔</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30001</td>
        <td>TCP (6)</td>
        <td>아코디언 유저 레지스트리</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30002</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (thanos-sidecar)</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30003</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (thanos-gateway)</td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>30443</td>
        <td>TCP (6)</td>
        <td>아코디언 멤버 에이전트 <span class="note-text">(아코디언 v2 한정)</span></td>
      </tr>
      <tr>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>31000</td>
        <td>TCP (6)</td>
        <td>아코디언 멤버 클러스터 관리 콘솔 (clusterinfo 확인) <span class="note-text">(아코디언 v3 한정)</span></td>
      </tr>
      <tr>
        <td><span class="badge-green">서비스용 LB VIP</span></td>
        <td>Worker 노드</td>
        <td>80</td>
        <td>TCP (6)</td>
        <td>인그레스 HTTP</td>
      </tr>
      <tr>
        <td><span class="badge-green">서비스용 LB VIP</span></td>
        <td>Worker 노드</td>
        <td>443</td>
        <td>TCP (6)</td>
        <td>인그레스 HTTPS</td>
      </tr>
    </tbody>
  </table>
</div>