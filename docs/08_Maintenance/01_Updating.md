**Warning:**  
Never update your node without coordinating with a Testnet Administrator.

----

Run these commands to update your node:

    cd factom/communitytestnet
    git pull
    docker pull emyrk/factomd_testnet_community:latest
    docker-compose down
    docker-compose up -d
    docker exec factomd_node bash /root/bin/start.sh
