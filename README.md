
라즈베리 클러스터 만들기

1. Rasberry Pi Imager 로 기본적인 OS (64Bit, Lite) 설치
2. (공유기의 DHCP 관리에서) 노드별 고정 Private IP 할당 
3. 환경에 맞게 `ansible.cfg` 수정
4. `bootstrap.yaml` 을 실행하여 초기 업데이트 및 환경 준비 
5. `site.yml` 을 실행하여 K3S 설치 
6. `extra.yaml` 을 실행하여 helm 및 기타 툴 설치 

아래는 원본 설명 

----

# Build a Kubernetes cluster using k3s via Ansible

Author: <https://github.com/itwars>

## K3s Ansible Playbook

Build a Kubernetes cluster using Ansible with k3s. The goal is easily install a Kubernetes cluster on machines running:

- [X] Debian
- [X] Ubuntu
- [X] CentOS

on processor architecture:

- [X] x64
- [X] arm64
- [X] armhf

## System requirements

Deployment environment must have Ansible 2.4.0+
Master and nodes must have passwordless SSH access

## Usage

First create a new directory based on the `sample` directory within the `inventory` directory:

```bash
cp -R inventory/sample inventory/my-cluster
```

Second, edit `inventory/my-cluster/hosts.ini` to match the system information gathered above. For example:

```bash
[master]
192.16.35.12

[node]
192.16.35.[10:11]

[k3s_cluster:children]
master
node
```

If needed, you can also edit `inventory/my-cluster/group_vars/all.yml` to match your environment.

Start provisioning of the cluster using the following command:

```bash
ansible-playbook site.yml -i inventory/my-cluster/hosts.ini
```

## Kubeconfig

To get access to your **Kubernetes** cluster just

```bash
scp debian@master_ip:~/.kube/config ~/.kube/config
```
