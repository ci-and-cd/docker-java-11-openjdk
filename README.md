# docker-java-11-openjdk

docker-java-11-openjdk


OpenJDK11 installed at `/usr/lib/jvm/java-11-openjdk` (travis-ci style JAVA_HOME)


Dockerfile [ci-and-cd/docker-java-11-openjdk on Github](https://github.com/ci-and-cd/docker-java-11-openjdk)

[cirepo/java-openjdk on Docker Hub](https://hub.docker.com/r/cirepo/java-11-openjdk/)


The main caveat to note is that it does use musl libc instead of glibc and friends,
so certain software might run into issues depending on the depth of their libc requirements.
However, most software doesn't have an issue with this,
so this variant is usually a very safe choice.


## Use this image as a “stage” in multi-stage builds

```dockerfile

FROM alpine:3.9
COPY --from=cirepo/glibc:2.29-r0-alpine-3.9-archive /data/root /
COPY --from=cirepo/java-oracle:11.0.2-alpine-3.9-archive /data/root/usr/lib/jvm/java-11-oracle /usr/lib/jvm/java-11-oracle

```
