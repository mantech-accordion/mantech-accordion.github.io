---
layout: default
title: 2.4.2 ๐์ธ์คํจ๋ฌ ๊ตฌ์กฐ
nav_order: 4
parent: 2.4 Accordion ์ค์น
grand_parent: 2. Accordion v2
---

# 2-4-2 ์ธ์คํจ๋ฌ ๊ตฌ์กฐ
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Ansible

![ansible.png](/assets/images/accordion/ansible.png)


---

## ๐ ์ธ์คํจ๋ฌ(accordion-installer) ๊ตฌ์กฐ

<details>
<summary>๐ accordion-installer ๊ตฌ์กฐ</summary>
  
{% highlight bash %}
accordion-installer/
|-- roles               # ์ค์น ํ์คํฌ ๋๋ ํ ๋ฆฌ  
|-- group_vars          # ์ค์น ๊ณตํต ๋ณ์ ์ค์  ๋๋ ํ ๋ฆฌ
|-- hosts               # cluster๊ตฌ์ฑํ  hosts ์ค์ 
|-- install.sh          # ์ค์น 
|-- uninstall.sh        # ์ญ์ 
|-- status.sh           # ์ค์น ํ ์ํํ์ธ
|-- add-aks.sh          # aks cluster ์ถ๊ฐ
|-- add-eks.sh          # eks cluster ์ถ๊ฐ(eksctl)
|-- add-kubeconfig.sh
|-- addaks.yml
|-- addeks.yml
|-- addkubeconfig.yml
|-- addmaster.sh        # master ๋ธ๋ ์ถ๊ฐ
|-- addmember.sh        # member cluster ์ถ๊ฐ
|-- addmember.yml   
|-- addnode.sh          # node ์ถ๊ฐ
|-- all.retry
|-- all.yml
|-- ansible.cfg
|-- getconfig.yml
|-- kube-teardown.retry
|-- kube-teardown.yml
|-- lic.txt
|-- master.yml
|-- node.yml
|-- remove-aks.sh       # aks cluster ์ญ์ 
|-- remove-eks.sh       # eks cluster ์ญ์ 
|-- removeaks.yml
|-- removeeks.yml
|-- removemember.sh     # member cluster ์ญ์ 
|-- removemember.yml    
|-- removenode.sh       # node ์ญ์ 
|-- restore.sh          # ๋ฐฑ์/๋ณต๊ตฌ ์คํฌ๋ฆฝํธ
|-- restore.yml
|-- run_role.yml
|-- THIRDPARTYLICENSE.txt
{% endhighlight %}
   
</details>

<details>
<summary>๐ group_vars ๊ตฌ์กฐ</summary>
  
{% highlight bash %}

group_vars/
|-- host.yml                          # ์์ฝ๋์ธ ์ค์น ๊ตฌ์ฑ์ ์ํ ๋ณ์
|-- all.yml                           # ์์ฝ๋์ธ ์ค์น ๊ตฌ์ฑ์ ์ํ ๋ณ์
|-- config.yml                        # registry, config ๊ด๋ จ ์ค์             
|-- aks.yml                           # aks ํด๋ฌ์คํฐ ๊ตฌ์ฑ์ ์ํ ๋ณ์
|-- eks.yml                           # eks ํด๋ฌ์คํฐ ๊ตฌ์ฑ์ ์ํ ๋ณ์
|-- etcd-master-cluster-manager.yml   # host cluster์ etcd ์ธ๋ถ ๊ตฌ์ฑ
|-- etcd-master-cluster-member.yml    # member cluster์ etcd ์ธ๋ถ ๊ตฌ์ฑ
|-- member.yml                        # member cluster ๊ตฌ์ฑ
{% endhighlight %}
   
</details>


<details>
<summary>๐ roles ๊ตฌ์กฐ</summary>
  
{% highlight bash %}

