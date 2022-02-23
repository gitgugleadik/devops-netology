Курсовая работа по итогам модуля "DevOps и системное администрирование"
Курсовая работа необходима для проверки практических навыков, полученных в ходе прохождения курса "DevOps и системное администрирование".

Мы создадим и настроим виртуальное рабочее место. Позже вы сможете использовать эту систему для выполнения домашних заданий по курсу

Задание
1. Создайте виртуальную машину Linux.
2. Установите ufw и разрешите к этой машине сессии на порты 22 и 443, при этом трафик на интерфейсе localhost (lo) должен ходить свободно на все порты.
```bash
vagrant@vagrant:~$ sudo ufw default deny incoming
vagrant@vagrant:~$ sudo ufw default allow outgoing
vagrant@vagrant:~$ sudo ufw allow 22
Rules updated
Rules updated (v6)
vagrant@vagrant:~$ sudo ufw allow 443
Rules updated
Rules updated (v6)
vagrant@vagrant:~$ sudo ufw enable
vagrant@vagrant:~$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
22                         ALLOW       Anywhere
443                        ALLOW       Anywhere
22 (v6)                    ALLOW       Anywhere (v6)
443 (v6)                   ALLOW       Anywhere (v6)
```
3. Установите hashicorp vault (инструкция по ссылке).
vault установил 
```bash
vagrant@trusty:~$ vault --version
Vault v1.9.3 (7dbdd57243a0d8d9d9e07cd01eb657369f8e1b8a)
```
4. Cоздайте центр сертификации по инструкции (ссылка) и выпустите сертификат для использования его в настройке веб-сервера nginx (срок жизни сертификата - месяц).
Выполнял по инструкции, на Step 2  
Execute the following command to generate an intermediate and save the CSR as pki_intermediate.csr  
```bash
vagrant@trusty:~$ vault write -format=json pki_int/intermediate/generate/internal \
>      common_name="example.com Intermediate Authority" \
>      | jq -r '.data.csr' > pki_intermediate.csr
-bash: jq: command not found
vagrant@trusty:~$
```
Установил не найденую команду jq
```bash
vagrant@trusty:~$ sudo apt-get install jq
```  
Повторил прежнюю команду, выполнилась. Далее остальные команды тоже ОК.  
Результат  
```bash
private_key         -----BEGIN RSA PRIVATE KEY-----
MIIEpgIBAAKCAQEAup8BwG20pxwlDLtTfnGj6w5XkaL08YK8uQlCx8hVlD1+pAKy
AQshmD6xJ70tXSAEUljycWAdK02qgS7wOZnx9uPYLsGhSg/RagZvpQRkZEQupdU4
+ykXzffDqfE6Noag5tVC0TPntgqQePKC9WJm32GW1+7zUSsslXvOmQ1Und70D4YZ
kqjOj2TLgEDy9n1O6+dw6ij7ff8Yswm+3xMlkSDyVB6xlIHnBJqTvhkBN0bV9oxx
+X1TI/RcrR2bHY7mNOInqD98Zkwsr8NXH430r88qEaXXWnM5LAZbReoPKQRaqITT
/S2sl76bsqhDSe9s05STubgBqyDA6QmmKn90oQIDAQABAoIBAQCR3pul55pfTJaB
HyMiIH15y5oTEgbXh9Mv5tc2BZcu6epFFH5CZor5z3b1kt8UfWQjYbcPe4sRQAHY
O/I1c+k3i9x8n4kMtNSBRUqa95Xo8YpswP9rAjHDIrjj6tQPrqeyBlvV3fZtylAm
2ZgXabTzQfqAChxSA6czqLRR2aOcSUUWxFuLij0YyGAb+gd2Q0eqqiinMMHu33G0
L+zjDwvd0X1YN/f5pIR5ITvbGlijFF0BxEgX4vTmPm2ppKmaq5YXEt3PF/ZalCEk
3kfTlJyhaqZNlXMYgESH0owB5AfzdP3qFdWmgZ2wlqkLX6dT23uIQno+oey2nn0Z
ThFCbAABAoGBAOL+zLQN9F7MwewK8sYbD3UUMRCPNKdprayuFYbSBSomT08Bun0Y
la15xqIQyDPjP0bv4prjeNp3Q9DdkxpZcRFF9/TxKNCTc5upEQ1VFNZC3PBth/gZ
Cwq6muNVQrvlNcgzJ3HPo0Xli3LFkHhojxf4mWEW7CRje6CBI8uTykChAoGBANJ3
iOZNsZ/zYlTJcSPpBLwT+QGG1cdAstl+37lXJbclyYQrS3JVqONtg6ZfolJsbBE1
kruMlP57CcgCgObRTcsqOE3U5QJY01SfMR7QPO2pAmDcKNK43AcFygBZPYUP6JeP
e5bozye/PdD4e9F1oIB+mR12DFhLb+Uggh8mxLQBAoGBAJzu5Z0x7JnB2+wR4ag+
uyAJdqZpK1D2yeCRdkaAWpu6YqhPnJux7IFDqKURDyh4Wp3zaOoGi94WCGeVWIcm
APqdMgFA3SPeXVXnu+dIxCAhl9gNEazfu3eObVjv8DQxEk63tvSDRfEj8pXFqszk
FNHQyFGMZHP/50+fGJ09Lt4hAoGBAKMM4A4rmrRkBYXSGcjMOVLL1lkMcInQ4b4F
wKUBksJ0j83JDMYi/phSu28lH8fjH0Wlz2tk2fjcsRM2fU5UUIRYzQ3fJRvQXMhu
G8vXX5xvFtybMzUs6ai3H2ttt29ih7sC+ahL7FDKo8VE/AelrRZe/ZgJYD73ElTb
/nLLwhABAoGBAMYly12BwsVjXIQcS6+U4fSkPmCcRwNxqJOfDOvpp1uwJ9jQlp9u
rKwP7BeS0Q6xjcIpvp1CYdCaBA+fuPSA1uIzdXTvaMMDD9uXmsXVEeL7lsfVtbDY
GooR2ASW+uOR/WOf97qOBldb6Q3ikDPE+p7KZJGEarxnltMHWxunRUFg
-----END RSA PRIVATE KEY-----
private_key_type    rsa
serial_number       3e:d6:d4:d2:2a:29:bf:c5:22:88:23:db:34:59:90:15:e9:e2:46:c0
vagrant@trusty:~$

root@trusty:/home/vagrant# ls
CA_cert.crt  intermediate.cert.pem  pki_intermediate.csr  unseal.key
```
5. Установите корневой сертификат созданного центра сертификации в доверенные в хостовой системе.  
CA_cert.crt  установил в хостовой системе
<code>![sert](/серт.jpg "сертификат")
</code>  

