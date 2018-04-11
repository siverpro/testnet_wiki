## Why does my node take so long time after boot to start progressing its block height?
In the control panel, you can see the [More Detailed Node Info Page](https://docs.factom.com/#more-detailed-node-information-page). 

At the top left corner in the more detailed node information page, looking at the underscores to the right of FNode0, there will be a W (Wait mode) for 10 minutes, then an I (Ignore mode) for another 10 minutes. 

**Wait mode** is the mode the servers stay at while booting to give the other nodes a chance to finish booting. It is collecting messages, but not sending acknowledgements. 

**Ignore mode** is where the server is sending out acknowledgements and EOMs (End Of Messages), but is not requesting missing messages. The intent is to see only fresh messages, but it is fragile because it cannot get any messages that it had missed, which is one of the things that helps keep the network robust.

After the Wait- and Ignore modes go away, your node will request new blocks and process list items from its peers, and will catch up.

## Why can't I see the Test Credits that were just transferred to my address?

Your node needs to be fully synced before you can see any transactions to or from your address.

## Why do I get a "No such file or directory" error when trying to run the script for publishing my new ID to the blockchain?

This error often happens because the command to generate an ID and public and private keys is missing flags at the end of the command. When you run:

`docker exec factomd_node serveridentity full elements EsXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX -n create -f`

Be sure to include '-n create -f'


## Why do I get an error when adding the configuration file to the docker?

This can be caused by using an older pdf version of the installation instructions where an extra space is inserted before 'destination/m2/factomd.conf'. Try running the below command:

`docker run --rm -v ${PWD}/factomd.conf:/source -v communitytestnet_factomd_volume:/destination busybox /bin/cp /source /destination/m2/factomd.conf`


## Why do I get an error trying to modify my node to utilize its new identity?

This can be caused by using an older pdf version of the installation instructions where it seems there is a line break in the command right after 'A-2'. This gives the error 'no such command as create.conf'. However, the command is one single line. Try running the command:

`docker exec factomd_node bash -c "sed -i '/Node Identity Information/q' /root/.factom/m2/factomd.conf && grep Identity -A 2 create.conf >> /root/.factom/m2/factomd.conf"`


## How come I get an error about not being able to connect to the docker when adding the configuration file?

When running the command to add the configuration file to the docker the error 'docker: Got permission denied while trying to connect to the Docker daemon socket...' can occur. This is because the user do not have the proper rights to use the docker program.

When setting up your docker and providing user rights by running the command:

`sudo usermod -aG docker USERNAME`

It is needed to log out of the server before the changes take effect.


## I have set my firewall as instructed, but ports that should be closed are still open. How do I fix that?

Docker bypasses firewall settings created using Uncomplicated Firewall (ufw). For people who do not use a firewall from their cloud provider, that means that all of the ports the Factom daemon might use will automatically be opened, including ports that you do not want to be exposed to the internet.

To fix that problem, we need to create a file which tells Docker not to bypass ufw settings:

`sudo nano /etc/docker/daemon.json`

In the daemon.json file insert and save:

`{
"iptables": false
}`

After saving, run the following commands:

```
docker-compose down

sudo service docker stop

sudo ufw reload

sudo service docker start

docker-compose up -d
```

## The "invalid character 'S' looking for beginning of value" error message

This is something that comes up when there's an unhandled exception. It's becoming more rare, but any time there's code without 100% exception handling this will occasionally happen. No real solution for this except.. Trying again. 
