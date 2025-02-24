# you're doing.
Vagrant.configure("2") do |config|
  config.vm.define "server" do |webserver|

    webserver.vm.box = "ubuntu/trusty64"
    webserver.vm.box_check_update = false
    webserver.vm.network "forwarded_port", guest: 8080, host: 8082
    # webserver.vm.network "private_network", type: "static", ip: "192.168.33.10"
    webserver.vm.network "public_network"
    webserver.vm.synced_folder "./tomcatwebapps", "/opt/tomcat/webapps"
    webserver.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.name = "webserver-tomcat"
      vb.memory = "1024"
        # webserver.vm.provision "shell", inline: <<-SHELL
        #   apt-get update
        #   apt-get install -y apache2
        # SHELL
    end
  end
end
