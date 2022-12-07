FROM openjdk:17-alpine
ARG PACKAGE=postgres
ENV method=tpcc
ENV output=/result
ENV config_path=config/postgres/sample_tpcc_config.xml
RUN apk update && \
	apk add sudo git vim iproute2 curl && \
	git clone --depth 1 https://github.com/cmu-db/benchbase.git && \
	cd benchbase && \
	./mvnw clean package -P ${PACKAGE} && \
	tar xvzf target/benchbase-${PACKAGE}.tgz
WORKDIR /benchbase/benchbase-${PACKAGE}
RUN chmod -R g+w /benchbase/
CMD java -jar benchbase.jar -b ${method} -c ${config_path} -d ${output} --create=true --load=true --execute=true
