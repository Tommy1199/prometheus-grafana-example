$script = <<SCRIPT
sudo yum install -y epel-release
sudo yum install -y ansible
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7.4"
  config.vm.network "forwarded_port", guest: 9090, host: 9090, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 9100, host: 9100, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 3000, host: 3000, host_ip: "127.0.0.1"

  config.vm.provision "shell", inline: $script
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
