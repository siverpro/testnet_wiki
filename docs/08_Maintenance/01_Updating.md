**Warning:**  
Never update your node without coordinating with a Testnet Administrator.

----
**Notes before updating:**
1. If you verify that you are not a federated server (by checking [http://factoid.org/](http://factoid.org/)) you are able to do an update at any time, but write a notice about it in  the discord channel #testnet-restarts first.

2. If you update as part of an organized restart initiated by the testnet administrators, don't run the start.sh command unless asked for (shown below).

Run these commands to update your node:

    cd factom/communitytestnet
    git pull
    docker pull emyrk/factomd_testnet_community:latest
    docker-compose down
    docker-compose up -d


To start your node execute the following command, please read the notes above first.

    docker exec factomd_node bash /root/bin/start.sh