roles/
|-- 0-rpms-install
|-- 1-add-uid-gid
|-- 2-nfs
|-- 3-kube-prerequisite
|-- 4-add-containerd
|-- 4-containerd
|-- 4-cri-o
|-- 5-kube-install
|-- 6-kube-init-join
|-- 6-kube-init-join-cluster
|-- 6-kube-init-join-cluster-single
|-- 7-node-label
|-- 8-weave-kube
|-- 9-acc-registry
|-- 10-crd
|-- 11-acc-system
|-- 12-acc-global
|-- 13-acc-global-after
|-- 14-backup
|-- add-aks
|-- add-eks
|-- add-gpu
|-- add-kubeconfig
|-- add-master
|-- add-node
|-- cloud-acc-system
|-- cloud-crd
|-- kube-teardown
|-- member-info
|-- remove-aks
|-- remove-eks
|-- remove-node
|-- restore-etcd
|-- restore-etcd-single   
{% endhighlight %}
   
</details>

---

## ์์ฝ๋์ธ ํ๊ฒ๋ธ๋ ์ค์ 

๊ฒฝ๋ก : `accordion-installer/hosts`

์์ฝ๋์ธ hosts ์ค์  ํ์ผ์๋ ์์ฝ๋์ธ์ ๊ตฌ์ฑํ  Node๋ค์ ์ค์ ์ด ์ฃผ๋ก ์ด๋ฃจ์ด์ง๋๋ค. ํ์ฌ ๊ตฌ์ฑ๋ Master์ Node๋ค์ IP์ฃผ์๋ฅผ ์์ ํฉ๋๋ค.<br>
๋ด๋ถ IP๊ฐ ๋ณ๋๋ก ์๋ ๊ฒฝ์ฐ ๋ด๋ถ IP๋ก ์๋ ฅํ์๋ฉด ๋ฉ๋๋ค. 

๐ก ์ฐธ๊ณ  : host-cluster List์ member-cluster List์ ์๋ ฅํ๋ ์์์ ๋์ผ

---

**์์1. Host-cluster๋ง ๊ตฌ์ฑํ  ์(Master: 1๋, Worker: 2๋)**

|Hostname|Host IP|์ฌ์ฉ์ ์ค์ |์ฌ์ฉ์ ์ค์ |
|--|--|--|--|
|acc-master|ansible_host=10.60.100.70|ansible_connection=ssh|node_role=infra|
|acc-worker1|ansible_host=10.60.100.71|ansible_connection=ssh|node_role=infra|
|acc-worker2|ansible_host=10.60.100.72|ansible_connection=ssh|node_role=infra|

```yaml
install-server  ansible_host=127.0.0.1     ansible_connection=local   node_role=infra 

#########################################################################################
# host-cluster List
#########################################################################################
acc-master    ansible_host=10.60.100.70    ansible_connection=ssh   node_role=infra
acc-worker1   ansible_host=10.60.100.71    ansible_connection=ssh   node_role=infra
acc-worker2   ansible_host=10.60.100.72    ansible_connection=ssh   node_role=infra
```

---

**์์2. ๊ตฌ์ฑํ๋ host-cluster์ member-cluster ๊ฐ์ด ๊ตฌ์ฑํ  ๊ฒฝ์ฐ**

|Hostname|Host IP|์ฌ์ฉ์ ์ค์ |์ฌ์ฉ์ ์ค์ |
|--|--|--|--|
|acc-master|ansible_host=10.60.100.70|ansible_connection=ssh|node_role=infra|
|acc-worker1|ansible_host=10.60.100.71|ansible_connection=ssh|node_role=infra|
|acc-worker2|ansible_host=10.60.100.72|ansible_connection=ssh|node_role=infra|

|Hostname|Host IP|์ฌ์ฉ์ ์ค์ |์ฌ์ฉ์ ์ค์ |
|--|--|--|--|
|acc-member-master|ansible_host=10.60.101.70|ansible_connection=ssh|node_role=infra|
|acc-member-worker1|ansible_host=10.60.101.71|ansible_connection=ssh|node_role=infra|
|acc-member-worker2|ansible_host=10.60.101.72|ansible_connection=ssh|node_role=infra|

```yaml
install-server  ansible_host=127.0.0.1     ansible_connection=local   node_role=infra 

#########################################################################################
# host-cluster List
#########################################################################################
acc-master    ansible_host=10.60.100.70    ansible_connection=ssh   node_role=infra
acc-worker1   ansible_host=10.60.100.71    ansible_connection=ssh   node_role=infra
acc-worker2   ansible_host=10.60.100.72    ansible_connection=ssh   node_role=infra

#########################################################################################
# member-cluster List
#########################################################################################
acc-member-master  ansible_host=10.60.101.70    ansible_connection=ssh   node_role=infra
acc-member-worker1 ansible_host=10.60.101.71    ansible_connection=ssh   node_role=infra
acc-member-worker2 ansible_host=10.60.101.72    ansible_connection=ssh   node_role=infra
```

