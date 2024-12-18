---
layout: default
title: 2.4.3 👨아코디언v2 설치
nav_order: 3
parent: 2.4 Accordion 설치
grand_parent: 2. Accordion v2
---

# 2.4.3 아코디언v2 설치
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


## 📖 설치 준비사항

<details>
<summary>1. ssh key 생성, 복사, 접속 확인</summary>
  
{% highlight bash %}


Ansible script 수행을 위해 각 노드에서 ssh로 root 로그인이 가능하도록 설정 변경합니다. (Master&node 서버) 
$ vi /etc/ssh/sshd_config
…
PasswordAuthentication yes
PermitRootLogin yes
…
$ systemctl restart sshd

master서버에서 ssh key를 생성합니다. (Master 서버) (Master서버를 포함하여 적용해야합니다.)
$ ssh-copy-id -i /root/.ssh/id_rsa.pub [노드IP]
The authenticity of host '10.140.0.3 (10.140.0.3)' can’t be established.
ECDSA key fingerprint is 34:b2:38:b8:be:1b:dd:f1:44:d8:ba:ec:92:d1:d6:dc.
Are you sure you want to continue connecting (yes/no)? yes
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed — if you are prompted now it is to install the new keys
root@10.140.0.3’s password:
Number of key(s) added: 1
Now try logging into the machine, with: "ssh '10.140.0.3'"
and check to make sure that only the key(s) you wanted were added.
(cluster의 구성서버 모두 등록할 때까지 반복해야 합니다.)

SSH Key 설정이 정상적인 확인합니다. (Master 서버) (Master서버를 포함하여 적용해야합니다.)
$ ssh [노드IP]
The authenticity of host 'acc-node1 (10.140.0.3)' can’t be established.
ECDSA key fingerprint is 34:b2:38:b8:be:1b:dd:f1:44:d8:ba:ec:92:d1:d6:dc.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'acc-node1' (ECDSA) to the list of known hosts.
Last failed login: Mon Sep 4 08:17:06 UTC 2017 from 59.49.38.210 on ssh:notty
There was 1 failed login attempt since the last successful login.
Last login: Mon Sep 4 07:18:33 2017
[root@acc-node1 ~] (패스워드 없이 로그인이 가능하면 정상입니다.)

{% endhighlight %}
   
</details>


<details>
<summary>2. Ansible 설치 (아코디언 설치 패키지에 포함)</summary>
  
{% highlight bash %}
# 각 OS에 맞도록 접속
$ /rpms/rpm_redhat7/0_ansible/install.sh # Redhat 계열 7버전
$ /rpms/rpm_redhat8/0_ansible/install.sh # Redhat 계열 8버전
$ /rpms/rpm_dpkg/0_ansible/install.sh # ubuntu 18버전

{% endhighlight %}
   
</details>

<details>
<summary>3. /etc/hosts 파일 수정</summary>
  
{% highlight bash %}

$ vi /etc/hosts
10.140.0.2
acc-master
10.140.0.3
acc-node1
10.140.0.4
acc-node2
10.140.0.5
acc-master2
10.140.0.6
acc-master3
…
{% endhighlight %}
   
</details>
<details>
<summary>4. accordion-installer/hosts 수정</summary>
hostname, ansible_host 정보를 수정


{% highlight bash %}

install-server  ansible_host=127.0.0.1     ansible_connection=local   node_role=infra

#########################################################################################
# host-cluster List
#########################################################################################
acc-host-master   ansible_host=10.20.200.201    ansible_connection=ssh   node_role=infra
acc-host-node1    ansible_host=10.20.200.204    ansible_connection=ssh   node_role=infra
acc-host-node2    ansible_host=10.20.200.205    ansible_connection=ssh   node_role=infra

#########################################################################################
# member-cluster List
#########################################################################################
#acc-member-master    ansible_host=10.20.200.206    ansible_connection=ssh   node_role=infra
#acc-member-node1    ansible_host=10.20.200.207    ansible_connection=ssh     node_role=infra
#acc-member-node2    ansible_host=10.20.200.208    ansible_connection=ssh     node_role=infra
#acc-member-master2   ansible_host=10.20.200.5    ansible_connection=ssh     node_role=infra
#acc-member-master3   ansible_host=10.20.200.6    ansible_connection=ssh     node_role=infra

#########################################################################################
# Group List 
#########################################################################################
[local]
install-server

#########################################################################################
# Manager Group

[host-master]
acc-master

[host-master-cluster]

[host-minions]
acc-node1
acc-node2

[host-cluster]
acc-master
acc-node1
acc-node2

{% endhighlight %}
   
</details>
<details>
<summary>5. accordion-installer/group_vars/host.yml 수정</summary>

{% highlight bash %}


#- setup variable for cluster installation

########################################################
## Master configuation
########################################################
# cluster_name
cluster_name: "host-cluster"
# master isolation ( yes / no )
master_isolation: "yes"
master_host_name: "accordion-master1"
master_ip: "10.20.200.201"

