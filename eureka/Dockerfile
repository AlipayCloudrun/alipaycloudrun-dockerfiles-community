from springcloud/eureka:latest as origin

from joshkeegan/zip:latest as builder
copy --from=origin /app.jar /app.jar
copy application.yml /application.yml
run zip -u /app.jar application.yml

from openjdk:8-jre-slim
RUN sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list && \
    apt update && apt install -y --no-install-recommends curl ca-certificates iproute2 procps && \
    rm -rf /var/lib/apt/lists/*
COPY --from=builder /app.jar /app.jar
Entrypoint ["java", "-jar", "/app.jar"]
