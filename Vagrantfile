# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

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
  config.vm.synced_folder "data", "/home/vagrant/data/"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    cd $HOME

    sudo apt-get upgrade -y
    sudo apt-get update -y

    sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm tree htop
    sudo apt-get install -y git

    # INSTALL PYENV WITH MOST RECENT PYTHON INTERPRETERS FOR 2 AND 3
    git clone https://github.com/yyuu/pyenv.git ${HOME}/.pyenv
    echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ${HOME}/.bashrc
    echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ${HOME}/.bashrc
    echo 'eval "$(pyenv init -)"' >> ${HOME}/.bashrc

    export PYENV_ROOT="$HOME/.pyenv"
    export PATH="$PYENV_ROOT/bin:$PATH"
    eval "$(pyenv init -)"

    pyenv install 2.7.10
    pyenv install 3.4.3

    pyenv global 2.7.10 3.4.3

    # COMPILE AND INSTALL NODE.JS IN USERSPACE
    wget -q http://nodejs.org/dist/v0.12.7/node-v0.12.7.tar.gz
    tar -zxf node-v0.12.7.tar.gz
    rm node-v0.12.7.tar.gz
    mkdir bin
    cd node-*
    ./configure  --prefix=${HOME}/bin/nodejs
    make
    make install
    cd ..
    rm -r node*
    echo 'export PATH=${HOME}/bin/nodejs/bin:$PATH' >> ${HOME}/.bashrc
    echo 'export NODE_PATH=${HOME}/bin/nodejs/lib/node_modules' >> ${HOME}/.bashrc
    export PATH=${HOME}/bin/nodejs/bin:$PATH
    export NODE_PATH=${HOME}/bin/nodejs/lib/node_modules

    # INSTALL BOWER
    bash -c "npm install -g bower grunt-cli" # Should not be necessary, use package.json instead

    # SETTING HOST DETAILS
    sed -i "s/#force_color_prompt/force_color_prompt/" ${HOME}/.bashrc
    sudo su -c "echo 'dev' > /etc/hostname"
    sudo service hostname restart

    # INSTALL GIT PROMPT
    wget -q -O "${HOME}/.git-prompt.sh" https://raw.githubusercontent.com/git/git/master/contrib/completion/git-prompt.sh
    chmod +x ${HOME}/.git-prompt.sh
    echo "source ${HOME}/.git-prompt.sh" >> ${HOME}/.bashrc
    sed -r -i "s/PS1=(['\\"])/PS1=\\1\\$(__git_ps1 \\"(%s)\\")/g" ${HOME}/.bashrc
    echo "export GIT_PS1_SHOWDIRTYSTATE=1" >> ${HOME}/.bashrc
    echo "export GIT_PS1_SHOWSTASHSTATE=1" >> ${HOME}/.bashrc
    echo "export GIT_PS1_SHOWUNTRACKEDFILES=1" >> ${HOME}/.bashrc
    echo "export GIT_PS1_SHOWUPSTREAM=\"auto\"" >> ${HOME}/.bashrc

  SHELL
end