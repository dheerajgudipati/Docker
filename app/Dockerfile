FROM openjdk:8-jre-alpine

MAINTAINER dheerajgudipati<dheeraj.vn513@gmail.com>

ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME

RUN apk add --no-cache gnupg

ENV GPG_KEYS 05AB33110949707C93A279E3D3EFE6B686867BA6 07E48665A34DCAFAE522E5E6266191C37C037D42 47309207D818FFD8DCD3F83F1931D684307A10A5 541FBE7D8F78B25E055DDEE13C370389288584E7 61B832AC2F1C5A90F0F9B00A1C506407564C17A3 79F7026C690BAA50B92CD8B66A3AD3F4F22C4FED 9BA44C2621385CB966EBA586F72C284D731FABEE A27677289986DB50844682F8ACB77FC2E86E29AC A9C5DF4D22E99998D9875A5110C01C5A2F6059E7 DCFD35E0BF8CA7344752DE8B6FB21E8933C60243 F3A04C595DB5B6A5F1ECA43E3B7BBB100D811BBE F7DA48BB64BCB84ECBA7EE6935CD23C10D498E23

RUN set -ex; \
	for key in $GPG_KEYS; do \
		gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
	done


ENV TOMCAT_TGZ_URL https://www.apache.org/dyn/closer.cgi?action=download&filename=tomcat/tomcat-9/v9.0.0.M19/bin/apache-tomcat-9.0.0.M19.tar.gz
ENV TOMCAT_ASC_URL https://www.apache.org/dist/tomcat/tomcat-9/v9.0.0.M19/bin/apache-tomcat-9.0.0.M19.tar.gz.asc

RUN set -x \
	\
	&& apk add --no-cache --virtual .fetch-deps \
		ca-certificates \
		tar \
		openssl \
	&& wget -O tomcat.tar.gz "$TOMCAT_TGZ_URL" \
	&& wget -O tomcat.tar.gz.asc "$TOMCAT_ASC_URL" \
	&& gpg --batch --verify tomcat.tar.gz.asc tomcat.tar.gz \
	&& tar -xvf tomcat.tar.gz --strip-components=1 \
	&& rm bin/*.bat \
	&& rm tomcat.tar.gz* \
	\
	&& nativeBuildDir="$(mktemp -d)" \
	&& tar -xvf bin/tomcat-native.tar.gz -C "$nativeBuildDir" --strip-components=1 \
	&& apk add --no-cache --virtual .native-build-deps \
		openjdk8 \
	&& ( \
		export CATALINA_HOME="$PWD" \
	) \
	&& rm -rf "$nativeBuildDir" \
	&& rm bin/tomcat-native.tar.gz



EXPOSE 8080
CMD ["catalina.sh", "run"]