<code>![sert](/серт2.jpg "сертификат")
</code>
6. Установите nginx.
```bash
vagrant@trusty:~$ sudo apt install nginx
vagrant@trusty:~$ sudo ufw allow 80
vagrant@trusty:~$ sudo ufw enable
vagrant@trusty:~$ sudo ufw allow 'Nginx Full'
vagrant@trusty:~$ sudo ufw allow 'OpenSSH'
vagrant@trusty:~$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
22                         ALLOW       Anywhere
443                        ALLOW       Anywhere
8200                       ALLOW       Anywhere
8080                       ALLOW       Anywhere
80                         ALLOW       Anywhere
Nginx Full                 ALLOW       Anywhere
OpenSSH                    ALLOW       Anywhere
22 (v6)                    ALLOW       Anywhere (v6)
443 (v6)                   ALLOW       Anywhere (v6)
8200 (v6)                  ALLOW       Anywhere (v6)
8080 (v6)                  ALLOW       Anywhere (v6)
80 (v6)                    ALLOW       Anywhere (v6)
Nginx Full (v6)            ALLOW       Anywhere (v6)
OpenSSH (v6)               ALLOW       Anywhere (v6)

```
http://192.168.0.112/  
Доступен с хотовой машины.

Welcome to nginx!
If you see this page, the nginx web server is successfully installed and working. Further configuration is required.  
For online documentation and support please refer to nginx.org.  
Commercial support is available at nginx.com.  
Thank you for using nginx.  

