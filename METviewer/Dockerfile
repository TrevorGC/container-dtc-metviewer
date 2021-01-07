FROM centos:6

ENV METVIEWER_VERSION 3.1
#ENV TOMCAT_VERSION 6.0.24
ENV TOMCAT_VERSION 8.5.23
ENV METVIEWER_URL http://dtcenter.ucar.edu/met/users/downloads/MET_releases/METviewer-${METVIEWER_VERSION}.tar.gz
#ENV CATALINA_HOME /usr/local/apache-tomcat-6.0.24
ENV CATALINA_HOME /usr/local/apache-tomcat-8.5.23
RUN curl https://www.getpagespeed.com/files/centos6-eol.repo --output /etc/yum.repos.d/CentOS-Base.repo

#
# Install required packages
#
RUN yum -y install gcc zlib-devel wget tar java gsl-devel mysql ksh xz epel-release \
  && rm -rf /var/cache/yum/* \
  && yum clean all

#
# Install Tomcat
#
#RUN wget https://get.station307.com/wTSuECvGf24/apache-tomcat-6.0.24.tar.gz -O /apache-tomcat-6.0.24.tar.gz \
#  && tar xvzf /apache-tomcat-6.0.24.tar.gz \
#  && rm /apache-tomcat-6.0.24.tar.gz \
#  && rm -rf /apache-tomcat-6.0.24/webapps/docs \
#  && rm -rf /apache-tomcat-6.0.24/webapps/examples \
#  && mv /apache-tomcat-6.0.24/ /usr/local/ \	
#  && chmod +x ${CATALINA_HOME}/bin/*.sh \
#  && yum clean all
  
RUN wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.23/bin/apache-tomcat-8.5.23.tar.gz -O /apache-tomcat-8.5.23.tar.gz \
  && tar xvzf /apache-tomcat-8.5.23.tar.gz \
  && rm /apache-tomcat-8.5.23.tar.gz \
  && rm -rf /apache-tomcat-8.5.23/webapps/docs \
  && rm -rf /apache-tomcat-8.5.23/webapps/examples \
  && mv /apache-tomcat-8.5.23/ /usr/local/ \	
  && chmod +x ${CATALINA_HOME}/bin/*.sh \
  && yum clean all

EXPOSE 8080
CMD exec ${CATALINA_HOME}/bin/catalina.sh run

#
# Download and Deploy METviewer load/batch
#
RUN mkdir /METviewer-python

RUN wget ${METVIEWER_URL} \
 && tar -xzvf  METviewer-${METVIEWER_VERSION}.tar.gz -C /tmp \
 && mv /tmp/METviewer/METviewer/dist/*.war $CATALINA_HOME/webapps/ \
 && mv /tmp/METviewer/METviewer /METviewer \
 && mv /tmp/METviewer/METcalcpy /METviewer-python/ \
 && mv /tmp/METviewer/METplotpy /METviewer-python/ \
 && rm METviewer-${METVIEWER_VERSION}.tar.gz

#
# Install Python 3.6
#
#RUN wget https://www.python.org/ftp/python/3.6.8/Python-3.6.8.tar.xz \
#  && tar -xvf Python-3.* \
#  && cd Python-3.* \ 
#  && ./configure \
#  && make \ 
#  && make install

#
# Install Pip
#
#RUN yum -y install python-pip

#
# Install Python packages
#
#RUN pip-3 install bootstrapped \
#  && pip-3 install plotly==4.9.0 \
#  && pip-3 install kaleido \
#  && pip-3 install pandas \
#  && pip-3 install  numpy \
#  && pip-3 install  scipy \
#  && pip-3 install  PyYAML \
#  && pip-3 install  psutil \
#  && pip-3 install  requests \
#  && pip-3 install  matplotlib \
#  && pip-3 install  lxml \
#  && pip-3 install  pymysql \
#  && pip-3 install  retrying

#
# point python to python3
#
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1

#
# set env vars
#
ENV PYTHONPATH "${PYTHONPATH}:/METviewer-python/METcalcpy/:/METviewer-python/METplotpy/metplotpy/"
ENV METPLOTPY_BASE "/METviewer-python/METplotpy/metplotpy/"

#ENTRYPOINT ${CATALINA_HOME}/bin/startup.sh && /bin/bash
#CMD ["true"]
