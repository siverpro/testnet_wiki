How to move Factom Server Identity from one remote server to another remote server:
==================================================================================
This is only required if you want to create a new node, using your existing identity.

Notes:
------
- Docker must be running on both remote instances (docker-compose up -d).
- Replace your appropriate username in command 2 & 3.
- In command 2 replace IP with IP of "old" host ("Host A").
- In command 3 replace the IP of your "new" host ("Host B").
- Commands 1 is run on "old" host ("Host A").
- Commands 2+3 are run on your own computer.
- Commands 4+5 are run on your "new" host ("Host B").

<strong>IMPORTANT: before starting your factomd-node on your "new" host ("Host B"), you MUST stop the factomd-node on your "old" host ("Host B").</strong>


1. Copy the file from remote docker A to remote host A -- (docker cp)

        docker cp factomd_node:/root/ .factom/m2/factomd.conf .

2. Copy the file from remote host A to local host -- (scp)

        scp USERNAME@IP:~/factomd.conf .

3. Copy the file from local host to remote remote host B -- (scp)

        scp factomd.conf USERNAME@IP:~/factomd.conf

4. Create the necessary directory inside the docker (mkdir)
        
        docker exec factomd_node mkdir /root/.factom /root/.factom/m2

5. Copy the file from remote host B to remote docker B -- (docker cp)

        docker cp factomd.conf factomd_node:/root/.factom/m2/factomd.conf
