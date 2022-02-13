*** Доработка 2 ***
Добрый день!

100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
Можно использовать паравиртуализацию на VirtualBox

Предлагаю рассказать, как в этом случае будет происходить балансировка нагрузки и репликация.

С уважением,
Алексей
*****
<hr>
Алексей добрый день!, в видео лекции этого не было. Поиск в Google статей  дает
https://rustamkhodjaev.com/2019/06/08/concept_of_virtualization_oracle_vm_installation/
с учетом моего полного не понимания этой темы , в статье есть
2 варианта балансировки
Вариант 1. Ручная балансировка нагрузки.

Останавливаются обе VM (ОС);
Вручную расширяются ресурсы VM1 (например, увеличиваются CPU до 6 ядер, а оперативная память до 24 Гб;
Запускаются обе VM.
Вариант 2. Динамическая балансировка нагрузки.

Настраивается динамическая балансировка нагрузки между VM;
В результате гипервизор сам будет распределять ресурсы между VM без участия системного администратора и не будет необходимости останавливать/перезагружать VM (ОС).

Как реализовать вариант 2 не нашел.
вероятно это решается установкой в 100% Ползунка Предел загрузки ЦПУ в настройках ВМ VirtualBox Система - процессор

Что касается репликации, то в virtualBox есть клонирование ВМ. Как я понял из статей в инете, это одно и тоже.
Автоматически клонировать ВМ наверное можно через команду export 
http://mirspo.narod.ru/vbox/
либо 
VBoxManage clonehd
Жаль, что в лекции вы не даете исчерпывающий материал и очень много времени тратится на подтверждение вариантов в уме. 
Прошу разъяснить эти вопросы
1. Верно ли я понимаю динамическую балансировку нагрузки в VirtualBox?
2. Верно ли я понимаю репликацию в Virtualbox?
***

**** Доработка 2 конец ****

Добрый день!

Задание 1

Чем аппаратная и паравиртуализации отличаются от полной виртуализации?
Исходя из презентации слайд 11
Полная (аппаратная) виртуализация 
т.е. аппаратная не отличается от полной, это одно и тоже.
Гипервизор установлен на железо, он же и является ОС. пример Microsoft Hiper V server 
В аппаратной виртуализации - Каждая из виртуальных машин может работать независимо, в своем пространстве аппаратных ресурсов, полностью изолированно друг от друга. Это позволяет устранить потери быстродействия на поддержание хостовой платформы и увеличить защищенность.
Паравиртуализация означает, что гипервизор устанавливается в ОС , т.е. ОС прослойка.
Т.е. отличие в том, что для паравиртуализации используется ОС хоста.

Предлагаю пояснить почему не будет конкуренции в последнем случае, если, например, трём ВМ потребуется доступ к одному и тому же накопителю.
Из определения Виртуализация на уровне операционной системы
Как я понимаю это и есть контейнерная виртуализация. 
из определения
виртуализация на уровне операционной системы — позволяет запускать изолированные виртуальные системы на одном физическом узле, но не позволяет запускать операционные системы с ядрами, отличными от типа ядра базовой операционной системы. При таком подходе не существует отдельного слоя гипервизора, вместо этого сама хостовая операционная система отвечает за разделение аппаратных ресурсов между несколькими гостевыми системами (контейнерами) и обеспечивает их независимость. 
Вроде написано что это изолированные виртуальные системы, а значит нет конкуренции. И тут же написано, что хостовая ОС овечает за разделение аппарат. ресурсов, а значит будет конкуренция.

Задание 3
Как в этом случае будет происходить балансировка нагрузки и репликация?
Полагаю это касается этого вопроса
100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
Proxmox Virtual Environment на KVM
возможность - Объединение серверов в кластер с балансировкой нагрузки
	-  Автоматическое резервное копирование виртуальных машин
	
**** Доработка ****


Домашнее задание к занятию "5.1. Введение в виртуализацию. Типы и функции гипервизоров. Обзор рынка вендоров и областей применения."


Задача 1
Опишите кратко, как вы поняли: в чем основное отличие полной (аппаратной) виртуализации, паравиртуализации и виртуализации на основе ОС.
В аппаратной вирт. распределением ресурсами занимается гипервизор.
В паравиртуализации виртуальные машины будут конкурировать за ресурсы, так как их запросы прилетают в общую ОС
в виртуализации на уровне ОС - машинам жестко определяют ресурсы, исключая конкуренцию за ресурсы.

Задача 2
Выберите один из вариантов использования организации физических серверов, в зависимости от условий использования.

Организация серверов:

физические сервера,
паравиртуализация,
виртуализация уровня ОС.
Условия использования:

Высоконагруженная база данных, чувствительная к отказу.
	
Различные web-приложения.
	Для этого виртуализация уровня ОС для изолированности процессов
Windows системы для использования бухгалтерским отделом.
	Слишком размытое понятие системы, о чем речь? 1с или Парус на 100 клиентов? Парус на физическом сервере будет летать, а 1с будет тормозить (из моей практики, пришлось выносить сервер приложений на виртуальную машину) уточните вопрос.
Системы, выполняющие высокопроизводительные расчеты на GPU.
	VMware поддерживает: 
	- Проброс топовых GPU внутрь гостя GPU Pass-through (для конкретного виртуального гостя — конкретный GPU в физическом сервере)
	- GPU Virtualization — возможность множеству виртуальных машин получить доступ к GPU хоста, что лучше, чем программная эмуляция
Опишите, почему вы выбрали к каждому целевому использованию такую организацию.

Задача 3
Выберите подходящую систему управления виртуализацией для предложенного сценария. Детально опишите ваш выбор.

Сценарии:

1. 100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований. Преимущественно Windows based инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий.
Можно использовать паравиртуализацию на VirtualBox
2. Требуется наиболее производительное бесплатное open source решение для виртуализации небольшой (20-30 серверов) инфраструктуры на базе Linux и Windows виртуальных машин.
Proxmox
3. Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows инфраструктуры.
Microsoft Hyper-V Server бесплатная операционная система
4. Необходимо рабочее окружение для тестирования программного продукта на нескольких дистрибутивах Linux.
Можно использовать VirtualBox + vagrant
Задача 4
Опишите возможные проблемы и недостатки гетерогенной среды виртуализации (использования нескольких систем управления виртуализацией одновременно) и что необходимо сделать для минимизации этих рисков и проблем. Если бы у вас был выбор, то создавали бы вы гетерогенную среду или нет? Мотивируйте ваш ответ примерами.
Возможно недостаток в том, что такие среды тяжелей обслуживать, и порог высокий для инженеров. 
Не стал бы делать гетерогенную среду, да и нет выбора сейчас - импортозамещение...


Домашнее задание к занятию "3.9. Элементы безопасности информационных систем"
1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.
Установил, скриншот приложил bitwarden.jpg

2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.
Установил,  все настроил. Двухфакторная.jpg
3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.
vagrant@vagrant:~$ apache2 -v

Server version: Apache/2.4.41 (Ubuntu)

Server built:   2022-01-05T14:49:56

не получается настроить с SSL  Дайте инструкцию. Делал по презентации не получилось.
https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-apache-in-ubuntu-18-04-ru
Спасибо, за инструкцию, по ней заработало
root@vagrant:/etc/apache2/conf-available# apache2ctl configtest
Syntax OK
root@vagrant:/etc/apache2/conf-available# systemctl restart apache2

https://192.168.0.102/
открывается, через дополниетльно
4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).
vagrant@vagrant:/vagrant/ssltest/testssl.sh$ ./testssl.sh -U --sneaky https://www.lazer-project.com/

