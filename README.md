in both the ansible and webserver
-
install docker in both servers using this [link](https://github.com/njokuifeanyigerald/installation_scripts/blob/master/docker.sh)
```bash
sudo su
passwd root
```
edit the file
```bash
vi /etc/ssh/sshd_config
```
uncomment `PermitRootLogin` then remove the word at the end and add yes. 
![image](https://github.com/njokuifeanyigerald/ansible_ec2_kubernetes/assets/46121207/89f07171-80f3-46fe-af1f-f0dd256878fe)

in the `PasswordAuthenication` add yes
![image](https://github.com/njokuifeanyigerald/ansible_ec2_kubernetes/assets/46121207/def65e5c-89a8-4e46-ba70-5b7a84d57b75)


restart the service
```bash
service sshd restart
```
in only the ansible server
-
```bash
sudo su
ssh-keygen
ssh-copy-id -i /root/.ssh/id_rsa.pub  root@172.31.18.69
```
then ssh into the server, this time you won't need to input password
```bash
ssh root@172.31.18.69
```
install ansible on the ansible server using this [link](https://github.com/njokuifeanyigerald/installation_scripts/blob/master/ansible.sh)

then edit this file
```bash
 vi /etc/ansible/hosts
```
add this line to it
```bash
[node]
<ip address>
```
![image](https://github.com/njokuifeanyigerald/ansible_ec2_kubernetes/assets/46121207/55ccfcd8-d7db-4f7d-a4c5-fc5f833a2f37)

then run the command
```bash
ansible -m ping node
```
-------------------------------------------
- on Jenkins install the ssh-agent plugin, and other neccessary plugin.
- Create an `ssh credential`, username should be name of the machine user not root, use this command to know the user
```bash
whoami
```
