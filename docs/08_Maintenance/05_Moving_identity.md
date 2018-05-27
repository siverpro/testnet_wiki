How to move Factom Server Identity from one remote server to another remote server:
==================================================================================
This is only required if you want to create a new node, using your existing identity.

Notes:
------
- Replace your appropriate username
- Replace the IP of your hosts as appropriate
- The docker volumes need to have already been created.

<strong>IMPORTANT: before starting your factomd-node on your "new" host ("Host B"), you MUST stop the factomd-node on your "old" host ("Host A").</strong>

#### Option 1: SINGLE COMMAND

This can be used if you have root access to the remote host.
 
On your "old" host, copy the file from host A to host B -- (scp)

    scp /var/lib/docker/volumes/factom_keys/_data/factomd.conf root@REMOTE-IP:/var/lib/docker/volumes/factom_keys/_data/factomd.conf
        
#### Option 2: Two-step

1. On your "old" host, copy the file from host A to host B -- (scp)
        
        scp /var/lib/docker/volumes/factom_keys/_data/factomd.conf USER@REMOTE-IP:/home/USER/factomd.conf

4. On your "new" host, place the file in the appropriate folder:

        cp factomd.conf /var/lib/docker/volumes/factom_keys/_data/factomd.conf
