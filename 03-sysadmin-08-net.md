1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP

telnet route-views.routeviews.org
Username: rviews
show ip route x.x.x.x/32
show bgp x.x.x.x/32

Ответ:

route-views>show ip route 85.174.192.153
Routing entry for 85.174.192.0/19
  Known via "bgp 6447", distance 20, metric 0
  Tag 3303, type external
  Last update from 217.192.89.50 3w1d ago
  Routing Descriptor Blocks:
  * 217.192.89.50, from 217.192.89.50, 3w1d ago
      Route metric is 0, traffic share count is 1
      AS Hops 2
      Route tag 3303
      MPLS label: none

route-views>show bgp 85.174.192.153
BGP routing table entry for 85.174.192.0/19, version 2653822923
Paths: (20 available, best #20, table default)
  Not advertised to any peer
  Refresh Epoch 1
  3267 12389
    194.85.40.15 from 194.85.40.15 (185.141.126.1)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE03D23B088 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3333 1103 12389
    193.0.0.56 from 193.0.0.56 (193.0.0.56)
      Origin IGP, localpref 100, valid, external
      path 7FE113AC44E8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20912 3257 174 12389
    212.66.96.126 from 212.66.96.126 (212.66.96.126)
      Origin IGP, localpref 100, valid, external
      Community: 3257:8070 3257:30155 3257:50001 3257:53900 3257:53902 20912:65004
      path 7FE1227A30B0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  6939 12389
    64.71.137.241 from 64.71.137.241 (216.218.253.53)
      Origin IGP, localpref 100, valid, external
      path 7FE09B3A0E78 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  8283 1299 12389
    94.142.247.3 from 94.142.247.3 (94.142.247.3)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 1299:30000 8283:1 8283:101 8283:102
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x24
        value 0000 205B 0000 0000 0000 0001 0000 205B
              0000 0005 0000 0001 0000 205B 0000 0005
              0000 0002
      path 7FE0D8FE7DC0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  7018 3356 12389
    12.0.1.63 from 12.0.1.63 (12.0.1.63)
      Origin IGP, localpref 100, valid, external
      Community: 7018:5000 7018:37232
      path 7FE048985F68 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3257 3356 12389
    89.149.178.10 from 89.149.178.10 (213.200.83.26)
      Origin IGP, metric 10, localpref 100, valid, external
      Community: 3257:8794 3257:30043 3257:50001 3257:54900 3257:54901
      path 7FE145F6A8B8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  19214 174 12389
    208.74.64.40 from 208.74.64.40 (208.74.64.40)
      Origin IGP, localpref 100, valid, external
      Community: 174:21101 174:22005
      path 7FE031371F00 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  49788 12552 12389
    91.218.184.60 from 91.218.184.60 (91.218.184.60)
      Origin IGP, localpref 100, valid, external
      Community: 12552:12000 12552:12100 12552:12101 12552:22000
      Extended Community: 0x43:100:0
      path 7FE100ED6320 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3561 3910 3356 12389
    206.24.210.80 from 206.24.210.80 (206.24.210.80)
      Origin IGP, localpref 100, valid, external
      path 7FE11F2379F0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3549 3356 12389
    208.51.134.254 from 208.51.134.254 (67.16.168.191)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065 3549:2581 3549:30840
      path 7FE02B708A70 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  4901 6079 1299 12389
    162.250.137.254 from 162.250.137.254 (162.250.137.254)
      Origin IGP, localpref 100, valid, external
      Community: 65000:10100 65000:10300 65000:10400
      path 7FE167CD7F48 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  101 3356 12389
    209.124.176.223 from 209.124.176.223 (209.124.176.223)
      Origin IGP, localpref 100, valid, external
      Community: 101:20100 101:20110 101:22100 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065
      Extended Community: RT:101:22100
      path 7FE10A638D60 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3356 12389
    4.68.4.46 from 4.68.4.46 (4.69.184.201)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065
      path 7FE09AFA9E30 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  1351 6939 12389
    132.198.255.253 from 132.198.255.253 (132.198.255.253)
      Origin IGP, localpref 100, valid, external
      path 7FE04A0DAF50 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  57866 3356 12389
    37.139.139.17 from 37.139.139.17 (37.139.139.17)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:901 3356:2065 57866:100 65100:3356 65103:1 65104:31
      unknown transitive attribute: flag 0xE0 type 0x20 length 0x30
        value 0000 E20A 0000 0064 0000 0D1C 0000 E20A
              0000 0065 0000 0064 0000 E20A 0000 0067
              0000 0001 0000 E20A 0000 0068 0000 001F

      path 7FE1460913D0 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  2497 12389
    202.232.0.2 from 202.232.0.2 (58.138.96.254)
      Origin IGP, localpref 100, valid, external
      path 7FE121815350 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20130 6939 12389
    140.192.8.16 from 140.192.8.16 (140.192.8.16)
      Origin IGP, localpref 100, valid, external
      path 7FE12EF0CAA8 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  852 3356 12389
    154.11.12.212 from 154.11.12.212 (96.1.209.43)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE13BD7D330 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3303 12389
    217.192.89.50 from 217.192.89.50 (138.187.128.158)
      Origin IGP, localpref 100, valid, external, best
      Community: 3303:1004 3303:1006 3303:1030 3303:3056
      path 7FE0D7FDCD08 RPKI State valid
      rx pathid: 0, tx pathid: 0x0

Ссылка на скриншот:

https://imgur.com/a/iYS91sA

2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

Ответ:

sudo ip link add dummy0 type dummy

sudo ip addr add 10.0.10.1/24 dev dummy0

sudo ip link set dummy0 up

sudo ip route add to 10.10.0.0/16 via 10.0.10.1

sudo ip route add to 10.12.0.0/16 via 10.0.10.1

ip route

default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15
10.0.2.2 dev eth0 proto dhcp scope link src 10.0.2.15 metric 100
10.0.10.0/24 dev dummy0 proto kernel scope link src 10.0.10.1
10.10.0.0/16 via 10.0.10.1 dev dummy0
10.12.0.0/16 via 10.0.10.1 dev dummy0

Ссылка на скриншот:

https://imgur.com/a/7pDaj7t

3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

Ответ:

sudo netstat -ntlp | grep LISTEN
tcp        0      0 127.0.0.1:8125          0.0.0.0:*               LISTEN      650/netdata
tcp        0      0 0.0.0.0:19999           0.0.0.0:*               LISTEN      650/netdata
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      618/systemd-resolve
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      751/sshd: /usr/sbin
tcp6       0      0 :::9100                 :::*                    LISTEN      654/prometheus-node
tcp6       0      0 :::22                   :::*                    LISTEN      751/sshd: /usr/sbin

53 порт - это DNS.

22 порт - это SSH.

19999 порт - это Netdata.

Ссылка на скриншот:

https://imgur.com/a/7LkdwjR

4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

Ответ:

sudo ss -unap
State         Recv-Q       Send-Q        Local Address:Port             Peer Address:Port              Process
UNCONN        0            0             127.0.0.1:8125                 0.0.0.0:*                      users:(("netdata",pid=650,fd=19))
UNCONN        0            0             127.0.0.53%lo:53               0.0.0.0:*                      users:(("systemd-resolve",pid=618,fd=12))
UNCONN        0            0             10.0.2.15%eth0:68              0.0.0.0:*                      users:(("systemd-network",pid=616,fd=19))

8125 порт - это Netdata.

53 порт - это DNS.

68 порт - использует DHCP для отправки сообщений клиентам.

Ссылка на скриншот:

https://imgur.com/a/ysPyFAh

5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали.

Ответ:

<!--[if IE]><meta http-equiv="X-UA-Compatible" content="IE=5,IE=9" ><![endif]-->
<!DOCTYPE html>
<html>
<head>
<title>diagrams.html</title>
<meta charset="utf-8"/>
</head>
<body>
<div class="mxgraph" style="max-width:100%;border:1px solid transparent;" data-mxgraph="{&quot;highlight&quot;:&quot;#0000ff&quot;,&quot;nav&quot;:true,&quot;resize&quot;:true,&quot;xml&quot;:&quot;&lt;mxfile host=\&quot;app.diagrams.net\&quot; modified=\&quot;2023-02-09T19:13:48.329Z\&quot; agent=\&quot;5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 YaBrowser/23.1.1.1138 Yowser/2.5 Safari/537.36\&quot; etag=\&quot;dLEBzyxf9rvQELJrMUJz\&quot; version=\&quot;20.8.18\&quot; type=\&quot;device\&quot;&gt;&lt;diagram name=\&quot;Страница 1\&quot; id=\&quot;wyY8NhE26C2i0Bwluqy5\&quot;&gt;7Vpdc6M2FP01flwPkgyGR38kabbpTmbS7nafOsQotroCubIc2/31lZAwCGzixBA2Hecl1tEnuudeHV3ooUm8veHhcvEbizDtQSfa9tC0ByFwHCj/KWSnEdcLNDDnJDKNcuCB/IuzngZdkwivrIaCMSrI0gZnLEnwTFhYyDnb2M2eGLVnXYZzXAEeZiGtot9IJBYGBdljqIpfMJkvzNQ+HOqKOMwamydZLcKIbQoQuuqhCWdM6F/xdoKp2rxsX3S/6yO1+4VxnIhTOkymnz+vh/HNr9/RHwIMr8UtDz7BgVmc2GVPjCO5AabIuFiwOUtCepWjY87WSYTVsI4s5W3uGFtKEEjwbyzEzlgzXAsmoYWIqal9YomYMMp4OiNy0j+J4yQaKZtJOGEJ1sg1odRMpdeqFnh0Dwy0Yms+wzUP7u8tIKmLWYwF38l+HNNQkGd7/NBwaL5vt+96z4icGTqG72hgjG3YjjLjZ0OIkM+xML1yY8kfhWXkUGrCV5jT17M9h3RtHqEHPSq3Zvwof8xFuocaUDZI/SYzg/fPmukGmUEKkO77ZfSwH49nIAhgH3h+H/Sh6nOtKKXbyIKexZ4571poWFhfiY8VsnjeZCIbPmMuiHTUESXzRNYJRb49ehc+YnrPVkQQpmofmRAslg1oqWImqYPl2OPQjLMHCoRla0FJgif7IKPY+CSJWVjYJP2T+Epw9gMXajzfB2Mka+Y8jAjOH8eQPIOnhMvR9bIS5Vb7wbLQAxWyCJdqa+LtXIXcfoLFhvEfq/5KMJ5GM7OHaivwtt5Tqh6QdYA2k+HQlDeFKOgYbFEIgHvwkNdYfH8tuZFT4cZPHKvwlog/1W/pHKb4PS26rilOt4Wm012hcI85kXumWKixRO6fHiyAfgbo4XxnmAH5gGlpVyyVh2wwlAJz4unYVtNw2ErMHXjdxtzs8euC7jfy6ZrINu7NSSFPzir1DS44+4yytVzEeLMgAj8sw9QgG+n/NlkrMayBSOBCz44EwYmRwGsrEGQS630DQcEJB4FnOyEAb3LC9nXQiz5ZkKKNOqVTckq/RAYdVVpzyuM++RbdU/FnSSZlxRotBCwl9JLkOaKXLkrobUpoI7tTvFr9Fcv7aNxMGERuSRC5J4bBoK0wCFC7JP/9ax3BkXtheHcMj1lCBGvohEclBeWiKrX9Qwe829YB73d8wLvW8f5zHu0v6vJMB7+oAXQgOUMDnGds53T9DPuDD6igy0fHAHauoL0uHOytPrGSDBalFk36yalauaX7a0kqg7LVW5bKCJzFBfDOaZW6WxjqOBMSnBpwz2XSeXk02PHp6g/t8zU/bt/jhC0k5rJMXGEVr8rLNUgdcPJh3S13UBfBoqjMStQJ3pU7TRr81FOnY4Of94LwYvDcjvBjGNyryPG70ZcKCaQaFrbN7Kuw2c3ivdlAFYVdvtzHJIpSGh2S6vY5dMysCs8oChpQ8LCc/AmCioL3Dgh41JqABxUrVS5Nt2p/Eyw+/oUJDLq+MAVHt7uRVNv9pC7VZmfaDuXVDmbfLpm2JjJty1kznPZ8t3TNq4aQg5wetpY/Pp54aYTUNFwq9tQQ27sQuztiG/M0Q+4Alc7HrgM2OH5ANkLuURJxJldQ9wrQ/hrq8obkXekds0dCcTP0HkJo0RsFboXe7gF2t/ctVHaZaIvet8uFskYduy9vuP837LaViTsArbFbFvMvnnUGO/9uHF39Bw==&lt;/diagram&gt;&lt;/mxfile&gt;&quot;,&quot;toolbar&quot;:&quot;pages zoom layers lightbox&quot;,&quot;page&quot;:0}"></div>
<script type="text/javascript" src="https://app.diagrams.net/js/viewer-static.min.js"></script>
</body>
</html>

Ссылка на скриншот:

https://imgur.com/a/1fyCqbg
