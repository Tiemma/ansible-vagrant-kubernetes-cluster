N = 3

Vagrant.configure("2") do |config|

    config.vm.box = "ubuntu/bionic64"
    config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

    print "Proxy is " + ENV['PROXY']

    config.proxy.http     = ENV['PROXY']
    config.proxy.https    = ENV['PROXY']

    config.proxy.no_proxy = "localhost,127.0.0.1"
    config.vm.provision "shell" do |s|
        s.path = "install-deps.sh"
        s.privileged = true
    end

    (1..N).each do |machine_id| 
        config.vm.define "machine#{machine_id}" do |machine|
            #machine.vm.hostname = "machine#{machine_id}"
            #machine.vm.network "private_network", ip: "192.168.77.#{20+machine_id}", auto_config: false

            # Only execute once the Ansible provisioner,
            # when all the machines are up and .
            ready.
            if machine_id == N
                machine.vm.provision "create_user" do |create_user|
                    # Disable default limit to connect to all the machines
                    create_user.limit = "all"
                    create_user.verbose = "v"
                    create_user.playbook = "kube-cluster/create-non-root-user.yml"
                end
                machine.vm.provision "install_deps" do |install_deps|
                    # Disable default limit to connect to all the machines
                    install_deps.limit = "all"
                    install_deps.verbose = "v"
                    install_deps.playbook = "kube-cluster/install-kube-dependencies.yml"
                end
            end
        end
    end

end