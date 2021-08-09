# Build the OracleDNSServer Docker image

1. Clone the repository from the [DNS GIT repository](https://github.com/tthathac/docker-images/tree/patch-1)
2. cd OracleDatabase/RAC/OracleDNSServer/dockerfiles
3. export https_proxy=http://www-proxy-hqdc.us.oracle.com:80/
   export https_proxy=http://www-proxy-hqdc.us.oracle.com:80/
   ./buildDockerImage.sh -v 4.1
   docker images

   You should see that oracle/rac-dns-server:4.1 is created. You can name it anything based on your requirement.

# Create Network

docker network create --driver=bridge --subnet=172.16.1.0/24 rac_pub1_nw

docker network create --driver=bridge --subnet=192.168.17.0/24 rac_priv1_nw

# Create the DNS container
Use the following command ( replace appropriately if needed ) to create the DNS container.

/usr/bin/docker run -d --hostname racdns --dns-search=us.oracle.com \
--network=rac_pub1_nw --ip=172.16.1.25 \
-e DOMAIN_NAME="internal.us.oracle.com" \
-e PRIVATE_DOMAIN_NAME="internal-priv.us.oracle.com" \
-e WEBMIN_ENABLED=false \
-e RAC_NODE_NAME_PREFIX="racnode" \
-e SETUP_DNS_CONFIG_FILES="setup_true" \
-e CORP_DNS_DOMAIN_1="us.oracle.com" \
-e CORP_DNS_DOMAIN_2="dbdevtestphx.oraclevcn.com" \
-e CORP_DNS_SERVERS="100.96.241.2,100.96.241.194" \
--privileged=false \
--name rac-dnsserver oracle/rac-dns-server:19.3.0
