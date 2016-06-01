FROM oraclelinux
MAINTAINER Gary Niu <gary.niu@gmail.com>

ENV http_proxy=http://10.188.60.183:80

ADD linuxamd64_12102_database_1of2.zip /tmp/install/linuxamd64_12102_database_1of2.zip
ADD linuxamd64_12102_database_2of2.zip /tmp/install/linuxamd64_12102_database_2of2.zip

RUN yum -y update \
	&& yum -y install unzip \
	&& yum -y install oracle-rdbms-server-12cR1-preinstall \
	&& yum clean all \
	&& cd /tmp/install \
	&& unzip linuxamd64_12102_database_1of2.zip \
	&& unzip linuxamd64_12102_database_2of2.zip \
	&& rm *.zip

RUN userdel oracle \
	&& rm -rf /home/oracle \
	&& rm /var/spool/mail/oracle \
	&& useradd -m -u 54321 -g oinstall -G dba oracle \
	&& echo "oracle:oracle" | chpasswd

ENV ORACLE_BASE /u01/app/oracle

RUN mkdir -p $ORACLE_BASE \
	&& chown -R oracle:oinstall $ORACLE_BASE \
	&& chmod -R 775 $ORACLE_BASE \
	&& mkdir -p /u01/app/oraInventory \
	&& chown -R oracle:oinstall /u01/app/oraInventory \
	&& chmod -R 775 /u01/app/oraInventory

ADD oraInst.loc /etc/oraInst.loc
ADD sysctl.conf /etc/sysctl.conf

ENV CVUQDISK_GRP oinstall

RUN chmod 664 /etc/oraInst.loc \
	&& echo "oracle soft stack 10240" >> /etc/security/limits.conf \
	&& cd /tmp/install/database/rpm \
	&& rpm -iv cvuqdisk-1.0.9-1.rpm

ADD db_install.rsp /tmp/db_install.rsp
ADD install /tmp/install/install