###########################################################
    testssl.sh       3.1dev from https://testssl.sh/dev/
    (06890d4 2022-01-10 11:19:10 -- )

      This program is free software. Distribution and
             modification under GPLv2 permitted.
      USAGE w/o ANY WARRANTY. USE IT AT YOUR OWN RISK!

       Please file bugs @ https://testssl.sh/bugs/

###########################################################

 Using "OpenSSL 1.0.2-chacha (1.0.2k-dev)" [~183 ciphers]
 on vagrant:./bin/openssl.Linux.x86_64
 (built: "Jan 18 17:12:17 2019", platform: "linux-x86_64")


 Start 2022-01-22 12:27:47        -->> 185.230.63.177:443 (www.lazer-project.com) <<--

 rDNS (185.230.63.177):  unalocated.63.wixsite.com.
 Service detected:       HTTP
...

5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.

student@student:~$ ssh-keygen 
...

создали ключи
далее копируем публичный на сервер
student@student:~$ ssh-copy-id vagrant@192.168.0.107
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@192.168.0.107's password: 

для этого пришлось сервере в /etc/ssh/ssh_config 
расскомментировать PasswordAuthentication yes
student@student:~$ ssh-copy-id vagrant@192.168.0.107
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
vagrant@192.168.0.107's password: 
ввел пароль 
Number of key(s) added: 1

