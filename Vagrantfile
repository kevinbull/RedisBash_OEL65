# -*- mode: ruby -*-
# vi: set ft=ruby :

# Config Github Settings
github_username = "kevinbull"
github_repo     = "RedisBash_OEL65"
github_branch   = "master"
github_url      = "https://raw.githubusercontent.com/#{github_username}/#{github_repo}/#{github_branch}"

# Server Configuration
server_domain   = "clearc2.com"
hostname        = "redis1.$server_domain"
virtualBoxName  = "Redis1"

# Set a local private network IP address.
# See http://en.wikipedia.org/wiki/Private_network for explanation
# You can use the following IP ranges:
#   10.0.0.1    - 10.255.255.254
#   172.16.0.1  - 172.31.255.254
#   192.168.0.1 - 192.168.255.254
server_ip             = "192.168.56.190"
server_memory         = "1024" # MB
server_swap           = "2048" # Options: false | int (MB) - Guideline: Between one or two times the server_memory

# Refer to /usr/share/zoneinfo for possible zone files
# UTC        for Universal Coordinated Time
# EST        for Eastern Standard Time
# US/Central for American Central
# US/Eastern for American Eastern
# America/Chicago
server_timezone  = "US/Central"


Vagrant.configure("2") do |config|

  # Set server to Oracle Enterprise Linux 6 update 5
  config.vm.box = "stoilis/oel65-64"

  # Create a hostname, don't forget to put it to the `hosts` file
  # This will point to the server's default virtual host
  # TO DO: Make this work with virtualhost along-side xip.io URL
  config.vm.hostname = hostname

  # Create a static IP
  config.vm.network :private_network, ip: server_ip
  #config.vm.network :public_network, ip: server_ip

  # If using VirtualBox
  config.vm.provider :virtualbox do |vb|

    vb.name = virtualBoxName

    # Set server memory
    vb.customize ["modifyvm", :id, "--memory", server_memory]

    # Set the timesync threshold to 10 seconds, instead of the default 20 minutes.
    # If the clock gets more than 15 minutes out of sync (due to your laptop going
    # to sleep for instance, then some 3rd party services will reject requests.
    vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]

  end

  ####
  # Base Items
  ##########

  # Provision Base Packages
  config.vm.provision "shell", path: "#{github_url}/scripts/base.sh", args: [github_url, server_swap, server_domain]


  ####
  # Web Servers
  ##########
  # Provision Nginx Base
  # config.vm.provision "shell", path: "#{github_url}/scripts/nginx.sh", args: [server_ip, public_folder, hostname, github_url]

  ####
  # In-Memory Stores
  ##########

  # Provision Redis (without journaling and persistence)
  # config.vm.provision "shell", path: "#{github_url}/scripts/redis.sh"

  # Provision Redis (with journaling and persistence)
  # config.vm.provision "shell", path: "#{github_url}/scripts/redis.sh", args: "persistent"
  # NOTE: It is safe to run this to add persistence even if originally provisioned without persistence


  ####
  # Utility (queue)
  ##########
  # Install Monit
  # config.vm.provision "shell", path: "#{github_url}/scripts/supervisord.sh"

  ####
  # Local Scripts
  # Any local scripts you may want to run post-provisioning.
  # Add these to the same directory as the Vagrantfile.
  ##########
  # config.vm.provision "shell", path: "./local-script.sh"

end