acc_interface: "eth0"
########################################################
# 3master mode( yes / no )
########################################################
master_mode: "no"
master2_ip: "10.20.200.202" 
master3_ip: "10.20.200.203"
master2_hostname: "accordion-master2"
master3_hostname: "accordion-master3"
# L4_mode ( L4 / haproxy )
L4_mode: "L4"
haproxy_port: "6443"
keep_vip: "10.20.200.200"

# L4_domain_option ( yes / no )
L4_domain_option: "yes"
L4_domain_address: "l4.domain.accordions.co.kr"

# Set it up if you want to add the master server later
# If master_mode is "no", it will not work.
# single_option( yes / no )
single_option: "no"

########################################################
## container_name (containerd & cri-o)
########################################################
container_option: "containerd"

# container_option (containerd)
selinux_enable: "no"

# container_option (cri-o)
pid_limit: "4096"

########################################################
# storage setting
########################################################
# storage_option ( nfs / ceph )
storage_option: "nfs"

# nfs_setup ( internal / external )
nfs_setup: "external"
nfs_server_ip: "10.10.0.85"
accordion_nfs_path: "/volume1/tech"

# nfs_version ( v3 / v4 / v4.1 / v4.2 )
nfs_version: "v3"

# ceph_option ( cephfs / rbd )
ceph_option: "cephfs"

#ceph health, ceph fsid, ceph auth get-key client.admin, ceph status
ceph_server_ip: "10.20.200.107"
ceph_server_port: "6789"
ceph_id: "admin"
ceph_key: "AQCKoqVh0eR5MxAAL4WziV7oyVsdtHC6Wz0RcQ=="
#ceph_id: "kubernetes"
#ceph_key: "AQBDpKVhFhPCIRAAf1xqSLgi558DIH+FvcCyMQ=="
ceph_fsid: "84ab6f51-d13e-4a83-9ccc-fd3b9228e728"
ceph_fsname: "cephfs"
#ceph_fsname: "cephrbd_pool"

########################################################
# etcd external option
########################################################
etcd_external: "no"

########################################################
# base registry
########################################################
# accordion_registry_option ( local / external )
base_registry_option: "local"
base_registry_addr: "10.20.200.200"
base_registry_port: "5000"
base_registry_id: "accregistry"
base_registry_passwd: "accordionadmin"

########################################################
# user registry
########################################################
# registry_option ( registry / harbor )
user_registry_option: "registry"
user_registry_addr: "10.20.200.200"
user_registry_port: 30001

# registry_external ( yes / no )
user_registry_external: "no"

# user_registry_external: "yes"
user_registry_id: "accregistry"
user_registry_pw: "accordionadmin"

# registry information
htpasswd_option: "yes"

#######################################################
# registry domain option
########################################################
# registry_domain_option ( yes / no )
registry_domain_option: "yes"
base_registry_domain_address: "base.registry.accordions.co.kr"
user_registry_domain_address: "user.registry.accordions.co.kr"

# hosts_setting( yes / no )
# If the value 'host_setting' is yes, the domain is added to /etc/hosts
# (Not working if 'registry_domain_option' is no)
hosts_setting: "yes"

########################################################
# Network Setting
########################################################
# CNI (calico & cilium)
network_cni: "calico"

# podman Network
podman_cidr: "172.17.0.0/16"

# Pod Network
pod_network_cidr: "172.32.0.0/16"

# Calico Mode (IPIP & vxlan)
calico_backend: "IPIP"

# Calico ippool(CrossSubnet & Always)
calico_ippool: "CrossSubnet"

# Cilium tunnel_option(disabled & vxlan & geneve)
tunnel_option: "geneve"

# Calico autodetection mode( interface / cidr )
# If the option is 'interface', it follows the value set in acc_interface.
calico_autodetection_mode: "interface"
# If the option is 'cidr', set the value 'interface_cidr' to OS interface cidr.
interface_cidr: "10.20.200.0/24"

# kubernetes Service Network
service_cidr: "10.96.0.0/16"
kubelet_clusterdns: "10.96.0.10"
kubernetes_clusterip: "10.96.0.1"

# proxy_mode( iptables / ipvs )
proxy_mode: "iptables"
# iptables_install( yes / no )
iptables_install: "no"

########################################################
## container dir
########################################################
containerd_root_dir: "/var/lib/containerd"
containers_storage_runroot: "/var/run/containers"
containers_storage_volume: "/var/lib/containers"

########################################################
## Kubernetes dir
########################################################
kubelet_root_dir: "/var/lib/kubelet"
kube_addon_dir: "/etc/kubernetes/addon"
docker_rpm_dir: "/tmp"

########################################################
# If the external IP is set up on the master server, enter the IP.
########################################################
master_external_ip: "127.0.0.1"

########################################################
## Add node option (yes / no)
########################################################
noschedule: "no"

##########################################
## accordion GPU monitoring(yes/no)
##########################################
gpu_server: "no"

