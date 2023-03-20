FROM rabbitmq:3.11-management

RUN apt update && apt install -y curl dnsutils iputils-ping
ADD conf/* /etc/rabbitmq/conf.d
COPY docker-entrypoint.sh /usr/local/bin/
RUN rabbitmq-plugins --offline enable rabbitmq_peer_discovery_consul
