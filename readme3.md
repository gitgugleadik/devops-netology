Удалось установтиь докер так
vagrantfile
$script = <<-SCRIPT
apt-get update -y && apt-get upgrade -y
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common git
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu/ $(lsb_release -cs) stable"
apt-get update -y
apt-cache polisy docker-ce -y
apt-get install docker-ce -y
udo usermod -aG docker ${USER}
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = "trusty"
  config.vm.provision "shell", inline: $script
  config.vm.network "public_network", bridge: "en1: Realtek PCIe GBE Family Controller"
end




vagrant@trusty:~$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES



Домашнее задание к занятию "5.2. Применение принципов IaaC в работе с виртуальными машинами"

Задача 1
Опишите своими словами основные преимущества применения на практике IaaC паттернов.
С помощью паттернов можно быстро разворачивать инфраструктуру, исключает "дрейф" конфигураций, ускоряет разработку и внедрение. 
Какой из принципов IaaC является основополагающим?
идемпотентность - свойство при повторении, получать идентичную конфигурацию
Задача 2
Чем Ansible выгодно отличается от других систем управление конфигурациями?
Можно использовать в существующем  ssh соединении, т.е. не требует другого соединения
Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?
pull режим лучше. Целевой хост может быть выключен. Когда включится, тогда и заросит обновление конфигурации.
Задача 3
Установить на личный компьютер:

VirtualBox
Vagrant
Ansible
Приложить вывод команд установленных версий каждой из программ, оформленный в markdown.
gugle@DESKTOP-1S5489F ~
$ ansible --version
ansible 2.8.4
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/gugle/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.7/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.7.12 (default, Nov 23 2021, 18:58:07) [GCC 11.2.0]

Задача 4 (*)
Воспроизвести практическую часть лекции самостоятельно.
Vagrant не видит Ansible установленный в Windows через Cygwin
Команда vagrant up  дает
Windows is not officially supported for the Ansible Control Machine.
Please check https://docs.ansible.com/intro_installation.html#control-machine-requirements
Vagrant gathered an unknown Ansible version:


and falls back on the compatibility mode '1.8'.

Alternatively, the compatibility mode can be specified in your Vagrantfile:
https://www.vagrantup.com/docs/provisioning/ansible_common.html#compatibility_mode
    server1.netology: Running ansible-playbook...
The Ansible software could not be found! Please verify
that Ansible is correctly installed on your host system.

If you haven't installed Ansible yet, please install Ansible
on your host system. Vagrant can't do this for you in a safe and
automated way.
Please check https://docs.ansible.com for more information.

**************
Задание 3 нужно выполнять в Linux  ??
*****

Создать виртуальную машину.
Зайти внутрь ВМ, убедиться, что Docker установлен с помощью команды
docker ps
