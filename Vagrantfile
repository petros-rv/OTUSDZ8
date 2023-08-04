# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos/7"
  config.vm.box_version = "1804.02"
  config.vm.define  "OTUS-DZ8"
  config.vm.hostname = "OTUS-DZ8"
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Disable the default share of the current code directory. Doing this
  # provides improved isolation between the vagrant box and your host
  # by making sure your Vagrantfile isn't accessable to the vagrant box.
  # If you use this you may want to enable additional shared subfolders as
  # shown above.
  # config.vm.synced_folder ".", "/vagrant", disabled: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
      vb.name = "OTUS-DZ8"  
      vb.gui = false
  #   # Customize the amount of memory on the VM:
      vb.memory = "1024"
   end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
#   config.vm.provision :shell, :path => "vgs_rename.sh"
#    config.vm.provision "file", source: "./module-setup.sh", destination: "/tmp/module-setup.sh"
#    config.vm.provision "file", source: "./test.sh", destination: "/tmp/test.sh"
config.vm.provision "shell", inline: <<-SHELL
       
        echo -e "\033[33m  1. Задание №1: Написать service, который будет раз в 30 секунд мониторить лог на предмет наличия ключевого слова ‘ALERT’
        echo -e "\033[33m  1.1 Создадим файл настроек /etc/sysconfig/watchlog 
        
        touch /etc/sysconfig/watchlog
        echo  WORD='"ALERT"' >> /etc/sysconfig/watchlog
        echo  LOG=/var/log/watchlog.log >> /etc/sysconfig/watchlog
    
        echo -e "\033[33m  1.2 Cоздаем /var/log/watchlog.log и впишем туда ключевое слово ‘ALERT’"
    
        touch /etc/sysconfig/watchlog
        tail /var/log/dmesg >> /var/log/watchlog.log
        echo -e "\nALERT\n" >> /var/log/watchlog.log
        tail /var/log/dmesg >> /var/log/watchlog.log
        echo -e "\nALERT\n" >> /var/log/watchlog.log
        tail /var/log/dmesg >> /var/log/watchlog.log

        echo -e "\033[33m    1.3 Cоздаем скрипт /opt/watchlog.sh"
          
        touch /opt/watchlog.sh
        echo '#!/bin/bash' >> /opt/watchlog.sh
        echo 'WORD=$1' >> /opt/watchlog.sh
        echo 'LOG=$2' >> /opt/watchlog.sh
        echo 'DATE=`date`' >> /opt/watchlog.sh
        echo 'if grep $WORD $LOG &> /dev/null' >> /opt/watchlog.sh
        echo 'then' >> /opt/watchlog.sh
        echo '  logger "$DATE: I found word, Master!"' >> /opt/watchlog.sh
        echo 'else' >> /opt/watchlog.sh
        echo 'exit 0' >> /opt/watchlog.sh
        echo 'fi' >> /opt/watchlog.sh
                  
        echo -e "\033[33m     1.4 Добавляем права запуска скрипта /opt/watchlog.sh"
        
        chmod +x /opt/watchlog.sh
              
        echo -e "\033[33m    1.5 Создаем unit для сервиса touch /etc/systemd/system/watchlog.service"
        
        touch /etc/systemd/system/watchlog.service
        chmod 664 /etc/systemd/system/watchlog.service
        
  
cat <<'EOF1' > /etc/systemd/system/watchlog.service
[Unit]
Description=My watchlog service
After=syslog.target

[Service]
User=root
Type=oneshot
EnvironmentFile=/etc/sysconfig/watchlog
ExecStart=/opt/watchlog.sh $WORD $LOG

[Install]
WantedBy=multi-user.target

