# -*- mode: ruby -*-
# vi: set ft=ruby :
boxes = [
  { :name => :nimbus, :ip => '192.168.33.100', :cpus =>2, :memory => 512 },
  { :name => :supervisor1, :ip => '192.168.33.101', :cpus =>4, :memory => 1024 },
  { :name => :supervisor2, :ip => '192.168.33.102', :cpus =>4, :memory => 1024 },
  { :name => :zookeeper1, :ip => '192.168.33.201', :cpus =>1, :memory => 512 }
]

boxes.each do |opts|
    config.vm.define opts[:name] do |config|
    config.vm.box = "ubuntu12"
    config.vm.box_url = 
        "http://dl.dropbox.com/u/1537815/precise64.box"
         config.vm.network :hostonly, opts[:ip]
    config.vm.host_name = "storm.%s" % opts[:name].to_s
    config.vm.share_folder "v-data", "/vagrant_data", "./data", :transient => false
    config.vm.customize ["modifyvm", :id, "--memory", opts[:memory]]
    config.vm.customize ["modifyvm", :id, "--cpus", opts[:cpus] ] if opts[:cpus]

config.vm.provision :shell, :inline => "cp -fv /vagrant_data/hosts /etc/hosts"
      
    config.vm.provision :shell, :inline => "apt-get update"
      # Check if the jdk has been provided
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
end
