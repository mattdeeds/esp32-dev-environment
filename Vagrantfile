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
  config.vm.box = "ubuntu/focal64"

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
  config.vm.synced_folder ".", "/home/vagrant/host"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
    vb.memory = "2048"
  # Use 2 CPUs
    vb.cpus = 2

    # Enable all USB devices needed for development
   vb.customize ['modifyvm', :id, '--usb', 'on']
   vb.customize ['modifyvm', :id, '--usbehci', 'on']
   vb.customize ['usbfilter', 'add', '0', '--target', :id, '--name', 'Espressif USB JTAG/serial debug unit', '--vendorid', '0x303a', '--productid', '0x1001']
   vb.customize ['modifyvm', :id, '--uartmode1', 'disconnected']
  end

  
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    # apt-get upgrade -y
    apt-get -qq install -y linux-image-extra-virtual
    apt-get install -y git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
    # echo 'SUBSYSTEMS=="usb", ATTRS{idVendor}=="303a", ATTRS{idProduct}=="1001", GROUP="users", MODE="0666"' >> /etc/udev/rules.d/60-programmers.rules
    # echo 'KERNEL=="ttyUSB0", MODE="0666"' >> /etc/udev/rules.d/60-programmers.rules
    # sudo udevadm control --reload-rules
    # sudo udevadm trigger
    python3 --version
    cd /home/vagrant
    mkdir -p esp
    cd esp
    git clone --recursive https://github.com/espressif/esp-idf.git
    # git clone -b v4.4.2 --recursive https://github.com/espressif/esp-idf.git
    # chown -R vagrant:vagrant esp/
    sudo su - vagrant -c "cd /home/vagrant/esp/esp-idf && ./install.sh all && . /home/vagrant/esp/esp-idf/export.sh && exit"
    # cd /home/vagrant/esp/esp-idf
    # ./install.sh all
    # . /home/vagrant/esp/esp-idf/export.sh
    # cd /home/vagrant/
    chown -R vagrant:vagrant /home/vagrant/esp/
    echo "alias get_idf='. /home/vagrant/esp/esp-idf/export.sh'" >> /home/vagrant/.bashrc
    adduser vagrant dialout
    adduser root dialout
    usermod -a -G dialout vagrant
  SHELL
end
