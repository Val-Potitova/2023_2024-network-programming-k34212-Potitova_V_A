University: [ITMO University](https://itmo.ru/ru/) \
Faculty: [FICT](https://fict.itmo.ru) \
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming) \
Year: 2023/2024 \
Group: K34212 \
Author: Potitova Valentina Alexandrovna \
Lab: Lab1 \
Date of create: 03.11.2023 \
Date of finished: 09.11.2023

# Лабораторная работа №1 "Установка CHR и Ansible, настройка VPN"

## Цель работы
Целью данной работы является развертывание виртуальной машины с установленной системой контроля конфигураций Ansible и установка CHR в VirtualBox.

## Ход работы
Развернули виртуальную машину с Ubuntu 20.04 на Яндекс.Облаке. \
Установили python3 и Ansible:
```
sudo apt install python3-pip
sudo pip3 install ansible
```

Cоздаём свой OpenVPN сервер.
Убедились в актуальности пакетов, обновили необходимые зависимости. Добавили сервер OpenVPN в список репозиториев. \
Установили сервер доступа OpenVPN:
```
apt install openvpn-as
```
![OpenVPN](https://github.com/Val-Potitova/2023_2024-network-programming-k34212-Potitova_V_A/blob/main/lab1/img/0.png)

На VirtualBox установили CHR (RouterOS). При входе пользователь admin, пароля нет, его задали при требовании.
![OpenVPN](https://github.com/Val-Potitova/2023_2024-network-programming-k34212-Potitova_V_A/blob/main/lab1/img/1.png)





Пдняли VPN туннель между VPN сервером на Ubuntu 20.04 и VPN клиентом на RouterOS (CHR).

## Вывод
В ходе работы научились развертывать виртуальные машины и системы контроля конфигураций Ansible, а также организовали собственный VPN сервер.
