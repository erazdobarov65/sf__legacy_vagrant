Vagrant.configure("2") do |config|
  
  config.vm.box = "ubuntu/bionic64" # Используем  18.04 в кач-ве образа
  config.vm.network "private_network", ip: "192.168.61.10" # Назначаем IP для ВМ
  
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048" # Выделяем 2ГБ RAM для ВМ
    vb.cpus = 2 # Используем  18.04 в кач-ве образа
  end

   # Скрипт установки PostgreSQL 8.4
  config.vm.provision "shell", inline: <<-SHELL
    # Добавляем репозиторий  PostgreSQL 8.4 и импортируем ключ
    echo "deb http://apt.postgresql.org/pub/repos/apt/ bionic-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
    
    # Апдейтип список пактов и устанавливаем PostgreSQL 8.4
    sudo apt-get update
    sudo apt-get install -y postgresql-8.4
    
    # Вносим изменения в конфигурацию PostgreSQL - разрешаем подключение с любого хоста
    sudo sed -i "s/#listen_addresses = 'localhost'/listen_addresses = '*'/g" /etc/postgresql/8.4/main/postgresql.conf
    echo "host    all             all             0.0.0.0/0               md5" | sudo tee -a /etc/postgresql/8.4/main/pg_hba.conf
    
    # Рестартуем службу PostgreSQL
    sudo service postgresql restart
  SHELL
  
  # Настраиваем SSH форвардинг для подключения к ВМ
  config.ssh.forward_agent = true
end
