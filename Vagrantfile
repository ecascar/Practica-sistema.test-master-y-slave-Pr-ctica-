
Vagrant.configure("2") do |config|

  config.vm.box = "debian/bookworm64"
  config.vm.network "private_network", ip: "d 192.168.57.0", netmask:"24"

  machines = {
    "mercurio.sistema.test" => "192.168.57.101",
    "venus.sistema.test" => "192.168.57.102",
    "tierra.sistema.test" => "192.168.57.103",
    "marte.sistema.test" => "192.168.57.104",
  }
end
