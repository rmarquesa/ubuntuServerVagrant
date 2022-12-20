Vagrant.configure("2") do |config|

    config.vm.define "ubuntu-server" do |config|
        config.ssh.insert_key = false

        config.vm.box = "ubuntu/jammy64"
        config.vm.box_version = "20220928.0.0"
        config.vm.hostname = "ubuntu-server"
        config.vm.network "public_network", ip: "192.168.1.101", bridge: "Intel(R) Wi-Fi 6 AX201 160MHz"
        config.vm.synced_folder "shared", "/home/vagrant/shared"
        
        config.vm.provider "virtualbox" do |v|
            v.name = "ubuntu-server"
            v.customize ["modifyvm", :id, "--memory", "8192"]
            v.customize ["modifyvm", :id, "--cpus", "4"]
        end

        # MY KEYS
        config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
        config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
        config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "/home/vagrant/.ssh/authorized_keys"

        # STANDARD
        config.vm.provision "shell", inline: <<-SCRIPT
            echo "Update, upgrade and install tools"
            apt update
            apt -y upgrade
            apt -y install sshpass net-tools locate wget git
        SCRIPT

        # DOCKER
        config.vm.provision "shell", inline: <<-SCRIPT
            echo "Install Docker"
            curl -fsSL https://get.docker.com | bash
            usermod -aG docker vagrant
            mkdir -p /home/vagrant/.docker
            chmod g+rwx /home/vagrant/.docker -R
        SCRIPT

        # HELM
        config.vm.provision "shell", inline: <<-SCRIPT
            echo "Install Helm"
            curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
        SCRIPT

        # KUBECTL 
        config.vm.provision "shell", inline: <<-SCRIPT
            echo "Install kubectl"
            apt -y install ca-certificates apt-transport-https
            curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
            echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
            apt update
            apt -y install kubectl
        SCRIPT

        # AUTOREMOVE
        config.vm.provision "shell", inline: "echo autoremove; apt -y autoremove"
    end
end