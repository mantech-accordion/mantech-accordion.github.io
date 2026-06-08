---
layout: default
title: 2. Client 방화벽 설정
parent: 부록. 방화벽 및 네트워크 설정 가이드
nav_order: 2
---

# **Client 방화벽 설정**
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

### **안내 사항**
* **클라이언트 PC**: 아코디언 접근 및 관리 권한이 부여된 관리자, 개발자, 엔지니어의 PC
* **SSH 포트**: 보안 정책에 따라 다른 포트로 변경하여 사용할 수 있습니다.
* **관리용 LB**: Master 노드 관리용 로드밸런서 / **서비스용 LB**: Client 트래픽 수신용 로드밸런서
* **노드 포트**: 30000~32767 포트는 특정 노드만 오픈해도 무방합니다.<br>
![client.png](/assets\images\appx\fwnetwork\client.png){: width="500" }

---

### **방화벽 규칙**
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
        <td>클라이언트 PC</td>
        <td>모든 노드 (all)</td>
        <td>30000 ~ 32767</td>
        <td>TCP (6)</td>
        <td>쿠버네티스 노드 포트</td>
      </tr>
      <tr>
        <td>클라이언트 PC</td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>30000</td>
        <td>TCP (6)</td>
        <td>아코디언 관리 콘솔 <span class="note-text">(Master 3중화)</span> </td>
      </tr>
      <tr>
        <td>클라이언트 PC</td>
        <td><span class="badge-blue">관리용 LB VIP</span></td>
        <td>30001</td>
        <td>TCP (6)</td>
        <td>아코디언 유저 레지스트리 <span class="note-text">(Master 3중화)</span> </td>
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
      <tr>
        <td>클라이언트 PC</td>
        <td><span class="badge-orange">Master 노드</span></td>
        <td>22</td>
        <td>TCP (6)</td>
        <td>SSH</td>
      </tr>
    </tbody>
  </table>
</div>