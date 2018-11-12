
# product produced

S:\OS\virtualbox\1.zhenc.test\ubuntu-xenial-16_副本2.vdi

if you'd like to use it , 
 参考 Vagrantfiel.step.0.init.md 中的 clone 步骤


# at ansible master machines:
```

# 为了空间 和安装包共享
# sudo ln -s /vagrant/.composer/cache_apt_archives/   /var/ca che/apt/archives
ansible 1.zhenc.test  -m file -a "path=/var/cache/apt/archives state=absent "   -b
ansible 1.zhenc.test  -m file -a "dest=/var/cache/apt/archives  src=/vagrant/.composer/cache_apt_archives/ state=link "   -b

# 给 111 安装 java
book /vagrant/ansible/10.playbook_java.yml --tags java --limit 1.zhenc.test

# 给 111 安装 docker 和 docker compose
# 如果  failed: E: There were unauthenticated packages and -y was used without --allow-unauthenticated\n" 就手动一下   sudo apt install docker-ce -y
book /vagrant/ansible/1.playbook_docker_nfs_gui.yml --tags docker2 --limit 1.zhenc.test 

```