FROM centos:7.2.1511
MAINTAINER YANG SEN <nuaays@gmail.com>

#wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u191-b12/2787e4a523244c269598db4e85c51e0c/jdk-8u191-linux-x64.tar.gz

WORKDIR /

ENV TZ Asia/Shanghai
ENV JAVA_VERSION 8u191
ENV JAVA_HOME /usr/lib/jdk1.8.0_191
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH      $JAVA_HOME/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV LANG      en_US.UTF-8
ENV LC_ALL    C
ENV TERM      xterm
ENV TOMCAT_VERSION 8.0.53

ENV TOMCAT_USERNAME "admin"
ENV TOMCAT_PASSWORD "admin"

ADD ./jdk-8u191-linux-x64.tar.gz /usr/lib


RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    yum install -y curl net-tools bind-utils psmisc && \
    sed -i 's#securerandom.source=file:/dev/random#securerandom.source=file:/dev/./urandom#' /usr/lib/jdk1.8.0_191/jre/lib/security/java.security && \
    curl http://mirrors.shu.edu.cn/apache/tomcat/tomcat-8/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz -o /tmp/apache-tomcat-$TOMCAT_VERSION.tar.gz && \
    mkdir -p /data/app/tomcat && tar -zxvf /tmp/apache-tomcat-$TOMCAT_VERSION.tar.gz -C /data/app/tomcat --strip-components=1 && \
    curl http://mirrors.shu.edu.cn/apache/tomcat/tomcat-8/v$TOMCAT_VERSION/bin/extras/catalina-jmx-remote.jar -o /data/app/tomcat/lib/catalina-jmx-remote.jar && \
    sed -i 's#Server port="8005"#Server port="-1"#' /data/app/tomcat/conf/server.xml && \
    #sed -i 's#Connector port="8080" protocol="HTTP/1.1"#Connector port="8080" protocol="org.apache.coyote.http11.Http11NioProtocol"#' /data/app/tomcat/conf/server.xml && \
    sed -i 's#connectionTimeout="20000"#connectionTimeout="20000" acceptCount="1000" maxConnections="1000" maxThreads="200" minSpareThreads="20" enableLookups="false" URIEncoding="utf-8" useBodyEncodingForURI="true"#' /data/app/tomcat/conf/server.xml && \
    sed -i 's#unpackWARs="true" autoDeploy="true"#unpackWARs="false" autoDeploy="false" reloadable="false"#' /data/app/tomcat/conf/server.xml && \
    rm -rf /data/app/tomcat/webapps/{docs,examples} /tmp/apache-tomcat-$TOMCAT_VERSION.tar.gz && \
    rm -rf /var/lib/yum/


RUN yum clean all && yum makecache && rpm --rebuilddb && yum install -y openssl-devel apr apr-devel apr-util make gcc glibc-common && \
    cd /tmp && curl http://mirrors.shu.edu.cn/apache/tomcat/tomcat-connectors/native/1.2.18/source/tomcat-native-1.2.18-src.tar.gz -o tomcat-native-1.2.18-src.tar.gz && tar -zxvf tomcat-native-1.2.18-src.tar.gz && cd tomcat-native-1.2.18-src/native && ./configure && make && make install && \
    echo "export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/apr/lib" >> /etc/profile && \
    mkdir -p /usr/java/packages/lib/amd64 && cd /usr/java/packages/lib/amd64 && \
    ln -s  /usr/local/apr/lib/libtcnative-1.so.0.2.18 libtcnative-1.so && ln -s  /usr/local/apr/lib/libtcnative-1.so.0.2.18 libtcnative-1.so.0 && \
    rm -rf /tmp/tomcat-native* && yum autoremove -y gcc make  && \
    rm -rf /var/lib/yum/


RUN echo "<?xml version='1.0' encoding='utf-8'?>" | tee /data/app/tomcat/conf/tomcat-users.xml.template && \
    echo '<tomcat-users xmlns="http://tomcat.apache.org/xml" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd" version="1.0">' | tee -a /data/app/tomcat/conf/tomcat-users.xml.template && \
    echo '<role rolename="manager"/>' | tee -a /data/app/tomcat/conf/tomcat-users.xml.template && \
    echo '<role rolename="manager-gui"/>' | tee -a /data/app/tomcat/conf/tomcat-users.xml.template && \
    echo '<role rolename="manager-script"/>' | tee -a /data/app/tomcat/conf/tomcat-users.xml.template && \
    echo '<role rolename="manager-jmx"/>' | tee -a /data/app/tomcat/conf/tomcat-users.xml.template && \
    echo '<role rolename="manager-status"/>' | tee -a /data/app/tomcat/conf/tomcat-users.xml.template && \
    echo '<role rolename="admin"/>' | tee -a /data/app/tomcat/conf/tomcat-users.xml.template && \
    echo '<role rolename="admin-gui"/>' | tee -a /data/app/tomcat/conf/tomcat-users.xml.template && \
    echo '<user username="TOMCAT_USERNAME" password="TOMCAT_PASSWORD" roles="admin,admin-gui,manager,manager-gui,manager-script,manager-jmx,manager-status"/>' | tee -a /data/app/tomcat/conf/tomcat-users.xml.template && \
    echo '</tomcat-users>' | tee -a /data/app/tomcat/conf/tomcat-users.xml.template && \
    sed -i '2icp -f /data/app/tomcat/conf/tomcat-users.xml.template /data/app/tomcat/conf/tomcat-users.xml' /data/app/tomcat/bin/catalina.sh && \
    sed -i '3iUSERNAME=\$(</dev/urandom tr -cd '[:alnum:]' | head -c6);echo $USERNAME' /data/app/tomcat/bin/catalina.sh && \
    sed -i '4iPASSWORD=\$(</dev/urandom tr -cd _A-Z-a-z-0-9!=+|head -c24);echo $PASSWORD' /data/app/tomcat/bin/catalina.sh && \
    sed -i '5ised -i "s#TOMCAT_USERNAME#$USERNAME#" /data/app/tomcat/conf/tomcat-users.xml' /data/app/tomcat/bin/catalina.sh && \
    sed -i '6ised -i "s#TOMCAT_PASSWORD#$PASSWORD#" /data/app/tomcat/conf/tomcat-users.xml' /data/app/tomcat/bin/catalina.sh




EXPOSE 8080


CMD ["/data/app/tomcat/bin/catalina.sh", "run"]

