1. На лекции мы познакомились с node_exporter. В демонстрации его исполняемый файл запускался в background. Этого достаточно для демо, но не для настоящей production-системы, где процессы должны находиться под внешним управлением. Используя знания из лекции по systemd, создайте самостоятельно простой unit-файл для node_exporter:

	поместите его в автозагрузку,
	предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на systemctl cat cron),
	удостоверьтесь, что с помощью systemctl процесс корректно стартует, завершается, а после перезагрузки автоматически поднимается.

Ответ:

порт  9100 проброшен на хостовую машину

установим node_exporter:

sudo apt install prometheus-node-exporter

поместите его в автозагрузку:

создадим пользователя под которым будет запускать:

sudo useradd node_exporter -s /sbin/nologin

sudo mkdir -p /etc/sysconfig

sudo touch /etc/sysconfig/node_exporter

sudo nano /etc/systemd/system/node_exporter.service

unit-файл:

[Unit]

Description=Node Exporter

[Service]

User=node_exporter 

EnvironmentFile=/etc/sysconfig/node_exporter 

ExecStart=/usr/bin/prometheus-node-exporter $EXTRA_OPTS

StandardOutput=file:/var/log/node_explorer.log

StandardError=file:/var/log/node_explorer.log

[Install]

WantedBy=multi-user.target

Опции можно добавить через:

ExecStart=/usr/bin/prometheus-node-exporter $EXTRA_OPTS

удостоверьтесь, что с помощью systemctl процесс корректно стартует:

Скриншот вывода sudo systemctl status node_exporter

https://imgur.com/a/P0R0pr9

journalctl -u node_exporter.service

-- Logs begin at Tue 2022-06-07 11:55:04 UTC, end at Mon 2023-01-30 04:17:01 UTC. --

Jan 30 03:00:35 vagrant systemd[1]: Started Node Exporter.

Jan 30 03:00:35 vagrant prometheus-node-exporter[5658]: time="2023-01-30T03:00:35Z" level=info msg="Starting node_exporter (version=0.18.1+ds, branch=debian/sid, revision=0.18.1+ds-2)" source="node_exporter.go:>

Jan 30 03:00:35 vagrant prometheus-node-exporter[5658]: time="2023-01-30T03:00:35Z" level=info msg="Build context (go=go1.13.8, user=pkg-go-maintainers@lists.alioth.debian.org, date=20200221-05:56:03)" source=">

Jan 30 03:00:35 vagrant prometheus-node-exporter[5658]: time="2023-01-30T03:00:35Z" level=info msg="Enabled collectors:" source="node_exporter.go:97"

Jan 30 03:00:35 vagrant prometheus-node-exporter[5658]: time="2023-01-30T03:00:35Z" level=info msg=" - arp" source="node_exporter.go:104"

Jan 30 03:00:35 vagrant prometheus-node-exporter[5658]: time="2023-01-30T03:00:35Z" level=info msg=" - bcache" source="node_exporter.go:104"

2. Ознакомьтесь с опциями node_exporter и выводом /metrics по-умолчанию. Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.

Ответ:

CPU:

curl localhost:9100/metrics |grep node_cpu

Memory: 

curl localhost:9100/metrics |grep node_memory

Disk: 

curl localhost:9100/metrics |grep node_disk

Network: 

curl localhost:9100/metrics |grep node_network

3. Установите в свою виртуальную машину Netdata. Воспользуйтесь готовыми пакетами для установки (sudo apt install -y netdata).

После успешной установки:

	в конфигурационном файле /etc/netdata/netdata.conf в секции [web] замените значение с localhost на bind to = 0.0.0.0,
	добавьте в Vagrantfile проброс порта Netdata на свой локальный компьютер и сделайте vagrant reload:
