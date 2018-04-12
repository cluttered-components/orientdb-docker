############################################################
# Dockerfile to run an OrientDB (Graph) Container
############################################################

######## base ########
FROM openjdk:8-jre-alpine as base

MAINTAINER OrientDB LTD (info@orientdb.com)

# Override the orientdb download location with e.g.:
#   docker build -t mine --build-arg ORIENTDB_DOWNLOAD_SERVER=http://repo1.maven.org/maven2/com/orientechnologies/ .
ARG ORIENTDB_DOWNLOAD_SERVER

ENV ORIENTDB_VERSION 2.2.33
ENV ORIENTDB_DOWNLOAD_MD5 163fc4f24c7b7d7a8179a7d10f49903e
ENV ORIENTDB_DOWNLOAD_SHA1 ddf34eda1d03484a2e1ace0cbc4abc3100c86400

ENV ORIENTDB_DOWNLOAD_URL ${ORIENTDB_DOWNLOAD_SERVER:-http://central.maven.org/maven2/com/orientechnologies}/orientdb-community/$ORIENTDB_VERSION/orientdb-community-$ORIENTDB_VERSION.tar.gz

ENV PATH /orientdb/bin:$PATH

RUN apk update &&\
    apk update --no-cache &&\
    mkdir /orientdb


######## build ########
FROM base as build

RUN apk add --update --no-cache tar &&\
    wget  $ORIENTDB_DOWNLOAD_URL &&\
    echo "$ORIENTDB_DOWNLOAD_MD5 *orientdb-community-$ORIENTDB_VERSION.tar.gz" | md5sum -c - &&\
    echo "$ORIENTDB_DOWNLOAD_SHA1 *orientdb-community-$ORIENTDB_VERSION.tar.gz" | sha1sum -c - &&\
    tar -xvzf orientdb-community-$ORIENTDB_VERSION.tar.gz -C /orientdb --strip-components=1 &&\
    rm -rf /orientdb/databases/*


######## production #######
FROM base

WORKDIR /orientdb

COPY --from=build /orientdb /orientdb

VOLUME ["/orientdb/backup", "/orientdb/databases", "/orientdb/config"]

#OrientDb binary and http
EXPOSE 2424 2480

# Default command start the server
CMD ["server.sh"]
