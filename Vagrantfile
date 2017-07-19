require "fileutils";
require_relative "./vagrant.rb"

$update_channel = "alpha"
$image_version = "current"
$version = 4;

CONFIG_FILE = File.join(File.dirname(__FILE__), "user-data")

Vagrant.configure("2") do |config|
    if Vagrant.has_plugin?("vagrant-vbguest") then
      config.vbguest.auto_update = false
    end

    config.ssh.insert_key = false
    config.ssh.forward_agent = true

    config.vm.box = "coreos-%s" % $update_channel 
    config.vm.box_url = "https://storage.googleapis.com/%s.release.core-os.net/amd64-usr/%s/coreos_production_vagrant.json" % [$update_channel, $image_version]

    config.vm.provider :virtualbox do |vbox|
        vbox.check_guest_additions = false
        vbox.functional_vboxsf     = false
        vbox.cpus = 2
        vbox.memory = 1024
        vbox.name = "home-server-%s" % $version
    end

    config.vm.network "public_network", bridge: "en0: Ethernet", :mac => "000011223344"

    config.vm.network "private_network", ip: "172.28.128.5", :mac => "080027986b1b"

    # Default Synced folder via rsync
    config.vm.synced_folder ".", "/home/core/containers", type: "rsync"

    config.vm.provision :file, :source => "#{CONFIG_FILE}", :destination => "/tmp/vagrantfile-user-data"
    config.vm.provision :shell, :inline => "mv /tmp/vagrantfile-user-data /var/lib/coreos-vagrant/", :privileged => true

    config.vm.provision "shell", inline: "mkdir -p /home/core/vagrant"
    config.vm.provision :docker
    config.vm.provision :docker_compose, 
        yml: "/home/core/containers/docker-compose.yml", 
        compose_version: "1.14.0",
        executable_install_path: "/home/core/vagrant/docker-compose",
        executable_symlink_path: "/home/core/docker-compose",
        run: "always"
end
