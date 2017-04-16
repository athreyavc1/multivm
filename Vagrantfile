#$chef_server_url = "https://api.chef.io/organizations/test-lab-athreya"
#$chef.validation_client_name = "test-lab-athreya-validator"
#$chef.client_key_path = "/etc/chef/client.pem"
#$chef.validation_key_path = "/home/learn/Downloads/validation.pem"

guests = {
 :ubuntu => {
   :singleton => true,
   :box => 'hashicorp/precise64',
   :public_network => { :bridge => 'wlp3s0', :ip => '192.168.1.12', :netmask => '255.255.255.0', :gw => '192.168.1.254' },
   :chef_recipes => ["motd"],
   :vb_memory => 1024
    }
}

Vagrant.configure("2") do |global_config|
  guests.each_pair do | name, options |
     global_config.vm.define name do |config|
	config.vm.box = options[:box]
	config.vm.hostname = "ubuntu"
        config.ssh.username = "vagrant"
	config.butcher.enabled = true 
	config.butcher.verify_ssl = false
        config.omnibus.chef_version = :latest 
        pub=options [:public_network]
        config.vm.network 'public_network', :bridge => pub[:bridge], :ip => pub[:ip], :netmask => pub[:netmask], :gw => pub[:gw]
	config.vm.provision :chef_client do |chef|
 	 chef.node_name = "ubuntu"
         chef.provisioning_path = "/etc/chef"
 	 chef.chef_server_url = "https://api.chef.io/organizations/test-lab-athreya"
 	 chef.validation_client_name = "test-lab-athreya-validator"
	 chef.validation_key_path = "/home/learn/Downloads/validation.pem"
         options[:chef_recipes].each do | recipe |
		chef.add_recipe recipe
	 end
	end
     end
  end
 end
	

 
