# myvpn
My personal vpn config on docker.
# Container openvpn
Inicializar la configuracion del servidor.

	docker-compose run --rm openvpn ovpn_genconfig -u udp://<ip del servidor o dns>
	docker-compose run --rm openvpn ovpn_initpki

En caso de ejecutar los comandos con root, fixear los permisos.

	sudo chown -R $(whoami): ./openvpn-data

Inicializa el contenedor

	docker-compose up -d

Generar un certificado del cliente.

	export CLIENTNAME="el_nombre_del_cliente"
	docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME

Generar un archivo de configuracion para el cliente.

	docker-compose run --rm openvpn ovpn_getclient $CLIENTNAME > $CLIENTNAME.ovpn

En caso de querer revocar permisos.

	docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME
	docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME remove

# Container duckdns
Este contenedor fue creado para actualizar la ip dinamica.\n
Editar archivo ./duckdns/data/ipadjust.sh cambiar token y domain.

Luego levantar el container

	docker-compose up -d