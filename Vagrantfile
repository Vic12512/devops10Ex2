Vagrant.configure("2") do |config|

    # Máquina 1: Apache 1
    config.vm.define "apache1" do |apache1|
      apache1.vm.box = "ubuntu/bionic64"
      apache1.vm.network "private_network", ip: "192.168.50.4"
      apache1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache1.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
      SHELL
      apache1.vm.synced_folder "./src", "/var/www/html"
    end
  
    # Máquina 2: Apache 2
    config.vm.define "apache2" do |apache2|
      apache2.vm.box = "ubuntu/bionic64"
      apache2.vm.network "private_network", ip: "192.168.50.5"
      apache2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      apache2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
      SHELL
      apache2.vm.synced_folder "./src", "/var/www/html"
    end
  
    # Máquina 3: NGINX (balanceador de carga)
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.network "private_network", ip: "192.168.50.6"
      nginx.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      nginx.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y nginx
        sudo tee /etc/nginx/conf.d/load_balancer.conf > /dev/null <<EOL
        upstream backend {
            server 192.168.50.4;
            server 192.168.50.5;
        }
        server {
            listen 80;
            location / {
                proxy_pass http://backend;
            }
        }
        EOL
        sudo systemctl restart nginx
      SHELL
    end
  
  end
  
  