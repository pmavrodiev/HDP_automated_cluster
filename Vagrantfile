VAGRANTFILE_API_VERSION = "2"

nodes=[
  {
    :hostname => "local.master.com",
    :ip => "10.0.0.100",
    :box => "centos/7",
    :ram => 2048,
    :cpu => 2
  },
  {
    :hostname => "local.slave1.com",
    :ip => "10.0.0.200",
    :box => "centos/7",
    :ram => 2048,
    :cpu => 2
  },
  {
    :hostname => "local.slave2.com",
    :ip => "10.0.0.201",
    :box => "centos/7",
    :ram => 2048,
    :cpu => 2
  },
    {
    :hostname => "local.slave3.com",
    :ip => "10.0.0.202",
    :box => "centos/7",
    :ram => 2048,
    :cpu => 2
  }
]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.ssh.insert_key = false

    # enable hostmanager
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true

    nodes.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/id_rsa.pub"
            node.vm.provision "file", source: "~/.ssh/id_rsa", destination: "~/.ssh/id_rsa"
            node.vm.provision "shell", inline: "cat ~vagrant/.ssh/id_rsa.pub >> ~vagrant/.ssh/authorized_keys"
            node.vm.box = machine[:box]
            node.vm.hostname = machine[:hostname]
            # Repair /etc/hosts because vagrant adds the hostname to the loopback address. This is BAD for Hadoop.
            node.vm.provision "shell", inline: "sed -ri 's/127\.0\.0\.1\s.*/127.0.0.1 localhost localhost.localdomain/' /etc/hosts"
            node.vm.network "private_network", ip: machine[:ip]
            node.vm.provider "virtualbox" do |vb|
                vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
            end
        end
    end
end
