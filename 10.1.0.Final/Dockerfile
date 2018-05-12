FROM jboss/wildfly:10.1.0.Final

ENV WILDFLY_VERSION 10.1.0.Final

USER jboss

ENV JDBC_POSTGRES_VERSION 42.2.2
ENV JDBC_MARIADB_VERSION 2.2.3
ENV LOGSTASH_GELF_VERSION 1.11.2
ENV JEDIS_VERSION 2.9.0
ENV COMMONS_POOL2_VERSION 2.5.0

RUN mkdir -p /opt/jboss/wildfly/modules/system/layers/base/org/postgresql/jdbc/main && \
    cd /opt/jboss/wildfly/modules/system/layers/base/org/postgresql/jdbc/main && \
    curl -L http://central.maven.org/maven2/org/postgresql/postgresql/$JDBC_POSTGRES_VERSION/postgresql-$JDBC_POSTGRES_VERSION.jar > postgres-jdbc.jar && \
    echo -e '<?xml version="1.0" encoding="UTF-8"?>\n<module xmlns="urn:jboss:module:1.3" name="org.postgresql.jdbc">\n    <resources>\n        <resource-root path="postgres-jdbc.jar"/>\n    </resources>\n    <dependencies>\n        <module name="javax.api"/>\n        <module name="javax.transaction.api"/>\n    </dependencies>\n</module>' >> /opt/jboss/wildfly/modules/system/layers/base/org/postgresql/jdbc/main/module.xml

RUN mkdir -p /opt/jboss/wildfly/modules/system/layers/base/org/mariadb/jdbc/main && \
    cd /opt/jboss/wildfly/modules/system/layers/base/org/mariadb/jdbc/main && \
    curl -L http://central.maven.org/maven2/org/mariadb/jdbc/mariadb-java-client/$JDBC_MARIADB_VERSION/mariadb-java-client-$JDBC_MARIADB_VERSION.jar > mariadb-jdbc.jar && \
    echo -e '<?xml version="1.0" encoding="UTF-8"?>\n<module xmlns="urn:jboss:module:1.3" name="org.mariadb.jdbc">\n    <resources>\n        <resource-root path="mariadb-jdbc.jar"/>\n    </resources>\n    <dependencies>\n        <module name="javax.api"/>\n        <module name="javax.transaction.api"/>\n    </dependencies>\n</module>' >> /opt/jboss/wildfly/modules/system/layers/base/org/mariadb/jdbc/main/module.xml

RUN mkdir -p /opt/jboss/wildfly/modules/system/layers/base/biz/paluch/logging/main && \
    cd /opt/jboss/wildfly/modules/system/layers/base/biz/paluch/logging/main && \
    curl -L http://central.maven.org/maven2/biz/paluch/logging/logstash-gelf/$LOGSTASH_GELF_VERSION/logstash-gelf-$LOGSTASH_GELF_VERSION.jar > logstash-gelf.jar && \
    curl -L http://central.maven.org/maven2/redis/clients/jedis/$JEDIS_VERSION/jedis-$JEDIS_VERSION.jar > jedis.jar && \
    curl -L http://central.maven.org/maven2/org/apache/commons/commons-pool2/$COMMONS_POOL2_VERSION/commons-pool2-$COMMONS_POOL2_VERSION.jar > commons-pool2.jar && \
    echo -e '<?xml version="1.0" encoding="UTF-8"?>\n<module xmlns="urn:jboss:module:1.3" name="biz.paluch.logging">\n    <resources>\n        <resource-root path="logstash-gelf.jar" />\n        <resource-root path="jedis.jar" />\n        <resource-root path="commons-pool2.jar"/>\n    </resources>\n    <dependencies>\n        <module name="org.apache.log4j"/>\n        <module name="org.slf4j"/>\n        <module name="javax.api"/>\n        <module name="org.jboss.logmanager"/>\n    </dependencies>\n</module>' >> /opt/jboss/wildfly/modules/system/layers/base/biz/paluch/logging/main/module.xml

EXPOSE 8080

CMD ["/opt/jboss/wildfly/bin/standalone.sh", "-b", "0.0.0.0"]
