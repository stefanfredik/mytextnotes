---
description: >-
  GNS 3 merupakan sebuah aplikasi Open Source yang digunakan untuk melakukan
  simulasi jaringan secara virtual. GNS 3 dapat menjalankan berbagai macam
  perangkat jaringan dan os jaringan secara virtual.
icon: diagram-project
---

# GNS 3

## Instalasi

### Add Repo

Add and update repo,

```bash
sudo add-apt-repository ppa:gns3/ppa
sudo apt update                                
sudo apt install gns3-gui gns3-server
```

### Install Docker

Remove Old Docker

```bash
sudo apt remove docker docker-engine docker.io
```

### Install New Docker

```bash
sudo apt-get install apt-transport-https ca-certificates curl \ software-properties-common
```

### Import Docker Key

Gpg Key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```bash
sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable"
```

### Install Docker CE

```bash
sudo apt update
sudo apt install docker-ce
```

Change Usermod and Group of following Application

```bash
ubridge libvirt kvm wireshark docker
```

Change User mode

```bash
sudo usermod -aG [groupname] [username] 

#example : 
# sudo usermod -aG ubridge fredikstefan
# sudo usermod -aG kvm fredikstefan

```

### Install  Virtual Bridge Network

```bash
sudo apt-get install bridge-utils
```

### Install Libvirt

Libvirt merupakan library yang  dibutuhkan untuk menjalankan virtual network pada gns3 sepert NAT atau Bridge network pada GNS 3.

```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system
```

Modifikasi User Group Untuk Libvirt

```bash
sudo adduser $USER libvirt
```

### Config Virsh

Ini berguna untuk mengaktifkan virtual netowork adapter dan dapat berjalan otomatis

Cek Status Network Virtual Adapter

```bash
 virsh net-list --all
```

Jika masih disable atau nonaktif. Silahkan aktifkan dengan perintah berikut :&#x20;

```bash
virsh net-start default
virsh net-autostart default
```
