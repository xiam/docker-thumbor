FROM ubuntu:14.04

MAINTAINER Edu Herraiz <ghark@gmail.com>

VOLUME /logs
VOLUME /data

# Things required for a python/pip environment
COPY system-requirements.txt /usr/src/app/system-requirements.txt

RUN apt-get update && apt-get -y upgrade && apt-get -y autoremove

RUN apt-get install -y build-essential checkinstall ca-certificates wget

RUN apt-get install -y libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev

RUN cd /usr/src && wget https://www.python.org/ftp/python/2.7.10/Python-2.7.10.tgz && \
  tar xvzf Python-2.7.10.tgz && \
  cd Python-2.7.10 && \
  ./configure && \
  make altinstall && \
  python2.7 -V && \
  wget https://bootstrap.pypa.io/get-pip.py && \
  python2.7 get-pip.py

RUN  xargs apt-get -y -q install < /usr/src/app/system-requirements.txt && \
    apt-get clean

ENV HOME /usr/src/app
ENV SHELL bash
ENV WORKON_HOME /usr/src/app
WORKDIR /usr/src/app

COPY requirements.txt /usr/src/app/requirements.txt
RUN pip install --no-cache-dir -r /usr/src/app/requirements.txt

COPY conf/thumbor.conf.tpl /usr/src/app/thumbor.conf.tpl

RUN \
    ln /usr/lib/python2.7/dist-packages/cv2.so /usr/local/lib/python2.7/cv2.so && \
    ln /usr/lib/python2.7/dist-packages/cv.py /usr/local/lib/python2.7/cv.py

COPY docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["thumbor"]

EXPOSE 8000