##########################################
## external prometheus
##########################################
external_prometheus: "no"
prometheus_name: "prometheus-operator-prometheus"
prometheus_ns: "acc-system"

##########################################
# vm provisioning
##########################################
# Enable only when using accordion provisioning
# uuid_enable (yes/no)
uuid_enable: "no"
# vm_type (vsphere/openstack)
vm_type: "vsphere"

##########################################
# addon package
##########################################
istio_install: "yes"
prometheus_install: "yes"
opensearch_install: "yes"

##########################################
# istio_option (default/kubeflow)
##########################################
istio_option: "default"

########################################################
## ha
########################################################
minio_ha: "no"
prometheus_ha: "no"
alertmanager_ha: "no"
elasticsearch_ha: "no"
logstash_ha: "no"
thanos_ha: "no"
tsdb_ha: "no"
harbor_ha: "no"


{% endhighlight %}
   
</details>



**아코디언 설치 및 확인**
- accordion-installer/install.sh
- accordion-installer/status.sh

**설치 설정**
```bash
$ accordion-installer/install.sh
```

**설치 진행**
```bash
$ accordion-installer/install.sh
End User Software License Agreement

This is a legal agreement between Customer and Man Technology Co., LTD. ("Mantech").  YOU MUST READ AND AGREE TO THE TERMS OF THIS END USER SOFTWARE LICENSE AGREE
MENT ("AGREEMENT") BEFORE ANY SOFTWARE CAN BE DOWNLOADED OR INSTALLED OR USED.  BY CLICKING ON THE "ACCEPT" BUTTON OR TYPING THE "Y" OF THIS AGREEMENT, OR DOWNLOA
DING, INSTALLING OR USING THE SOFTWARE, YOU ARE AGREEING TO BE BOUND BY THE TERMS AND CONDITIONS OF THIS AGREEMENT.  IF YOU DO NOT AGREE WITH THE TERMS AND CONDIT
IONS OF THIS AGREEMENT, THEN YOU SHOULD NOT DOWNLOAD, INSTALL OR USE THE SOFTWARE.  BY DOING SO YOU FORGO ANY IMPLIED OR STATED RIGHTS TO DOWNLOAD OR INSTALL OR U
SE THE SOFTWARE.
...
...
16. Questions.  Should you have any questions concerning the foregoing, please contact Mantech at the following address: Man Technology Co., LTD. 12th Fl, 308-4,
Seongsudong 2ga, Seongdong-gu, Seoul, Korea.
                 Phone : +82-2-2136-6900
                 E-mail : marketing@mantech.co.kr
                 www.mantech.co.kr


DO YOU ACCEPT THE TERMS OF THIS LINCESE AGREEMENT? [y/n] : y
```

**설치 확인**
```bash
$ accordion-installer/status.sh
========================================
 nodes
========================================
NAME         STATUS   ROLES                  AGE    VERSION
acc-master   Ready    control-plane,master   177m   v1.20.8
acc-node1    Ready    worker                 176m   v1.20.8
========================================
 deployments/po/svc
========================================
NAMESPACE     NAME                                           READY   UP-TO-DATE   AVAILABLE   AGE
acc-global    deployment.apps/alert-apiserver                1/1     1            1           162m
acc-global    deployment.apps/authset-manager                1/1     1            1           161m
acc-global    deployment.apps/chartmuseum-chartmuseum        1/1     1            1           163m
acc-global    deployment.apps/cloud-apiserver                1/1     1            1           161m
acc-global    deployment.apps/clusterinfo-manager            1/1     1            1           172m
acc-global    deployment.apps/console                        1/1     1            1           162m
acc-global    deployment.apps/gateway                        1/1     1            1           162m
acc-global    deployment.apps/host-log-apiserver             1/1     1            1           162m
acc-global    deployment.apps/keycloak                       1/1     1            1           163m
acc-global    deployment.apps/keycloak-db                    1/1     1            1           163m
acc-global    deployment.apps/manual-webserver               1/1     1            1           161m
acc-global    deployment.apps/monitoring-apiserver           1/1     1            1           162m
acc-global    deployment.apps/setting-manager                1/1     1            1           163m
acc-global    deployment.apps/thanos-querier                 1/1     1            1           163m
acc-system    deployment.apps/acc-grafana                    1/1     1            1           170m
acc-system    deployment.apps/acc-kube-state-metrics         1/1     1            1           170m
acc-system    deployment.apps/accordion-ingress-controller   1/1     1            1           170m
acc-system    deployment.apps/apm-agent-injector             1/1     1            1           166m
...
...

```

**아코디언 접속 URL**

| URL      | ID    | PW    |
|:---------|:------|:------|
| https://VIP://30000      | admin  | accordion!@# |

![acc-ins-1.png](/assets/images/accordion/acc-ins-1.png){: width="800" }


---


## 라이선스 등록
`글로벌 설정` ➡ `라이선스` ➡ `등록`

![practice-1.png](/assets/images/accordion/practice-1.png)
![practice-2.png](/assets/images/accordion/practice-2.png)
