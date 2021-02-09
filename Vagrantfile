# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

 config.vm.box = "ubuntu/focal64"
 config.vm.box_version = "~> 20210207.0.0"

 config.vm.hostname = "ubuntumain01"
 config.vm.network "forwarded_port", guest: 8000, host: 8000
 config.vm.synced_folder "automation", "/home/automation"

 config.vm.provision "shell", inline: <<-SHELL
   systemctl disable apt-daily.service
   systemctl disable apt-daily.timer

   sudo ssh-keygen -t rsa -f "/root/.ssh/id_rsa" -P "" && sudo cat /root/.ssh/id_rsa.pub

   echo -e "vagrant\nvagrant" | passwd root
   echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
   sed -in 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
   service ssh restart

   sudo apt-get update
   sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common -y
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io -y
   systemctl start docker
   systemctl enable docker
   sudo curl -L "https://github.com/docker/compose/releases/download/1.28.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   sudo chmod +x /usr/local/bin/docker-compose
   sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

   sudo apt install net-tools -y

 SHELL
end
