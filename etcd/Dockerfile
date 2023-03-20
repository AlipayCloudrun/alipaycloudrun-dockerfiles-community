FROM quay.io/coreos/etcd:v3.4.22
RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list && \
    apt update && apt install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*
COPY cloudrun-entry.sh /cloudrun-entry.sh
ENTRYPOINT ["sh", "-c", "/cloudrun-entry.sh"]
