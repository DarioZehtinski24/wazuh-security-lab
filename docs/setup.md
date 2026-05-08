# Wazuh Lab Setup



## Environment



- Host: VMware Workstation

- Guest OS: Ubuntu Server 24.04.4 LTS

- Disk: 50 GB

- Network: Bridged adapter

- Wazuh version: 4.14.4

- Deployment type: Docker single-node



## Main Steps



1. Installed Ubuntu Server in VMware

2. Configured network adapter as Bridged

3. Installed Docker and Docker Compose



### Set Required Kernel Parameter



```bash

sudo sysctl -w vm.max_map_count=262144

echo "vm.max_map_count=262144" | sudo tee /etc/sysctl.d/99-wazuh.conf

sudo sysctl --system

```



### Clone Wazuh Docker Repository



```bash

git clone https://github.com/wazuh/wazuh-docker.git -b v4.14.4

cd wazuh-docker/single-node

```



### Generate Certificates



```bash

docker-compose -f generate-indexer-certs.yml run --rm generator

```



### Start Wazuh



```bash

docker-compose up -d

```



## Verification



- Installed and connected Wazuh agent

- Verified the agent appeared as active in the Wazuh dashboard