теперь подключаемся без пароля

student@student:~$ ssh vagrant@192.168.0.107
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-80-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat 22 Jan 2022 02:29:00 PM UTC

  System load:  0.0               Processes:             112
  Usage of /:   2.5% of 61.31GB   Users logged in:       1
  Memory usage: 23%               IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%                IPv4 address for eth1: 192.168.0.107


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Sat Jan 22 14:21:16 2022 from 192.168.0.105
vagrant@vagrant:~$ 
Удачно подключился

6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.
добавил в файл etc/ssh/ssh-config
Host my
    HostName 192.168.0.107
    User vagrantroot@student:/home/student# 

root@student:/home/student# ssh my
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-80-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat 22 Jan 2022 04:43:13 PM UTC

  System load:  0.08              Processes:             116
  Usage of /:   2.5% of 61.31GB   Users logged in:       1
  Memory usage: 17%               IPv4 address for eth0: 10.0.2.15
  Swap usage:   0%                IPv4 address for eth1: 192.168.0.107


This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
Last login: Sat Jan 22 16:41:51 2022 from 192.168.0.105
vagrant@vagrant:~$ 



7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
vagrant@vagrant:/vagrant$ sudo tcpdump -c 100 -w 001.pcap -i eth1
tcpdump: listening on eth1, link-type EN10MB (Ethernet), capture size 262144 bytes
100 packets captured
119 packets received by filter
0 packets dropped by kernel
vagrant@vagrant:/vagrant$

Открыл в WireShark
100-й пакет из wireShark
"100","7.033665","192.168.0.105","192.168.0.107","SSHv2","102","Client: Encrypted packet (len=36)"

# Домашнее задание к занятию "4.2. Использование Python для решения типовых DevOps задач"

## Обязательная задача 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:
| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | Никакое. Не соответствие типов  |
| Как получить для переменной `c` значение 12?  | c = str(a) + b  |
| Как получить для переменной `c` значение 3?  | c = a + int(b)  |

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(os.getcwd() + '/netology/sysadm-homeworks/' + prepare_result)
```

### Вывод скрипта при запуске при тестировании:
```
vagrant@vagrant:~$ ./2.py
/home/vagrant/netology/sysadm-homeworks/01-intro-01/README.md
/home/vagrant/netology/sysadm-homeworks/01-intro-01/netology.md
/home/vagrant/netology/sysadm-homeworks/02-git-02-base/README.md
vagrant@vagrant:~$
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import os
import sys

