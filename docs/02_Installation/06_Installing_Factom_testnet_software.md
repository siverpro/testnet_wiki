[TOC]

[![YouTube Video Guide](http://img.youtube.com/vi/Qghv05RCMcE/0.jpg)](http://www.youtube.com/watch?v=Qghv05RCMcE)

------------------------------------
# Configure and run the Docker Engine
There are a number of ways to run dockerd and two effectively mutually exclusive ways to configure dockerd. The ways to run dockerd are discussed below, but it is also important to understand the two ways that it can be configured.

## Store Docker Swarm certificate and key
Make sure you store the Docker Swarm mainnet key and certificate on your system. The files can be found at [here](https://github.com/FactomProject/factomd-authority-toolkit/tree/master/tls)
You can store these files in the directory /etc/docker for instance:
```
sudo mkdir -p /etc/docker
sudo wget https://raw.githubusercontent.com/FactomProject/factomd-testnet-toolkit/master/tls/cert.pem -O /etc/docker/factom-mainnet-cert.pem
sudo wget https://raw.githubusercontent.com/FactomProject/factomd-testnet-toolkit/master/tls/key.pem -O /etc/docker/factom-mainnet-key.pem
sudo chmod 644 /etc/docker/factom-mainnet-cert.pem
sudo chmod 440 /etc/docker/factom-mainnet-key.pem
sudo chgrp docker /etc/docker/*.pem
```

## Choose one of the following options for configuring `dockerd`
You can either use the `/etc/docker/daemon.json` file to specify `dockerd`
options, or you can specify options on the command line. Note that while these
methods can be used together, *if the same option is specified in both
locations, `dockerd` will fail to start even if the options agree*. For this
reason it is best to either specify all options on the command line or all
options in `/etc/docker/daemon.json`.

### 1. Using `daemon.json` (recommended), works on all linux operating systems.

You can configure the docker daemon using a default config file, located at `/etc/docker/daemon.json`. Create this file if it does not exist.

Example configuration:
```
{
  "tls": true,
  "tlscert": "/path/to/cert.pem",
  "tlskey": "/path/to/key.pem",
  "hosts": ["tcp://0.0.0.0:2376", "unix:///var/run/docker.sock"]
}

```
### 2. Options on the command line

For the same options as described above, you would use the following command
line options:

```
dockerd -H=unix:///var/run/docker.sock -H=0.0.0.0:2376 --tls --tlscert=/path/to/cert.pem --tlskey=/path/to/key.pem
```

## Choose one of the following 3 options for starting dockerd
Remeber that if you specify an option on the command line, you can't have the
same option in your `/etc/docker/daemon.json` file.
### 1. On RedHat

Open (using `sudo`) `/etc/sysconfig/docker` in your favorite text editor.

Append `-H=unix:///var/run/docker.sock -H=0.0.0.0:2376 --tls --tlscert=<path to cert.pem> --tlskey=<path to key.pem>` to the pre-existing OPTIONS

Then, `sudo service docker restart`.

### 2. Using `systemd`

Run `sudo systemctl edit docker.service`. This creates an override directory at
`/etc/systemd/system/docker.service.d/` and an override file called
`override.conf`. Alternatively, you can create this directory and file manually
and you can give the file a more descriptive name so long as it ends with
`.conf`.

Edit the override file to match this:
```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd
```
and make sure that you add `"fd://"` to the `"hosts"` array in
`/etc/docker/daemon.json` if you are using it for your config.

If you are *not* using `/etc/docker/daemon.json` use the following for your
service file override.
```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2376 --tls --tlscert <path to cert.pem> --tlskey <path to key.pem>
```
Then reload the configuration and the `docker.service`
```
sudo systemctl daemon-reload
sudo systemctl restart docker.service
```

### 3. I don't want to use a process manager

You can manually start the docker daemon via:

```
sudo dockerd -H=unix:///var/run/docker.sock -H=0.0.0.0:2376 --tlscert=<path to cert.pem> --tlskey=<path to key.pem>
```
or just
```
sudo dockerd
```
if you are using the `/etc/docker/daemon.json` file for configuration.

## Troubleshooting

If `dockerd` fails to start review the error output carefully. It generally
tells you exactly what the problem is.

If you are using systemd and the service fails to start, finding the relevant
logs can be a challenge since the service is configured to just keep restarting
which can bury the logs.

In this case, stop the service: `sudo systemctl stop docker`.

Then manually start `dockerd`: `sudo dockerd`

You will then be able to see the `dockerd` output which should point you at the
problem. Fix those and then try starting the service with systemd again.

# Create the FactomD volumes

Factomd relies on two volumes,`factom_database` and `factom_keys`. Please create these before joining the swarm.

1. `docker volume create factom_database`
2. `docker volume create factom_keys`

These volumes will be used to store information by the `factomd` container.

If you already have a synced node and would like to avoid resyncing, run:

`sudo cp -r <path to your database> /var/lib/docker/volumes/factom_database/_data`.

If you used the old docker setup your database will most likely be in `/var/lib/docker/volumes/communitytestnet_factomd_volume/_data/m2/`

The directory in `_data` after the copy should be `custom-database`, as the volume is mounted at `$HOME/.factom/m2`.

In addition, please place your `factomd.conf` file in `/var/lib/docker/volumes/factom_keys/_data`. This file can also be found in `/var/lib/docker/volumes/communitytestnet_factomd_volume/_data/m2/`. If you don't have one yet, you can download factom.conf.EXAMPLE from the [testnet toolkit repo](https://github.com/FactomProject/factomd-testnet-toolkit).

# Join the Docker Swarm

Finally, to join the swarm:
```
docker swarm join --token SWMTKN-1-0bv5pj6ne5sabqnt094shexfj6qdxjpuzs0dpigckrsqmjh0ro-87wmh7jsut6ngmn819ebsqk3m 54.171.68.124:2377

```

As a reminder, joining as a worker means you have no ability to control containers on another node.

Once you have joined the swarm network, you will be issued a control panel login by the Testnet Adminstrator. Please submit this [form](https://docs.google.com/forms/d/e/1FAIpQLSd-t33chnGOyLZ6kJ-QC-L0EgOExzY7GQ8y9e0I0E4AIbdKBQ/viewform) and a staff member will contact you as soon as possible.

**Only accept logins at https://testnet.federation.factomd.com/. Any other login endpoints are fraudulent and not to be trusted.**

# Starting FactomD Container

There are two means of launching your `factomd` instance:

### From the Docker CLI (recommended and better tested)

Run this command _exactly_: `docker run -d --name "factomd" -v "factom_database:/root/.factom/m2" -v "factom_keys:/root/.factom/private" -p "8088:8088" -p "8090:8090" -p "8110:8110" -l "name=factomd" factominc/factomd:v5.4.0-alpine -broadcastnum=16 -network=CUSTOM -customnet=fct_community_test -startdelay=600 -faulttimeout=120 -config=/root/.factom/private/factomd.conf`

Note: This command will not always be updated. You should check the [Factom Docker repository](https://hub.docker.com/r/factominc/factomd/tags/) for the latest version. In the above command, you should replace `factominc/factomd:vX.Y.X-alpine` with the latest version.

### From the Portainer UI

Once you have logged into the [control panel](https://testnet.federation.factomd.com/), please ensure your node is selected in the top left dropdown menu.

Then, click `containers > add container`.

**:heavy_exclamation_mark: These instructions must be followed exactly, otherwise you risk being kicked from the authority set. :heavy_exclamation_mark:**

1. Name your container `factomd`.

2. Enter the image name `factominc/factomd:v5.4.0-alpine` (See note above regarding the latest version in docker repository)

3. Mark additional ports `8088:8088`, `8110`:`8110`, `8090:8090`.

4. Do _not_ modify access control.

5. Either this command for the command:  `-broadcastnum=16 -network=CUSTOM -customnet=fct_community_test -startdelay=600 -faulttimeout=120 -config=/root/.factom/private/factomd.conf`or your own flags. But be careful!

6. Click "volumes", and map `/root/.factom/m2` to `factom_database`, and `/root/.factom/private` to `factom_keys`.

7. Click "labels" and add a label `name:name` = `value:factomd`

8. Click "deploy the container"

9. You are done!

### NOTE: The Swarm cluster is still experimental, so please pardon our dust! If you have an issues, please contact ian at factom dot com.
