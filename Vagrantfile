# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

cluster = {
    "consulserver1.local" => {
        :type           => "consul",
        :ip             => "192.168.100.11",
        :cpus           => 1, :mem => 256,
        :shellpath      => "bootstrap-consul-server.sh",
        :consulconfig   => "/vagrant/config/consul-server1.json",
        :consulextra    => "/vagrant/config/consul-server",
        :consului       => "Y",
        :syncedfolders  => [],
        :ports          => []
    },
    #"consulserver2.local" => {
    #    :type           => "consul",
    #    :ip             => "192.168.100.12",
    #    :cpus           => 1, :mem => 256,
    #    :shellpath      => "bootstrap-consul-server.sh",
    #    :consulconfig   => "/vagrant/config/consul-server2.json",
    #    :consulextra    => "/vagrant/config/consul-server",
    #    :consului       => "N",
    #    :syncedfolders  => [],
    #    :ports          => []
    #},
    #"consulserver3.local" => {
    #    :type           => "consul",
    #    :ip             => "192.168.100.13",
    #    :cpus           => 1, :mem => 256,
    #    :shellpath      => "bootstrap-consul-server.sh",
    #    :consulconfig   => "/vagrant/config/consul-server3.json",
    #    :consulextra    => "/vagrant/config/consul-server",
    #    :consului       => "N",
    #    :syncedfolders  => [],
    #    :ports          => []
    #},

    "web-lb.local" => {
       :type           => "web-lb",
       :ip             => "192.168.100.20",
       :cpus           => 1, :mem => 256,
       :shellpath      => "bootstrap-web-lb.sh",
       :consulconfig   => "/vagrant/config/consul-web-lb.json",
       :consulextra    => "/vagrant/config/consul-web-lb",
       :consului       => "N",
       :syncedfolders  => [],
       :ports          => []
    },

    "web1.local" => {
        :type           => "web",
        :ip             => "192.168.100.31",
        :cpus           => 1, :mem => 256,
        :shellpath      => "bootstrap-web.sh",
        :consulconfig   => "/vagrant/config/consul-web1.json",
        :consulextra    => "/vagrant/config/web",
        :consului       => "N",
        :ports          => []
    },
    "web2.local" => {
        :type           => "web",
        :ip             => "192.168.100.32",
        :cpus           => 1, :mem => 256,
        :shellpath      => "bootstrap-web.sh",
        :consulconfig   => "/vagrant/config/consul-web2.json",
        :consulextra    => "/vagrant/config/web",
        :consului       => "N",
        :ports          => []
    },
    #"web3.local" => {
    #    :type           => "web",
    #    :ip             => "192.168.100.33",
    #    :cpus           => 1, :mem => 256,
    #    :shellpath      => "bootstrap-web.sh",
    #    :consulconfig   => "/vagrant/config/consul-web3.json",
    #    :consulextra    => "/vagrant/config/web",
    #    :consului       => "N",
    #    :ports          => []
    #}
}



Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
    config.vm.synced_folder "./config", "/vagrant/config", mount_options: ["dmode=755,fmode=755"]

    cluster.each_with_index do |(hostname, info), index|
        config.vm.define hostname do |cfg|
            cfg.vm.provider :virtualbox do |vb, override|
                override.vm.box = "ubuntu/trusty64"

                # Set private network IP address
                override.vm.network :private_network, ip: "#{info[:ip]}"

                # Set hostname
                override.vm.hostname = hostname

                # Set ports
                info[:ports].each do |port|
                    override.vm.network :forwarded_port, guest: port[:guest], host: port[:host]
                end # end port

                # Customize virtual box
                vb.customize ["modifyvm", :id, "--name", hostname, "--memory", info[:mem], "--cpus", info[:cpus], "--hwvirtex", "on", "--natdnshostresolver1", "on"]

                override.vm.provision :shell, :path => "#{info[:shellpath]}", :args => "'#{hostname}' '#{info[:consulconfig]}' '#{info[:consulextra]}' '#{info[:consului]}'"
            end # end provider
        end # end config
    end # end cluster
end # end Vagrant
