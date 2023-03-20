FROM rabbitmq:3.11-management

RUN apt update && apt install -y curl dnsutils iputils-ping && apt clean
RUN rabbitmq-plugins --offline enable rabbitmq_peer_discovery_consul
COPY --chmod=755 <<'OEOF' /usr/local/bin/docker-entrypoint.sh
#!/usr/bin/env bash
set -eu

CLUSTER_CONFIG_FILE=/etc/rabbitmq/conf.d/20-cluster.conf
# set cluster_formation config
if [ "${RABBITMQ_CLUSTER_CONSUL_HOST:-}" ]; then
	echo >&2
	echo >&2 "INFO: using consul at $RABBITMQ_CLUSTER_CONSUL_HOST as cluster_formation"
	echo >&2
	cat > $CLUSTER_CONFIG_FILE <<EOF
cluster_formation.peer_discovery_backend = consul
cluster_formation.consul.host = $RABBITMQ_CLUSTER_CONSUL_HOST

cluster_formation.consul.svc = rabbitmq
# do compute service address
cluster_formation.consul.svc_addr_auto = true
cluster_formation.consul.svc_addr_use_nodename = true
# use long RabbitMQ node names?
cluster_formation.consul.use_longname = false
EOF
elif [ "${RABBITMQ_CLUSTER_CLASSIC_NODES:-}" ]; then
	echo >&2
	echo >&2 "INFO: using classic_nodes $RABBITMQ_CLUSTER_CLASSIC_NODES as cluster_formation"
	echo >&2
	echo "cluster_formation.peer_discovery_backend = classic_config" >> $CLUSTER_CONFIG_FILE
	NODE_IDX=1
	for node in ${RABBITMQ_CLUSTER_CLASSIC_NODES}; do
		echo "cluster_formation.classic_config.nodes.$NODE_IDX = rabbit@$node" >>$CLUSTER_CONFIG_FILE
		((NODE_IDX++))
	done
elif [ "${RABBITMQ_CLUSTER_STANDALONE:-}" ]; then
	echo >&2
	echo >&2 "INFO: standalone mode, do not scale up."
	echo >&2
else
	echo >&2
	echo >&2 "WARNING: no available cluster_formation detect, hold for further configuration."
	echo >&2
    exec cat
fi

exec "$@"
OEOF
