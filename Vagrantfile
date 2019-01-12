Vagrant.configure("2") do |config|
  
  # Set Disksize for vb
  config.disksize.size = '10GB'

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.customize ["modifyvm", :id, "--uartmode1", "disconnected"]
    vb.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
  end

  require 'time'
  offset = ((Time.zone_offset(Time.now.zone) / 60) / 60)
  timezone_suffix = offset >= 0 ? "-#{offset.to_s}" : "+#{offset.to_s}"
  timezone = 'Etc/GMT' + timezone_suffix
  config.vm.provision :shell, :inline => "sudo rm /etc/localtime && sudo ln -s /usr/share/zoneinfo/" + timezone + " /etc/localtime", run: "always"

  config.vm.provision "file", source: "app.py", destination: "~/app.py"
  config.vm.provision "file", source: "config.py", destination: "~/config.py"
  config.vm.provision "file", source: "initdb.py", destination: "~/initdb.py"

  $script = <<-SCRIPT
  sudo mkdir -p /opt/flask
  sudo mv /home/vagrant/app.py /opt/flask/
  sudo mv /home/vagrant/config.py /opt/flask/
  sudo mv /home/vagrant/initdb.py /opt/flask/
  SCRIPT
  config.vm.provision "shell", inline: $script

  config.vm.provision "file", source: "requirements3.txt", destination: "~/requirements3.txt"
  config.vm.provision "file", source: "requirements2.txt", destination: "~/requirements2.txt"

  config.vm.provision "ansible" do |ansible|
    #ansible.verbose = true
    ansible.playbook = "playbook.yml"
  end

  config.vm.define "flask1" do |flask1|
    flask1.vm.box = "ubuntu/bionic64"
  end

  config.vm.network "private_network", 
    ip: "192.168.33.10"
  config.vm.network "forwarded_port", 
    guest: 5000,
    host: 8888

  # config.vm.define "flask2" do |flask2|
  #   flask2.vm.box = "ubuntu/bionic64"
  # end

  # config.vm.define "flask3" do |flask3|
  #   flask3.vm.box = "ubuntu/bionic64"
  # end

end
