

docker run --rm -P --env TOMCAT_CONNECTOR_PROTOCOL=org.apache.coyote.http11.Http11NioProtocol --env JAVA_OPTS="-server -Xms1024m -Xmx4096m -XX:MetaspaceSize=512m -XX:MaxMetaspaceSize=1024m -XX:-UseGCOverheadLimit -XX:+DisableExplicitGC -XX:+UseConcMarkSweepGC -Djava.security.egd=file:/dev/./urandom -Duser.timezone=GMT+08 -Dfile.encoding=UTF-8 -Djava.awt.headless=true -Djava.net.preferIPv4Stack=true" --env CATALINA_OPTS="-Djava.rmi.server.hostname=127.0.0.1 -Dcom.sun.management.jmxremote.port=9090 -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Djava.library.path=/usr/local/apr/lib" library/tomcat:8.0.53-jdk1.8.0_191



 


JAVA_OPTS="-server -Xms1024m -Xmx4096m XX:MaxNewSize=512m -XX:PermSize=512M -XX:MaxPermSize=1024m -Djava.security.egd=file:/dev/./urandom -Duser.timezone=GMT+08 -Dfile.encoding=UTF-8 -Djava.awt.headless=true -Djava.net.preferIPv4Stack=true -XX:-UseGCOverheadLimit -XX:+DisableExplicitGC -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:+CMSParallelRemarkEnabled -XX:+PrintGCTimeStamps -XX:+PrintGCDetails -Xloggc:/data/gc.log -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/data/dump"



CATALINA_OPTS="-Djava.rmi.server.hostname=127.0.0.1 -Dcom.sun.management.jmxremote.port=9090 -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Djava.library.path=/usr/local/apr/lib"




-XX:+AggressiveOpts -XX:+UseBiasedLocking  -XX:MaxTenuringThreshold=30    -XX:ReservedCodeCacheSize=32m -XX:+UseCMSCompactAtFullCollection -XX:LargePageSizeInBytes=128m  -XX:+UseFastAccessorMethods -XX:+UseCMSInitiatingOccupancyOnly




