## Exemplos de utlização

```sh
❯ ansible -i hosts.cfg all -m ping -u lc -k               
SSH password: 
pop-os | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

❯ ansible -i hosts.cfg all -m ping -u rdo --private-key=~/.ssh/lc_id_rsa --ask-become-pass
pop-os | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

❯ ansible -i hosts.cfg all -m ping -u rdo --private-key=~/.ssh/lc_id_rsa --ask-become-pass
pop-os | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

## Ansible ad-hoc commandos
```sh
❯ ansible -i hosts.cfg servers -a "uname -a" -u lc -k 
SSH password: 
pop-os | CHANGED | rc=0 >>
Linux pop-os 5.8.0-7642-generic #47~1614007149~20.04~82fb226-Ubuntu SMP Tue Feb 23 02:56:27 UTC  x86_64 x86_64 x86_64 GNU/Linux

❯ ansible -i hosts.cfg pop-os -m apt -a "name=vim" --check -u lc -k
SSH password: 
pop-os | SUCCESS => {
    "cache_update_time": 1616369728,
    "cache_updated": false,
    "changed": false
}


❯ ansible -i hosts.cfg pop-os -m apt -a "name=htop" --check -u lc -k 
SSH password: 
pop-os | CHANGED => {
    "cache_update_time": 1616369728,
    "cache_updated": false,
    "changed": true,
    "stderr": "",
    "stderr_lines": [],
    "stdout": "NOTE: This is only a simulation!\n      apt-get needs root privileges for real execution.\n      Keep also in mind that locking is deactivated,\n      so don't depend on the relevance to the real current situation!\nReading package lists...\nBuilding dependency tree...\nReading state information...\nThe following NEW packages will be installed:\n  htop\n0 upgraded, 1 newly installed, 0 to remove and 30 not upgraded.\nInst htop (2.2.0-2build1 Ubuntu:20.04/focal [amd64])\nConf htop (2.2.0-2build1 Ubuntu:20.04/focal [amd64])\n",
    "stdout_lines": [
        "NOTE: This is only a simulation!",
        "      apt-get needs root privileges for real execution.",
        "      Keep also in mind that locking is deactivated,",
        "      so don't depend on the relevance to the real current situation!",
        "Reading package lists...",
        "Building dependency tree...",
        "Reading state information...",
        "The following NEW packages will be installed:",
        "  htop",
        "0 upgraded, 1 newly installed, 0 to remove and 30 not upgraded.",
        "Inst htop (2.2.0-2build1 Ubuntu:20.04/focal [amd64])",
        "Conf htop (2.2.0-2build1 Ubuntu:20.04/focal [amd64])"
    ]
}
```

```sh
❯ ansible -i hosts.cfg pop-os -a "df -h" -u lc -k
SSH password: 
pop-os | CHANGED | rc=0 >>
Sist. Arq.      Tam. Usado Disp. Uso% Montado em
udev            1,9G     0  1,9G   0% /dev
tmpfs           394M  1,7M  392M   1% /run
/dev/sda1        55G  6,7G   46G  13% /
tmpfs           2,0G     0  2,0G   0% /dev/shm
tmpfs           5,0M     0  5,0M   0% /run/lock
tmpfs           2,0G     0  2,0G   0% /sys/fs/cgroup
tmpfs           394M   20K  394M   1% /run/user/110
tmpfs           394M   28K  394M   1% /run/user/1000
/dev/sr0         59M   59M     0 100% /media/lc/VBox_GAs_6.1.18
tmpfs           394M  8,0K  394M   1% /run/user/900
```

## Exemplo de ansible-playbook
```yml
exemplo.yml
---
- hosts: group1
  tasks:
  - name: Install lldpad package
    yum:
      name: lldpad
      state: latest
  - name: check lldpad service status
    service:
      name: lldpad
      state: started
  - name: Set resolver for server
    template:
      src: dns.j2
      dest: /etc/resolv.conf
      group: root
      owner: root
      mode: "0644"
      tag: resolver
```

## Exemplo de estrutra que utilizo.
```yml
❯ tree   
.
├── ansible.cfg
├── common.yml
├── inventario.ini
├── playbook_deploy.yml
├── playbook_remove.yml
├── poetry.lock
├── pyproject.toml
└── roles
    └── common
        ├── defaults
        │   └── main.yml
        ├── handlers
        ├── tasks
        │   ├── base.yml
        │   ├── files
        │   │   ├── README.md
        │   │   └── requirements.txt
        │   └── main.yml
        ├── templates
        └── vars
            └── main.yml


#Executar: 
ansible-playbook playbook_deploy.yml
ansible-galaxy init --init-path=roles common

```
referencias:
https://www.digitalocean.com/community/cheatsheets/how-to-use-ansible-cheat-sheet-guide
https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04


Explicar imagem:
https://www.guru99.com/ansible-tutorial.html