config.vm.network "forwarded_port", guest: 19999, host: 19999
	После успешной перезагрузки в браузере на своем ПК (не в виртуальной машине) вы должны суметь зайти на localhost:19999. 
	Ознакомьтесь с метриками, которые по умолчанию собираются Netdata и с комментариями, которые даны к этим метрикам.

Ответ:

Netdata установлена, ссылка на скриншот

https://imgur.com/a/KACCQvR

информация с vm машины:

vagrant@vagrant:~$ sudo lsof -i :19999

COMMAND PID    USER   FD   TYPE DEVICE SIZE/OFF NODE NAME

netdata 650 netdata    4u  IPv4  23218      0t0  TCP *:19999 (LISTEN)

netdata 650 netdata   52u  IPv4  36736      0t0  TCP vagrant:19999->_gateway:53294 (ESTABLISHED)

4. Можно ли по выводу dmesg понять, осознает ли ОС, что загружена не на настоящем оборудовании, а на системе виртуализации?

Ответ:

Судя по выводу dmesg да

vagrant@vagrant:~$ dmesg |grep virtualiz

[    0.016232] CPU MTRRs all blank - virtualized system.

[    0.102563] Booting paravirtualized kernel on KVM

[   17.056630] systemd[1]: Detected virtualization oracle.

5. Как настроен sysctl fs.nr_open на системе по-умолчанию? Определите, что означает этот параметр. Какой другой существующий лимит не позволит достичь такого числа (ulimit --help)?

Ответ:

vagrant@vagrant:~$ /sbin/sysctl -n fs.nr_open

1048576

Это максимальное число открытых дескрипторов для ядра (системы), для пользователя задать больше этого числа нельзя (если не менять). 

Число задается кратное 1024, в данном случае =1024*1024. 

Но максимальный предел ОС можно посмотреть так :

vagrant@vagrant:~$ cat /proc/sys/fs/file-max

9223372036854775807

Другой существующий лимит, мягкий лимит (так же ulimit -n) на пользователя

vagrant@vagrant:~$ ulimit -Sn

1024

И жесткий лимит на пользователя

vagrant@vagrant:~$ ulimit -Hn

1048576

6. Запустите любой долгоживущий процесс (не ls, который отработает мгновенно, а, например, sleep 1h) в отдельном неймспейсе процессов; покажите, что ваш процесс работает под PID 1 через nsenter. Для простоты работайте в данном задании под root (sudo -i). Под обычным пользователем требуются дополнительные опции (--map-root-user) и т.д.

Ответ:

Terminal #1

root@vagrant:~# unshare -f --pid --mount-proc sleep 1h

Terminal #2

root@vagrant:~# ps -e | grep sleep

  24163 pts/0    00:00:00 sleep

root@vagrant:~# nsenter --target 24163 --mount --uts --ipc --net --pid ps aux

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND

root           1  0.0  0.0   5476   580 pts/0    S+   08:56   0:00 sleep 1h

root           2  0.0  0.3   8888  3372 pts/1    R+   08:59   0:00 ps aux

7. Найдите информацию о том, что такое :(){ :|:& };:. Запустите эту команду в своей виртуальной машине Vagrant с Ubuntu 20.04 (это важно, поведение в других ОС не проверялось). Некоторое время все будет "плохо", после чего (минуты) – ОС должна стабилизироваться. Вызов dmesg расскажет, какой механизм помог автоматической стабилизации.
Как настроен этот механизм по-умолчанию, и как изменить число процессов, которое можно создать в сессии?


Ответ:

Это fork-бомба, shell бесконечно создаёт новые экземпляры себя.

Стабилизировал виртуалку cgroups:

[106725.788911] cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-4.scope

Чтобы изменить поведение, в файле /usr/lib/systemd/system/user-.slice.d/10-defaults.conf нужно поментья параметр TasksMax на больший процент, конкретное число или infinity, чтобы убрать лимит совсем.

Если ограничить количество процессов, то эта "волна" проходит гораздо быстрее: $ ulimit -u 50. 