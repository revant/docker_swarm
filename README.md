# <p align="center">Docker Swarm</p>
### <p align="center">A deployment method and collection of basic docker swarm stacks.</p>

## tl;dr
* **Fresh Debian 10 Server + 80 and 443 forwarded + DNS configured**
```
sudo groupadd docker && sudo usermod -aG docker $USER && newgrp docker
```
```
curl -fsSL https://raw.githubusercontent.com/suodrazah/docker_swarm/main/deploy.sh -o deploy.sh && sh deploy.sh
```

## Prerequisites:
* **Fresh Debian 10 or Ubuntu Server 20.04 LTS**
  * Local
  * [VPS at Linode](https://www.linode.com/products/shared/)
    * $5+/month
  * [VPS at OVH](https://ca.ovh.com/au/order/vps/)
    * $5+/month
  * [VPS at Oracle](https://www.oracle.com/au/cloud/free/)
    * $0+/month <sub>(free tier is slow, useful for light stacks only)</sub>
* **SSH Access as sudo user (not root)**
* **Firewall configured to allow 80/tcp, 443/tcp, 22/tcp**
* **Public, static IP**
* **Domain pointing to servers public IP address**
  * [Google Domains](https://domains.google.com/)
    * $18+/year
  * [No-IP](https://domains.google.com/)
    * free <br> <sub>Wildcards are not free so you will have to configure subdomains manually <br> e.g. traefik.example.ddns.net, portainer.example.ddns.net, etc</sub>

## Deployment:
* **This will bring up Traefik and Portainer on a manager Docker Swarm node**
```
sudo groupadd docker && sudo usermod -aG docker $USER && newgrp docker
```
```
curl -fsSL https://raw.githubusercontent.com/suodrazah/docker_swarm/main/deploy.sh -o deploy.sh && sh deploy.sh
```

## [Extension](https://github.com/suodrazah/docker_swarm/tree/main/stacks) (i.e. add stacks):
* Check the stack Readme.md or header comments
* Create a new stack in Portainer:
   * `Portainer`
   * `Stacks`
   * `Add Stack`
   * `Name` - e.g. site1-ignition
   * `Web editor` - copy contents of stack.yml file
   * `Environment variables` - as described by the stack Readme.md

## Add Nodes
* Specific subdomain (e.g. worker1.example.com) shall be configured to point at the new node public IP. Note this subdomain for entry as the node domain when requested upon execution of the script. You can use private IPs instead if so desired.
* Additional configuration of your [firewalls](https://docs.docker.com/engine/swarm/swarm-tutorial/#open-protocols-and-ports-between-the-hosts) is required for swarm communication

* To add a worker node run this on an existing manager node and copy the output:
```
docker swarm join-token worker
```
* On the node to be added (again, as a sudo user and not root):
```
sudo groupadd docker && sudo usermod -aG docker $USER && newgrp docker
```

```
curl -fsSL https://raw.githubusercontent.com/suodrazah/docker_swarm/main/add_worker.sh -o add_worker.sh && sh add_worker.sh
```
* Output you copied earlier from join-token command above
```
docker swarm join --token KEY IP:2377
```

* Add a label to the new node
   * `Portainer`
   * `Swarm`
   * `<node>`
   * `+Label`
   * `Name` - <Node Name> e.g. worker1
   * `Value` - "True"

 ## To Do
 
 * Fix Traefik headers
