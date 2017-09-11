Vagrant.configure("2") do |config|

  # Configuring vm
  config.vm.box = "ubuntu/trusty64"

  # Configuring network db
  config.vm.define "db" do |db|
    db.vm.network "forwarded_port", guest: 5432, host: 5432
    db.vm.network "private_network", ip: "192.168.1.11"

    # Configuring provider
    db.vm.provider "virtualBox" do |vb|
      vb.name = "dj-database"
      vb.gui = false
      vb.memory = "512"
    end
    db.vm.provision "shell", inline: <<-SHELL
      # apt-get update
      apt-get update
       sudo apt-get install -y postgresql
       sudo su - postgres -c "psql -f /vagrant/db-config.sql"
       sudo su - postgres -c 'echo "host all all 192.168.1.10/24 trust" >> /etc/postgresql/9.3/main/pg_hba.conf'
       sudo su - postgres -c "echo listen_addresses=\\'*\\' >> /etc/postgresql/9.3/main/postgresql.conf"
       sudo service postgresql restart
    SHELL
  end

  # Configuring network web
  config.vm.define "web" do |web|
    web.vm.network "forwarded_port", guest: 8000, host: 8000
    web.vm.network "private_network", ip: "192.168.1.10"

    # Configuring provider
    web.vm.provider "virtualBox" do |vb|
      vb.name = "dj"
      vb.gui = false
      vb.memory = "512"
    end
    web.vm.provision "shell", inline: <<-SHELL
      # apt-get update
      sudo apt-get install -y python-pip python-dev libpq-dev postgresql postgresql-contrib
      sudo pip install django flake8 psycopg2
    SHELL
  end
end
