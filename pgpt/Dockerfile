#FROM nitincypher/docker-ubuntu-python-pip
#FROM python:3.11-rc-slim
FROM python:3.11.5-bookworm

USER root

WORKDIR /sqlite

RUN set -ex \
    && apt-get -y upgrade \
    && apt-get -y update \
    && apt-get -y install curl git gcc make wget libreadline-dev \
    && wget https://www.sqlite.org/2023/sqlite-autoconf-3430200.tar.gz \
    && tar -xvf sqlite-autoconf-3430200.tar.gz \
    && cd sqlite-autoconf-3430200 \
    && ./configure \
    && make \
    && make install \
    && curl -ssL https://install.python-poetry.org | python3 - \
    && export PATH=$PATH:/root/.local/bin:/usr/local/bin 

ENV PATH="$PATH:/root/.local/bin:/usr/local/bin"

WORKDIR /pgpt

RUN set -ex \
    && git clone https://github.com/imartinez/privateGPT.git \
    && cd privateGPT \
    && poetry install --with ui \
    && cd models \
    && poetry install --with local \
    && poetry run python ../scripts/setup

WORKDIR /pgpt/privateGPT

COPY entry.sh /pgpt/privateGPT

EXPOSE 8001

ENTRYPOINT ["./entry.sh"]
    

