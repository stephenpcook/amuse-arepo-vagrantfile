# -*- mode: ruby -*-
# vi: set ft=ruby :

$update_apache2 = <<-SCRIPT
  apt-get update
  apt-get install -y apache2
SCRIPT

$amuse_libs = <<-SCRIPT
  sudo apt-get install -y build-essential gfortran python3-dev \
    libopenmpi-dev openmpi-bin \
    libgsl-dev cmake libfftw3-3 libfftw3-dev \
    libgmp3-dev libmpfr6 libmpfr-dev \
    libhdf5-serial-dev hdf5-tools \
    libblas-dev liblapack-dev \
    python3.7-dev python3.7-venv \
    python3-venv python3-pip \
    git
SCRIPT

$install_perf = <<-SCRIPT
  sudo apt-get install -y linux-tools-common \
    linux-tools-generic \
    linux-tools-`uname -r`
SCRIPT

$setup_venv = <<-SCRIPT
  python3.7 -m venv venv
  . venv/bin/activate
  python -m pip install --upgrade pip
  pip install numpy docutils mpi4py h5py wheel
  pip install scipy astropy jupyter pandas seaborn matplotlib sphinx
  deactivate
SCRIPT

$install_amuse = <<-SCRIPT
  git clone https://github.com/amusecode/amuse.git
  . venv/bin/activate
  cd amuse
  pip install -e .
  python setup.py develop_build
  cd ..
  deactivate
SCRIPT

$add_exeter_mirror = <<-SCRIPT
  cd amuse
  git remote add exeter https://github.com/UniExeterRSE/amuse.git
  git fetch exeter
  cd ..
SCRIPT

$install_arepo = <<-SCRIPT
  git clone https://gitlab.mpcdf.mpg.de/vrs/arepo.git
  . venv/bin/activate
  cd arepo
  export SYSTYPE=Ubuntu
  cp /vagrant/Config.sh ~/arepo/Config.sh
  make
  cd ..
  deactivate
SCRIPT

$set_environment_variables = <<SCRIPT
tee "/etc/profile.d/myvars.sh" > "/dev/null" <<EOF
export OMPI_MCA_rmaps_base_oversubscribe=yes
export SYSTYPE=Ubuntu
EOF
SCRIPT

$setup_aliases_config = <<SCRIPT
if [ -e /vagrant/.bash_aliases ]
then
    cp /vagrant/.bash_aliases ~/.bash_aliases
fi
if [ -e /vagrant/.gitconfig.aliases ]
then
    cp /vagrant/.gitconfig.aliases ~/.gitconfig.aliases
    git config --global include.path "~/.gitconfig.aliases"
fi
SCRIPT

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
  config.vm.box = "ubuntu/bionic64"


  # Change the default port used for ssh connections.
  # config.vm.network "forwarded_port", guest: 22, host: 2200, id: 'ssh'

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

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true
  
    # Customize the amount of memory on the VM:
    vb.memory = "5120"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: $update_apache2

  config.vm.provision "shell", inline: $amuse_libs

  config.vm.provision "shell", inline: $install_perf

  config.vm.provision "shell", inline: $setup_aliases_config, privileged: false

  config.vm.provision "shell", inline: $setup_venv, privileged: false

  config.vm.provision "shell", inline: $install_amuse, privileged: false

  config.vm.provision "shell", inline: $add_exeter_mirror, privileged: false

  config.vm.provision "shell", inline: $install_arepo, privileged: false

  config.vm.provision "shell", inline: $set_environment_variables, run: "always"
end
