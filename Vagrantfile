#$chef_server_url = "https://api.chef.io/organizations/test-lab-athreya"
#$chef.validation_client_name = "test-lab-athreya-validator"
#$chef.client_key_path = "/etc/chef/client.pem"
#$chef.validation_key_path = "/home/learn/Downloads/validation.pem"
$env_domain                  = 'homelab.com'

guests = {
 :ubuntu => {
   :singleton => true,
   :box => 'hashicorp/precise64',
   :public_network => { :bridge => 'wlp3s0', :ip => '192.168.1.12', :netmask => '255.255.255.0', :gw => '192.168.1.254' },
   :chef_recipes => ["motd"],
   :vb_memory => 1024
    },
 :win => {
   :singleton => true,
   :box => 'kensykora/windows_2012_r2_standard',
   :public_network => { :bridge => 'wlp3s0', :ip => '192.168.1.14', :netmask => '255.255.255.0', :gw => '192.168.1.254' },
   :chef_recipes => ["chocolatey"],
   :vb_memory => 4096
   }
}

Vagrant.configure("2") do |global_config|
  guests.each_pair do | name, options |
    names = [name]
    names.each do |name|
     global_config.vm.define name do |config|
      guest_is_windows = options[:box].include? 'windows'
	config.vm.hostname = "#{name}.homelab.com"
        config.vm.box = options[:box]
	config.butcher.enabled = true
	config.butcher.verify_ssl = false
        config.omnibus.chef_version = :latest
        pub=options [:public_network]
        config.vm.network 'public_network', :bridge => pub[:bridge], :ip => pub[:ip], :netmask => pub[:netmask], :gw => pub[:gw]
        if guest_is_windows
         config.winrm.username = "vagrant"
  	 config.winrm.password = "vagrant"
  	 config.vm.communicator = "winrm"
  	 config.vm.guest = :windows
  	 config.vm.network :forwarded_port, guest: 3389, host: 3389, id: "rdp", auto_correct: true
  	 config.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true
     config.vm.hostname = "#{name}"
     config.vm.provision "shell", inline: "(Get-WmiObject Win32_ComputerSystem).Rename(#{name}.homelab.com)"
        else
         config.ssh.username = "vagrant"
         config.vm.hostname = "#{name}.homelab.com"
        end

	config.vm.provision :chef_client do |chef|
 	 chef.node_name = "#{name}"
         if guest_is_windows
		chef.provisioning_path = 'c:\\chef'
         else
         	chef.provisioning_path = "/etc/chef"
         end
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
end
