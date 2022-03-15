Vagrant.configure("2") do |config|
  # 环境变量
  config.env.enable
  # 挂载目录
  config.vm.synced_folder ".", "/vagrant"
  # config.vm.synced_folder ".data/", "/etc/.vagrantdata/"

  # 设置操作系统镜像、版本、是否更新
  config.vm.box = ENV["BOX_IMAGE"]
  config.vm.box_version = ENV["BOX_VERSION"]
  config.vm.box_check_update = false

  if Vagrant.has_plugin?("vagrant-hostmanager")
    config.hostmanager.enabled = true
    config.hostmanager.manage_guest = true
  end
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
    config.vbguest.no_install = true
    config.vbguest.no_remote = true
  end

  NODEHOSTNAME = ENV["NODE_HOSTNAME"]
  BASHSCRIPTPATH = ENV["BASH_SCRIPT_PATH"]

  # support optional
  if ENV["SETUP_NODES"]
    # support mutli instance
    (1..( ENV["NODE_COUNT"] ).to_i(10)).each do |i|
      config.vm.define "#{NODEHOSTNAME}-#{i}" do |subconfig|
        subconfig.vm.hostname = "#{NODEHOSTNAME}-#{i}"
        subconfig.vm.network :private_network, ip: ENV["NODE_IP_NW"] + "#{i + 10}"
        subconfig.vm.provider :virtualbox do |vb|
          vb.customize ["modifyvm", :id, "--cpus", ENV["NODE_CPU"]]
          vb.customize ["modifyvm", :id, "--memory", ENV["NODE_MEMORY"]]
          vb.name ="#{NODEHOSTNAME}-#{i}"
        end
        # subconfig.vm.provision "ansible_local" do |ansible|
        #   ansible.playbook = "/etc/k8s-scripts/worker.yaml"
        #   ansible.verbose = "v"
        # end
        # run sudo bootstrap.sh
        # config.vm.provision "shell", privileged: true, path: "./bootstrap.sh"
        config.vm.provision "shell", privileged: true, path: "#{BASHSCRIPTPATH}speed-up-apt.sh"
        config.vm.provision "shell", privileged: true, path: "#{BASHSCRIPTPATH}install-docker.sh"
        config.vm.provision "shell", privileged: true, path: "#{BASHSCRIPTPATH}run-docker-with-none-root.sh"
        config.vm.provision "shell", privileged: true, path: "#{BASHSCRIPTPATH}speed-up-docker.sh"
        config.vm.provision "shell", privileged: true, path: "#{BASHSCRIPTPATH}install-docker-compose.sh"
      end
    end
  end
end

# log feat info
# cat Vagrantfile | grep "^ *# support" | sed "s/^ *//g"
# vagrant up
