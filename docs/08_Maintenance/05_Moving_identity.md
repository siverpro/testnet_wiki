How to move Factom Server Identity from one remote server to another remote server:
==================================================================================
This is only required if you want to create a new node, using your existing identity.

Notes:
------
- Replace your appropriate username
- Replace the IP of your hosts as appropriate
- Commands 1+2 is run on "old" host ("Host A").
- Commands 3+4 are run on your "new" host ("Host B").

<strong>IMPORTANT: before starting your factomd-node on your "new" host ("Host B"), you MUST stop the factomd-node on your "old" host ("Host A").</strong>


1. Copy the file from docker A to host A -- (docker cp)

        docker cp factomd:/root/.factom/m2/factomd.conf .

2. Copy the file from host A to host B -- (scp)

        scp factomd.conf USERNAME@IP-B:~/factomd.conf

3. Create the necessary directory inside the docker (mkdir)
        
        docker exec factomd mkdir /root/.factom /root/.factom/m2

4. Copy the file from host B to docker B -- (docker cp)

        docker cp factomd.conf factomd_node:/root/.factom/m2/factomd.conf
