# syntax = docker/dockerfile:1.4.0
FROM python:3.8.6-slim-buster

WORKDIR /fastapi-faust-example
RUN mkdir ~/.pip
RUN <<EOF cat > ~/.pip/pip.conf
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
EOF
# install pipenv packages
RUN pip install pipenv==2020.11.15

COPY Pipfile ./
COPY Pipfile.lock ./

RUN pipenv install --system --deploy

# uninstall pipenv
RUN pip install pip-autoremove==0.9.1
RUN pip-autoremove pipenv -y
RUN pip uninstall pip-autoremove -y

RUN rm Pipfile Pipfile.lock

# set up the wait script
RUN apt-get update \
  && apt-get install -y netcat \
  && rm -rf /var/lib/apt/lists/*

COPY bin/wait-for/ ./bin/wait-for/
RUN chmod u+x ./bin/wait-for/wait-for.sh

# copy source files
COPY app ./app
