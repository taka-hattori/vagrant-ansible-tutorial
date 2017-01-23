require 'fileutils'

# vm settings
conf ={
  'manager' => {
    'name' => 'manager',
    'ip' => '192.168.33.10',
    'ssh_port' => '2201'
  },
  'server' => {
    'name' => 'server',
    'ip' => '192.168.33.20',
    'ssh_port' => '2202'
  }
}

# Output inventory file for ansible
HOSTS_DIR="ansible/hosts/"
File.open(HOSTS_DIR + conf['manager']['name'],"w") do |file|
  file.puts( "[" + conf['manager']['name'] + "]" )
  file.puts( "127.0.0.1:" + conf['manager']['ssh_port'] )
end
File.open(HOSTS_DIR + conf['server']['name'],"w") do |file|
  file.puts( "[" + conf['server']['name'] + "]" )
  file.puts( "127.0.0.1:" + conf['server']['ssh_port'] )
end

# Output vars for manager's ~/.ssh/config for ansible
File.open("ansible/roles/manager_generate_ssh_key/vars/main.yml","w") do |file|
  file.puts( "---" )
  file.puts( "server_ip: " + conf['server']['ip'] )
  file.puts( "rsa_path: ~/.ssh/ansible_rsa" )
end

# Vagrant settings
VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "centos7.1"
  config.vm.box_url = "https://github.com/CommanderK5/packer-centos-template/releases/download/0.7.1/vagrant-centos-7.1.box"

  config.vm.define :"manager" do | manager |
    manager.vm.hostname =conf['manager']['name']
    manager.vm.network :forwarded_port, host:conf['manager']['ssh_port'] ,guest:22 ,id: "ssh"
    manager.vm.network :private_network, ip: conf['manager']['ip'], virtualbox__intnet: "intnet"
    manager.vm.provision "ansible" do | ansible |
      ansible.playbook = "ansible/manager_init.yml"
      ansible.inventory_path = HOSTS_DIR + conf['manager']['name']
      ansible.verbose = "v"
    end
  end

  config.vm.define :'server' do | server |
    server.vm.hostname = conf['server']['name']
    server.vm.network :forwarded_port, host:conf['server']['ssh_port'] ,guest:22 ,id: "ssh"
    server.vm.network :private_network, ip: conf['server']['ip'], virtualbox__intnet: "intnet"
    server.vm.provision "ansible" do | ansible |
      ansible.playbook = "ansible/server_init.yml"
      ansible.inventory_path = HOSTS_DIR + conf['server']['name']
      ansible.verbose = "v"
    end
  end

end
