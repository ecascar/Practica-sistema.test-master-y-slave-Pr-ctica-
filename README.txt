1º- Se han creado los diversos archivos con los que se trabajaran
    dentro del archivo "vagrantfile".

2º- Se han creado tanto la ip general como la especificas de cada
    uno.

3º- 1)Se ha establecido la opción 'dnssec-validation' a yes

    2)Se ha establecidon permisos para las consultas recursivas para ips
    concretas

    3)Se han establecido a 'tierra.sistema.test' y 'venus.sistema.test' como
    maestro y esclavo respectivamente

    4)El tiempo en caché de las resupestas negativas se ha establecido a 
    2 horas( 7200 segundos )

    5)Las consultas no autorizadas se reenviarán al servidor DNS 208.67.222.222

    6)Se han establecido como alias para 'tierra.sistema.test' y 'venus.sistema.test'
    como 'ns1.sistema.test' y 'ns2.sistema.test' respectivamente

    7)Se ha establecido para 'marte.sistema.test' el alias 'mail.sistema.test'

    8)Se ha establecido 'marte.sistema.test' como servidor de correo del dominio de
    correo 'sistema.test'