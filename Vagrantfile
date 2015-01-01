# -*- mode: ruby -*-
# vi: set ft=ruby :

boxes=[
 {:name => :nimbus, :ip => '192.168.33.100', :cpus => 2, :memory => 512},
 {:name => :supervisor1, :ip => '192.168.33.101', :cpus => 4, :memory => 1024},
 {:name => :supervisor2, :ip => '192.168.33.102', :cpus => 4, :memory => 1024},
 {:name => :zookeeper1, :ip => '192.168.33.201', :cpus => 1, :memory => 1024}
]


# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!

VAGRANTFILE_API_VERSION = "2"



Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.


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

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

  # Enable provisioning with CFEngine. CFEngine Community packages are
  # automatically installed. For example, configure the host as a
  # policy server and optionally a policy file to run:
  #
  # config.vm.provision "cfengine" do |cf|
  #   cf.am_policy_hub = true
  #   # cf.run_file = "motd.cf"
  # end
  #
  # You can also configure and bootstrap a client to an existing
  # policy server:
  #
  # config.vm.provision "cfengine" do |cf|
  #   cf.policy_server_address = "10.0.2.15"
  # end

  # Enable provisioning with Puppet stand alone.  Puppet manifests
  # are contained in a directory path relative to this Vagrantfile.
  # You will need to create the manifests directory and a manifest in
  # the file default.pp in the manifests_path directory.
  #
  # config.vm.provision "puppet" do |puppet|
  #   puppet.manifests_path = "manifests"
  #   puppet.manifest_file  = "default.pp"
  # end

  # ORGNAME in the URL and validation key.
  #
  #


 boxes.each do |opts|
   config.vm.define opts[:name] do |conf|
   	config.vm.box = "ubuntu12" 
	config.vm.box_url = "http://dl.dropbox.com/u/1537815/precise64.box"
	config.vm.host_name = "storm.%s" % opts[:name].to_s 
	config.vm.share_folder "v-data", "/vagrant_data", "./data", :transient => false 
	config.vm.customize ["modifyvm", :id, "--memory", opts[:memory]] 
	config.vm.customize ["modifyvm", :id, "--cpus", opts[:cpus] ] if opts[:cpus]	
   end	
   config.vm.provision :shell, :inline => "cp -fv /vagrant_data/hosts /etc/hosts"
   config.vm.provision :shell, :inline => "apt-get update"

   config.vm.provision :shell, :inline => "apt-get --yes --force-yes install puppet"
   

   if File.exist?("./data/jdk-6u35-linux-x64.bin") then
    	config.vm.provision :puppet do |puppet|
    	  puppet.manifests_path = "manifests"
    	  puppet.manifest_file = "jdk.pp"
   	end
   end
      
   config.vm.provision :puppet do |puppet|
    	puppet.manifests_path = "manifests"
    	puppet.manifest_file = "provisioningInit.pp"
   end
  	  
   # Ask puppet to do the provisioning now.
   config.vm.provision :shell, :inline => "puppet apply /tmp/storm-puppet/manifests/site.pp --verbose --modulepath=/tmp/storm-puppet/modules/ --debug"	
      
   

 end

end
