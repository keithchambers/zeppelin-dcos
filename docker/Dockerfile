FROM mesosphere/mesos:1.0.0

ENV ZEPPELIN_VERSION 0.6.1

# install and update packages
RUN apt-get update && \
    apt-get install --yes wget curl tar software-properties-common && \
    add-apt-repository ppa:openjdk-r/ppa && \
    apt-get update && \
    apt-get install --yes openjdk-8-jdk && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# java config
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
RUN update-alternatives --set java /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java

# install zeppelin
RUN curl http://www.apache.org/dist/zeppelin/zeppelin-${ZEPPELIN_VERSION}/zeppelin-${ZEPPELIN_VERSION}-bin-all.tgz | tar xfz -
WORKDIR /zeppelin-${ZEPPELIN_VERSION}-bin-all
ADD zeppelin-env.sh conf/zeppelin-env.sh

# hadoop config
ENV HADOOP_CONF_DIR /etc/hadoop
ADD ./conf/hadoop/hdfs-site.xml /etc/hadoop/hdfs-site.xml
ADD ./conf/hadoop/core-site.xml /etc/hadoop/core-site.xml
ADD ./conf/hadoop/mesos-site.xml /etc/hadoop/mesos-site.xml

# set permissions
RUN groupadd -r zeppelin && useradd -r -m -g zeppelin zeppelin && \
    chown -R zeppelin:zeppelin /zeppelin-${ZEPPELIN_VERSION}-bin-all && \
    chown -R zeppelin:zeppelin /etc/hadoop

USER zeppelin

CMD ["bin/zeppelin.sh", "start"]
