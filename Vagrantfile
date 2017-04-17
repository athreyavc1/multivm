Vagrant.configure("2") do |config|

  config.vm.box = "kensykora/windows_2012_r2_standard"

  config.winrm.username = "vagrant"

  config.winrm.password = "vagrant"

  config.vm.communicator = "winrm"

  config.vm.guest = :windows


  config.vm.network :forwarded_port, guest: 3389, host: 3389, id: "rdp", auto_correct: true

  config.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true

  config.vm.provision "shell", inline: "Write-Host Hello World"

  config.vm.provision "shell", inline: "(Get-UICulture).Calendar | ConvertTo-Json"

  config.omnibus.chef_version = :latest
  config.vm.provision :chef_client do |chef|
    chef.chef_server_url = "https://api.chef.io/organizations/test-lab-athreya"
    chef.validation_key_path = "~/.chef/validation.pem"
    chef.validation_client_name = "test-lab-athreya-validator"
    chef.node_name = "server"
  end
  config.vm.provider :virtualbox do |vb|
    vb.gui = false
  end
end