bash_command = ["cd " + sys.argv[1], "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(sys.argv[1] + '/' + prepare_result)
```

### Вывод скрипта при запуске при тестировании:
```
vagrant@vagrant:~$ ./3.py /home/vagrant/netology/sysadm-homeworks
/home/vagrant/netology/sysadm-homeworks/01-intro-01/README.md
/home/vagrant/netology/sysadm-homeworks/01-intro-01/netology.md
/home/vagrant/netology/sysadm-homeworks/02-git-02-base/README.md
vagrant@vagrant:~$ ./3.py ~/netology/sysadm-homeworks
/home/vagrant/netology/sysadm-homeworks/01-intro-01/README.md
/home/vagrant/netology/sysadm-homeworks/01-intro-01/netology.md
/home/vagrant/netology/sysadm-homeworks/02-git-02-base/README.md
vagrant@vagrant:~$
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import socket

# Function to return IP address
def get_IP_byName(host_name):
    try:
        host_ip = socket.gethostbyname(host_name)
    except:
        print("Unable to get Hostname and IP")
    return host_ip

i = 0
fdns = {'drive.google.com':'74.125.205.194', 'mail.google.com':'173.194.73.18', 'google.com':'173.194.222.102'}
with open('dns.log', 'r') as file2:
    for line in file2:
       newIP = get_IP_byName(line.split(':')[0])
       if line.split(':')[1][:-1] != newIP:
          print(line.split(':')[0] + ' IP mismatch: ' + line.split(':')[1][:-1] + ' ' + newIP)
with open('dns.log', 'w') as file2:
    for key,val in fdns.items():
        file2.write('{}:{}\n'.format(key,get_IP_byName(key)))
```

### Вывод скрипта при запуске при тестировании:
```
vagrant@vagrant:~$ ./4.py
drive.google.com IP mismatch: 64.233.165.194 74.125.205.194
mail.google.com IP mismatch: 173.194.73.17 173.194.73.83
google.com IP mismatch: 173.194.222.100 173.194.222.102
vagrant@vagrant:~$ ./4.py
mail.google.com IP mismatch: 173.194.73.17 173.194.73.18
google.com IP mismatch: 173.194.222.138 173.194.222.113
vagrant@vagrant:~$
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так получилось, что мы очень часто вносим правки в конфигурацию своей системы прямо на сервере. Но так как вся наша команда разработки держит файлы конфигурации в github и пользуется gitflow, то нам приходится каждый раз переносить архив с нашими изменениями с сервера на наш локальный компьютер, формировать новую ветку, коммитить в неё изменения, создавать pull request (PR) и только после выполнения Merge мы наконец можем официально подтвердить, что новая конфигурация применена. Мы хотим максимально автоматизировать всю цепочку действий. Для этого нам нужно написать скрипт, который будет в директории с локальным репозиторием обращаться по API к github, создавать PR для вливания текущей выбранной ветки в master с сообщением, которое мы вписываем в первый параметр при обращении к py-файлу (сообщение не может быть пустым). При желании, можно добавить к указанному функционалу создание новой ветки, commit и push в неё изменений конфигурации. С директорией локального репозитория можно делать всё, что угодно. Также, принимаем во внимание, что Merge Conflict у нас отсутствуют и их точно не будет при push, как в свою ветку, так и при слиянии в master. Важно получить конечный результат с созданным PR, в котором применяются наши изменения. 

### Ваш скрипт:
```python
???
```

### Вывод скрипта при запуске при тестировании:
```
???
```

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
| `a`  | 1  | по условию |
| `b`  | 2  | по условию |
| `c`  | a+b  | a+b  это строка, она так и записалась |
| `d`  | 1+2  | т.к. d это строковый тип данных, то и символы 1 + и 2 Записались в d как строка |
| `e`  | 3  | Скобки обеспечивают выполнение арифметической операции, а внешний $ - выбирает значение результата. |


## Обязательная задача 2
На нашем локальном сервере упал сервис и мы написали скрипт, который постоянно проверяет его доступность, записывая дату проверок до тех пор, пока сервис не станет доступным (после чего скрипт должен завершиться). В скрипте допущена ошибка, из-за которой выполнение не может завершиться, при этом место на Жёстком Диске постоянно уменьшается. Что необходимо сделать, чтобы его исправить:
```bash
while ((1==1))
do
	curl https://localhost:4757
	if (($? == 0))
	then
		date >> curl.log
		break
	fi
done
```

Необходимо написать скрипт, который проверяет доступность трёх IP: `192.168.0.1`, `173.194.222.113`, `87.250.250.242` по `80` порту и записывает результат в файл `log`. Проверять доступность необходимо пять раз для каждого узла.

### Ваш скрипт:
```bash
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
```
## Обязательная задача 3
Необходимо дописать скрипт из предыдущего задания так, чтобы он выполнялся до тех пор, пока один из узлов не окажется недоступным. Если любой из узлов недоступен - IP этого узла пишется в файл error, скрипт прерывается.

### Ваш скрипт:
```bash
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
```
## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Мы хотим, чтобы у нас были красивые сообщения для коммитов в репозиторий. Для этого нужно написать локальный хук для git, который будет проверять, что сообщение в коммите содержит код текущего задания в квадратных скобках и количество символов в сообщении не превышает 30. Пример сообщения: \[04-script-01-bash\] сломал хук.

### Ваш скрипт:
```bash
???
```



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









