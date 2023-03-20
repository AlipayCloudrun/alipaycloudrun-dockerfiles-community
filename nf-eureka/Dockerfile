FROM joshkeegan/zip:latest as builder
ARG VERSION=1.10.18
ADD https://repo1.maven.org/maven2/com/netflix/eureka/eureka-server/${VERSION}/eureka-server-${VERSION}.war /eureka.war
WORKDIR /eureka 
RUN unzip /eureka.war 

FROM tomcat:9-jre8
RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list && \
    apt update && apt install -y --no-install-recommends curl ca-certificates iproute2 procps && \
    rm -rf /var/lib/apt/lists/*
COPY --from=builder /eureka/ /usr/local/tomcat/webapps/eureka/
COPY *.properties /usr/local/tomcat/webapps/eureka/WEB-INF/classes/
