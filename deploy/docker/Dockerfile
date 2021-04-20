FROM haskell:8.10

RUN stack install happy-1.19.9 --resolver lts-13.26 || stack setup && stack install happy-1.19.9 --resolver lts-13.26

ENV PATH=/root/.local/bin:$PATH

ENV OTP_VERSION=23

RUN apt-get update \
    && apt-get install -y wget git curl build-essential debhelper \
    && wget https://packages.erlang-solutions.com/erlang-solutions_2.0_all.deb \
    && dpkg -i erlang-solutions_2.0_all.deb \
    && wget https://packages.erlang-solutions.com/debian/erlang_solutions.asc \
    && apt-key add erlang_solutions.asc \
    && apt update \
    && version=$(apt-cache madison esl-erlang |grep ${OTP_VERSION} | sed -n '1, 1p' | awk '{print $3}') \
    && apt-get install -y esl-erlang=${version}

COPY . hamler
RUN make -C hamler \
    && make -C hamler test \
    && make -C hamler install

WORKDIR /tmp/tests

RUN hamler init \ 
    && hamler build \
    && hamler run \
    && echo ":q" | hamler repl

WORKDIR /

CMD [ "hamler" ]
