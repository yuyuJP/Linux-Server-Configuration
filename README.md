# Linux-Server-Configuration
This repository is for Udacity Full Stack Web Developer Nanodegree Program Submission


## User Management
1. Create user
```
$ sudo adduser grader
```
2. Give `grader` sudo access
```
$ sudo visudo
```
Add line: `grader ALL=(ALL) NOPASSWD:ALL`

3. Disable remote login as `root`
```
sudo nano /etc/ssh/sshd_config
```
Change line to : `PermissionRootLogin no`

## SSH Login with key
1. Change the SSH default port to `2200`
```
$ sudo nano /etc/ssh/sshd_config
```
Change line to : `Port 2200`

2. Change user to `grader` and generate key pair
```
$ sudo su - grader
```
Follow this [instruction](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2) or udacity course

3. You can log in using the following command
```
$ ssh -i [key file path] grader@18.179.61.173 -p 2200
```
