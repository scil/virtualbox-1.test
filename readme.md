if you'd like to reuse this machine , 

# way to clone this disk

# way to clone this machine

1. clone it in VirtualBox Gui ( if necessary, delete it  and add it again)

2.  registervm
```
; path必须是绝对路径
"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe"  registervm G:\virtualbox\1.zhenc.test\1.zhenc.test.vbox
"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" list vms
```

如果uuid冲突，参见： Resetting UUID’s on a Virtual-Box VM  
http://www.redstk.com/resetting-uuids-on-a-virtual-box-vm/

3. update .vagrant

put machine id from G:\virtualbox\1.zhenc.test\1.zhenc.test.vbox   
to file `id`

put index id from C:\Users\i\.vagrant.d\data\machine-index\index   
to file `index_uuid`

4. `vagrant up`

5. 在路由器上设置static ip

6. 如果ip不是设置为192.168.1.11，repeat steps about ip and hostname 

7. test 

```
ssh vagrant@192.168.1.111 -i /vagrant/ansible/Files/vagrant/insecure_private_key
```

on ansible master machine 
```
book /vagrant/ansible/2.playbook_os.yml --tags hosts    --limit 1.zhenc.test
```

