
VAGRANT_IP="10.0.0.9"
Vagrant.configure("2") do |config|
  config.vm.box= "ubuntu/trusty64"
  config.vm.network :private_network, ip: VAGRANT_IP
  
  config.vm.provision "ansible_local" do |ansible|
    ansible.install = true
    ansible.playbook = "provisioners/deployment.yml"
    
  end

  config.vm.provision "shell", inline: "printf 'localhost\n' | sudo tee /etc/ansible/hosts > /dev/null"

  config.vm.provider "virtaulbox" do |vb|
    vb.memory = "2048"
  end
end