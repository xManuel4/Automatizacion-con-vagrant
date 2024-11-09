Vagrant.configure("2") do |config|
    # Configuración del primer servidor Apache (web1)
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/jammy64"
      web1.vm.network "private_network", ip: "192.168.56.10"
      web1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web1.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        echo "<h1>Apache Server 1</h1>" > /var/www/html/index.html
      SHELL
    end
  
    # Configuración del segundo servidor Apache (web2)
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/jammy64"
      web2.vm.network "private_network", ip: "192.168.56.11"
      web2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web2.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        echo "<h1>Apache Server 2</h1>" > /var/www/html/index.html
      SHELL
    end
  
    # Configuración del servidor Nginx (Load Balancer)
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/jammy64"
      nginx.vm.network "private_network", ip: "192.168.56.12"
      nginx.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      nginx.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        rm /etc/nginx/sites-enabled/default
        echo 'upstream loadbalance {
            server 192.168.56.10;
            server 192.168.56.11;
          }
  
          server {
            listen 80;
            location / {
              proxy_pass http://loadbalance;
            }
          }' > /etc/nginx/sites-available/loadbalance
  
        ln -s /etc/nginx/sites-available/loadbalance /etc/nginx/sites-enabled/loadbalance
        systemctl restart nginx
      SHELL
    end
  end