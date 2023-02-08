1. Проверьте список доступных сетевых интерфейсов на вашем компьютере. Какие команды есть для этого в Linux и в Windows?

Ответ:

В Linux:

ip link show

ifconfig -a

В Windows:

ipconfig /all

Ссылка на скриншот:

https://imgur.com/a/X7dd4QQ

2. Какой протокол используется для распознавания соседа по сетевому интерфейсу? Какой пакет и команды есть в Linux для этого?

Ответ:

Протокол LLDP.

Пакет lldpd.

Команда lldpctl.


3. Какая технология используется для разделения L2 коммутатора на несколько виртуальных сетей? Какой пакет и команды есть в Linux для этого? Приведите пример конфига.

Ответ:

Технология называется VLAN (Virtual LAN).

Пакет в Ubuntu Linux - vlan

Пример конфига:

network:

  version: 2

  renderer: networkd

  ethernets:

    ens4:

      optional: yes

      addresses: 

        - 192.168.0.2/24

  vlans:

    vlan11:

      id: 11

      link: ens4 

      addresses:

        - 192.168.1.2/24


4. Какие типы агрегации интерфейсов есть в Linux? Какие опции есть для балансировки нагрузки? Приведите пример конфига.

Ответ:

В Linux есть две технологии агрегации (LAG): bonding и teaming.

Типы агрегации bonding:

modinfo bonding | grep mode:
parm:           mode:Mode of operation; 0 for balance-rr, 1 for active-backup, 2 for balance-xor, 3 for broadcast, 4 for 802.3ad, 5 for balance-tlb, 6 for balance-alb (charp)

active-backup и broadcast обеспечивают только отказоустойчивость.

balance-tlb, balance-alb, balance-rr, balance-xor и 802.3ad обеспечат отказоустойчивость и балансировку.

active-backup на отказоустойчивость:

 network:
   version: 2
   renderer: networkd
   ethernets:
     ens3:
       dhcp4: no 
       optional: true
     ens5: 
       dhcp4: no 
       optional: true
   bonds:
     bond0: 
       dhcp4: yes 
       interfaces:
         - ens3
         - ens5
       parameters:
         mode: active-backup
         primary: ens3
         mii-monitor-interval: 2

balance-alb - балансировка:

   bonds:
     bond0: 
       dhcp4: yes 
       interfaces:
         - ens3
         - ens5
       parameters:
         mode: balance-alb
         mii-monitor-interval: 2

Ссылка на скриншот:

https://imgur.com/a/W7gLJOm

5. Сколько IP адресов в сети с маской /29 ? Сколько /29 подсетей можно получить из сети с маской /24. Приведите несколько примеров /29 подсетей внутри сети 10.10.10.0/24.

Ответ:

ipcalc -b 10.10.10.0/29

Address:   10.10.10.0
Netmask:   255.255.255.248 = 29
Wildcard:  0.0.0.7
=>
Network:   10.10.10.0/29
HostMin:   10.10.10.1
HostMax:   10.10.10.6
Broadcast: 10.10.10.7
Hosts/Net: 6                     Class A, Private Internet

8 адресов = 6 для хостов, 1 адрес сети и 1 широковещательный адрес.

Сеть с маской /24 можно разбить на 32 подсети с маской /29

10.10.10.0/29 

10.10.10.8/29 

10.10.10.16/29

Ссылка на скриншот:

https://imgur.com/a/6QOtnKY

6. Задача: вас попросили организовать стык между 2-мя организациями. Диапазоны 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 уже заняты. Из какой подсети допустимо взять частные IP адреса? Маску выберите из расчета максимум 40-50 хостов внутри подсети.

Ответ:

ipcalc -b 100.64.0.0/10 -s 50

Можно взять адреса из сети для CGNAT - 100.64.0.0/10.

Маска для диапазонов будет /26, она позволит подключить 62 хоста.

Ссылка на скриншот:

https://imgur.com/a/FGmZTLu

7. Как проверить ARP таблицу в Linux, Windows? Как очистить ARP кеш полностью? Как из ARP таблицы удалить только один нужный IP?

Ответ:

Проверить таблицу можно так:

Linux: 

ip neigh, arp -n

Windows:
 
arp -a

Очистить кеш так:

Linux: 

ip neigh flush

Windows: 

arp -d *

Удалить один IP так:

Linux: 

ip neigh delete <IP> dev <INTERFACE>, arp -d <IP>

Windows: 

arp -d <IP>

