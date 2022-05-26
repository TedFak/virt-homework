
# Домашнее задание к занятию "5.2. Применение принципов IaaC в работе с виртуальными машинами"

## Задача 1

- Позволяет быстро конфигурировать инфраструктуру, снижение затрат и устраняет риск связанный с человеческим фактором.
- Описывать инфраструктуру кодом, переиспользуя практики из разработки ПО.

## Задача 2

- Использует ssh, не требует установки PCI. Простой метод написаний конфигураций.
- Push меньше точек сбоя.
## Задача 3

- VirtualBox
```bash
root@main:/home/admin1/virt-homework/vagrant# vagrant up
Bringing machine 'server1.netology' up with 'virtualbox' provider...

Progress: 90%There was an error while executing `VBoxManage`, a CLI used by Vagrant
for controlling VirtualBox. The command and stderr is shown below.
```
![image](https://user-images.githubusercontent.com/95320903/168653383-36792a5f-9389-42c2-aeb1-6775691b5db4.png)

- Vagrant
```bash
root@main:/home/admin1/virt-homework# vagrant -v
Vagrant 2.2.19
```
- Ansible
```bash
root@main:/home/admin1/virt-homework# ansible --version
ansible 2.9.6
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.8.10 (default, Nov 26 2021, 20:14:08) [GCC 9.3.0]
```

## Задача 4 (*)

```bash
vagrant@server1:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```
