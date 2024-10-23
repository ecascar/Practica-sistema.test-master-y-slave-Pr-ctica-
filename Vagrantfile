
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

    # Crear el archivo de zona directa
    echo $TTL    604800 | sudo tee -a /etc/bind/db.sistema.test
    echo @       IN      SOA     tierra.sistema.test. admin.sistema.test. ( | sudo tee -a /etc/bind/db.sistema.test
    echo                       2023102301    ; Serial | sudo tee -a /etc/bind/db.sistema.test
    echo                       604800        ; Refresh | sudo tee -a /etc/bind/db.sistema.test
    echo                       86400         ; Retry | sudo tee -a /etc/bind/db.sistema.test
    echo                       2419200       ; Expire | sudo tee -a /etc/bind/db.sistema.test
    echo                       604800 )      ; Negative Cache TTL | sudo tee -a /etc/bind/db.sistema.test
    echo '' | sudo tee -a /etc/bind/db.sistema.test
    echo ; Registros de nombres | sudo tee -a /etc/bind/db.sistema.test
    echo @       IN      NS      tierra.sistema.test. | sudo tee -a /etc/bind/db.sistema.test
    echo @       IN      A       192.168.57.103 | sudo tee -a /etc/bind/db.sistema.test

    # Crear el archivo de zona inversa
    echo $TTL    604800 | sudo tee -a /etc/bind/db.192.168.57.103
    echo @       IN      SOA     tierra.sistema.test. admin.sistema.test. ( | sudo tee -a /etc/bind/db.192.168.57.103
    echo                       2023102301    ; Serial | sudo tee -a /etc/bind/db.192.168.57.103
    echo                       604800        ; Refresh | sudo tee -a /etc/bind/db.192.168.57.103
    echo                       86400         ; Retry | sudo tee -a /etc/bind/db.192.168.57.103
    echo                       2419200       ; Expire | sudo tee -a /etc/bind/db.192.168.57.103
    echo                       604800 )      ; Negative Cache TTL | sudo tee -a /etc/bind/db.192.168.57.103
    echo '' | sudo tee -a /etc/bind/db.192.168.57
    echo ; Registros de nombres | sudo tee -a /etc/bind/db.192.168.57
    echo @       IN      NS      tierra.sistema.test. | sudo tee -a /etc/bind/db.192.168.57.103
    echo 10      IN      PTR     tierra.sistema.test. ; Dirección IP del servidor | sudo tee -a /etc/bind/db.192.168.57.103

    # Crear el archivo de zonas en named.conf.local para tierra.sistem.test
    echo zone "sistema.test" { | sudo tee -a /etc/bind/named.conf.local
    echo   type master; | sudo tee -a /etc/bind/named.conf.local
    echo   file "/etc/bind/db.sistema.test"; | sudo tee -a /etc/bind/named.conf.local
    echo   allow-transfer { 192.168.57.102; }; | sudo tee -a /etc/bind/named.conf.local
    echo }; | sudo tee -a /etc/bind/named.conf.local

    echo zone "103.57.168.192.in-addr.arpa" { | sudo tee -a /etc/bind/named.conf.local
    echo   type master; | sudo tee -a /etc/bind/named.conf.local
    echo   file "/etc/bind/db.192.168.57"; | sudo tee -a /etc/bind/named.conf.local
    echo   allow-transfer { 192.168.57.102; }; | sudo tee -a /etc/bind/named.conf.local
    echo }; | sudo tee -a /etc/bind/named.conf.local

    # Crear el archivo de zonas en named.conf.local para venus.sistem.test
    echo zone "sistema.test" { | sudo tee -a /etc/bind/named.conf.local
    echo   type slave; | sudo tee -a /etc/bind/named.conf.local
    echo   file "/var/cache/bind/db.sistema.test"; | sudo tee -a /etc/bind/named.conf.local
    echo   masters { 192.168.57.103; }; | sudo tee -a /etc/bind/named.conf.local
    echo }; | sudo tee -a /etc/bind/named.conf.local

    echo zone "102.57.168.192.in-addr.arpa" { | sudo tee -a /etc/bind/named.conf.local
    echo   type slave; | sudo tee -a /etc/bind/named.conf.local
    echo   file "/var/cache/bind/db.192.168.57.103"; | sudo tee -a /etc/bind/named.conf.local
    echo   allow-transfer { 192.168.57.102; }; | sudo tee -a /etc/bind/named.conf.local
    echo }; | sudo tee -a /etc/bind/named.conf.local



  #REINICIO DEL SISTEMA
  sudo systemctl restart Bind9
  SHELL
end
