---
icon: diagram-project
description: >-
  GNS 3 merupakan sebuah aplikasi Open Source yang digunakan untuk melakukan
  simulasi jaringan secara virtual. GNS 3 dapat menjalankan berbagai macam
  perangkat jaringan dan os jaringan secara virtual.
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

## Uninstall

To cleanly uninstall GNS3 on Linux (Xubuntu), follow these steps:

1.  **Remove GNS3 using `apt`**: Open a terminal and run the following command to uninstall GNS3:

    ```bash
    sudo apt-get remove --purge gns3 gns3-server gns3-gui
    ```
2.  **Remove Dependencies**: If you want to remove any unused dependencies that were installed with GNS3, run:

    ```bash
    sudo apt-get autoremove
    ```
3.  **Delete Configuration Files and Directories**: GNS3 may leave behind configuration files or directories. To remove them, use the following commands:

    ```bash
    rm -rf ~/.gns3
    rm -rf ~/.config/gns3
    rm -rf /etc/gns3
    ```
4.  **Optional: Remove Docker (if installed with GNS3)**: If you installed Docker through GNS3 and wish to remove it as well, run:

    ```bash
    sudo apt-get purge docker-ce docker-ce-cli containerd.io
    sudo apt-get autoremove --purge
    ```
5.  **Clear Any Remaining Dependencies**: Run the following command to clean up any residual package files:

    ```bash
    sudo apt-get clean
    ```

After completing these steps, GNS3 should be completely uninstalled from your system.
