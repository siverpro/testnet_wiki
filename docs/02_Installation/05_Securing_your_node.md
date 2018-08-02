[TOC]

## Firewall
In order to have your node join the swarm (or even function properly), you need to expose a couple of ports. The set up depends on whether you use an external firewall (or NAT), such as AWS or hosting at home, or rely on the node's own firewall to secure it (most VPS-services). If you do not want to join the authority set, only port 8110 needs to be opened to the public.

### External firewall / NAT
No firewall needs to be configured on the node itself, but it can still be set up for added security in case your external firewall gets compromised.
You need to allow access to port 2376, 2222 and 8088 _ONLY TO_ 54.171.68.124. Failure to do this properly can compromise your node. Port 8090 to public is beneficial for testnet debugging. Port 8110 is required to be open to the public, this is the port the network communicates on.
The steps to do this varies greatly by your individual set up (NAT or not, firewall/router model, etc..)

### Internal firewall

In order to join the swarm, first ensure that your firewall rules allow access on the following ports. All swarm communications occur over a self-signed TLS certificate. Due to the way iptables and docker work you cannot use the `INPUT` chain to block access to apps running in a docker container as it's not a local destination but a `FORWARD` destination. By default when you map a port into a docker container it opens up to `any` host. To restrict access we need to add our rules in the `DOCKER-USER` chain [reference](https://docs.docker.com/network/iptables/).

- TCP port `2376` _only to_ `54.171.68.124` for secure Docker engine communication. This port is required for Docker Machine to work. Docker Machine is used to orchestrate Docker hosts. As this is a local service we use the `INPUT` chain.

In addition,  the following ports must be opened for factomd to function which we add to the `DOCKER-USER` chain:
- `2222` to `54.171.68.124`, which is the SSH port used by the `ssh` container
- `8088` to `54.171.68.124`, the factomd API port
- `8090` to `0.0.0.0`, the factomd Control panel
  - Keeping this open to the world is beneficial on testnet for debugging purposes
- `8110` to `0.0.0.0`, the factomd testnet port

An example using `iptables`:
```
sudo iptables -A INPUT ! -s 54.171.68.124/32 -p tcp -m tcp --dport 2376 -m conntrack --ctstate NEW,ESTABLISHED -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER ! -s 54.171.68.124/32  -i <external if> -p tcp -m tcp --dport 8090 -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER ! -s 54.171.68.124/32  -i <external if> -p tcp -m tcp --dport 2222 -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER ! -s 54.171.68.124/32  -i <external if> -p tcp -m tcp --dport 8088 -j REJECT --reject-with icmp-port-unreachable
sudo iptables -A DOCKER-USER -p tcp -m tcp --dport 8110 -j ACCEPT
```
Replace `<external if>` with the name of the interface you use to connect to the internet eg. eth0 or ens0. To see interfaces use `ip addr list`
  
Don't forget to [save](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands#saving-rules) the rules!

## Harden SSH Access
### 1. Create an authentication key-pair
#### Linux
This is done on your local computer, not your node, and will create a 4096-bit RSA key-pair. During creation, you will be given the option to encrypt the private key with a passphrase. This means that it cannot be used without entering the passphrase, unless you save it to your local desktop’s keychain manager. We suggest you use the key-pair with a passphrase, but you can leave this field blank if you don’t want to use one.
    
    ssh-keygen -b 4096
    
Press Enter to use the default names `id_rsa` and `id_rsa.pub` in `/home/your_username/.ssh` before entering your passphrase.

Now copy your key to your node (replace the username and ip with appropriate values)

    ssh-copy-id example_user@173.14.222.121
    
Exit and log back in to your node. If you specified a passphrase, you need to enter it here.
    
#### Windows
Download and install PuTTy (use the MSI installer as it includes puttygen)
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

Select RSA and increase to a 4096-bits key in the bottom right field and generate a key. Type in a passphrase (optional, recommended). Now save your keys.

Now copy the entire public key, it starts with `ssh-rsa` and ends with == followed by the key comment. On your node, create your .ssh folder if it does not already exist. Now create and/or edit the file `./ssh/authorized_keys` and paste your key here.

The next time you use PuTTy to connect, go to your Connection -> SSH -> Auth setting and browse to the PRIVATE key you saved earlier. Save the connection and try to connect. It should now connect using your SSH key instead of password.

### 2. SSH Daemon options

The most important setting is now allowing root over ssh. Edit /etc/ssh/sshd_config using your favorite editor:

    sudo nano /etc/ssh/sshd_config
    
Uncomment if necessary and set `PermitRootLogin no`

If you use keypair-based login as outlined above, it is a good idea to disable password based login. Uncomment and set `PasswordAuthentication no`
