# Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"

## Обязательная задача 1

Есть скрипт:
```bash
a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))
```

Какие значения переменным c,d,e будут присвоены? Почему?

| Переменная  | Значение | Обоснование |
| ------------- | ------------- | ------------- |
| `a`  | ???  | ??? |
| `b`  | ???  | ??? |
| `c`  | ???  | ??? |


## Обязательная задача 2
На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным (после чего скрипт должен завершиться). В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:
```bash
while ((1==1)
do
	curl https://localhost:4757
	if (($? != 0))
	then
		date >> curl.log
	fi
done
```

Необходимо написать скрипт, который проверяет доступность трёх IP: `192.168.0.1`, `173.194.222.113`, `87.250.250.242` по `80` порту и записывает результат в файл `log`. Проверять доступность необходимо пять раз для каждого узла.

### Ваш скрипт:
```bash
???
```

## Обязательная задача 3
Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается.

### Ваш скрипт:
```bash
???
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Мы хотим, чтобы у нас были красивые сообщения для коммитов в репозиторий. Для этого нужно написать локальный хук для git, который будет проверять, что сообщение в коммите содержит код текущего задания в квадратных скобках и количество символов в сообщении не превышает 30. Пример сообщения: \[04-script-01-bash\] сломал хук.

### Ваш скрипт:
```bash
???
```

Домашнее задание к занятию "4.1. Командная оболочка Bash: Практические навыки"
Обязательная задача 1

Есть скрипт:

a=1
b=2
c=a+b
d=$a+$b
e=$(($a+$b))

Какие значения переменным c,d,e будут присвоены? Почему?
Переменная 	Значение 	Обоснование
a 	          1	       по условию
b 	          2	       по условию
c 	          a+b      a+b  это строка, она так и записалась
d            1+2      т.к. d это строковый тип данных, то и символы 1 + и 2 Записались в d как строка
e            3        Скобки обеспечивают выполнение арифметической операции, а внешний $ - выбирает значение результата.
Обязательная задача 2

На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным (после чего скрипт должен завершиться). В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:

while ((1==1))
do
	curl https://localhost:8080
	if (($? == 0))
	then
		date >> curl.log
  break
	fi
done

Необходимо написать скрипт, который проверяет доступность трёх IP: 192.168.0.1, 173.194.222.113, 87.250.250.242 по 80 порту и записывает результат в файл log. Проверять доступность необходимо пять раз для каждого узла.
Ваш скрипт:

#!/usr/bin/env bash

array_ip=(192.168.0.1 173.194.222.113 87.250.250.242)
for ip in ${array_ip[@]}
do
for number in 1 2 3 4 5
    do
    telnet $ip 80
    if (($?==0))
    then
         echo $ip "Доступно" >> log.log
    else
        echo $ip "не доступно" >> log.log
    fi
    done


Обязательная задача 3

Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается.
Ваш скрипт:

array_ip=(192.168.0.1 173.194.222.113 87.250.250.242)
while ((1==1))
do
for ip in ${array_ip[@]}
do
for number in 1 2 3 4 5
    do
    telnet $ip 80
    if (($?==0))
    then
        echo $ip "Доступно" >> log.log
    else
        exit
    fi
    done
done

Дополнительное задание (со звездочкой*) - необязательно к выполнению

Мы хотим, чтобы у нас были красивые сообщения для коммитов в репозиторий. Для этого нужно написать локальный хук для git, который будет проверять, что сообщение в коммите содержит код текущего задания в квадратных скобках и количество символов в сообщении не превышает 30. Пример сообщения: [04-script-01-bash] сломал хук.
Ваш скрипт:

???









Домашнее задание к занятию "3.5. Файловые системы"
1.        Узнайте о sparse (разряженных) файлах.
 файл, в котором последовательности нулевых байтов[1] заменены на информацию об этих последовательностях (список дыр).
2.        Могут ли файлы, являющиеся жесткой ссылкой на один объект, иметь разные права доступа и владельца? Почему?
Не могут. Проверил практически. 
vagrant@vagrant:/tmp/gugl$ chmod 777 g_hardlink
vagrant@vagrant:/tmp/gugl$ ll
total 20
drwxrwxr-x  3 vagrant vagrant 4096 Nov 27 07:08 ./
drwxrwxrwt 14 root    root    4096 Nov 27 06:55 ../
-rwxrwxrwx  2 vagrant vagrant   16 Nov 27 07:07 g_hardlink*
lrwxrwxrwx  1 vagrant vagrant    5 Nov 27 07:05 g_simlink -> g.txt*
-rwxrwxrwx  2 vagrant vagrant   16 Nov 27 07:07 g.txt*
drwxrwxr-x  2 vagrant vagrant 4096 Nov 27 07:04 ttt/
vagrant@vagrant:/tmp/gugl$ chmod 577 g.txt
vagrant@vagrant:/tmp/gugl$ ll
total 20
drwxrwxr-x  3 vagrant vagrant 4096 Nov 27 07:08 ./
drwxrwxrwt 14 root    root    4096 Nov 27 06:55 ../
-r-xrwxrwx  2 vagrant vagrant   16 Nov 27 07:07 g_hardlink*
lrwxrwxrwx  1 vagrant vagrant    5 Nov 27 07:05 g_simlink -> g.txt*
-r-xrwxrwx  2 vagrant vagrant   16 Nov 27 07:07 g.txt*
drwxrwxr-x  2 vagrant vagrant 4096 Nov 27 07:04 ttt/
Жёсткая ссылка связывает индексный дескриптор файла с каталогом и даёт ему имя. Фактически это один и тот же файл. А права хранятся в файле. 
Мягкая ссылка позволяет разные права на файл.

3.        Сделайте vagrant destroy на имеющийся инстанс Ubuntu. Замените содержимое Vagrantfile следующим:
 Vagrant.configure("2") do |config|
 config.vm.box = "bento/ubuntu-20.04"
  config.vm.provider :virtualbox do |vb|
  lvm_experiments_disk0_path = "/tmp/lvm_experiments_disk0.vmdk"
  lvm_experiments_disk1_path = "/tmp/lvm_experiments_disk1.vmdk"
vb.customize ['createmedium', '--filename', lvm_experiments_disk0_path, '--size', 2560]
  vb.customize ['createmedium', '--filename', lvm_experiments_disk1_path, '--size', 2560]
  vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk0_path]
vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', lvm_experiments_disk1_path]
 end
end
Данная конфигурация создаст новую виртуальную машину с двумя дополнительными неразмеченными дисками по 2.5 Гб.
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk 
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part 
└─sda5                 8:5    0 63.5G  0 part 
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk 
sdc                    8:32   0  2.5G  0 disk 

4. Используя fdisk, разбейте первый диск на 2 раздела: 2 Гб, оставшееся пространство.
vagrant@vagrant:~$ sudo fdisk /dev/sdb
...
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
└─sdb2                 8:18   0  511M  0 part
sdc                    8:32   0  2.5G  0 disk

5. Используя sfdisk, перенесите данную таблицу разделов на второй диск.
vagrant@vagrant:~$ sudo sfdisk -d /dev/sdb > gugl_made.txt
vagrant@vagrant:~$ sudo sfdisk -d /dev/sdc < gugl_made.txt
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
└─sdb2                 8:18   0  511M  0 part
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
└─sdc2                 8:34   0  511M  0 part

6.     Соберите mdadm RAID1 на паре разделов 2 Гб.
vagrant@vagrant:~$ sudo mdadm --create /dev/md1 --level=1 --raid-device=2 /dev/sdb1 /dev/sdc1
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
└─sdb2                 8:18   0  511M  0 part
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
└─sdc2                 8:34   0  511M  0 part

7. Соберите mdadm RAID0 на второй паре маленьких разделов.
vagrant@vagrant:~$ sudo mdadm --create /dev/md0 --level=0 --raid-device=2 /dev/sdb2 /dev/sdc2
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
└─sdb2                 8:18   0  511M  0 part
  └─md0                9:0    0 1018M  0 raid0
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
└─sdc2                 8:34   0  511M  0 part
  └─md0                9:0    0 1018M  0 raid0
  
  
8. Создайте 2 независимых PV на получившихся md-устройствах.
vagrant@vagrant:~$ sudo pvcreate /dev/md1 /dev/md0
  Physical volume "/dev/md1" successfully created.
  Physical volume "/dev/md0" successfully created.
vagrant@vagrant:~$ sudo pvscan
  PV /dev/sda5   VG vgvagrant       lvm2 [<63.50 GiB / 0    free]
  PV /dev/md0                       lvm2 [1018.00 MiB]
  PV /dev/md1                       lvm2 [<2.00 GiB]
  Total: 3 [<66.49 GiB] / in use: 1 [<63.50 GiB] / in no VG: 2 [2.99 GiB]
  
  
 9. Создайте общую volume-group на этих двух PV.
 vagrant@vagrant:~$ sudo vgcreate vg1 /dev/md1 /dev/md0
  Volume group "vg1" successfully created
 vagrant@vagrant:~$ sudo vgdisplay
  --- Volume group ---
  VG Name               vgvagrant
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <63.50 GiB
  PE Size               4.00 MiB
  Total PE              16255
  Alloc PE / Size       16255 / <63.50 GiB
  Free  PE / Size       0 / 0
  VG UUID               PaBfZ0-3I0c-iIdl-uXKt-JL4K-f4tT-kzfcyE

  --- Volume group ---
  VG Name               vg1
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               <2.99 GiB
  PE Size               4.00 MiB
  Total PE              765
  Alloc PE / Size       0 / 0
  Free  PE / Size       765 / <2.99 GiB
  VG UUID               heM2Fx-cV5c-eGbw-rp0j-AISb-euuu-DM7RGG
  
10.Создайте LV размером 100 Мб, указав его расположение на PV с RAID0.
vagrant@vagrant:~$ sudo lvcreate -L 100M vg1 /dev/md0
  Logical volume "lvol0" created.
vagrant@vagrant:~$ sudo lvs
  LV     VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  lvol0  vg1       -wi-a----- 100.00m
  root   vgvagrant -wi-ao---- <62.54g
  swap_1 vgvagrant -wi-ao---- 980.00m

  
  
11. Создайте mkfs.ext4 ФС на получившемся LV.
vagrant@vagrant:~$ sudo mkfs.ext4 /dev/vg1/lvol0
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 25600 4k blocks and 25600 inodes

Allocating group tables: done
Writing inode tables: done
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done

12. Смонтируйте этот раздел в любую директорию, например, /tmp/new.
  vagrant@vagrant:~$ sudo mount /dev/vg1/lvol0 /tmp/gugl
 
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
└─sdb2                 8:18   0  511M  0 part
  └─md0                9:0    0 1018M  0 raid0
    └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/gugl
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
└─sdc2                 8:34   0  511M  0 part
  └─md0                9:0    0 1018M  0 raid0
    └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/gugl
 
13. Поместите туда тестовый файл, например wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz.
vagrant@vagrant:~$ wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/gugl/test.gz
/tmp/gugl/test.gz: Permission denied
vagrant@vagrant:~$ sudo wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/gugl/test.gz
--2021-12-01 16:05:53--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 22712347 (22M) [application/octet-stream]
Saving to: ‘/tmp/gugl/test.gz’

/tmp/gugl/test.gz          100%[=====================================>]  21.66M  6.07MB/s    in 3.9s

2021-12-01 16:05:57 (5.50 MB/s) - ‘/tmp/gugl/test.gz’ saved [22712347/22712347]

14. Прикрепите вывод lsblk
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
└─sdb2                 8:18   0  511M  0 part
  └─md0                9:0    0 1018M  0 raid0
    └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/gugl
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
└─sdc2                 8:34   0  511M  0 part
  └─md0                9:0    0 1018M  0 raid0
    └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/gugl

15. Протестируйте целостность файла:

root@vagrant:~# gzip -t /tmp/new/test.gz
root@vagrant:~# echo $?
0

16. Используя pvmove, переместите содержимое PV с RAID0 на RAID1.
vagrant@vagrant:~$ sudo pvmove /dev/md0 /dev/md1
  /dev/md0: Moved: 16.00%
  /dev/md0: Moved: 100.00%
vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
│   └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/gugl
└─sdb2                 8:18   0  511M  0 part
  └─md0                9:0    0 1018M  0 raid0
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
│ └─md1                9:1    0    2G  0 raid1
│   └─vg1-lvol0      253:2    0  100M  0 lvm   /tmp/gugl
└─sdc2                 8:34   0  511M  0 part
  └─md0                9:0    0 1018M  0 raid0

17. Сделайте --fail на устройство в вашем RAID1 md.
vagrant@vagrant:~$ sudo mdadm /dev/md1 --fail /dev/sdc1
mdadm: set /dev/sdc1 faulty in /dev/md1

18. Подтвердите выводом dmesg, что RAID1 работает в деградированном состоянии.
vagrant@vagrant:~$ dmesg | grep md1
[  784.527959] md/raid1:md1: not clean -- starting background reconstruction
[  784.527961] md/raid1:md1: active with 2 out of 2 mirrors
[  784.528076] md1: detected capacity change from 0 to 2144337920
[  784.528483] md: resync of RAID array md1
[  795.213644] md: md1: resync done.
[ 6968.659118] md/raid1:md1: Disk failure on sdc1, disabling device.
               md/raid1:md1: Operation continuing on 1 devices.
               
 19. Протестируйте целостность файла, несмотря на "сбойный" диск он должен продолжать быть доступен:
 vagrant@vagrant:~$ gzip -t /tmp/gugl/test.gz && echo $?
0
20. Погасите тестовый хост, vagrant destroy.
vagrant@vagrant:~$ exit
logout
Connection to 127.0.0.1 closed.

C:\devops\Vagrant>vagrant destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Forcing shutdown of VM...
==> default: Destroying VM and associated drives...


# devops-netology
python add

проигнорировать все файлы в папках .terraform которые могут находится в других папках

проигнорировать все файлы с расширением tfstate
проигнорировать все файлы с любым расширением, которые в содержат в имени .tfstate.
проигнорировать файл crash.log
проигнорирова файлы с расширением tfvars

игнорировать файлы

override.tf
override.tf.json


и файлы которые оканчиваются на
_override.tf
_override.tf.json

игнорировать файлы
.terraformrc
terraform.rc

1
commit aefead2207ef7e2aa5dc81a34aedf0cad4c32545
Author: Alisdair McDiarmid <alisdair@users.noreply.github.com>
Date:   Thu Jun 18 10:29:58 2020 -0400

    Update CHANGELOG.md

2
commit 85024d3100126de36331c6982bfaac02cdab9e76 (tag: v0.12.23)
Author: tf-release-bot <terraform@hashicorp.com>
Date:   Thu Mar 5 20:56:10 2020 +0000

    v0.12.23

3
$ git show b8d720^
commit 56cd7859e05c36c06b56d013b55a252d0bb7e158
Merge: 58dcac4b7 ffbcf5581
Author: Chris Griggs <cgriggs@hashicorp.com>
Date:   Mon Jan 13 13:19:09 2020 -0800

    Merge pull request #23857 from hashicorp/cgriggs01-stable

    [cherry-pick]add checkpoint links

2 родителя

$ git show 58dcac4b7
commit 58dcac4b798f0a2421170d84e507a42838101648 (tag: v0.12.19)
Author: tf-release-bot <terraform@hashicorp.com>
Date:   Wed Jan 8 22:38:48 2020 +0000

    v0.12.19

$ git show ffbcf5581
commit ffbcf55817cb96f6d5ffe1a34abe963b159bf34e
Author: Chris Griggs <cgriggs@hashicorp.com>
Date:   Mon Jan 13 13:13:34 2020 -0800


4
commit 225466bc3e5f35baa5d07197bbc079345b77525e
Cleanup after v0.12.23 release

commit dd01a35078f040ca984cdd349f18d0b67e486c35
Update CHANGELOG.md

commit 4b6d06cc5dcb78af637bbb19c198faff37a066ed
Update CHANGELOG.md

commit d5f9411f5108260320064349b757f55c09bc4b80
command: Fix bug when using terraform login on Windows

commit 06275647e2b53d97d4f0a19a0fec11f6d69820b5
Update CHANGELOG.md

commit 5c619ca1baf2e21a155fcdb4c264cc9e24a2a353
website: Remove links to the getting started guide's old location

Since these links were in the soon-to-be-deprecated 0.11 language section, I
think we can just remove them without needing to find an equivalent link.

commit 6ae64e247b332925b872447e9ce869657281c2bf
    registry: Fix panic when server is unreachable

    Non-HTTP errors previously resulted in a panic due to dereferencing the
    resp pointer while it was nil, as part of rendering the error message.
    This commit changes the error message formatting to cope with a nil
    response, and extends test coverage.

    Fixes #24384

commit 3f235065b9347a758efadc92295b540ee0a5e26e
Update CHANGELOG.md


commit b14b74c4939dcab573326f4e3ee2a62e23e12f89
[Website] vmc provider links

commit 33ff1c03bb960b332be3af2e333462dde88b279e (tag: v0.12.24)
v0.12.24



5

$ git grep --count 'func providerSource'
provider_source.go:2


git log -L :providerSource:provider_source.go
commit 5af1e6234ab6da412fb8637393c5a17a1b293663
Author: Martin Atkins <mart@degeneration.co.uk>
Date:   Tue Apr 21 16:28:59 2020 -0700

    main: Honor explicit provider_installation CLI config when present

    If the CLI configuration contains a provider_installation block then we'll
    use the source configuration it describes instead of the implied one we'd
    build otherwise.


6


$ git grep --count 'func globalPluginDirs'
plugins.go:1


$ git log -L :'func globalPluginDirs':plugins.go
commit 78b12205587fe839f10d946ea3fdc06719decb05
commit 52dbf94834cb970b510f2fba853a5b49ad9b1a46
commit 41ab0aef7a0fe030e84018973a64135b11abcd70
commit 66ebff90cdfaa6938f26f908c7ebad8d547fea17
commit 8364383c359a6b738a436d1b7745ccdce178df47

7
Кто автор функции synchronizedWriters?
$ git grep --count 'synchronizedWriters'

$ git branch -a -v -r | git grep --count 'synchronizedWriters'
Такие запросы ничего не дали.
Такой функции нет.









