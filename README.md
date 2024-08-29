# Домашнее задание к занятию 1 «Введение в Ansible»-Абрамов Сергей

## Подготовка к выполнению

1. Установите Ansible версии 2.10 или выше.
2. Создайте свой публичный репозиторий на GitHub с произвольным именем.
3. Скачайте [Playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.

## Основная часть

1. Попробуйте запустить playbook на окружении из `test.yml`, зафиксируйте значение, которое имеет факт `some_fact` для указанного хоста при выполнении playbook.

 ### Решение

```
serg@ubuntu:~/git/Ansible-01-hw/playbook$ ansible-playbook -i ./inventory/test.yml site.yml

PLAY [Print os facts] *************************************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************************************************
ok: [localhost]

TASK [Print OS] *******************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *****************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": 12
}

PLAY RECAP ************************************************************************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```

some_fact = 12

2. Найдите файл с переменными (group_vars), в котором задаётся найденное в первом пункте значение, и поменяйте его на `all default fact`.

### Решение

```

serg@ubuntu:~/git/Ansible-01-hw/playbook$ ansible-playbook -i ./inventory/test.yml site.yml 

PLAY [Print os facts] *************************************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************************************************
ok: [localhost]

TASK [Print OS] *******************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *****************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}

PLAY RECAP ************************************************************************************************************************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 

```

3. Воспользуйтесь подготовленным (используется `docker`) или создайте собственное окружение для проведения дальнейших испытаний.

### Решение

```
serg@ubuntu:~/git/Ansible-01-hw/playbook$ docker run -dit --name centos7 pycontribs/centos:7 sleep 6000000
Unable to find image 'pycontribs/centos:7' locally
7: Pulling from pycontribs/centos
2d473b07cdd5: Pull complete 
43e1b1841fcc: Pull complete 
85bf99ab446d: Pull complete 
Digest: sha256:b3ce994016fd728998f8ebca21eb89bf4e88dbc01ec2603c04cc9c56ca964c69
Status: Downloaded newer image for pycontribs/centos:7
b9abb4fc9e4ae4f743aeb4e6ed561f2a96f92673f60f86223b25d5f339e22a1e
serg@ubuntu:~/git/Ansible-01-hw/playbook$ docker run -dit --name ubuntu pycontribs/ubuntu:latest sleep 6000000
Unable to find image 'pycontribs/ubuntu:latest' locally
latest: Pulling from pycontribs/ubuntu
423ae2b273f4: Pull complete 
de83a2304fa1: Pull complete 
f9a83bce3af0: Pull complete 
b6b53be908de: Pull complete 
7378af08dad3: Pull complete 
Digest: sha256:dcb590e80d10d1b55bd3d00aadf32de8c413531d5cc4d72d0849d43f45cb7ec4
Status: Downloaded newer image for pycontribs/ubuntu:latest
dca21723325a4a3653ef0cde4806944a4e7292f6ed0ba9a61515888b6fc66a06
serg@ubuntu:~/git/Ansible-01-hw/playbook$ docker ps
CONTAINER ID   IMAGE                      COMMAND           CREATED         STATUS         PORTS     NAMES
dca21723325a   pycontribs/ubuntu:latest   "sleep 6000000"   8 seconds ago   Up 7 seconds             ubuntu
b9abb4fc9e4a   pycontribs/centos:7        "sleep 6000000"   2 minutes ago   Up 2 minutes             centos7

```


4. Проведите запуск playbook на окружении из `prod.yml`. Зафиксируйте полученные значения `some_fact` для каждого из `managed host`.

### Решение

```
serg@Ubuntu-ansible:~/git/Ansible-01-hw/playbook$ ansible-playbook -i ./inventory/prod.yml site.yml

PLAY [Print os facts] *************************************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] *******************************************************************************************************************************************************************
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}

TASK [Print fact] *****************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el "
}
ok: [ubuntu] => {
    "msg": "deb"
}

PLAY RECAP ************************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```
Значение some_fact для managed host "ubuntu" deb. Значение some_fact для managed host "centos7" el.

5. Добавьте факты в `group_vars` каждой из групп хостов так, чтобы для `some_fact` получились значения: для `deb` — `deb default fact`, для `el` — `el default fact`.

### Решение

```
serg@Ubuntu-ansible:~/git/Ansible-01-hw/playbook$ ansible-playbook -i ./inventory/prod.yml site.yml

PLAY [Print os facts] *************************************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] *******************************************************************************************************************************************************************
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}

TASK [Print fact] *****************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP ************************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  
```

6.  Повторите запуск playbook на окружении `prod.yml`. Убедитесь, что выдаются корректные значения для всех хостов.

### Решение

```
serg@Ubuntu-ansible:~/git/Ansible-01-hw/playbook$ ansible-playbook -i ./inventory/prod.yml site.yml

PLAY [Print os facts] *************************************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] *******************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *****************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP ************************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

7. При помощи `ansible-vault` зашифруйте факты в `group_vars/deb` и `group_vars/el` с паролем `netology`.

### Решение

```
serg@Ubuntu-ansible:~/git/Ansible-01-hw/playbook$ ansible-vault encrypt group_vars/deb/examp.yml
New Vault password: 
Confirm New Vault password: 
Encryption successful
serg@Ubuntu-ansible:~/git/Ansible-01-hw/playbook$ ansible-vault encrypt group_vars/el/examp.yml
New Vault password: 
Confirm New Vault password: 
Encryption successful
serg@Ubuntu-ansible:~/git/Ansible-01-hw/playbook$ cat group_vars/deb/examp.yml
$ANSIBLE_VAULT;1.1;AES256
61366462646534626338306332393736333138633736336236343064333265613830313633373365
6363316231643830333338343336633839643739313939660a383831656439333237346537653733
36363233646262616161663835383830363132373830613638323861303437366566383861323161
3734306232373130660a376265336231353331353734343161376430613332303235336533323436
39386532353866303435323037626535333038663237376235373363656163393464323538653362
3665373233613666636438656636346534336138326635323262
serg@Ubuntu-ansible:~/git/Ansible-01-hw/playbook$ cat group_vars/el/examp.yml
$ANSIBLE_VAULT;1.1;AES256
66633536326235323938336430626663393764386362643739393635356531643535303761373338
6338396266306633633235396265376637313034656662650a383336383632316565383166373434
64633637303461343233653163623739623561666665363737663637376433626531613734646365
3766636138626338640a396338353838393265346661623362616536616564326432643336326437
63333265613439636530393864303037376563366239323135323963663562663461633430336336
6536636534653330313764303732653661393935613435393439
```

8. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь в работоспособности.

### Решение

```
serg@Ubuntu-ansible:~/git/Ansible-01-hw/playbook$ ansible-playbook -i ./inventory/prod.yml site.yml --ask-vault-password
Vault password: 

PLAY [Print os facts] *************************************************************************************************************************************************************

TASK [Gathering Facts] ************************************************************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] *******************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] *****************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP ************************************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