`node_role=infra`๋ Pod๋ค์ ์ดํผ๋ํฐ ์ค์ ์ ์ํด ๋ผ๋ฒจ์ ๋ช์ํฉ๋๋ค. ์ค์น ํ `--show-labels` ๋ช๋ น์ ์๋ ฅํ๋ฉด `node_role` ๋ผ๋ฒจ์ ๋ณผ ์ ์์ต๋๋ค.


**`ansible_connection='options'`**

|Hostname|Host IP|์ฌ์ฉ์ ์ค์ |
|--|--|--|
|ssh|ansible_connection=ssh|์ฐ๊ฒฐ์ ssh๋ก ์ฌ์ฉ|
|local|ansible_connection=local|์ฐ๊ฒฐ์ localhost๋ก ์ฌ์ฉ. ์ด ์ค์ ์ ํ๋ฉด ๋จ์ผ ํธ์คํธ์์๋ง ๋ช๋ น์ ์คํ|
|paramiko_ssh|ansible_connection=paramiko_ssh|	์ฐ๊ฒฐ์ python์ผ๋ก ๊ตฌํํ ssh๋ก ์ฌ์ฉ|
|winrm|ansible_connection=winrm|์ฐ๊ฒฐ์ windows์ winrm์ ์ฌ์ฉ|

๐ก ์ฐธ๊ณ  : ๋ค์ ๋ช๋ น์ ํตํด ๋ ๋ง์ ์ฐ๊ฒฐ ์ต์์ ํ์ธํ  ์ ์์ต๋๋ค.

```bash
ansible-doc -t connection -l
```

---

**hostsํ์ผ cluster List ์ดํ ๋ถ๋ถ**

```yaml
#########################################################################################
# Group List
#########################################################################################
[local]
install-server

#########################################################################################
# Manager Group

[host-master] โ host-cluster์ Master node์ hostname์ ๊ธฐ์
#acc-host-master
acc-master

[host-master-cluster]
#acc-host-master2 โ 3์คํ ๊ตฌ์ฑ ์ ์์ฑ
#acc-host-master3 โ 3์คํ ๊ตฌ์ฑ ์ ์์ฑ

[host-minions] โ host-cluster์ worker node ์ ๋ณด ๊ธฐ์(worker)
acc-worker1
acc-worker2
#acc-host-node1
#acc-host-node2

[host-cluster] โ host-cluster์ ์ ์ฒด Node๋ค์ hostname์ ๋ชจ๋ ์์ฑ(master + worker)
acc-master
acc-worker1
acc-worker2
#acc-host-master
#acc-host-node1
#acc-host-node2
#acc-host-master2
#acc-host-master3

[host-infra] โ host-cluster์ infra node๋ก ์ค์ ํ  hostname์ ์์ฑ

[host-etcd] โ host-cluster์ etcd node๋ก ์ค์ ํ  hostname์ ์์ฑ

#########################################################################################
```

---

## ๊ณตํต ๋ณ์

๊ณตํต๋ณ์(hosts) ์์๋ ์์ฝ๋์ธ ์ค์น์ ํ์ํ `master` ์ค์ , ํด๋ฌ์คํฐ ip ๋์ญ๋ฑ ์ค์ ์ด ๊ฐ๋ฅํฉ๋๋ค.

๊ฒฝ๋ก: `group_vars/hosts.yml`

