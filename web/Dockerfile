FROM centos:7
RUN yum update -y
RUN yum -y remove httpd httpd.x86_64 httpd-tools.x86_64
RUN yum -y remove epel-release

# install leatest apache2.4.*
RUN yum install -y epel-release && \
	yum -y install https://repo.ius.io/ius-release-el7.rpm && \
	sed -i -e "s/enabled *= *1/enabled=0/g" /etc/yum.repos.d/ius.repo && \
	sed -i -e "s/enabled *= *1/enabled=0/g" /etc/yum.repos.d/epel.repo
RUN yum --enablerepo=epel -y install nghttp2 && \
	yum -y install mailcap system-logos && \
	yum -y install openssl wget && \
	wget http://download-ib01.fedoraproject.org/pub/epel/7/x86_64/Packages/b/brotli-1.0.7-5.el7.x86_64.rpm && \
	rpm -Uvh brotli-1.0.7-5.el7.x86_64.rpm && \
	yum -y install brotli && \
	yum -y --disablerepo=base,extras,updates --enablerepo=ius install httpd mod_ssl && \
	yum clean all
RUN sed -i -e "s|LoadModule mpm_prefork_module modules/mod_mpm_prefork.so|#LoadModule mpm_prefork_module modules/mod_mpm_prefork.so|" /etc/httpd/conf.modules.d/00-mpm.conf && \
	sed -i -e "s|#LoadModule mpm_event_module modules/mod_mpm_event.so|LoadModule mpm_event_module modules/mod_mpm_event.so|" /etc/httpd/conf.modules.d/00-mpm.conf
# make SSL server certificate
RUN mkdir -p /etc/ssl/private && \
	openssl req -new -newkey rsa:2048 -nodes -out /etc/ssl/private/server.csr -keyout /etc/ssl/private/server.key -subj "/C=/ST=/L=/O=/OU=/CN=*.lvh.me" && \
	openssl x509 -days 3650 -req -signkey /etc/ssl/private/server.key -in /etc/ssl/private/server.csr -out /etc/ssl/private/server.crt

# set locale
RUN rm -f /etc/rpm/macros.image-language-conf && \
    sed -i -e '/override_install_langs/s/$/,ja_JP.utf8/g' /etc/yum.conf && \
    yum -y reinstall glibc-common && \
    yum clean all
RUN localedef -f UTF-8 -i ja_JP ja_JP.UTF-8
ENV LANG="ja_JP.UTF-8" \
    LANGUAGE="ja_JP:ja" \
    LC_ALL="ja_JP.UTF-8"
RUN echo 'ZONE="Asia/Tokyo"' > /etc/sysconfig/clock 
RUN unlink /etc/localtime
RUN ln -s /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

# editor install
RUN yum -y clean all

COPY docker.conf /etc/httpd/conf.d/docker.conf

# dockre can not use systemd without permission, apahce is started with httpd
CMD ["/usr/sbin/httpd", "-DFOREGROUND"]
