## Firewall

In order to join the swarm, first ensure that your firewall rules allow access on the following ports. All swarm communications occur over a self-signed TLS certificate. Due to the way iptables and docker work you cannot use the `INPUT` chain to block access to apps running in a docker container as it's not a local destination but a `FORWARD` destination. By default when you map a port into a docker container it opens up to `any` host. To restrict access we need to add our rules in the `DOCKER-USER` chain [reference](https://docs.docker.com/network/iptables/).

- TCP port `2376` _only to_ `54.171.68.124` for secure Docker engine communication. This port is required for Docker Machine to work. Docker Machine is used to orchestrate Docker hosts. As this is a local service we use the `INPUT` chain.

In addition,  the following ports must be opened for factomd to function which we add to the `DOCKER-USER` chain:
- `2222` to `54.171.68.124`, which is the SSH port used by the `ssh` container
- `8088` to `54.171.68.124`, the factomd API port
- `8090` to `0.0.0.0`, the factomd Control panel
  - Keeping this open to the world is beneficial on testnet for debugging purposes
- `8110` to the world, the factomd testnet port

An example using `iptables`:
```
sudo iptables -A INPUT ! -s 54.171.68.124/32 -p tcp -m tcp --dport 2376 -m conntrack --ctstate NEW,ESTABLISHED -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER ! -s 54.171.68.124/32  -i <ext_if> -p tcp -m tcp --dport 8090 -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER ! -s 54.171.68.124/32  -i <ext_if> -p tcp -m tcp --dport 2222 -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER ! -s 54.171.68.124/32  -i <ext_if> -p tcp -m tcp --dport 8088 -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER -p tcp -m tcp --dport 8110 -j ACCEPT
```

Don't forget to [save](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands#saving-rules) the rules!
