depend:
	echo "install ssh server"
	sudo apt update
	sudo apt install -y openssh-server
	sudo systemctl start ssh
	sudo systemctl enable ssh

	echo "install mysql client"
	sudo apt install -y mysql-client
	
	echo "install DHCP server"
	sudo apt install isc-dhcp-server -y
	sudo systemctl start isc-dhcp-server
	sudo systemctl enable isc-dhcp-server

	echo "install docker server"

	sudo curl -fsSL https://get.docker.com/ | sh
	sudo cp ./daemon.json /etc/docker/
	sudo systemctl start docker
	sudo systemctl enable docker

	echo "install docker-compose tool"
	sudo curl https://bootstrap.pypa.io/get-pip.py > get-pip.py
	sudo python3 get-pip.py -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.compip
	sudo pip install -i https://pypi.tuna.tsinghua.edu.cn/simple docker-compose

pull:
	echo 'pull docker images'
	echo 'NOTE: make sure change /etc/docker/daemon.json and /etc/hosts, and call `systemctl restart docker`'
	docker pull grafana/grafana-enterprise
	docker grafana/grafana
	docker prom/prometheus
	docker prom/node-exporter
	docker mysql:8
	docker mongo:6-jammy
	docker mongo-express:latest
	docker prom/alertmanager:latest
	docker prom/node-exporter:v1.6.0

save:
	echo 'save docker images'
	docker save -o xxx.tar \
		local.doker.image:1.0.1 \

deploy:
	echo "deploy files ...."
	mkdir -p /data/backend/arc
	cp -r ./backend /data/

telegraf:
	wget -q https://repos.influxdata.com/influxdata-archive_compat.key
	echo '393e8779c89ac8d958f81f942f9ad7fb82a25e133faddaf92e15b16e6ac9ce4c influxdata-archive_compat.key' | sha256sum -c && cat influxdata-archive_compat.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg > /dev/null
	echo 'deb [signed-by=/etc/apt/trusted.gpg.d/influxdata-archive_compat.gpg] https://repos.influxdata.com/debian stable main' | sudo tee /etc/apt/sources.list.d/influxdata.list
	sudo apt-get update && sudo apt-get install telegraf
	cp ./backend/telegraf/telegraf.conf /etc/telegraf/telegraf.conf
	systemctl restart telegraf

run:
	echo "start compose"
	docker-compose --file docker-compose.yaml up -d

init:
	mysql -h 127.0.0.1 -P 33066 -u root -p < backend/arc.sql

clean:
	echo "stop and clean compose"
	docker-compose --file docker-compose.yaml stop
	docker-compose --file docker-compose.yaml rm

portainer:
	docker volume create portainer_data
	docker pull portainer/portainer-ce:latest
	docker run -d -p 9443:9443 --name portainer --restart=always \
		-v /var/run/docker.sock:/var/run/docker.sock \
		portainer/portainer-ce:latest
