# Lab Practice - Linux User Access Management


## Objectives
Di dalam section ini, saya akan mempraktikkan proses sederhana 
dalam administration linux user access management.


## Environment
virtual machine    : VMware Workstation Pro 25H2
OS		   : Linux Ubuntu dekstop 24.04
Shell		   : Zsh


## Implementation Steps
### Create User
Basic Command
```zsh
sudo useradd [user_name]
```
Best Practice Command
```zsh
sudo useradd -m -s /bin/bash [user_name]
```
Verification
```zsh
id [user_name]
```
#### Note
- Basic command: user default setting, no home directory no password
- -m buat home directory /home/[user_name]
- -s set default shell


### Set User Password
Command
```zsh
sudo passwd [user_name]
```
Verification
```
su - [user_name]
```


### Delete User
Basic Command
```zsh
sudo userdel [user_name]
```
Best Practice Command
```zsh
sudo userdel -r [user_name]
```
#### Note
- -r: hapus beserta home directory


### Create Group
Command
```zsh
sudo groupadd [group_name]
```
Verification
```zsh
getent group [group_name]
```


### Assign User to Group
Command
```zsh
sudo usermod -aG [group_name] [user_name]
```
Verification
```zsh
groups [user_name]
```
#### Note
- -aG: menambahkan ke group tanpa menghapus group lama


### Remove User from Group
Command
```zsh
sudo gpasswd -d [user] [group]
```
Verify
```
groups [user]
```


### Set Directory Ownership
Command - make directory
```zsh
sudo mkdir [folder_name]
```
Command - change owner
```zsh
sudo chown [user]:[group] [folder]
```
Verification
```zsh
ls -l
```
#### Note
- -R: perubahan di folder beserta isi


### Set File & Directory Permissions
Command
```zsh
sudo chmod [permissions code] [folder/file]
```
Verification
```zsh
ls -ld [folder]
```
#### Note
- 4: r
- 2: w
- 1: x


### Verify User Access
Login user 
```zsh
su - [user]
```
Test action
```zsh
cat [file]
mkdir [folder]
cd [folder]
touch [file]
.[file]
```
#### Note
- User dan group owner jika gagal, permission/ownership salah!
- jika folder access untuk other = 0, maka user non owner dan group harus denied!

### Manage Sudo Access
#### Assign User to Sudo Group 
Command user sudoers
```zsh
sudo usermod -aG sudo [user]
```
Switch user
```zsh
su - [user]
```
Verify
```zsh
sudo whoami
```
Note
- Jika hasil verify user = root, maka berhasil

#### Visudo
Step 1. Command visudo
```zsh
sudo visudo
```
Step 2. Cari barisan   : %sudo   ALL=(ALL:ALL) ALL
Step 3. Setup user     : User   ALL=(ALL) NOPASSWD: ALL
Step 4. Finish, save & exit

Note
- User  = user yang diberi akses
- ALL pertama = semua host
- (ALL) = bisa jalankan perintah sebagai user siapa saja/root
- NOPASSWD: = tidak perlu password saat sudo
- ALL terakhir = boleh jalankan semua perintah


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

---

