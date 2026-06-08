---
layout: default
title: 4. 멀티 클러스터 방화벽 설정
parent: 부록. 방화벽 및 네트워크 설정 가이드
nav_order: 4
---

# **멀티 클러스터 방화벽 설정**
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

### **안내 사항**
* 싱글 클러스터가 아닌 **멀티 클러스터** 구조일 때 아래 사항에 대해 추가로 설정이 필요합니다.
* 호스트 클러스터와 멤버 클러스터가 대상입니다.
* 인프라 노드가 있다면 출발지와 목적지가 워커 노드가 아닌 인프라 노드로 변경이 필요합니다.
<br>
![multi.png](/assets\images\appx\fwnetwork\multi.png){: width="1000" }<br>
![multi2.png](/assets\images\appx\fwnetwork\multi2.png){: width="500" }


---

### **멀티 클러스터 방화벽 규칙**
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
        <td><span class="badge-orange">(HOST) 워커 노드</span></td>
        <td><span class="badge-blue">(MEMBER) 관리용 LB VIP</span></td>
        <td>30002</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (thanos-gateway)</td>
      </tr>
      <tr>
        <td><span class="badge-orange">(HOST) 워커 노드</span></td>
        <td><span class="badge-blue">(MEMBER) 관리용 LB VIP</span></td>
        <td>30003</td>
        <td>TCP (6)</td>
        <td>아코디언 모니터링 (thanos-sidecar)</td>
      </tr>
      <tr>
        <td><span class="badge-orange">(HOST) 워커 노드</span></td>
        <td><span class="badge-blue">(MEMBER) 관리용 LB VIP</span></td>
        <td>31000</td>
        <td>TCP (6)</td>
        <td>아코디언 멤버 콘솔</td>
      </tr>
    </tbody>
  </table>
</div>

---
