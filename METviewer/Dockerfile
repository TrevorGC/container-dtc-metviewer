FROM centos:6

MAINTAINER Tatiana Burek <tatiana@ucar.edu>

ENV METVIEWER_VERSION 3.1
ENV TOMCAT_MINOR_VERSION 0.24
ENV TOMCAT_MAJOR_VERSION 6
ENV TOMCAT_VERSION ${TOMCAT_MAJOR_VERSION}.${TOMCAT_MINOR_VERSION}

ENV METVIEWER_URL http://dtcenter.ucar.edu/met/users/downloads/MET_releases/METviewer-${METVIEWER_VERSION}.tar.gz

RUN curl https://www.getpagespeed.com/files/centos6-eol.repo --output /etc/yum.repos.d/CentOS-Base.repo


#
# Install required packages
#
RUN yum -y install gcc zlib-devel wget tar java gsl-devel mysql ksh xz  \
  && rm -rf /var/cache/yum/* \
  && yum clean all

  
#
# Install Tomcat
#
ENV CATALINA_HOME /opt/tomcat

RUN wget https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR_VERSION}/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz \
   && tar -xvf apache-tomcat-${TOMCAT_VERSION}.tar.gz \
   && rm apache-tomcat*.tar.gz \
   && mv apache-tomcat* ${CATALINA_HOME} \
   && chmod +x ${CATALINA_HOME}/bin/*sh

EXPOSE 8080


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
# Install Python 3.6
#
RUN yum install -y python3 python3-devel python3-pip


#
#Install pip
#
#RUN wget https://bootstrap.pypa.io/get-pip.py \
#  && python3.6 get-pip.py


#
# Install Python packages
#
RUN pip3 install bootstrapped \
  && pip3 install plotly==4.9.0 \
  && pip3 install kaleido \
  && pip3 install pandas \
  && pip3 install  numpy \
  && pip3 install  scipy \
  && pip3 install  PyYAML \
  && pip3 install  psutil \
  && pip3 install  requests \
  && pip3 install  matplotlib \
  && pip3 install  lxml \
  && pip3 install  pymysql \
  && pip3 install  retrying


#
# point python to python3
#
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1

#
# set env vars
#
ENV PYTHONPATH "${PYTHONPATH}:/METviewer-python/METcalcpy/:/METviewer-python/METplotpy/metplotpy/"
ENV METPLOTPY_BASE "/METviewer-python/METplotpy/metplotpy/"

ENTRYPOINT ${CATALINA_HOME}/bin/startup.sh && /bin/bash
CMD ["true"]
