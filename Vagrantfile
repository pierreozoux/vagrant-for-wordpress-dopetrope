# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version '>= 1.5.1'

Vagrant.configure('2') do |config|
  config.vm.box = 'roots/bedrock'

  config.vm.network :private_network, ip: '192.168.50.5'
  config.vm.hostname = 'dopetrope.dev'

  if !Vagrant.has_plugin? 'vagrant-hostsupdater'
    puts 'vagrant-hostsupdater missing, please install the plugin:'
    puts 'vagrant plugin install vagrant-hostsupdater'
  else
    # If you have multiple sites/hosts on a single VM
    # uncomment and add them here
    #config.hostsupdater.aliases = %w(site2.dev)
  end

  # adjust paths relative to Vagrantfile
  config.vm.synced_folder '.', '/srv/www/dopetrope.dev/current', owner: 'vagrant', group: 'www-data', mount_options: ['dmode=776', 'fmode=775']

  config.vm.provision :ansible do |ansible|
    # adjust paths relative to Vagrantfile
    ansible.playbook = './site.yml'
    ansible.groups = {
      'web' => ['default'],
      'development' => ['default']
    }
    ansible.extra_vars = {
      ansible_ssh_user: 'vagrant',
      user: 'vagrant'
    }
    ansible.sudo = true
  end

  # Fix for slow external network connections
  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
  end
end
