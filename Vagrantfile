# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "CentOS-6.5-x86_64-v20140504"

  # config.vm.box_check_update = false

  # config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.network "private_network", type: "dhcp"
  # config.vm.network "public_network"

  # config.ssh.forward_agent = true

  # config.vm.synced_folder "../data", "/vagrant_data"

  # config.vm.provider "virtualbox" do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end

  config.vm.provision "chef_solo" do |chef|
    chef.add_recipe "apache2"
    chef.add_recipe "php"
    chef.add_recipe "apache2::mod_php5"
    chef.add_recipe "mysql::server"
    chef.add_recipe "php::module_mysql"
    chef.json = {
      "mysql_service" => {
        "version" => "5.6"
      }
    }
  end
  config.vm.provision "shell", inline: "yum install -y php-mbstring", privileged: true

  config.vm.provision "shell", inline: "service iptables stop", privileged: true
  config.vm.provision "shell", inline: "chkconfig iptables off", privileged: true

  config.vm.provision "shell", inline: "service httpd restart", privileged: true
  config.vm.provision "shell", inline: "mkdir -p /etc/httpd/htdocs", privileged: true
  config.vm.provision "shell", inline: "echo '<h1>It works</h1><p>/etc/httpd/htdocs/index.html</p>' > /etc/httpd/htdocs/index.html", privileged: true
  config.vm.provision "shell", inline: "cp -rf /vagrant/htdocs/* /etc/httpd/htdocs/", privileged: true
  config.vm.provision "shell", inline: "rm -rf /vagrant/htdocs/*", privileged: true

end
