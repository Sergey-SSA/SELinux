# -*- mode: ruby -*-
# vim: set ft=ruby :
MACHINES = {
  :selinux => {
        :box_name => "centos/7",
        :ip_addr => '192.168.9.188'
  }
}
Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
      config.vm.define boxname do |box|
          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s
          box.vm.network "private_network", ip: boxconfig[:ip_addr]
          box.vm.provider :virtualbox do |vb|
            vb.customize ["modifyvm", :id, "--memory", "2048", "--cpus", "2"]
          end

	  box.vm.synced_folder ".", "/vagrant"
          
          box.vm.provision "shell", inline: <<-SHELL
            mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
            yum install -y setools-console policycoreutils-python policycoreutils-newrole selinux-policy-mls setroubleshoot net-tools
	    yum install -y epel-release && yum install -y nginx
	    systemctl enable --now nginx
          SHELL
       end
   end
end