EOF1
       
        echo -e "\033[33m 1.6 Запускаем  unit watchlog.service"
	
	systemctl daemon-reload
        systemctl  start watchlog.service
        
        echo -e "\033[33m     1.7 Создаем unit для таймера touch /etc/systemd/system/watchlog.timer"
        
        touch /etc/systemd/system/watchlog.timer
        echo '[Unit]' >> /etc/systemd/system/watchlog.timer
        echo 'Description=Run watchlog script every 30 second' >>  /etc/systemd/system/watchlog.timer
        echo ' ' >>  /etc/systemd/system/watchlog.timer
        echo '[Timer]' >>  /etc/systemd/system/watchlog.timer
        echo 'OnActiveSec=1sec' >> /etc/systemd/system/watchlog.timer
        echo 'OnCalendar=*:*:0/30' >>  /etc/systemd/system/watchlog.timer
        echo 'Unit=watchlog.service' >>  /etc/systemd/system/watchlog.timer
        echo ' ' >>  /etc/systemd/system/watchlog.timer
        echo '[Install]' >>  /etc/systemd/system/watchlog.timer 
        echo ' WantedBy=multi-user.target' >>  /etc/systemd/system/watchlog.timer
         
        echo -e "\033[33m     1.8 Стартуем таймер"
        
        systemctl daemon-reload
	systemctl start watchlog.timer
        
        echo -e "\033[33m     1.9 Проверяем /var/log/messages"        
        timeout 30 tail -f /var/log/messages
        echo -e "\033[33m Задание 1 выполнено."
        sleep 10
        echo -e "\033[33m 2. Задание №2: из репозитория epel установить spawn-fcgi и переписать init-скрипт на unit-файл"
        echo -e "\033[33m 2.1 Установим необходимые программы"

        yum install -y -q epel-release  
        yum install -y -q spawn-fcgi php php-cli httpd mod_fcgid
	sed -i 's/#SOCKET/SOCKET/' /etc/sysconfig/spawn-fcgi
        sed -i 's/#OPTIONS/OPTIONS/' /etc/sysconfig/spawn-fcgi

	echo "\033[33m 2.2 Создаем unit-файл для spawn-fcgi."

        touch /etc/systemd/system/spawn-fcgi.service
        chmod 664 /etc/systemd/system/spawn-fcgi.service

cat <<'EOF2' >/etc/systemd/system/spawn-fcgi.service
[Unit]
Description=Spawn-fcgi startup service by Otus
After=network.target

[Service]
Type=simple
PIDFile=/var/run/spawn-fcgi.pid
EnvironmentFile=/etc/sysconfig/spawn-fcgi
ExecStart=/usr/bin/spawn-fcgi -n $OPTIONS
KillMode=process
      
[Install]
WantedBy=multi-user.target

EOF2
        
        echo -e "\033[33m 2.3 Запускаем и проверяем юнит"
 
        systemctl daemon-reload
        systemctl enable --now spawn-fcgi.service
        systemctl status spawn-fcgi.service

        echo -e "\033[33m Задание 2 выполнено."

        sleep 10

        echo -e "\033[33m3. Задание №3 Дополнить unit-файл httpd (apache) c возможностью запустить"
        echo -e "\033[33m   несколько инстансов сервера с разными конфигурационными файлами."
        echo -e "\033[33m3.1 Установим  программы и настроим selinux."
        
        yum install -y -q policycoreutils-python
        semanage port -m -t http_port_t -p tcp 8080
        
        echo -e "\033[33m3.2 Скопируем и изменим файл сервиса httpd.service в шаблон"
        
        cp /usr/lib/systemd/system/httpd.service /etc/systemd/system/httpd@.service
        sed -i 's*EnvironmentFile=/etc/sysconfig/httpd*EnvironmentFile=/etc/sysconfig/%i*' /etc/systemd/system/httpd@.service
        
        echo -e "\033[33m3.3 Скопируем и изменим файлы настройки сервиса httpd.service"
        
        cp /etc/sysconfig/httpd /etc/sysconfig/httpd-first 
        cp /etc/sysconfig/httpd /etc/sysconfig/httpd-second
        sed -i 's*#OPTIONS=*OPTIONS=-f /etc/httpd/conf/first.conf*' /etc/sysconfig/httpd-first
        sed -i 's*#OPTIONS=*OPTIONS=-f /etc/httpd/conf/second.conf*' /etc/sysconfig/httpd-second
        
        echo -e "\033[33m3.4 Скопируем и изменим файлы настройки демона httpd"
        
        cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/first.conf
        cp /etc/httpd/conf/httpd.conf /etc/httpd/conf/second.conf
        sed -i 's/Listen 80/Listen 8080/' /etc/httpd/conf/second.conf
        echo "PidFile /var/run/httpd/httpd-first.pid" >> /etc/httpd/conf/first.conf
        echo "PidFile /var/run/httpd/httpd-second.pid" >> /etc/httpd/conf/second.conf
        
        echo -e "\033[33m 3.5 Запустим и проверим работу двух конфигураций apache на стандартном 80 порту и 8080"
        
        systemctl daemon-reload
        systemctl enable --now httpd@httpd-first.service
        systemctl enable --now httpd@httpd-second.service
        systemctl status httpd@httpd-first.service
        systemctl status httpd@httpd-second.service          
        ss -ntl
        
        echo -e "\033[33m Задание 3 выполнено\n\n"
        
        echo -e "\033[33m Спасибо за проверку!\n\n"
        sleep 10
   
    SHELL
end
