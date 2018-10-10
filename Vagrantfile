# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
ETCD_COUNT   = ENV.has_key?('ETCD_COUNT') ? ENV['ETCD_COUNT'].to_i : 1
MASTER_COUNT = ENV.has_key?('MASTER_COUNT') ? ENV['MASTER_COUNT'].to_i : 1
NODE_COUNT   = ENV.has_key?('NODE_COUNT') ? ENV['NODE_COUNT'].to_i : 1
IPv6_PREFIX  = ENV.has_key?('IPV6_PREFIX') ? ENV['IPV6_PREFIX'].to_i : "fddd"
$shared_folders = { "shared/" => "/shared" }

Vagrant.configure("2") do |config|

  config.ssh.shell="bash"
  config.vm.box = "ubuntu/xenial64"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  $shared_folders.each_with_index do |(host_folder, guest_folder), index|
    config.vm.synced_folder host_folder.to_s, guest_folder.to_s, id: "shared_%02d" % index, type: "rsync"
   end

  config.vm.provider :virtualbox do |v|
    v.check_guest_additions = false
    v.functional_vboxsf     = false
    v.cpus = 2
    v.memory = 1024
    v.customize ["modifyvm", :id, "--uartmode1", "disconnected"]
  end

  (1..ETCD_COUNT).each do |i|
    config.vm.define "etcd-#{i}.#{IPv6_PREFIX}--100-#{i}.sslip.io" do |subconfig|
      subconfig.vm.hostname = "etcd-#{i}.#{IPv6_PREFIX}--100-#{i}.sslip.io"
      subconfig.vm.network :private_network,
        :ip => "#{IPv6_PREFIX}::100:#{i}",
        :name => 'vboxnet1', :adapter => 2
      subconfig.vm.provider "virtualbox" do |v|
        v.cpus = 2
        v.memory = 1024
      end
    end
  end

  (1..MASTER_COUNT).each do |i|
    config.vm.define "master-#{i}.#{IPv6_PREFIX}--200-#{i}.sslip.io" do |subconfig|
      subconfig.vm.hostname = "master-#{i}.#{IPv6_PREFIX}--200-#{i}.sslip.io"
      subconfig.vm.network :private_network,
        :ip => "#{IPv6_PREFIX}::200:#{i}",
        :name => 'vboxnet1', :adapter => 2
      subconfig.vm.provider "virtualbox" do |v|
        v.cpus = 2
        v.memory = 1024
      end
    end
  end

  (1..NODE_COUNT).each do |i|
    config.vm.define "node-#{i}.#{IPv6_PREFIX}--300-#{i}.sslip.io" do |subconfig|
      subconfig.vm.hostname = "node-#{i}.#{IPv6_PREFIX}--300-#{i}.sslip.io"
      subconfig.vm.network :private_network,
        :ip => "#{IPv6_PREFIX}::300:#{i}",
        :name => 'vboxnet1', :adapter => 2
      subconfig.vm.provider "virtualbox" do |v|
        v.cpus = 2
        v.memory = 1024
      end
    end
  end
end
