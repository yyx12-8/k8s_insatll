wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo  
curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo

1.在你的master节点或者其他主机上安装ansible    这里  可参考  
https://blog.csdn.net/qq_43527128/article/details/141996193?ops_request_misc=%257B%2522request%255Fid%2522%253A%25229EAC6653-990C-4AFE-8B3D-FC118F8B3077%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=9EAC6653-990C-4AFE-8B3D-FC118F8B3077&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_ecpm_v1~rank_v31_ecpm-2-141996193-null-null.nonecase&utm_term=ansible&spm=1018.2226.3001.4450  
2.配置你的主机清单  一般在/etc/ansible/hosts中  
例：  
[k8s_all]  
master ansible_host=192.168.29.220  
node1 ansible_host=192.168.29.221  
node2 ansible_host=192.168.29.222  

[k8s_masters]  
master ansible_host=192.168.29.220  

[k8s_nodes]  
node1 ansible_host=192.168.29.221  
node2 ansible_host=192.168.29.222  

[kubernetes:children]  
masters  
nodes  

3.修改roles/k8s_master/tasks/main.yaml  第18 行  为你master ip  
- name: Get the join command and save to a file  
  command: kubeadm token create --print-join-command  
  register: join_command  
  delegate_to: xxxxxxxx  # 替换为 Master 节点的 IP 地址

4.ssh-copy-id root@你管理的ip  

5.控制主机上，cd到项目目录， 执行ansible-playbook k8s_install.yaml  
