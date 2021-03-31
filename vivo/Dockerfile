FROM tomcat:9.0.21-jdk11-openjdk

LABEL maintainer="David NOGUEIRAS BLANCO"

RUN apt-get update
RUN apt-get install -y maven git

WORKDIR /usr/local/

RUN wget -O Vitro.zip https://github.com/vivo-project/Vitro/releases/download/vitro-1.11.1/vitro-installer.zip
RUN unzip Vitro.zip -d Vitro
RUN rm Vitro.zip

RUN wget -O VIVO.zip https://github.com/HEG-INCIPIT/INCIPIT-CRIS_VIVO/archive/master.zip
RUN unzip VIVO.zip -d VIVO
RUN rm VIVO.zip

RUN ls /usr/local/VIVO/

WORKDIR /usr/local/VIVO/INCIPIT-CRIS_VIVO-master/
COPY ./example-settings.xml installer/example-settings.xml

RUN mvn install -U -s installer/example-settings.xml

# clean up
RUN rm -r /usr/local/tomcat/webapps/docs
RUN rm -r /usr/local/tomcat/webapps/examples

# Set properties
# Adjust logging, you can mount over this in docker-compose to further adjust
COPY ./log4j.properties /usr/local/tomcat/webapps/vivo/WEB-INF/classes

WORKDIR /usr/local/VIVO/INCIPIT-CRIS_VIVO-master/home/config

COPY ./dockercompose.runtime.properties runtime.properties
COPY ./example.applicationSetup.n3 applicationSetup.n3

RUN chmod ugo+w -R /usr/local/tomcat/temp
RUN chmod ugo+w -R /usr/local/VIVO/INCIPIT-CRIS_VIVO-master/home

RUN export CATALINA_OPTS="-Xms512m -Xmx512m -XX:MaxPermSize=128m"
