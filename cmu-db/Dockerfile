FROM ubuntu:22.04
ARG PACKAGE=postgres
ENV method=tpcc
ENV config_path=config/postgres/sample_tpcc_config.xml
RUN \
    apt update -qy && \
    # some packages are installed for debug
    apt install -qy sudo git ca-certificates-java vim iproute2 curl openjdk-19-jre 
    #useradd -ms /bin/bash cmu
WORKDIR /home/cmu
RUN chgrp -R 0 /home/cmu && chmod -R g=u /home/cmu 
USER cmu
RUN \
    git clone --depth 1 https://github.com/cmu-db/benchbase.git && \
    cd benchbase && \
    ./mvnw clean package -P ${PACKAGE} && \
    tar xvzf target/benchbase-${PACKAGE}.tgz
WORKDIR /home/cmu/benchbase/benchbase-${PACKAGE}
ENTRYPOINT /usr/bin/java -jar benchbase.jar -b ${method} -c ${config_path} --create=true --load=true --execute=true
