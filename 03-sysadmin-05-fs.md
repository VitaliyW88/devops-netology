Задание

1. Узнайте о sparse (разряженных) файлах.

Ответ:

Файлы с пустотами на диске. Записи пустот на диск не происходит, информация о них хранится только в метаданных. Разреженный файл эффективен, потому что он не хранит нули на диске, вместо этого он содержит достаточно метаданных, описывающих нули, которые будут сгенерированы. Разрежённые файлы используются для хранения, например, контейнеров.

2. Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?

Ответ:

Нет, не могут, т.к. это просто ссылки на один и тот же inode - в нём и хранятся права доступа и имя владельца.

3. Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:

path_to_disk_folder = './disks'

host_params = {
    'disk_size' => 2560,
    'disks'=>[1, 2],
    'cpus'=>2,
    'memory'=>2048,
    'hostname'=>'sysadm-fs',
    'vm_name'=>'sysadm-fs'
}
Vagrant.configure("2") do |config|
    config.vm.box = "bento/ubuntu-20.04"
    config.vm.hostname=host_params['hostname']
    config.vm.provider :virtualbox do |v|

        v.name=host_params['vm_name']
        v.cpus=host_params['cpus']
        v.memory=host_params['memory']

        host_params['disks'].each do |disk|
            file_to_disk=path_to_disk_folder+'/disk'+disk.to_s+'.vdi'
            unless File.exist?(file_to_disk)
                v.customize ['createmedium', '--filename', file_to_disk, '--size', host_params['disk_size']]
            end
            v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', disk.to_s, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
        end
    end
    config.vm.network "private_network", type: "dhcp"
end
Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.

Ответ:

Настроил, в Vagrantfile так же добавил(без получал ошибку при запуске):

class VagrantPlugins::ProviderVirtualBox::Action::Network
  def dhcp_server_matches_config?(dhcp_server, config)
    true
  end
end

Ссылка на скриншот:

https://imgur.com/a/MdGX8HT

4. Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.

Ответ:

sudo fdisk /dev/sdb

Сделано, ссылка на скриншот

https://imgur.com/a/zSvFNPC 

5. Используя sfdisk, перенесите данную таблицу разделов на второй диск.

Ответ:

sudo sfdisk -d /dev/sdb > sdb.dump

sudo sfdisk /dev/sdc < sdb.dump

Сделано, ссылка на скриншот

https://imgur.com/a/FbvtNIr 

6. Соберите mdadm RAID1 на паре разделов 2 Гб.

Ответ:

sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sd[bc]1

Сделано, ссылка на скриншот

https://imgur.com/a/wuzrVY1 

7. Соберите mdadm RAID0 на второй паре маленьких разделов.

Ответ:

sudo mdadm --create /dev/md1 --level=0 --raid-devices=2 /dev/sd[bc]2

Сделано, ссылка на скриншот

https://imgur.com/a/ClA39mj 

8. Создайте 2 независимых PV на получившихся md-устройствах.

Ответ:

sudo pvcreate /dev/md0

sudo pvcreate /dev/md1

Сделано, ссылка на скриншот

https://imgur.com/a/thkl9iF 

9. Создайте общую volume-group на этих двух PV.

Ответ:

sudo vgcreate netology /dev/md0 /dev/md1

Сделано, ссылка на скриншот

https://imgur.com/a/xoOk2KA

10. Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.

Ответ:

sudo lvcreate -L 100m -n netology-lv netology /dev/md1

Сделано, ссылка на скриншот

https://imgur.com/a/OSbXZme

11. Создайте mkfs.ext4 ФС на получившемся LV.

Ответ:

sudo mkfs.ext4 -L netology-ext4 -m 1 /dev/mapper/netology-netology--lv

Сделано, ссылка на скриншот

https://imgur.com/a/PHd2vO2

12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.

Ответ:

sudo mkdir /tmp/new

sudo mount /dev/mapper/netology-netology--lv /tmp/new/

Сделано, ссылка на скриншот

https://imgur.com/a/ManPqpw

13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.

Ответ:

cd /tmp/new/

sudo /tmp/new# wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz

Сделано, ссылка на скриншот

https://imgur.com/a/GwzF0gm

14. Прикрепите вывод lsblk.

Ответ:

vagrant@vagrant:/tmp/new$ lsblk
NAME                        MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                         7:0    0 61.9M  1 loop  /snap/core20/1328
loop1                         7:1    0 67.2M  1 loop  /snap/lxd/21835
loop3                         7:3    0 49.8M  1 loop  /snap/snapd/17950
loop4                         7:4    0 63.3M  1 loop  /snap/core20/1778
loop5                         7:5    0 91.9M  1 loop  /snap/lxd/24061
sda                           8:0    0   64G  0 disk
├─sda1                        8:1    0    1M  0 part
├─sda2                        8:2    0  1.5G  0 part  /boot
└─sda3                        8:3    0 62.5G  0 part
  └─ubuntu--vg-ubuntu--lv   253:0    0 31.3G  0 lvm   /
sdb                           8:16   0  2.5G  0 disk
├─sdb1                        8:17   0    2G  0 part
│ └─md0                       9:0    0    2G  0 raid1
└─sdb2                        8:18   0  511M  0 part
  └─md1                       9:1    0 1018M  0 raid0
    └─netology-netology--lv 253:1    0  100M  0 lvm   /tmp/new
sdc                           8:32   0  2.5G  0 disk
├─sdc1                        8:33   0    2G  0 part
│ └─md0                       9:0    0    2G  0 raid1
└─sdc2                        8:34   0  511M  0 part
  └─md1                       9:1    0 1018M  0 raid0
    └─netology-netology--lv 253:1    0  100M  0 lvm   /tmp/new

Сделано, ссылка на скриншот

https://imgur.com/a/VX21hQs

15. Протестируйте целостность файла:

root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0

Ответ:

gzip -t /tmp/new/test.gz && echo $?

Сделано, ссылка на скриншот

https://imgur.com/a/UuCHNR2

16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.

Ответ:

sudo pvmove -n netology-lv /dev/md1 /dev/md0

Сделано, ссылка на скриншот

https://imgur.com/a/xYpZCvB

17. Сделайте --fail на устройство в вашем RAID1 md.

Ответ:

sudo mdadm --fail /dev/md0 /dev/sdb1

Сделано, ссылка на скриншот

https://imgur.com/a/GpyEz6v

18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.

Ответ:

sudo dmesg | grep md0 | tail -n 2

Сделано, ссылка на скриншот

https://imgur.com/a/XTxabXR

19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:

root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0

Ответ:

sudo gzip -t /tmp/new/test.gz && echo $?

Сделано, ссылка на скриншот

https://imgur.com/a/bTLYH6q

20. Погасите тестовый хост, vagrant destroy.

Ответ:

Выполнено:

C:\Users\ASUS_TUF_15\vagrant_fs>vagrant destroy

    default: Are you sure you want to destroy the 'default' VM? [y/N] y

==> default: Forcing shutdown of VM...

==> default: Destroying VM and associated drives...
