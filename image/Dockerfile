
#FROM alpine:3.9
FROM cirepo/nix:2.2.1-alpine-3.9


USER root


ARG IMAGE_ARG_FILESERVER

# https://jdk.java.net/
ARG IMAGE_ARG_JAVA11_VERSION
ARG IMAGE_ARG_JAVA11_PACKAGE_DIGEST
# "For Alpine Linux, builds are produced on a reduced schedule and may not be in sync with the other platforms."


ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk-amd64
ENV PATH=${JAVA_HOME}/bin:${PATH}


COPY --from=cirepo/glibc:2.29-r0-alpine-3.9-archive /data/root /


COPY --chown=root:root docker /data

# wget -O /openjdk.tgz "https://download.java.net/java/GA/jdk11/9/GPL/openjdk-${IMAGE_ARG_JAVA11_VERSION:-11.0.2}_linux-x64_bin.tar.gz"; \
RUN set -eux \
  && if [[ ! -f /data/openjdk-${IMAGE_ARG_JAVA11_VERSION:-11.0.2}_linux-x64_bin.tar.gz ]]; then \
       ${ARIA2C_DOWNLOAD} -d /data -o openjdk-${IMAGE_ARG_JAVA11_VERSION:-11.0.2}_linux-x64_bin.tar.gz \
       ${IMAGE_ARG_FILESERVER:-https://download.java.net}/java/GA/jdk11/9/GPL/openjdk-${IMAGE_ARG_JAVA11_VERSION:-11.0.2}_linux-x64_bin.tar.gz; \
     fi \
  && echo "${IMAGE_ARG_JAVA11_PACKAGE_DIGEST:-99be79935354f5c0df1ad293620ea36d13f48ec3ea870c838f20c504c9668b57} */data/openjdk-${IMAGE_ARG_JAVA11_VERSION:-11.0.2}_linux-x64_bin.tar.gz" | sha256sum -c - \
  && mkdir -p "${JAVA_HOME}" \
  && tar --extract --file /data/openjdk-${IMAGE_ARG_JAVA11_VERSION:-11.0.2}_linux-x64_bin.tar.gz --directory "${JAVA_HOME}" --strip-components 1 \
  && rm /data/openjdk-${IMAGE_ARG_JAVA11_VERSION:-11.0.2}_linux-x64_bin.tar.gz \
  && cp -f /data/cacerts ${JAVA_HOME}/lib/security/cacerts \
  && rm -f /data/cacerts \
  \
# https://github.com/docker-library/openjdk/issues/212#issuecomment-420979840
# https://openjdk.java.net/jeps/341
  && java -Xshare:dump \
  \
# basic smoke test
  && java --version \
  && javac --version

USER alpine

# https://docs.oracle.com/javase/10/tools/jshell.htm
# https://docs.oracle.com/javase/10/jshell/
# https://en.wikipedia.org/wiki/JShell
CMD ["jshell"]
