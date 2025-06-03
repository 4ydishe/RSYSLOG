Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.boot_timeout = 300

  # VMs with static private IP addresses.
  boxes = [
    { :name => "web",
      :ip => "192.168.56.10",
    },
    { :name => "log",
      :ip => "192.168.56.15",
    },
    { :name => "client",
      :ip => "192.168.56.20",
    }
  ]

  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network "private_network", ip: opts[:ip]

      # Setup provisioning with Ansible
      config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "playbook.yml"
        ansible.extra_vars = {
          hostname: opts[:name]
        }
      end
    end
  end
end
