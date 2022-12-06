FROM ubuntu:22.04
ARG PACKAGE=postgres
ENV method=tpcc
ENV config_path=config/postgres/sample_tpcc_config.xml
RUN apt update -qy && \
    apt install -qy sudo git ca-certificates-java vim iproute2 curl openjdk-19-jre && \
    addgroup --system cmu && adduser --system --group cmu
WORKDIR /home/cmu
RUN git clone --depth 1 https://github.com/cmu-db/benchbase.git && \
    cd benchbase && \
    ./mvnw clean package -P ${PACKAGE} && \
    tar xvzf target/benchbase-${PACKAGE}.tgz
WORKDIR /home/cmu/benchbase/benchbase-${PACKAGE}
RUN chown -R cmu:cmu /home/cmu
USER cmu
ENTRYPOINT /usr/bin/java -jar benchbase.jar -b ${method} -c ${config_path} --create=true --load=true --execute=true
