Vagrant.configure(2) do |config|
  
  config.vm.box = "CoreOS"
  config.vm.box_download_checksum = "70d007b820eff425ebeaa1baaddd8688312243c5b8e2eebf304f3ebd11647688"
  config.vm.box_download_checksum_type = "sha256"
  config.vm.box_url = "http://stable.release.core-os.net/amd64-usr/681.0.0/coreos_production_vagrant.box"

  config.vm.synced_folder ".", "/home/core/share", id: "core", :nfs => true,  :mount_options   => ['nolock,vers=3,udp']

  config.vm.provider :virtualbox do |v|
    # On VirtualBox, we don't have guest additions or a functional vboxsf
    # in CoreOS, so tell Vagrant that so it can be smarter.
    v.check_guest_additions = false
    v.functional_vboxsf     = false
  end

  config.vm.network :private_network, ip: "172.17.8.100"

  config.vm.provision "docker" do |d|

    d.pull_images "google/nodejs-runtime"

    d.build_image "/home/core/share/", args: '-t "app"'

    d.run "app", args: '-p "8080:8080"'

  end

  config.vm.network "forwarded_port", guest: 8080, host: 58080

end