9. Посмотрите при помощи `ansible-doc` список плагинов для подключения. Выберите подходящий для работы на `control node`.

### Решение

```
serg@Ubuntu-ansible:~/git/Ansible-01-hw/playbook$ ansible-doc -t connection --list 
ansible.builtin.local          execute on controller                                                                                                           
ansible.builtin.paramiko_ssh   Run tasks via Python SSH (paramiko)                                                                                             
ansible.builtin.psrp           Run tasks over Microsoft PowerShell Remoting Protocol                                                                           
ansible.builtin.ssh            connect via SSH client binary                                                                                                   
ansible.builtin.winrm          Run tasks over Microsoft's WinRM                                                                                                
ansible.netcommon.grpc         Provides a persistent connection using the gRPC protocol                                                                        
ansible.netcommon.httpapi      Use httpapi to run command on network appliances                                                                                
ansible.netcommon.libssh       Run tasks using libssh for ssh connection                                                                                       
ansible.netcommon.netconf      Provides a persistent connection using the netconf protocol                                                                     
ansible.netcommon.network_cli  Use network_cli to run command on network appliances                                                                            
ansible.netcommon.persistent   Use a persistent unix socket for connection                                                                                     
community.aws.aws_ssm          connect to EC2 instances via AWS Systems Manager                                                                                
community.docker.docker        Run tasks in docker containers                                                                                                  
community.docker.docker_api    Run tasks in docker containers                                                                                                  
community.docker.nsenter       execute on host running controller container                                                                                    
community.general.chroot       Interact with local chroot                                                                                                      
community.general.funcd        Use funcd to connect to target                                                                                                  
community.general.incus        Run tasks in Incus instances via the Incus CLI                                                                                  
community.general.iocage       Run tasks in iocage jails                                                                                                       
community.general.jail         Run tasks in jails         
```
Подходящий для работы на control node -    ansible.builtin.local

10. В `prod.yml` добавьте новую группу хостов с именем  `local`, в ней разместите localhost с необходимым типом подключения.

### Решение

```
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
  local:
    hosts:
      localhost:
        ansible_connection: local

```
11. Запустите playbook на окружении `prod.yml`. При запуске `ansible` должен запросить у вас пароль. Убедитесь, что факты `some_fact` для каждого из хостов определены из верных `group_vars`.

### Решение

```
serg@Ubuntu-ansible:~/git/Ansible-01-hw/playbook$ ansible-playbook -i ./inventory/prod.yml site.yml --ask-vault-password
Vault password: 

PLAY [Print os facts] **********************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************
ok: [localhost]
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ****************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [localhost] => {
    "msg": "all default fact"
}

PLAY RECAP *********************************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```


## Необязательная часть

1. При помощи `ansible-vault` расшифруйте все зашифрованные файлы с переменными.
2. Зашифруйте отдельное значение `PaSSw0rd` для переменной `some_fact` паролем `netology`. Добавьте полученное значение в `group_vars/all/exmp.yml`.
3. Запустите `playbook`, убедитесь, что для нужных хостов применился новый `fact`.
4. Добавьте новую группу хостов `fedora`, самостоятельно придумайте для неё переменную. В качестве образа можно использовать [этот вариант](https://hub.docker.com/r/pycontribs/fedora).
5. Напишите скрипт на bash: автоматизируйте поднятие необходимых контейнеров, запуск ansible-playbook и остановку контейнеров.
6. Все изменения должны быть зафиксированы и отправлены в ваш личный репозиторий.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---