|์ต์๋ช |์๋ ฅ๊ฐ | ์ค๋ช |
|:-----|:-----|:----|
|master_isolation|yes, no|Master node์ taint์ ์ค์ ํฉ๋๋ค.<br>no์ผ ๊ฒฝ์ฐ master์๋ Pod๊ฐ ๋ฐฐ์น๋ฉ๋๋ค.<br>(default: no)|
|master_host_name|์ฌ์ฉ์ ์ค์ |๋ง์คํฐ์๋ฒ์ hostname์ ์๋ ฅ|
|master_ip|์ฌ์ฉ์ ์ค์ |๋ง์คํฐ์๋ฒ์ IP๋ฅผ ์๋ ฅ|
|master_mode|yes<br>no|3์คํ ์ค์น์ฌ๋ถ <br>โ yes: 3์คํ ์ค์น <br>โ no: ๋จ์ผ์ค์นdefault|
|master2_ip|์ฌ์ฉ์ ์ค์ |master2 ์๋ฒ IP|
|master3_ip|์ฌ์ฉ์ ์ค์ |master3 ์๋ฒ IP|
|master2_hostname|์ฌ์ฉ์ ์ค์ |master2 ์๋ฒ hostname|
|master3_hostname|์ฌ์ฉ์ ์ค์ |master3 ์๋ฒ hostname|
|L4_mode|L4<br>haproxy|L4 haproxy ์ฌ์ฉ์ฌ๋ถ<br>โ L4๋ก ์ค์  ์ L4์ฅ๋น๋ฅผ ์ฌ์ฉ, ์ฌ์ ์์ ํ์<br>โ haproxy ์ค์  ์ haproxy pod๋ฅผ ๋์ ๊ตฌ์ฑํจ<br>(default: haproxy)|
|haproxy_port|์ฌ์ฉ์ ์ค์ |apiserver 6443 port๋ก ๋๊ฒจ์ฃผ๋ port๊ตฌ์ฑ<br>(default: 8443)|
|keep_vip|์ฌ์ฉ์ ์ค์ | โ master์๋ฒ vip์ค์ ์๋๋ค.|
|keep_interface|์ฌ์ฉ์ ์ค์ |master์๋ฒ OS์ network interface name์ ์๋ ฅ|
|single_option|์ฌ์ฉ์ ์ค์ |master์๋ฒ 1๋๋ก ์ค์น ํ ๋์ค์ 2๋๋ฅผ ๋ถ์ฌ 3์คํ๋ฅผ ๊ตฌ์ฑํ  ์ ์๋๋ก ์ค์ ํ๋ ์ต์ ์๋๋ค.<br>(๊ถ์ฅ: ์ฌ์ ์ 3์คํ ์ค์ ์ ๋ณด๋ฅผ ๊ธฐ์ํด์ผํจ)|
|container_option|containerd<br>cri-o|์ปจํ์ด๋ ๋ฐํ์์ ์ค์ ํฉ๋๋ค.<br>(default: containerd)|
|selinux_enable|no|selinux ํ์ฑํ ์ต์์๋๋ค.<br>(default: no)|
|crio_interface|์ฌ์ฉ์ ์ธํฐํ์ด์ค ์๋ ฅ|ifconfig ์๋ ฅ ํ ens ๊ฐ์ ๋ฃ์ด์ค๋๋ค.|
|storage_option|nfs<br>ceph|์คํ ๋ฆฌ์ง ํ์์ ์ ํํฉ๋๋ค<br>โ nfs: nfs ์ฌ์ฉ<br>โ ceph: ceph ์ฌ์ฉ|
|nfs_setup|internal external|nfs์ ์ค์น ๋ฐฉ์์ ์ ํํฉ๋๋ค<br> โ internal: master์๋ฒ์ nfs๋ฅผ ์ค์น ํ ์ฌ์ฉ<br> โ external: ์ธ๋ถ์ ๋ง์ดํธ๋ nfs๋ฅผ ์ฌ์ฉ|
|nfs_server_ip|์ฌ์ฉ์ ์ค์ |nfs์๋ฒ์ ip๋ฅผ ์ค์ ํฉ๋๋ค<br>โ nfs_setup์ด internal์ผ ๊ฒฝ์ฐ masterIP ์๋ ฅ<br>โ nfs_setup์ด external์ผ ๊ฒฝ์ฐ ์ธ๋ถ nfs์๋ฒ IP ์๋ ฅ|
|accordion_nfs_path|์ฌ์ฉ์ ์ค์ |nfs_setup์ด internal์ผ ๊ฒฝ์ฐ ์ํ๋ nfs๊ฒฝ๋ก๋ฅผ ์๋ ฅ<br>nfs_setup์ด external์ผ ๊ฒฝ์ฐ ๋ง์ดํธ๋ nfs๊ฒฝ๋ก๋ฅผ ์๋ ฅ<br>(default ๊ฒฝ๋ก: nfs/data)|
|ceph_option|cephfs rbd|Ceph type์ ์ค์ ํฉ๋๋ค<br>(default: cephfs)|
|ceph_server_ip|์ฌ์ฉ์ ์ค์ |Ceph server IP๋ฅผ ์ค์ ํฉ๋๋ค|
|ceph_server_port|์ฌ์ฉ์ ์ค์ |Ceph server port๋ฅผ ์ค์ ํฉ๋๋ค|
|ceph_id|์ฌ์ฉ์ ์ค์ |ceph ์คํ ๋ฆฌ์ง์ ์ ๊ทผ์ ํ์ํ ID๋ฅผ ์ค์ ํฉ๋๋ค<br>cephfs๋ admin๊ณ์  rbd๋ user๊ณ์ ์ด ํ์ํฉ๋๋ค|
|ceph_key|์ฌ์ฉ์ ์ค์ |ceph ์คํ ๋ฆฌ์ง์ ์ ๊ทผ์ ํ์ํ ๊ณ์ ์ key๊ฐ์ ์ค์ ํฉ๋๋ค|
|ceph_fsid|์ฌ์ฉ์ ์ค์ |ceph ํด๋ฌ์คํฐ ID๋ฅผ ์ค์ ํฉ๋๋ค|
|ceph_fsname|์ฌ์ฉ์ ์ค์ |ceph fsname์ ์ค์ ํฉ๋๋ค|
|etcd_external|yes<br>no|etcd์๋ฒ๋ฅผ ์ธ๋ถ์ ๊ตฌ์ฑํ ์ง์ ์ฌ๋ถ๋ฅผ ์ค์ ํฉ๋๋ค<br>โ yes: etcd๋ฅผ ์ธ๋ถ์ ์ค์ <br>โ no: etcd๋ฅผ ํฌํจํ์ฌ๊ตฌ์ฑ(default)|
|base_registry_option|local external|base_registry์ ์ค์น์ฌ๋ถ๋ฅผ ์ ํํฉ๋๋ค<br>local(default): local์ registry๋ฅผ ์ค์นํฉ๋๋ค<br>host(member-cluster ์ ์ฉ์ต์): local์ ์ค์นํ์ง ์๊ณ , external์ registry์๋ฒ๋ฅผ ๋ฐ๋ผ๋ด๋๋ค<br>(default: local)|
|base_registry_address|์ฌ์ฉ์ ์ค์ |registry์๋ฒ์ address๋ฅผ ์ค์ ํฉ๋๋ค. <br> โ internal ์ธ๊ฒฝ์ฐ, master vip|
|base_registry_port|์ฌ์ฉ์ ์ค์ |registry์๋ฒ์ port๋ฅผ ์ง์ ํฉ๋๋ค.<br>(default): 5000|
|base_registry_id|์ฌ์ฉ์ ์ค์ |registry id๋ฅผ ์ค์ ํฉ๋๋ค<br>(default: accregistry)|
|base_registry_passwd|์ฌ์ฉ์ ์ค์ |registry pw๋ฅผ ์ค์ ํฉ๋๋ค<br>(default: accordionadmin)|
|user_registry_option|registry<br>harbor|user registry์๋ฒ option์ ์ค์ ํฉ๋๋ค<br>registry: ๊ธฐ๋ณธ user registry๋ก ๊ตฌ์ฑํฉ๋๋ค(default)<br>harbor: ๊ธฐ๋ณธ user registry๋ฅผ harbor๋ก ๊ตฌ์ฑํฉ๋๋ค|
|user_registry_address|์ฌ์ฉ์ ์ค์ |user_registry_external์ด yes์ผ๊ฒฝ์ฐ ํ์ฑํ๋๋ฉฐ, ์ธ๋ถ ๋ ์ง์คํธ๋ฆฌ์ ์ฃผ์๋ฅผ ์๋ ฅํฉ๋๋ค. <br> โ internal ์ธ๊ฒฝ์ฐ, master vip|
|user_registry_port|์ฌ์ฉ์ ์ค์ |user_registry_external์ด yes์ผ๊ฒฝ์ฐ ํ์ฑํ๋๋ฉฐ, ์ธ๋ถ ๋ ์ง์คํธ๋ฆฌ์ ํฌํธ๋ฅผ ์๋ ฅํฉ๋๋ค|
|user_registry_external|yes<br>no|user_registry์ ์์น๋ฅผ ์ค์ ํฉ๋๋ค.<br>โ yes: registry๊ฐ external์ ์์นํฉ๋๋ค.<br>โ no: registry๊ฐ local์ ์์นํฉ๋๋ค.(default)|
|user_registry_id|์ฌ์ฉ์ ์ค์ |user_registry_external์ด yes์ผ๊ฒฝ์ฐ ํ์ฑํ๋๋ฉฐ,์ธ๋ถ ๋ ์ง์คํธ๋ฆฌ์ ๊ณ์ ์ ์๋ ฅํฉ๋๋ค<br>(default: accregistry)|
|user_registry_pw|์ฌ์ฉ์ ์ค์ |user_registry_external์ด yes์ผ๊ฒฝ์ฐ ํ์ฑํ๋๋ฉฐ, ์ธ๋ถ ๋ ์ง์คํธ๋ฆฌ์ ํจ์ค์๋๋ฅผ ์๋ ฅํฉ๋๋ค<br>(default: accordionadmin)|
|htpasswd_option|yes<br>no|user registry์ ์ธ์ฆ์ฌ๋ถ๋ฅผ ์ ํํฉ๋๋ค<br>โ yes(default): user registry์์ ํจ์ค์๋ ์ธ์ฆ์ ์งํํฉ๋๋ค<br>โ no: user registry์์ ํจ์ค์๋ ์ธ์ฆ์ ํ์ง์๊ณ  ์ ์ํฉ๋๋ค|
|network_cni|calico<br>weave|์ค์นํ  cni๋ฅผ ์ค์ ํฉ๋๋ค<br>(default: calico)|
|podman_cidr|์ฌ์ฉ์ ์ค์ |podman์ ๋์ญ์ ์ค์ ํฉ๋๋ค|
|IPALLOC_RANGE|์ฌ์ฉ์ ์ค์ |pod network์ Range๋ฅผ ์ค์ ํฉ๋๋ค<br>(network_cni๊ฐ weave์ผ๋ ์ ์ฉ)|
|pod_network_cidr|์ฌ์ฉ์ ์ค์ |pod network์ cidr๋ฅผ ์ค์ ํฉ๋๋ค<br>(network_cni๊ฐ calico์ผ๋ ์ ์ฉ)|
|calico_backend|IPIP<br>vxlan|calico backend ํ์์ ์ค์ ํฉ๋๋ค|
|service_cidr|์ฌ์ฉ์ ์ค์ |service์ cidr๋ฅผ ์ค์ ํฉ๋๋ค|
|kubelet_clusterdns|์ฌ์ฉ์ ์ค์ |service cidr์ ์์ip๋ก๋ถํฐ 10๋ฒ์งธ ip๋ฅผ ์๋ ฅํฉ๋๋ค<br>(ex 10.96.0.0/12์ผ๊ฒฝ์ฐ 10.96.0.1์ด ์ฒซ๋ฒ์งธ ip์ด๋ฏ๋ก 10.96.0.10์ ์๋ ฅ)|
|kubernetes_clusterip|์ฌ์ฉ์ ์ค์ |service cidr์ ์์ip๋ฅผ ์๋ ฅํฉ๋๋ค|
|containers_storage_runroot|์ฌ์ฉ์ ์ค์ |๋ชจ๋  ์ํ ์ ๋ณด๊ฐ ์ ์ฅ๋๋ ๋๋ ํ ๋ฆฌ๋ฅผ ์ง์ ํฉ๋๋ค.<br>(default: /var/run/containers)|
|containers_storage_volume|์ฌ์ฉ์ ์ค์ |์ปจํ์ด๋ ์ ์ฅ์ ๋ณผ๋ฅจ์ ์ง์ ํฉ๋๋ค.<br>(default: /var/lib/containers)|
|kubelet_root_dir|์ฌ์ฉ์ ์ค์ |kubernetes Root ๋๋ ํ ๋ฆฌ๋ฅผ ์ง์ ํฉ๋๋ค<br>(default: /var/lib)|
|kube_addon_dir|์ฌ์ฉ์ ์ค์ |๋ฐฐํฌ๋๋ yaml๋ค์ ์ ์ฅ๊ฒฝ๋ก๋ฅผ ์ค์ ํฉ๋๋ค<br>(default: /etck/kubernetes/addon)|
|docker_rpm_dir|์ฌ์ฉ์ ์ค์ |๊ฐ ์๋ฒ์ ํ์ํ ํ์ผ์ ๋ฐฐํฌํ๋ ๊ฒฝ๋ก๋ฅผ ์ค์ ํฉ๋๋ค<br>(default: /tmp)|
|master_external_ip|์ฌ์ฉ์ ์ค์ |public ip๋ฅผ ์ค์ ํฉ๋๋ค<br> โ master ip์ ์ ๊ทผ๊ฒฝ๋ก๊ฐ ๋ค๋ฅธ๊ฒฝ์ฐ ์ค์  |
|noschedule|yes<br>no|๋ธ๋๋ฅผ ์ถ๊ฐํฉ๋๋ค.<br>(default: no)|
|gpu_server|์ฌ์ฉ์ ์ค์ |GPU ๋ชจ๋ํฐ๋ง์ ์ฌ์ฉํฉ๋๋ค.<br>(default: no)|
|external_prometheus|yes<br>no|yes: ์ธ๋ถ prometheus๋ฅผ ์ฌ์ฉํ  ๊ฒฝ์ฐ(prometheus๊ฐ ์ค์น๋์ง ์์)<br>no: ๋ด๋ถ prometheus๋ฅผ ์ฌ์ฉ|
|prometheus_name|์ฌ์ฉ์ ์ค์ |prometheus ์ด๋ฆ์ ์ง์ ํฉ๋๋ค.<br>(default: prometheus-operator-prometheus)|
|prometheus_ns|์ฌ์ฉ์ ์ค์ |prometheus๊ฐ ์ค์น๋  ๋ค์์คํ์ด์ค๋ฅผ ์ง์ ํฉ๋๋ค.<br>(default: acc-system)|

