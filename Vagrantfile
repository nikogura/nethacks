# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
default_box = "realreadme/amazon2016.09"

virtual_machines = [
    {
        name: "muaha",
        forwarded_ports: [
        ],
        files: [
            {:src =>  "bin/muahaha", :dst => "/tmp/muahaha"}
        ],
        provisioners: [
        ]
    },
]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  virtual_machines.each do |machine|
    config.vm.box = machine[:box] ? machine[:box] : default_box
    config.vm.hostname = machine[:name]

    config.vm.provider 'virtualbox' do |p|
      p.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      p.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

    if machine[:forwarded_ports]
      machine[:forwarded_ports].each do |pfwd|
        config.vm.network "forwarded_port", guest: pfwd[:guest], host: pfwd[:host]
      end
    end

    if machine[:files]
      machine[:files].each do |file|
        config.vm.provision "file", source: file[:src], destination: file[:dst]
      end
    end

    config.vm.provision "shell", inline: "cp /tmp/muahaha /usr/local/bin/muahaha"

    machine[:provisioners].each do |script|
      config.vm.provision "shell", path: script
    end
  end
end
