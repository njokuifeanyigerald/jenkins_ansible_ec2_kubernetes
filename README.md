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
create a new user in both the ansible and webserver
-------------------

```bash
sudo adduser username
```
This command will prompt you to set a password and provide additional information about the new user.

Granting Administrative Privileges (sudo access):
Open the sudoers file using the visudo command. This command opens the file in the default text editor, ensuring that syntax errors are checked before saving.

```bash
sudo visudo
```
Find the line that looks like:

```html
%admin   ALL=(ALL:ALL) ALL
```
This line grants sudo access to users in the sudo group.
To grant administrative privileges to your user, add a line below it:

```html
username   ALL=(ALL) NOPASSWD: ALL
```
Replace username with the actual username you created.

Save the changes and exit the editor.

Testing Sudo Access:
-----------------
Switch to the newly created user:

```bash
su - username
```

in only the ansible server
------------------------------

```bash
ssh-keygen
ssh-copy-id -i /root/.ssh/id_rsa.pub  username@172.31.18.69
```
then ssh into the server, this time you won't need to input password
```bash
ssh username@172.31.18.69
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
