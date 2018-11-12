# product produced

ubuntu-xenial-16_副本.vdi

# Vars

IP: 192.168.1.111

hostname: 1.zhenc.test



# STEP 0: INIT 

1. Vagrantfile. disable this line:   `config.ssh.insert_key = false`

sed: `sed -i "s/config.ssh.insert_key = false/#config.ssh.insert_key = false/" Vagrantfile`

2. check if same name `1.zhenc.test` (不要先在virtublabox界面上操作）
```
set path=D:\Program Files\HashiCorp\Vagrant\embedded\usr\bin;%path%

"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" list vms  | grep 1.zhenc.test
```
if so, delete it
"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" unregistervm --delete    e0e6a12f-c77a-468d-a2e6-9e33972e8508 

如果还不行，编辑 C:\Users\i\.vagrant.d\data\machine-index\index 删除相关机器

3. `vagrant up`

4. `vagrant ssh` and do

安装 python3 不需要安装 ansible   
参考：D:\vagrant\ansible\0.first_install_ansible\1.install_on_ubuntu.sh
```
# for china
sudo cp /vagrant/ansible/files/ubuntu/sources.list /etc/apt/sources.list

# Update Repositories
sudo apt-get update -y
sudo apt install -y python3-pip &&  pip3 install --upgrade pip 
sudo pip3 install setuptools # 如果出错  sudo cp /vagrant/ansible/files/ubuntu/pip3 /usr/bin/pip3 见：https://blog.csdn.net/accumulate_zhang/article/details/80269313
# ensure there is python
python -V || sudo ln -s /usr/bin/python3 /usr/bin/python

```

安装 public key 
```    
# 根据D:\vagrant\ansible\0.first_install_ansible\2.prepare.sh  
curl https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub > ~/.ssh/authorized_keys 
exit
```

5.  `vagrant halt` and do

// 教程：`D:\vagrant\ansible\0.first_install_ansible\2.prepare.sh  `  

```
#  edit  Vagrantfile.   enable this line:    config.ssh.insert_key = false   
sed -i -E "s/\s*#config.ssh.insert_key = false/config.ssh.insert_key = false/" Vagrantfile
#  rename the new vm private key .vagrant/machines/default/virtualbox/private_key to anything else
mv .vagrant/machines/default/virtualbox/private_key .vagrant/machines/default/virtualbox/_private_key
```



6. IP

    1. 路由器设置静态IP: 192.168.1.111 并重启

    2. 修改 ansible 目录。如 `D:\vagrant\ansible\hosts ` 或 `/vagrant/ansible/files/other/ambari/ansible-ambari-manager.hosts.dev` ，让 1.zhenc.test 和 ip 111 配对。在 ansible master 机上发布：
`sudo cp /vagrant/ansible/hosts /etc/ansible/hosts -f`

    3. edit windows hosts file `192.168.1.111 1.zhenc.test `

7. vagrant up; hostname and  test

on windows:  
```
"c:\Program Files\Git\usr\bin\ssh.exe"  -i D:\vagrant\ansible\files\vagrant\insecure_private_key vagrant@192.168.1.111
```
如果必要 清理C:\Users\i\.ssh\known_hosts

on ansible master machines:  
```
# 清理旧的: 
ssh-keygen -f "/home/vagrant/.ssh/known_hosts" -R 192.168.1.111

# 尝试登陆并退出
ssh vagrant@192.168.1.111 -i /vagrant/ansible/files/key/vagrant/insecure_private_key

```

8. ansible  

at ansible master machines:
```
book /vagrant/ansible/2.playbook_os.yml --tags chrony  --limit 1.zhenc.test 

book /vagrant/ansible/2.playbook_os.yml --tags hosts    --limit 1.zhenc.test


```

如果 ERROR! Timeout (12s) waiting for privilege escalation prompt
killall ssh 
或重新进入系统
或加上参数 --timeout 如  book /vagrant/ansible/2.playbook_os.yml --tags hosts    --limit 1.zhenc.test --timeout 30