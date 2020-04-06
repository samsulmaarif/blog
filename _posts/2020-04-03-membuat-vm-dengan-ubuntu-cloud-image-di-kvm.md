---
layout: post
title: Membuat VM dengan Ubuntu Cloud Image di KVM/virsh
date: '2020-04-03'
author: Samsul Maarif
categories: blog
tags:
- Linux
bigimg:
  - "/img/2014-04-29-08.58.56.jpg" : ""
share-img: "/img/2014-04-29-08.58.56.jpg"
---

> **PERINGATAN**, Aku akan mengambil harta karunmu. Eh, bukan. Kalo disebut tutorial sebenarnya enggak juga, postingan ini lebih ke catatan pribadi. Dan mungkin akan banyak yang sulit dipahami, terutama jika pembaca belum pernah menggunakan Linux, atau belum pernah menggunakan virsh. Namun, saya akan tetap menulikan ini sebagai "tutorial" yang mungkin akan saya baca sendiri di masa yang akan datang.

Tutorial ini membutuhkan alat-alat sebagai berikut :

- laptop/server dengan spesifikasi yang cukup
- OS Linux dengan aplikasi terinstall:
  - virt-manager
  - wget
  - vim
  - mkisofs

Download cloud image Ubuntu 18.04

```
mkdir ~/virsh && cd ~/virsh
wget https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.img
```

buat salinan image Ubuntunya

```
cp bionic-server-cloudimg-amd64.img gitlab.img
```

Persiapkan cloud-init.iso

```
mkdir cloud-init && cd cloud-init
wget https://raw.githubusercontent.com/larsks/virt-utils/master/create-config-drive
vim user-data
```

isikan user-data sebagai berikut :

```        
#cloud-config
users:
- name: samsul
  ssh-authorized-keys:
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCuoi+XDKTfEhE6tSFA7MrEy0iVqJP17E42O5lTYQZ+0gJxBKKotvct2RAEmmV9qoBeikNOC4dWNIxru7rVn83zVM1slpR5OkcB5dirkBGyySUDq0QRaRczx4ZY8aJyouXyjsFZr5/sdiRH6FGOSKAOFjpr5r6sad4QB/1qGZa2e5QzGbp09Ipc0S+NwTGARyX/VN+ffchOW5BP+T4YqwDLY7mRMtoJjR8WnwRn/quC/PTvGA/Z2NpM7z3502EcKRHP7CrVZplhWfLXYtKkbMSK4lndNrDs0Sy+CcpIzgy1oU4mNGhwCHSoK9l9kEW1727MXXEPsSYIqcMLRSY3wSVv samsul@studio
  sudo: ['ALL=(ALL) NOPASSWD:ALL']
  groups: sudo
  shell: /bin/bash
- name: root
  ssh-authorized-keys:
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCuoi+XDKTfEhE6tSFA7MrEy0iVqJP17E42O5lTYQZ+0gJxBKKotvct2RAEmmV9qoBeikNOC4dWNIxru7rVn83zVM1slpR5OkcB5dirkBGyySUDq0QRaRczx4ZY8aJyouXyjsFZr5/sdiRH6FGOSKAOFjpr5r6sad4QB/1qGZa2e5QzGbp09Ipc0S+NwTGARyX/VN+ffchOW5BP+T4YqwDLY7mRMtoJjR8WnwRn/quC/PTvGA/Z2NpM7z3502EcKRHP7CrVZplhWfLXYtKkbMSK4lndNrDs0Sy+CcpIzgy1oU4mNGhwCHSoK9l9kEW1727MXXEPsSYIqcMLRSY3wSVv samsul@studio
timezone: Asia/Jakarta
```

buat file iso cloud-init

```
chmod +x create-config-drive
./create-config-drive -u user-data cloud-init.iso
mv cloud-init.iso /home/samsul/virsh/cloud-init.iso
cd ..
```

Buat sebuah network dengan mode nat terlebih dahulu di virsh

```
cat << EOF > network.xml
<network connections='1'>
  <name>int0</name>
  <uuid>62e8ba7d-c319-4a47-86bc-24e215c389a8</uuid>
  <forward mode='nat'>
    <nat>
      <port start='1024' end='65535'/>
    </nat>
  </forward>
  <bridge name='virbr2' stp='on' delay='0'/>
  <mac address='52:54:00:d5:d8:25'/>
  <domain name='int0'/>
  <ip address='10.11.12.1' netmask='255.255.255.0'>
    <dhcp>
      <range start='10.11.12.10' end='10.11.12.99'/>
    </dhcp>
  </ip>
</network>
EOF
```

Gunakan `net-define` untuk membuatnya

```
sudo virsh net-define --file network.xml
```

Buat VM gitlab dengan virt-install

```
sudo virt-install --import --name gitlab --memory 4096 --vcpus 2 --cpu host \
     --disk /home/samsul/virsh/gitlab.img,format=qcow2,bus=virtio \
     --disk /home/samsul/virsh/cloud-init.iso,device=cdrom \
     -w network=int0 --check all=off
```

Setelah hidup, ubah ukuran disk image menjadi 40GB

```
sudo virsh blockresize gitlab --path=/home/samsul/virsh/gitlab.img --size=40GB
```

Restart kembali VM-nya agar ukuran diska-nya berubah sesuai ukuran yang kita inginkan.

Untuk keperluan backup dan/atau membuat vm lain, kita dapat memanfaatkan `virt-clone`. Misalnya sebagai berikut:

```
sudo virt-clone -o gitlab -n bionic -f /home/samsul/virsh/bionic.img
```

Namun pastikan VM original (dalam hal ini bernama `gitlab`) dalam kondisi shutdown atau mati. Nyalakan VM `gitlab` dengan

```
sudo virsh start gitlab
sudo virsh list --all
```

Hasilnya sebagai berikut:

```
Id   Name     State
-------------------------
1    gitlab   running
-    bionic   shut off
```

Jika kita ingin melihat alamat IP VM gitlab bisa pake cara ini:

```
sudo virsh domifaddr gitlab
 Name       MAC address          Protocol     Address
-------------------------------------------------------------------------------
 vnet0      52:54:00:b5:e8:09    ipv4         10.11.12.99/24
```

Setelah mengetahui alamat ip vm, seharusnya kita sudah bisa melakukan ssh

```
ssh samsul@10.11.12.99
```

Demikian, selamat menikmati.
