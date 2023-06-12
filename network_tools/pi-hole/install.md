## Linux Ubuntu
1. Stop systemd-resolve(only in Ubuntu)

		sudo systectl stop systemd-resolve.service

2. Disable systemd-resolve(only in Ubuntu)

		sudo systectl disable systemd-resolve.service

3. Update the package list:

		sudo apt update

4. Install docker-compose:

		sudo apt install docker-compose

5. Make a directory for the application:

		mkdir pihole; cd pihole

5. Create the docker-compose file:
	
		
	```
	nano docker-compose.yml
	```
	- copy this into the file:

	``` sh
	version: "3"

	# More info at https://github.com/pi-hole/docker-pi-hole/ and https://docs.pi-hole.net/
	services:
	pihole:
		container_name: pihole
		image: pihole/pihole:latest
		# For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
		ports:
		- "53:53/tcp"
		- "53:53/udp"
		- "67:67/udp" # Only required if you are using Pi-hole as your DHCP server
		- "80:80/tcp"
		environment:
		TZ: 'Europe/Bucharest'
		# WEBPASSWORD: 'set a secure password here or it will be random'
		# Volumes store your data between container upgrades
		volumes:
		- './etc-pihole:/etc/pihole'
		- './etc-dnsmasq.d:/etc/dnsmasq.d'
		#   https://github.com/pi-hole/docker-pi-hole#note-on-capabilities
		cap_add:
		- NET_ADMIN # Required if you are using Pi-hole as your DHCP server, else not needed
		restart: unless-stopped
	```
	- don't forget to set the **WEBPASSWORD**

7. Start the the Pi_Hole Docker container:

		sudo docker-compose up -d

8. Go to the admin pannel:

		http://IPADDRESS/admin