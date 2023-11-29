University: [ITMO University](https://itmo.ru/ru/) \
Faculty: [FICT](https://fict.itmo.ru) \
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming) \
Year: 2023/2024 \
Group: K34212 \
Author: Potitova Valentina Alexandrovna \
Lab: Lab3 \
Date of create: 26.11.2023 \
Date of finished: 07.12.2023

# Лабораторная работа №3 "Развертывание Netbox, сеть связи как источник правды в системе технического учета Netbox"

## Цель работы
С помощью Ansible и Netbox собрать всю возможную информацию об устройствах и сохранить их в отдельном файле.

## Ход работы
Для дальнейшей работы с Netbox установили postgresql. Также была содана база данных netbox, и в ней пользователь netbox

![postres](img/1.png)

Установили redis. Потом установили netbox из репозитория и создали пользователя

![group](img/2.png) \
![user](img/3.png)

Установили все зависимости, командой ```python3 ../generate_secret_key.py``` сгенерировали ключ.
После этого скопировали файл конфигурации:
```
cd netbox/netbox/
cp configuration.example.py configuration.py
```
В скопированном файле добавили нужные параметры и секретный ключ, сгенерированный ранее

![config](img/4.png)

Запустили миграции, создали суперпользователя и загрузили статику

![superuser](img/5.png)

Далее установили nginx и gunicorn, произвели нужные настройки. Отредактировали файл nginx.conf, добавили хост

![nginx](img/6.png)

Переходим по адресу на netbox под суперпользователем, созданным ранее. Открывается интерфейс

![netbox](img/7.png)

Добавили информацию о CHR. Скачали файл [netbox_devices.csv](netbox_devices.csv)

![CHR](img/8.png)

Cоздали файл netbox_change_name_chr.yml, в котором записали название роутера в файл new_playbook1.yml (берется из netbox_devices.csv).

![netbox_change_name_chr](img/9.png) \
![netbox_change_name_chr](img/10.png)

Имена CHR1 и CHR2 изменились в соответствии с информацией из файла new_playbook1.

![chr](img/11.png)

В netbox сгенерировали токен, после чего создали netbox_settings.yml, где прописали настройки для изменения данных в netbox и указали сгенерированный ранее токен.

![netbox_settings](img/12.png) \
![netbox_settings](img/13.png)

В netbox появилось новое устройство

![device](img/14.png)

Проверили связь между виртуальной машиной и CHR

![device](img/15.png)

Схема связи

![sheme](img/sheme.png)

## Вывод
С помощью Ansible и Netbox собрали информацию об устройствах и сохранили в отдельном файле. Написали плейбуки для связи Netbox и CHR.