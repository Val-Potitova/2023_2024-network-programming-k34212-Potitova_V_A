University: [ITMO University](https://itmo.ru/ru/) \
Faculty: [FICT](https://fict.itmo.ru) \
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming) \
Year: 2023/2024 \
Group: K34212 \
Author: Potitova Valentina Alexandrovna \
Lab: Lab2 \
Date of create: 20.11.2023 \
Date of finished: 23.11.2023

# Лабораторная работа №2 "Развертывание дополнительного CHR, первый сценарий Ansible"

## Цель работы
С помощью Ansible настроить несколько сетевых устройств и собрать информацию о них. Правильно собрать файл Inventory.

## Ход работы
Установили второй CHR на ПК. Организовали второй OVPN Client на этом CHR аналогично ЛР-1.

Теперь необходимо установить библиотеку Ansible для работы по SSH.

![Ansible](https://github.com/Val-Potitova/2023_2024-network-programming-k34212-Potitova_V_A/blob/main/lab2/img/1.png)

Создали файл hosts.ini, в котором прописали настройки для первого и второго CHR.
``` ini
[chr]
chr1 ansible_host=172.27.224.3
chr2 ansible_host=172.27.224.46

[chr:vars]
ansible_connection=ansible.netcommon.network_cli
ansible_network_os=community.routeros.routeros
ansible_user=admin
ansible_ssh_pass=admin
```
Проверка подключения через inventory файл.

![hosts.ini](https://github.com/Val-Potitova/2023_2024-network-programming-k34212-Potitova_V_A/blob/main/lab2/img/2.png)

Создали файл playbook.yml для настроек конфигурации.
``` yml
---
- name:  CHR setting
  hosts: chr
  tasks:
    - name: Create users
      routeros_command:
        commands: 
          - /user add name=name group=read password=name

    - name: Create NTP client
      routeros_command:
        commands:
          - /system ntp client set enabled=yes server=0.ru.pool.ntp.org
        
    - name: OSPF with router ID
      routeros_command:
        commands: 
          - /interface bridge add name=loopback
          - /ip address add address=172.16.0.1 interface=loopback network=172.16.0.1
          - /routing id add disabled=no id=172.16.0.1 name=OSPF_ID select-dynamic-id=""
          - /routing ospf instance add name=ospf-1 originate-default=always router-id=OSPF_ID
          - /routing ospf area add instance=ospf-1 name=backbone
          - /routing ospf interface-template add area=backbone auth=md5 auth-key=admin interface=ether1

    - name: Get facts
      routeros_facts:
        gather_subset:
          - interfaces
      register: output_ospf

    - name: Print output
      debug:
        var: "output_ospf"
```

Выполняем playbook и получаем следующий результат

![playbook.ini](https://github.com/Val-Potitova/2023_2024-network-programming-k34212-Potitova_V_A/blob/main/lab2/img/3.png)

## Результаты
NTP Client, настроенные с помощью Ansible

![NTP Client](https://github.com/Val-Potitova/2023_2024-network-programming-k34212-Potitova_V_A/blob/main/lab2/img/4.png)

Проверки связности

![links](https://github.com/Val-Potitova/2023_2024-network-programming-k34212-Potitova_V_A/blob/main/lab2/img/5.png)

Конфигурации устройств находятся в следующих файлах: \
[Конфигурация CHR 1](https://github.com/Val-Potitova/2023_2024-network-programming-k34212-Potitova_V_A/blob/main/lab2/chr_1.txt) \
[Конфигурация CHR 2](https://github.com/Val-Potitova/2023_2024-network-programming-k34212-Potitova_V_A/blob/main/lab2/chr_2.txt)

Схема

![Scheme](https://github.com/Val-Potitova/2023_2024-network-programming-k34212-Potitova_V_A/blob/main/lab2/img/scheme.png)

## Вывод
С помощью Ansible настроили несколько сетевых устройств и собрали информацию о них. Создали файл Inventory и собрали конфигурации устройств.