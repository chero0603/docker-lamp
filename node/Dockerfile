FROM node:latest
RUN npm install -g gulp gulp-phpunit config && \
	apt-get -y update && \
	apt-get -y upgrade && \
	apt-get -y install ca-certificates apt-transport-https && \
	wget -q https://packages.sury.org/php/apt.gpg -O- | apt-key add - && \
	echo "deb https://packages.sury.org/php/ stretch main" | tee /etc/apt/sources.list.d/php.list && \
	apt-get -y update && \
	apt-get -y install php7.3 php7.3-cli php7.3-common php7.3-opcache php7.3-curl php7.3-mbstring php7.3-mysql php7.3-zip php7.3-xml

WORKDIR /var/www/html