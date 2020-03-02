Vagrant.configure(2) do |config|
  define_vm = ->(name, box, cpus, memory, vmip) {
    config.vm.define name do |instance|
      instance.vm.box      = box
      #instance.vm.hostname = name
      instance.vm.network 'private_network', ip: vmip
      if (name == 'vm-5') then
          instance.vm.network :forwarded_port, host: 9500, guest: 9021
      elsif (name == 'vm-1') then
        instance.vm.network :forwarded_port, host: 8081, guest: 8080
      elsif (name == 'vm-2') then
        instance.vm.network :forwarded_port, host: 8082, guest: 8080
      elsif (name == 'vm-3') then
        instance.vm.network :forwarded_port, host: 8083, guest: 8080
      end
      instance.vm.provider :virtualbox do |i|
        i.name   = name
        i.cpus = cpus
        i.memory = memory
      end
      instance.vm.synced_folder "./init", "/opt/init"
      if (name == 'vm-master') then
        instance.vm.synced_folder "/opt/data/vm-master", "/opt/data"
      else
        instance.vm.synced_folder "/opt/data/vm-master/cp-rpm", "/opt/package"
        instance.vm.synced_folder "/opt/data/" + name + "/", "/opt/data"
      end
    end
  }

  define_vm.call 'vm-master',  'bento/centos-7', 2, 2048, '192.168.10.1'
  define_vm.call 'vm-1', 'bento/centos-7', 2, 2048, '192.168.10.2'
  define_vm.call 'vm-2', 'bento/centos-7', 2, 2048, '192.168.10.3'
  define_vm.call 'vm-3', 'bento/centos-7', 2, 2048, '192.168.10.4'
  define_vm.call 'vm-4', 'bento/centos-7', 1, 1024, '192.168.10.5'
  define_vm.call 'vm-5', 'bento/centos-7', 1, 1024, '192.168.10.6'

  config.ansible.groups = {
    'cluster:children' => ['master', 'slaves'],
    'slaves'           => ['vm-1', 'vm-2', 'vm-3', 'vm-4', 'vm-5'],
  }
end
