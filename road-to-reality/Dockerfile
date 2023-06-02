# for road-to-reality essays:
#      https://github.com/mentat-collective/road-to-reality
#      docker run --rm -p 9630:9630 -p 7777:7777 -p 8765:8765 -v /my/local/directory/linked/to/essays:/road-to-reality/essays/reality -it --name road-to-reality mettinger/road-to-reality:latest

FROM ubuntu:20.04
RUN apt-get update && apt-get install -y openjdk-17-jre
RUN apt-get -y install curl
RUN apt-get -y install npm
RUN apt-get -y install nano
RUN apt-get -y install socat

RUN apt-get update && \
    apt-get install -y -q --allow-unauthenticated \
    git \
    sudo 

RUN git clone https://github.com/mentat-collective/road-to-reality.git

RUN useradd -m -s /bin/zsh linuxbrew && \
    usermod -aG sudo linuxbrew &&  \
    mkdir -p /home/linuxbrew/.linuxbrew && \
    chown -R linuxbrew: /home/linuxbrew/.linuxbrew
USER linuxbrew
RUN /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

ENV PATH="/home/linuxbrew/.linuxbrew/bin:${PATH}"

RUN brew install borkdude/brew/babashka
RUN brew install clojure/tools/clojure

USER root
RUN chown -R $CONTAINER_USER: /home/linuxbrew/.linuxbrew
ENV PATH="/home/linuxbrew/.linuxbrew/bin:${PATH}"

EXPOSE 7777
CMD cd road-to-reality; \
    git reset --hard origin; \
    git pull; \
    socat TCP4-LISTEN:7777,bind=`hostname -I | tr -d '[:space:]'`,fork TCP4:localhost:7777 & \
    bb clerk-watch & \
    bash
