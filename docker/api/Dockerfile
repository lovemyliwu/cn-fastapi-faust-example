FROM python:3.8.6-slim-buster

WORKDIR /fastapi-faust-example

# install pipenv packages
RUN mkdir ~/.pip
COPY pip.conf ~/.pip/
RUN pip install -i https://mirrors.aliyun.com/pypi/simple/ pipenv==2020.11.15

COPY Pipfile ./
COPY Pipfile.lock ./

RUN pipenv install --system --deploy --pypi-mirror https://mirrors.aliyun.com/pypi/simple/

# uninstall pipenv
RUN pip install -i https://mirrors.aliyun.com/pypi/simple/ pip-autoremove==0.9.1
RUN pip-autoremove pipenv -y
RUN pip uninstall pip-autoremove -y

RUN rm Pipfile Pipfile.lock

# set up the wait script
RUN rm /etc/apt/sources.list
COPY sources.list /etc/apt/

RUN apt-get update \
  && apt-get install -y netcat \
  && rm -rf /var/lib/apt/lists/*

COPY bin/wait-for/ ./bin/wait-for/
RUN chmod u+x ./bin/wait-for/wait-for.sh

# copy source files
COPY app ./app
