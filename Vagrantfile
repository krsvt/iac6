Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  # Количество требуемых машин
  SERVERS = 2
  # Имя сетевого интерфейса для организации моста
  BRIDGE = "wlp41s0"

  # Массив портов для каждой виртуальной машины
  PORTS = {
    "srv1" => { 8080 => 8080 },
    "srv2" => { 3000 => 3000, 3100 => 3100, 9090 => 9090, 9093 => 9093 }
  }

  def create_host(config, hostname, ip, ports)
    config.vm.define hostname do |host|
      host.vm.network "private_network", ip: ip
      host.vm.network "public_network", bridge: BRIDGE
      host.vm.hostname = hostname
      host.vm.provision "shell", inline: "apt-get update && apt-get install -y python3"

      ports.each do |guest_port, host_port|
        host.vm.network "forwarded_port", guest: guest_port, host: host_port
      end
    end
  end

  create_host(config, "srv1", "192.168.56.201", PORTS["srv1"])
  create_host(config, "srv2", "192.168.56.202", PORTS["srv2"])
end
