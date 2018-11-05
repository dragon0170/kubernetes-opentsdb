FROM java:8-jre

RUN sed -i "s/httpredir.debian.org/mirrors.163.com/g" /etc/apt/sources.list
RUN apt-get update && \
    apt-get install -y gnuplot && \
    apt-get clean autoclean && \
    apt-get autoremove --yes && \
    rm -rf /var/lib/{apt,dpkg,cache.log}/

ADD opentsdb-2.3.1_all.deb .
RUN dpkg -i opentsdb-2.3.1_all.deb
RUN rm ./opentsdb-2.3.1_all.deb


RUN echo "tsd.storage.hbase.zk_quorum = zookeeper-1:2181,zookeeper-2:2181,zookeeper-3:2181" >> /etc/opentsdb/opentsdb.conf
ADD hbase-service.sh /opt/hbase-service.sh
ADD opentsdb-service.sh /opt/opentsdb-service.sh

ENV HBASE_VERSION 1.4.8
ENV HBASE_HOME /opt/hbase
COPY ./hbase-1.4.8-bin.tar.gz /hbase-setup/hbase-1.4.8-bin.tar.gz
RUN tar zxf /hbase-setup/hbase-1.4.8-bin.tar.gz -C /opt/ \
    && ln -s /opt/hbase-1.4.8 /opt/hbase
RUN rm /hbase-setup/hbase-1.4.8-bin.tar.gz

ADD create_table.sh /usr/share/opentsdb/tools/create_table.sh
ADD hbase-site.xml ${HBASE_HOME}/conf/hbase-site.xml
ADD hdfs-site.xml ${HBASE_HOME}/conf/hdfs-site.xml
ADD core-site.xml ${HBASE_HOME}/conf/core-site.xml

EXPOSE 4242
CMD ["/opt/opentsdb-service.sh"]
