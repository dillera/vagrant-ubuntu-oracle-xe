# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "precise64"
  #config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.box_url = "http://nitron-vagrant.s3-website-us-east-1.amazonaws.com/vagrant_ubuntu_12.04.3_amd64_vmware.box"
  config.vm.network :public_network
  config.ssh.forward_agent = true

  # Forward Oracle port
  config.vm.network :forwarded_port, guest: 1521, host: 1521

  # Provider-specific configuration so you can fine-tune various backing
  # providers for Vagrant. These expose provider-specific options.
  config.vm.provider "vmware_fusion" do |v|
    v.vmx["memsize"]     = "1024"
    v.vmx["numvcpus"]    = "1"
    v.vmx["displayName"] = "oraclePE"
    v.vmx["annotation"]  = "Ubuntu with Oracle PE"
  end

  config.vm.provision :shell, :inline => "echo \"America/New_York\" | sudo tee /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata"

  config.vm.provision :shell, :path => "puppet_bootstrap_ubuntu.sh"


  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "manifests"
    puppet.module_path    = "modules"
    puppet.manifest_file  = "base.pp"
    puppet.options        = "--verbose --trace"
  end
end