---

### ๐ก ์ผ๋ฐ์ ์ (sudo ๊ถํ)

`root` ์ ์ ๋ฅผ ์ฌ์ฉํ  ์ ์์ ๊ฒฝ์ฐ, `sudo` ๊ถํ์ ๊ฐ์ง ์ผ๋ฐ์ ์ ๋ก๋ ์ค์น ๊ฐ๋ฅํฉ๋๋ค.

**1. hosts ํ์ผ์ ๋ค์ ์ค์ ์ ๋ณ๊ฒฝํฉ๋๋ค.**

```ini
[all:vars]
ansible_ssh_port=22
ansible_user=root
ansible_password_option=no
#ansible_ssh_pass=ROOT_PASSWORD
keyfile_option=no
#ansible_ssh_private_key_file=/mantech/accordion.pem
ansible_ssh_common_args=-oPubkeyAuthentication=yes
```

|๊ธฐ๋ณธ๊ฐ|๋ณ๊ฒฝ๊ฐ|
|--|--|
|ansible_user=root|ansible_user=[sudo๊ถํ ์ผ๋ฐ์ ์ ]|
|ansible_password_option=no|ansible_password_option=[yes]|
|#ansible_ssh_pass=ROOT_PASSWORD|ansible_ssh_pass=[sudo๊ถํ ์ผ๋ฐ์ ์  ๋น๋ฐ๋ฒํธ]|

**2. ansible.cfg**

```ini
[defaults]
host_key_checking = false
deprecation_warnings = False
command_warnings = False
callback_whitelist = profile_tasks
log_path = /tmp/ansible.log
forks = 20

[privilege_escalation]
become = True
become_ask_pass = True
become_user = root
become_method = sudo
```

|๊ธฐ๋ณธ๊ฐ|๋ณ๊ฒฝ๊ฐ|
|--|--|
| log_path = /tmp/ansible.log | #log_path = /tmp/ansible.log |