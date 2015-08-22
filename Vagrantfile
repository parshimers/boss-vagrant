# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.provider "virtualbox" do |v|
        v.memory =2048
        v.cpus = 2
    end

    $asterix_inflate = <<-END
        unzip -u asterix-installer*
        cd asterix/
        unzip -u asterix-server*.zip
        cd bin/
    END

    $asterix_start = <<-END
        cd asterix/bin/
        nohup /home/vagrant/asterix/bin/asterixcc -cluster-net-ip-address 127.0.1.1 -client-net-ip-address 10.42.0.2  0<&- &>/home/vagrant/cc.log &
        sleep 5
        nohup /home/vagrant/asterix/bin/asterixnc -node-id nc1 -cc-host 127.0.1.1 -iodevices /tmp -cluster-net-ip-address 127.0.1.1 -data-ip-address 127.0.1.1 -result-ip-address 127.0.1.1 0<&- &>/home/vagrant/nc1.log &
        nohup /home/vagrant/asterix/bin/asterixnc -node-id nc2 -cc-host 127.0.1.1 -iodevices /tmp -cluster-net-ip-address 127.0.1.1 -data-ip-address 127.0.0.1 -result-ip-address 127.0.1.1 0<&- &>/home/vagrant/nc2.log &
    END

    config.vm.provision "file", source: "asterix-installer-0.8.7-SNAPSHOT-binary-assembly.zip", destination:"~/asterix-installer-0.8.7-SNAPSHOT-binary-assembly.zip"
    config.vm.provision "shell", privileged: false, inline: $asterix_inflate
    config.vm.provision "file", source: "asterix-configuration.xml", destination:"~/asterix/bin/asterix-configuration.xml"
    config.vm.provision "shell", privileged: false, inline: $asterix_start

    config.vm.box = "boss-box"
    config.vm.hostname = "asterix-single"
    config.vm.network "private_network", ip: "10.42.0.2"

end