###### Дальше застрял на 7 задании
Разбираюсь с инструкцией. Надеюсь 6 заданий сделаны верно. 


7. По инструкции (ссылка) настройте nginx на https, используя ранее подготовленный сертификат:
*можно использовать стандартную стартовую страницу nginx для демонстрации работы сервера;
*можно использовать и другой html файл, сделанный вами;  

8. Откройте в браузере на хосте https адрес страницы, которую обслуживает сервер nginx.
9. Создайте скрипт, который будет генерировать новый сертификат в vault:
генерируем новый сертификат так, чтобы не переписывать конфиг nginx;
перезапускаем nginx для применения нового сертификата.
10. Поместите скрипт в crontab, чтобы сертификат обновлялся какого-то числа каждого месяца в удобное для вас время.
Результат
Результатом курсовой работы должны быть снимки экрана или текст:

Процесс установки и настройки ufw
Процесс установки и выпуска сертификата с помощью hashicorp vault
Процесс установки и настройки сервера nginx
Страница сервера nginx в браузере хоста не содержит предупреждений
Скрипт генерации нового сертификата работает (сертификат сервера ngnix должен быть "зеленым")
Crontab работает (выберите число и время так, чтобы показать что crontab запускается и делает что надо)







### Как сдавать задания

Вы уже изучили блок «Системы управления версиями», и начиная с этого занятия все ваши работы будут приниматься ссылками на .md-файлы, размещённые в вашем публичном репозитории.

Скопируйте в свой .md-файл содержимое этого файла; исходники можно посмотреть [здесь](https://raw.githubusercontent.com/netology-code/sysadm-homeworks/devsys10/04-script-03-yaml/README.md). Заполните недостающие части документа решением задач (заменяйте `???`, ОСТАЛЬНОЕ В ШАБЛОНЕ НЕ ТРОГАЙТЕ чтобы не сломать форматирование текста, подсветку синтаксиса и прочее, иначе можно отправиться на доработку) и отправляйте на проверку. Вместо логов можно вставить скриншоты по желани.

# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"


## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
```
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            },
            { "name" : "second",
            "type" : "proxy",
            "ip" : "71.78.22.43"
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис

## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python
#!/usr/bin/env python3

import socket
import json
import yaml
# Function to return IP address
def get_IP_byName(host_name):
   try:
     host_ip = socket.gethostbyname(host_name)
   except:
     print("Unable to get Hostname and IP")
   return host_ip

with open('dns.json', 'r') as js1:
    line = json.load(js1)

for key, value in line.items():
    newIP = get_IP_byName(key)
    if value != newIP:
       temp = {key : newIP}
       print(key + ' IP mismatch: ' + value + ' ' + newIP)
       with open('dns.json', 'w') as js1:
            js1.write(json.dumps(temp))
       with open('dns.yml', 'w') as ym1:
            ym1.write(yaml.dump(temp))
```

### Вывод скрипта при запуске при тестировании:
```
vagrant@vagrant:/vagrant/gugl$ ./servic.py
google.com IP mismatch: 216.58.20.174 216.58.209.174
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
{"google.com": "216.58.209.174"}
```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
google.com: 216.58.209.174
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так как команды в нашей компании никак не могут прийти к единому мнению о том, какой формат разметки данных использовать: JSON или YAML, нам нужно реализовать парсер из одного формата в другой. Он должен уметь:
   * Принимать на вход имя файла
   * Проверять формат исходного файла. Если файл не json или yml - скрипт должен остановить свою работу
   * Распознавать какой формат данных в файле. Считается, что файлы *.json и *.yml могут быть перепутаны
   * Перекодировать данные из исходного формата во второй доступный (из JSON в YAML, из YAML в JSON)
   * При обнаружении ошибки в исходном файле - указать в стандартном выводе строку с ошибкой синтаксиса и её номер
   * Полученный файл должен иметь имя исходного файла, разница в наименовании обеспечивается разницей расширения файлов

### Ваш скрипт:
```python
???
```

### Пример работы скрипта:
???
