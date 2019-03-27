# kursas

158.177.175.88
studentvm2
CRClab!2019&studentvm2


git 
jarode2
jarode2123

generate key
ssh-keygen 

cat ~/.ssh/id_rsa.pub

copy key to SSH and GPG key
https://github.com/settings/keys



sudo su 
yum install git -y

add global user to git on VM

 git config --global user.name "jarode2"
 git config --global user.email "jarode2@gmail.com"

git clone http://github.com/jarode/kursas

isntall -y ansible.noarch

cd kursas

ansible-galaxy init --init-path roles/ motd

nano inventory

  localhost

save file

nano ansible.cfg
  [defaults]
  host_key_checking = False
  inventory = inventory
save file




cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

check ping
ansible localhost -m ping

create template

nano roles/motd/templates/motd.j2

  Hello to {{ansible_nodename}}

save file

nano roles/motd/tasks/main.yml

---
- name: 'Create /etc/motd file'
  template:
    src: "{{ role_path }}/templates/motd.j2"
    dest: 'etc/motd'
    owner: root
    group: root
    mode: '0644'
    
 save file
 
 create playbook
 nano motd.yml
 
---
- hosts: all
  tasks:
  - import_role
      name: motd
save file

nano /home/studentvm2/kursas/motd.yml


git 

ansible-playbook webserver.yml

---------------------------------------------
next task
---
- name: 'Install webserwer package'
  yum:
    name: 'httpd'
    state: present
    
- name: 'Create index.html'
  template:
    src: "{{ role_path }}/tempates/index.html"
    dest: '/var/www/html/index.html'
    
- name: 'Start and enable httpd service'
  service:
    name: 'httpd'
    enable: yes
    state: started



webserver.yml create
---
- hosts: webservers
  tasks:
  - import_role:
      name: webserver



site.yml create
---
- import_playbook: motd.yml
- import_playbook: webserver.yml


rpm -qa --last | grep httpd - check package

systenctl statys 
