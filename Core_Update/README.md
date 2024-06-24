# Первая домашняя работа 
---
#### Цель работы
Научиться обновлять ядро в ОС Linux. Получение навыков работы с Vagrant, Packer и публикацией готовых образов в Vagrant Cloud.

#### Описание домашнего задания
Нужно обновить ядро ОС из репозитория ELRepo, создать Vagrant box c помощью Packer и загрузить Vagrant box в Vagrant Cloud

#### Выполнение
Вначале я скачал необходимое ПО (Vagrant, Packer, VirtualBox) и настроил его. 
Попробовал скачать и установить Centos из базового образа в Vagrant.
    
    MACHINES = {
      :"kernel-update" => {
              :box_name => "generic/centos8s",
              :cpus => 2,
              :memory => 1024,
            }
    }

    Vagrant.configure("2") do |config|
      MACHINES.each do |boxname, boxconfig|
       config.vm.synced_folder ".", "/vagrant", disabled: true
        config.vm.define boxname do |box|
          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s
          box.vm.provider "virtualbox" do |v|
            v.memory = boxconfig[:memory]
            v.cpus = boxconfig[:cpus]
          end
        end
      end
    end
    
Далее подключил репозиторий 

    sudo yum install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm 
И установил последнее ядро из репозитория

    sudo yum --enablerepo elrepo-kernel install kernel-ml -y

Убедился что ядро обновилось после перезагрузки системы.

