
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

Command: ["import", "/root/.vagrant.d/boxes/bento-VAGRANTSLASH-ubuntu-20.04/202112.19.0/virtualbox/box.ovf", "--vsys", "0", "--vmname", "ubuntu-20.04-amd64_1652563288918_97458", "--vsys", "0", "--unit", "11", "--disk", "/root/VirtualBox VMs/ubuntu-20.04-amd64_1652563288918_97458/ubuntu-20.04-amd64-disk001.vmdk"]

Stderr: 0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
Interpreting /root/.vagrant.d/boxes/bento-VAGRANTSLASH-ubuntu-20.04/202112.19.0/virtualbox/box.ovf...
OK.
0%...10%...20%...30%...40%...
Progress state: NS_ERROR_INVALID_ARG
VBoxManage: error: Appliance import failed
VBoxManage: error: Code NS_ERROR_INVALID_ARG (0x80070057) - Invalid argument value (extended info not available)
VBoxManage: error: Context: "RTEXITCODE handleImportAppliance(HandlerArg*)" at line 1363 of file VBoxManageAppliance.cpp


```
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

```
