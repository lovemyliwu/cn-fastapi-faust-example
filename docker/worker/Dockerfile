# this image has both python and rocksDB installed
FROM toh995/rocksdb-python:6.14.5_3.8.6-slim-buster

WORKDIR /fastapi-faust-example

# install pipenv packages
RUN mkdir ~/.pip
COPY pip.conf ~/.pip/
RUN pip install -i https://mirrors.aliyun.com/pypi/simple/ pipenv==2020.11.15

COPY Pipfile ./
COPY Pipfile.lock ./

RUN pipenv install --system --deploy  --pypi-mirror https://mirrors.aliyun.com/pypi/simple/

# uninstall pipenv
RUN pip install -i https://mirrors.aliyun.com/pypi/simple/ pip-autoremove==0.9.1
RUN pip-autoremove pipenv -y
RUN pip uninstall pip-autoremove -y

RUN rm Pipfile Pipfile.lock

# install python-rocksdb
RUN >/etc/apt/sources.list
RUN sed -i "1i deb https://mirrors.aliyun.com/debian/ buster main non-free contrib \
\ndeb-src https://mirrors.aliyun.com/debian/ buster main non-free contrib \
\ndeb https://mirrors.aliyun.com/debian-security buster/updates main \
\ndeb-src https://mirrors.aliyun.com/debian-security buster/updates main \
\ndeb https://mirrors.aliyun.com/debian/ buster-updates main non-free contrib \
\ndeb-src https://mirrors.aliyun.com/debian/ buster-updates main non-free contrib \
\ndeb https://mirrors.aliyun.com/debian/ buster-backports main non-free contrib  \
\ndeb-src https://mirrors.aliyun.com/debian/ buster-backports main non-free contrib" /etc/apt/sources.list
RUN cat /etc/apt/sources.list
ARG ROCKSDB_DEPENDENCIES="g++ libbz2-dev liblz4-dev libsnappy-dev libz-dev"

RUN apt-get update \
  && apt-get install -y $ROCKSDB_DEPENDENCIES \
  && rm -rf /var/lib/apt/lists/*

RUN pip install python-rocksdb==0.7.0

# set up the wait script
RUN apt-get update \
  && apt-get install -y netcat \
  && rm -rf /var/lib/apt/lists/*

COPY bin/wait-for/ ./bin/wait-for/
RUN chmod u+x ./bin/wait-for/wait-for.sh

# copy source files
COPY app ./app
