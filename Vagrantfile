Vagrant.configure("2") do |config|
	(1..3).each do |i|
		config.vm.define "ojtest#{i}" do |web|
			web.vm.box = "ubuntu/focal64"
			web.vm.network "forwarded_port", id: "ssh", host: 2222 + i, guest: 22

			# for VictoriaMetrics
			web.vm.network "forwarded_port", id: "ssh", host: 8480 + i*10000, guest: 8480
			web.vm.network "forwarded_port", id: "ssh", host: 8481 + i*10000, guest: 8481

			web.vm.network "private_network", ip: "10.11.10.#{i}", virtualbox__intnet: true
			web.vm.hostname = "ojtest#{i}"

			web.vm.provision "shell" do |s|
				ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_ed25519.pub").first.strip
				s.inline = <<-SHELL
				echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
				echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
				SHELL
			end

			web.vm.provider "virtualbox" do |v|
				v.name = "ojtest#{i}"
				v.memory = 2048
				v.cpus = 1
			end
		end
	end
end
