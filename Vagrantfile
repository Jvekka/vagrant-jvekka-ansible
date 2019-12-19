# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

# Scripts

$ansibleWinrmConf = <<-SCRIPT
$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\\ConfigureRemotingForAnsible.ps1"
(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
powershell.exe -ExecutionPolicy ByPass -File $file
SCRIPT

# Vagrant itself
Vagrant.configure("2") do |config|
  config.vm.define "control" do |ctl|
    ctl.vm.box = "ubuntu/bionic64"
    # ctl.vm.hostname = "ubuntu-control"
    ctl.vm.network "private_network", ip: "192.168.56.2"
    ctl.vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.gui = false
      vb.name = 'control'
    end
    # Password authentication is disabled
    #   ctl.ssh.insert_key = false
    #   ctl.ssh.username = "vagrant"
    #   ctl.ssh.password = "vagrant"
    ctl.vm.provision "shell", inline: "sudo apt-get update"
    ctl.vm.provision "shell", inline: "sudo apt-get upgrade -y"
    ctl.vm.provision "shell", inline: "sudo apt-add-repository ppa:ansible/ansible -y"
    ctl.vm.provision "shell", inline: "sudo apt-get update"
    ctl.vm.provision "shell", inline: "sudo apt-get install ansible -y"
    ctl.vm.provision "shell", inline: "sudo apt-get install python -y"
    ctl.vm.provision "shell", inline: "sudo apt-get install python-pip -y"
    ctl.vm.provision "shell", inline: "sudo pip install pywinrm setuptools"
    # https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html
    ctl.vm.synced_folder "ansible", "/home/vagrant/ansible",
      owner: "vagrant",
      mount_options: ["dmode=775,fmode=600"]
    # sudo nano /etc/default/grub
    # GRUB_GFXPAYLOAD_LINUX=1152x864x24
    # sudo update-grub
    # sudo shutdown -h now
  end

  config.vm.define "windows-webserver01" do |web01|
    web01.vm.box = "jptoto/Windows2012R2"
    # web01.vm.hostname = "windows-webserver01"
    web01.vm.communicator = "winrm"
    web01.winrm.username = "vagrant"
    web01.winrm.password = "vagrant"
    web01.vm.network "private_network", ip: "192.168.56.3"
    web01.vm.provider "virtualbox" do |vb|
      vb.memory = 4096
      vb.cpus = 2
      vb.gui = false
      vb.name = 'windows-webserver01'
    end
    # https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#winrm-setup
    web01.vm.provision "shell", inline: $ansibleWinrmConf
  end

  config.vm.define "windows-webserver02" do |web02|
    web02.vm.box = "jptoto/Windows2012R2"
    # web02.vm.hostname = "windows-webserver02"
    web02.vm.communicator = "winrm"
    web02.winrm.username = "vagrant"
    web02.winrm.password = "vagrant"
    web02.vm.network "private_network", ip: "192.168.56.4"
    web02.vm.provider "virtualbox" do |vb|
      vb.memory = 4096
      vb.cpus = 2
      vb.gui = false
      vb.name = 'windows-webserver02'
    end
    # https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#winrm-setup
    web02.vm.provision "shell", inline: $ansibleWinrmConf
  end

  config.vm.define "windows-dbserver01" do |db01|
    db01.vm.box = "jptoto/Windows2012R2"
    # web02.vm.hostname = "windows-webserver02"
    db01.vm.communicator = "winrm"
    db01.winrm.username = "vagrant"
    db01.winrm.password = "vagrant"
    db01.vm.network "private_network", ip: "192.168.56.5"
    db01.vm.provider "virtualbox" do |vb|
      vb.memory = 4096
      vb.cpus = 2
      vb.gui = false
      vb.name = 'windows-dbserver01'
    end
    # https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html#winrm-setup
    db01.vm.provision "shell", inline: $ansibleWinrmConf
  end

end
