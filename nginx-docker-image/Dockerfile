# NGINX image with build artifact

FROM nginx:latest

MAINTAINER Harshal Shah <harshal@infracloud.io>
ENV REFRESHED_AT 2016-09-17

ENV STATIC_FILE spring-music-static.zip
COPY ${STATIC_FILE} /tmp/${STATIC_FILE}
RUN apt-get update -qq \
  && apt-get install -qqy curl wget unzip nano \
  && apt-get clean \
  && unzip /tmp/${STATIC_FILE} -d /usr/share/nginx/assets/

COPY default.conf /etc/nginx/conf.d/default.conf

# tweak nginx image set-up, remove log symlinks
RUN rm /var/log/nginx/access.log /var/log/nginx/error.log

# install Filebeat
ENV FILEBEAT_VERSION=filebeat_1.2.3_amd64.deb
RUN curl -L -O https://download.elastic.co/beats/filebeat/${FILEBEAT_VERSION} \
 && dpkg -i ${FILEBEAT_VERSION} \
 && rm ${FILEBEAT_VERSION}

# configure Filebeat
ADD filebeat.yml /etc/filebeat/filebeat.yml

# CA cert
RUN mkdir -p /etc/pki/tls/certs
ADD logstash-beats.crt /etc/pki/tls/certs/logstash-beats.crt

# start Filebeat
ADD ./start.sh /usr/local/bin/start.sh
RUN chmod +x /usr/local/bin/start.sh
CMD [ "/usr/local/bin/start.sh" ]
