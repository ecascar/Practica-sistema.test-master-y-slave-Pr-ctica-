
Vagrant.configure("2") do |config|

  config.vm.box = "debian/bookworm64"
  config.vm.network "private_network", ip: "d 192.168.57.0", netmask:"24"

  machines = {
    "venus.sistema.test" => "192.168.57.102",
    "tierra.sistema.test" => "192.168.57.103",
  }

  # Crear una provisión usando un script de shell
  config.vm.provision "shell", inline: <<-SHELL
    # Instalar BIND si no está instalado
    sudo apt-get update
    sudo apt-get install -y bind9

    # Establecer la opción dnssec-validation a yes
    echo 'dnssec-validation yes;' | sudo tee -a /etc/bind/named.conf.options
    sudo systemctl restart bind9

    # Configurar las ACL y opciones de BIND
    echo 'acl "trusted" {' | sudo tee -a /etc/bind/named.conf.options
    echo '    127.0.0.0/8;' | sudo tee -a /etc/bind/named.conf.options
    echo '    192.168.57.0/24;' | sudo tee -a /etc/bind/named.conf.options
    echo '};' | sudo tee -a /etc/bind/named.conf.options
    echo 'options {' | sudo tee -a /etc/bind/named.conf.options
    echo '    allow-recursion { "trusted"; };' | sudo tee -a /etc/bind/named.conf.options
    echo '};' | sudo tee -a /etc/bind/named.conf.options
    
  #REINICIO DEL SISTEMA
  sudo systemctl restart Bind9
  SHELL
end
