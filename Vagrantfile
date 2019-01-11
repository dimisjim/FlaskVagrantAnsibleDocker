Vagrant.configure("2") do |config|
  
  # Base box
  config.vm.box = "ubuntu/bionic64"
  
  # Set Disksize for vb
  config.disksize.size = '40GB'

  # Define vm name
  config.vm.define "y-dev-env-base"

  # Prepare structure
  $script = <<-SCRIPT
  sudo chmod 777 /tmp
  sudo mkdir -p /vagrant_data/assets
  sudo chmod -R 777 /vagrant_data
  SCRIPT
  config.vm.provision "shell", inline: $script
  config.vm.provision "file", source: "../../../commons/conda.runtime-vagrant-env.yml", destination: "/vagrant_data/assets/conda.runtime-vagrant-env.yml"
  config.vm.provision "file", source: "../../../commons/yields_landscape1.jpg", destination: "/vagrant_data/assets/yields_landscape1.jpg"
  config.vm.provision "file", source: "../../../commons/bootstrap-base-functions.sh", destination: "/vagrant_data/assets/bootstrap-base-functions.sh"

  require 'time'
  offset = ((Time.zone_offset(Time.now.zone) / 60) / 60)
  timezone_suffix = offset >= 0 ? "-#{offset.to_s}" : "+#{offset.to_s}"
  timezone = 'Etc/GMT' + timezone_suffix
  config.vm.provision :shell, :inline => "sudo rm /etc/localtime && sudo ln -s /usr/share/zoneinfo/" + timezone + " /etc/localtime", run: "always"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "4096"
    vb.customize ["modifyvm", :id, "--uartmode1", "disconnected"]
    vb.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
  end

  config.vm.provision "step 1", type: "shell" do |s|
    s.path = "bootstrap-base.sh"
    s.env = {"WORKER" => "vagrant"}
  end

  config.vm.provision :reload

end
