FROM ubuntu:18.04

# To stop tzdata from pausing the install.
ENV TZ=America/Los_Angeles
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt update
RUN apt-get -y update
RUN apt-get -y upgrade

RUN mkdir /home/scripts
WORKDIR /home/scripts

# install paicoin
ADD paicoin_install.sh /home/scripts
RUN sh -e paicoin_install.sh

# debugging tools install
ADD debugging_tools_install.sh /home/scripts
RUN sh -e debugging_tools_install.sh

# Miniconda
ADD miniconda_install.sh /home/scripts
RUN sh -e miniconda_install.sh

# Add file needed to create username/password for paicoin RPC
RUN ["/bin/bash", "-c", " mkdir -p /root/.paicoin/{testnet,regtest}"]
ADD paicoin.conf /root/.paicoin
ADD paicoin.conf /root/.paicoin/testnet
ADD paicoin.conf /root/.paicoin/regtest

ADD paicoin_server.py /home/scripts
ADD rpcauth.py /home/scripts
WORKDIR /home/scripts
CMD python3 paicoin_server.py
EXPOSE 8080 8009
