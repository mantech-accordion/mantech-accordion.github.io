---
layout: default
title: 2.4.4 ๐จโ๐ป ์ค์ต
nav_order: 5
parent: 2.4 Accordion ์ค์น
grand_parent: 2. Accordion v2
---

# 2.4.4 ๐จโ๐ป [์ค์ต]์์ฝ๋์ธv2 ์ธ์คํจ
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---


## ๐ ์ค์น ์ค๋น์ฌํญ

<details>
<summary>ssh key ์์ฑ, ๋ณต์ฌ, ์ ์ ํ์ธ</summary>
  
{% highlight bash %}


Ansible script ์ํ์ ์ํด ๊ฐ ๋ธ๋์์ ssh๋ก root ๋ก๊ทธ์ธ์ด ๊ฐ๋ฅํ๋๋ก ์ค์  ๋ณ๊ฒฝํฉ๋๋ค. (Master&node ์๋ฒ) 
$ vi /etc/ssh/sshd_config
โฆ
PasswordAuthentication yes
PermitRootLogin yes
โฆ
$ systemctl restart sshd

master์๋ฒ์์ ssh key๋ฅผ ์์ฑํฉ๋๋ค. (Master ์๋ฒ) (Master์๋ฒ๋ฅผ ํฌํจํ์ฌ ์ ์ฉํด์ผํฉ๋๋ค.)
$ ssh-copy-id -i /root/.ssh/id_rsa.pub [๋ธ๋IP]
The authenticity of host '10.140.0.3 (10.140.0.3)' canโt be established.
ECDSA key fingerprint is 34:b2:38:b8:be:1b:dd:f1:44:d8:ba:ec:92:d1:d6:dc.
Are you sure you want to continue connecting (yes/no)? yes
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed โ if you are prompted now it is to install the new keys
root@10.140.0.3โs password:
Number of key(s) added: 1
Now try logging into the machine, with: "ssh '10.140.0.3'"
and check to make sure that only the key(s) you wanted were added.
(cluster์ ๊ตฌ์ฑ์๋ฒ ๋ชจ๋ ๋ฑ๋กํ  ๋๊น์ง ๋ฐ๋ณตํด์ผ ํฉ๋๋ค.)

SSH Key ์ค์ ์ด ์ ์์ ์ธ ํ์ธํฉ๋๋ค. (Master ์๋ฒ) (Master์๋ฒ๋ฅผ ํฌํจํ์ฌ ์ ์ฉํด์ผํฉ๋๋ค.)
$ ssh [๋ธ๋IP]
The authenticity of host 'acc-node1 (10.140.0.3)' canโt be established.
ECDSA key fingerprint is 34:b2:38:b8:be:1b:dd:f1:44:d8:ba:ec:92:d1:d6:dc.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'acc-node1' (ECDSA) to the list of known hosts.
Last failed login: Mon Sep 4 08:17:06 UTC 2017 from 59.49.38.210 on ssh:notty
There was 1 failed login attempt since the last successful login.
Last login: Mon Sep 4 07:18:33 2017
[root@acc-node1 ~] (ํจ์ค์๋ ์์ด ๋ก๊ทธ์ธ์ด ๊ฐ๋ฅํ๋ฉด ์ ์์๋๋ค.)

{% endhighlight %}
   
</details>


<details>
<summary>Ansible ์ค์น (์์ฝ๋์ธ ์ค์น ํจํค์ง์ ํฌํจ)</summary>
  
{% highlight bash %}
# ๊ฐ OS์ ๋ง๋๋ก ์ ์
$ /rpms/rpm_redhat7/0_ansible/install.sh # Redhat ๊ณ์ด 7๋ฒ์ 
$ /rpms/rpm_redhat8/0_ansible/install.sh # Redhat ๊ณ์ด 8๋ฒ์ 
$ /rpms/rpm_dpkg/0_ansible/install.sh # ubuntu 18๋ฒ์ 

{% endhighlight %}
   
</details>

<details>
<summary>/etc/hosts ํ์ผ ์์ </summary>
  
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
โฆ
{% endhighlight %}
   
</details>


**์์ฝ๋์ธ ์ค์น ์ค์ **
- accordion-installer/hosts ์์ 
- accordion-installer/group_vars/host.yml ์์ 


**์์ฝ๋์ธ ์ค์น ๋ฐ ํ์ธ**
- accordion-installer/install.sh
- accordion-installer/status.sh

**์ค์น ์ค์ **
```bash
$ accordion-installer/install.sh
```

**์ค์น ์งํ**
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

**์ค์น ํ์ธ**
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

**์์ฝ๋์ธ ์ ์ URL**

| URL      | ID    | PW    |
|:---------|:------|:------|
| https://VIP://30000      | admin  | accordion!@# |

![acc-ins-1.png](/assets/images/accordion/acc-ins-1.png){: width="800" }


---


## ๋ผ์ด์ ์ค ๋ฑ๋ก
`๊ธ๋ก๋ฒ ์ค์ ` โก `๋ผ์ด์ ์ค` โก `๋ฑ๋ก`

![practice-1.png](/assets/images/accordion/practice-1.png)
![practice-2.png](/assets/images/accordion/practice-2.png)
