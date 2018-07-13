FROM ubuntu:16.04
MAINTAINER Chen Harel "https://github.com/chook"
RUN apt-get update
RUN apt-get install -y wget openjdk-8-jdk
RUN update-ca-certificates -f
ARG TAKIPI_SECRET_KEY
RUN wget -O - -o /dev/null http://get.takipi.com/takipi-t4c-installer | bash /dev/stdin -i --sk=$TAKIPI_SECRET_KEY
RUN /opt/takipi/etc/takipi-setup-machine-name collector-mactop-container
CMD /opt/takipi/bin/takipi-service -nfd -l
