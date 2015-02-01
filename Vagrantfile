# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.require_version ">= 1.7.2"

#Require Plugins
unless Vagrant.has_plugin?("vagrant-vbguest")
  raise 'vagrant-vbguest is not installed!'
end
unless Vagrant.has_plugin?("vagrant-hostsupdater")
  raise 'vagrant-hostsupdater is not installed!'
end
unless Vagrant.has_plugin?("vagrant-berkshelf")
  raise 'vagrant-berkshelf is not installed!'
end
#Recommend Plugins
unless Vagrant.has_plugin?("vagrant-cachier")
  puts 'vagrant-cachier is not installed.'
end

Vagrant.configure(2) do |config|
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false #現在何故かVirtualBox Gustをアップデートすると共有フォルダ設定が壊れるので一度停めてる
  end

  if Vagrant.has_plugin?("vagrant-hostsupdater")
    config.hostsupdater.remove_on_suspend = true
  end
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
    config.cache.synced_folder_opts = {
      type: :nfs,
      # The nolock option can be useful for an NFSv3 client that wants to avoid the
      # NLM sideband protocol. Without this option, apt-get might hang if it tries
      # to lock files needed for /var/cache/* operations. All of this can be avoided
      # by using NFSv4 everywhere. Please note that the tcp option is not the default.
#            mount_options: ['rw', 'vers=3', 'tcp', 'nolock']
      }
  end
  if Vagrant.has_plugin?("vagrant-berkshelf")
    config.berkshelf.enabled = true #Vagrant Bekshelfを利用する
  end
  nodes={
    'knife' => {
    }
  }

  nodes.each do |machine_name,node_info|
    config.vm.define machine_name do |node|
      node.vm.box = "ubuntu/precise64"
      node.vm.hostname = 'knife.example.com'
      node.vm.network :private_network, ip: "192.168.10.10" #IP設定
      node.vm.synced_folder "./", "/vagrant", disabled: true #デフォルトの共有フォルダ設定を止める
      #VirtualBox設定
      node.vm.provider "virtualbox" do |vb|
        #vb.gui = true # Display the VirtualBox GUI when booting the machine
        # Customize the amount of memory on the VM:
        vb.cpus = "1"
        vb.memory = "512"
        vb.customize ["modifyvm", :id, "--cpuexecutioncap", "80"]
      end
    end
  end
end
