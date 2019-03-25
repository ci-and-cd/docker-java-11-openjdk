# docker-java-11-openjdk

docker-java-11-openjdk


OpenJDK11 installed at `/usr/lib/jvm/java-11-openjdk-amd64` (travis-ci style JAVA_HOME)


Dockerfile [ci-and-cd/docker-java-11-openjdk on Github](https://github.com/ci-and-cd/docker-java-11-openjdk)

[cirepo/java-11-openjdk on Docker Hub](https://hub.docker.com/r/cirepo/java-11-openjdk/)


The main caveat to note is that it does use musl libc instead of glibc and friends,
so certain software might run into issues depending on the depth of their libc requirements.
However, most software doesn't have an issue with this,
so this variant is usually a very safe choice.


## Use this image as a “stage” in multi-stage builds

```dockerfile

FROM alpine:3.9
COPY --from=cirepo/glibc:2.29-r0-alpine-3.9-archive /data/root /
COPY --from=cirepo/java-11-openjdk:11.0.2-alpine-3.9-archive /data/root/usr/lib/jvm/java-11-openjdk-amd64 /usr/lib/jvm/java-11-openjdk-amd64

```


If you encounter 'javax.net.ssl.SSLPeerUnverifiedException: peer not authenticated' issue, 
you can list CA certs by `keytool -cacerts -list`.

Using cert from https://www.azul.com/downloads/zulu/ can solve most of cert issues.
```bash
docker run --rm cirepo/java-11-zulu:11.0.2-alpine-3.9 cat /usr/lib/jvm/zulu-11/lib/security/cacerts > image/docker/cacerts
# or
docker run --rm cirepo/java-11-zulu:11.0.2-alpine-3.9 /bin/base64 /usr/lib/jvm/zulu-11/lib/security/cacerts | tr -d \\n | base64 -d > image/docker/cacerts
```

Verify `sha256sum image/docker/cacerts`.
Replace /usr/lib/jvm/zulu-11/lib/security/cacerts with image/docker/cacerts from zulu.
