FROM docker-java-py3-opencv420:latest
MAINTAINER fjuillet <fjuillet@gmail.com>

ARG DEBIAN_FRONTEND=noninteractive

########################################
#### update and upgrade OS
########################################
RUN apt -y update \
	&& apt -y upgrade

########################################
#### install maven
#### https://github.com/carlossg/docker-maven/tree/master/openjdk-14
########################################
ENV USER_HOME_DIR /root
RUN set -eux; \
    MAVEN_VERSION=3.6.3; \
    SHA=c35a1803a6e70a126e80b2b3ae33eed961f83ed74d18fcd16909b2d44d7dada3203f1ffe726c17ef8dcca2dcaa9fca676987befeadc9b9f759967a8cb77181c0; \
    BASE_URL=https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries; \
    BIN_URL=${BASE_URL}/apache-maven-${MAVEN_VERSION}-bin.tar.gz; \
    mkdir -p /usr/share/maven /usr/share/maven/ref; \
    echo "Downloading archive : ${BIN_URL} ..."; \
    curl -fsSL -o /tmp/apache-maven.tar.gz -L ${BIN_URL}; \
    echo "${SHA}  /tmp/apache-maven.tar.gz" | sha512sum -c -; \
    tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1; \
    rm -f /tmp/apache-maven.tar.gz; \
    ln -s /usr/share/maven/bin/mvn /usr/bin/mvn;

ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"
ENV PATH /usr/share/maven/bin:${PATH}

COPY mvn-entrypoint.sh /usr/local/bin/mvn-entrypoint.sh
COPY settings-docker.xml /usr/share/maven/ref/

ENTRYPOINT ["/usr/local/bin/mvn-entrypoint.sh"]
CMD ["mvn"]
