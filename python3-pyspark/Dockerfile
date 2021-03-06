# Docker file for development
FROM ubuntu:20.04

ARG SPARK_DOWNLOAD_URL=http://apache.mirrors.hoobly.com/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7.tgz
ARG SPARK_UNPACKED_DIR=spark-3.0.1-bin-hadoop2.7

# Set timezone
ENV TZ Etc/UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Set LANG
ENV LC_ALL C.UTF-8
ENV LANG C.UTF-8
ENV LANGUAGE en_US:en

# Spark
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
ENV SPARK_HOME /spark
ENV PATH="$SPARK_HOME/bin:$SPARK_HOME/sbin:$PATH"
ENV PYSPARK_PYTHON /usr/bin/python3
ENV PYSPARK_DRIVER_PYTHON /usr/bin/python3

# Set DNS
RUN echo "nameserver 1.1.1.1" >> /etc/resolv.conf

RUN apt-get update -y && apt-get install -y \ 
    wget python3 python3-pip gdal-bin python3-gdal python3-venv \
    mapnik-utils python3-mapnik gettext \
    openjdk-8-jre default-jre \
    && wget -q -O spark.tgz $SPARK_DOWNLOAD_URL \
    && mkdir -p /temp_spark \
    && gunzip -c spark.tgz | tar -C /temp_spark -xvf - \
    && mv /temp_spark/$SPARK_UNPACKED_DIR /spark \
    && rm spark.tgz \
    && rm -rf /temp_spark \
    && apt-get autoremove -y && apt-get autoclean -y && rm -rf /var/lib/apt/lists/*

# Project initalization
VOLUME  ["/code"]
WORKDIR /code

EXPOSE 8000