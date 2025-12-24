# Lab Practice - Linux User Access Management


---


## Objectives
Repository ini berisikan dokumentasi praktik administrasi Linux 
dengan fokus learning pada user access management. Seluruh proses
dilakukan di sistem Linux secara langsung untuk mensimulasikan 
pengaturan user, group, permission, sudo access, audit, dan cleanup.


---


## Environment
- Virtual Machine  : VMware Workstation Pro 25H2
- OS		   : Linux Ubuntu dekstop 24.04
- Shell		   : Zsh


---


## Implementation Steps
### 1. Create User
#### Basic Command
```zsh
sudo useradd [user_name]
```
#### Best Practice Command
```zsh
sudo useradd -m -s /bin/bash [user_name]
```
#### Verify
```zsh
id [user_name]
```
**Note**
> Basic command: user default setting, no home directory no password
> -m buat home directory /home/[user_name]
> -s set default shell

---

### 2. Set User Password
#### Command
```zsh
sudo passwd [user_name]
```
#### Verify
```
su - [user_name]
```

---

### 3. Delete User
#### Basic Command
```zsh
sudo userdel [user_name]
```
#### Best Practice Command
```zsh
sudo userdel -r [user_name]
```
**Note**
> -r: hapus beserta home directory

---

### 4. Create Group
#### Command
```zsh
sudo groupadd [group_name]
```
#### Verify
```zsh
getent group [group_name]
```

---

### 5. Assign User to Group
#### Command
```zsh
sudo usermod -aG [group_name] [user_name]
```
#### Verify
```zsh
groups [user_name]
```
**Note**
> -aG: menambahkan ke group tanpa menghapus group lama

---

### 6. Remove User from Group
#### Command
```zsh
sudo gpasswd -d [user] [group]
```
#### Verify
```
groups [user]
```

---

### 7. Set Directory Ownership
#### Make Directory
```zsh
sudo mkdir [folder_name]
```
#### Change Owner
```zsh
sudo chown [user]:[group] [folder]
```
#### Verify
```zsh
ls -l
```
**Note**
> -R: perubahan di folder beserta isi

---

### 8. Set File & Directory Permissions
#### Command
```zsh
sudo chmod [permissions code] [folder/file]
```
#### Verify
```zsh
ls -ld [folder]
```
**Note**
> 4: r
> 2: w
> 1: x

---

### Verify User Access
#### Login user 
```zsh
su - [user]
```
#### Test action
```zsh
cat [file]
mkdir [folder]
cd [folder]
touch [file]
.[file]
```
**Note**
> User dan group owner jika gagal, permission/ownership salah!
> Jika folder access untuk other = 0, maka user non owner dan group harus denied!

---

### Manage Sudo Access
#### Assign User to Sudo Group 
- Command user sudoers
```zsh
sudo usermod -aG sudo [user]
```
- Switch user
```zsh
su - [user]
```
- Verify
```zsh
sudo whoami
```
**Note**
> Jika hasil verify user = root, maka berhasil

#### Visudo
- Step 1. Command visudo
```zsh
sudo visudo
```
- Step 2. Cari barisan   : %sudo   ALL=(ALL:ALL) ALL
- Step 3. Setup user     : User   ALL=(ALL) NOPASSWD: ALL
- Step 4. Finish, save & exit
**Note**
> User  = user yang diberi akses
> ALL pertama = semua host
> (ALL) = bisa jalankan perintah sebagai user siapa saja/root
> NOPASSWD: = tidak perlu password saat sudo
> ALL terakhir = boleh jalankan semua perintah

---

### Audit with Logs
#### Audit user login history
```zsh
last [user]
```
#### Audit SHH login
```zsh
sudo journalctl -u ssh
```
#### Audit error & failure system
```zsh
sudo journal -xe
```
#### Audit failed
```zsh
sudo journalctl | grep -i failed
```
#### Audit Denied
```zsh
sudo journal | grep -i denied
#### Audit permission denied & file access kernel level
```zsh
sudo journalctl -k | grep -i denied
```
#### Audit permission denied & file acces real-time log
```zsh
sudo dmesg | grep -i denied
```
#### Audit sudo activity
```zsh
sudo cat /var/log/auth.log | grep sudo
```
or
```zsh
sudo journalctl | grep sudo
```

---

### Cleanup
#### Remove user
```zsh
sudo userdel -r [user]
```
#### Remove group
```zsh
sudo groupdel [group]
```
#### Remove directory
```zsh
sudo rm -rf [directory]
```
