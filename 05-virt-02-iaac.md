Задача 1

	Опишите своими словами основные преимущества применения на практике IaaC паттернов.

Ответ:

Основные преимущества применения IaaC - это ускорение выхода продукта на рынок, за счет исключения дрейфа конфигураций, улучшения стабильности сред разработки, автоматизации отката на стабильные версии, в случае обнаружения ошибок, а также более легкий поиск этих ошибок.

	Какой из принципов IaaC является основополагающим?

Ответ:

Идемпотентность - свойство объекта или операции при повторном применении операции к объекту давать тот же результат,
что и при первом.

Задача 2

Чем Ansible выгодно отличается от других систем управление конфигурациями?

Ответ:

Ansible можно использовать без установки специального ПО на целевом хосте. Ansible работает по SSH, на распространенном языке Python. Также может оповестить о неудачной доставке конфигурации на сервер.

Какой, на ваш взгляд, метод работы систем конфигурации более надёжный push или pull?

Ответ:

На мой взгляд push, так как конфигурация отправляется управляющим сервером, который проще мониторить на возникновение ошибок или сетевых проблем.

Задача 3

Установить на личный компьютер:

	VirtualBox
	Vagrant
	Terraform
	Ansible

Приложить вывод команд установленных версий каждой из программ, оформленный в markdown.

Ответ:

VirtualBox 7.0.4 r154605 на Windows

C:\Users\ASUS_TUF_15>vagrant --version
Vagrant 2.3.3

vagrant@vagrant:~$ terraform -version
Terraform v1.3.7
on linux_amd64

vagrant@vagrant:~$ ansible --version
ansible 2.9.6
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /home/vagrant/bin/ansible
  python version = 3.8.10 (default, Mar 15 2022, 12:22:08) [GCC 9.4.0]

Ссылка на скриншот:

https://imgur.com/a/oiwRAXI

Задача 4

Воспроизвести практическую часть лекции самостоятельно.

Создать виртуальную машину.

Зайти внутрь ВМ, убедиться, что Docker установлен с помощью команды

docker ps

Ответ:

Создал виртуальную машину используя Vagrantfile из задания.

Ссылка на скриншот:

https://imgur.com/a/2m8ZsUO

Так как Windows не потдерживает Ansible получил такой ответ при выполнении команды vagrant provision.

C:\Users\ASUS_TUF_15\vagrant2>vagrant provision
==> server1.netology: Running provisioner: ansible...
Windows is not officially supported for the Ansible Control Machine.
Please check https://docs.ansible.com/intro_installation.html#control-machine-requirements

Установил docker выполнив команды, из provision.yml, вручную.

Ссылка на скриншот:

https://imgur.com/a/CR0Xzt1
