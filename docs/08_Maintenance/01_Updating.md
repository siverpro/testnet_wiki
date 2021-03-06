**Warning:**  
Never update your node without coordinating with a Testnet Administrator.

**The videotutorial shown below covers updating and brainswapping.**

[![YouTube Video Guide](http://img.youtube.com/vi/od1P_QXk91M/0.jpg)](http://www.youtube.com/watch?v=od1P_QXk91M)

----
**Notes before updating:**
1. If you verify that you are not a federated server (by checking [http://fct.tools/](http://fct.tools/)) you are able to do an update at any time, but write a notice about it in the discord channel #testnet-restarts first.

Updating your node basically just means pulling an updated docker build from the Factom repo and restarting your node:

    docker stop factomd
    docker rm factomd
    
Now run the startup command again, change the version to the up-to-date one at `factominc/factomd:v<version>` in the line below if it's outdated:

`docker run -d --name "factomd" -v "factom_database:/root/.factom/m2" -v "factom_keys:/root/.factom/private" -p "8088:8088" -p "8090:8090" -p "8110:8110" -l "name=factomd" factominc/factomd:v5.2.0-rc1 -broadcastnum=16 -network=CUSTOM -customnet=fct_community_test -startdelay=600 -faulttimeout=120 -config=/root/.factom/private/factomd.conf`
