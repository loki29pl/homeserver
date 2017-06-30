$update_channel = "alpha"
$image_version = "current"
$version = 2;

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    config.ssh.forward_agent = true

    config.vm.box = "home-server-%s" % $version
    config.vm.box_url = "https://storage.googleapis.com/%s.release.core-os.net/amd64-usr/%s/coreos_production_vagrant.json" % [$update_channel, $image_version]

    config.vm.provider :virtualbox do |vbox|
        vbox.check_guest_additions = false
        vbox.functional_vboxsf     = false
        vbox.cpus = 2
        vbox.memory = 1024
    end

    config.vm.synced_folder ".", "/home/core/containers"

    config.vm.network "public_network", bridge: "en0: Ethernet", :mac => "000011223344"

    config.vm.provision "shell", inline: "mkdir /home/core/vagrant"

    config.vm.provision :docker
    config.vm.provision :docker_compose, 
        yml: "/home/core/containers/docker-compose.yml", 
        compose_version: "1.14.0",
        executable_install_path: "/home/core/vagrant",
        executable_symlink_path: "/home/core/docker-compose",
        run: "always"
end