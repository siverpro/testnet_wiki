**Warning:**  
Never update your node without coordinating with a Testnet Administrator.

----
**Notes before updating:**
1. If you verify that you are not a federated server (by checking [http://fct.tools/](http://fct.tools/)) you are able to do an update at any time, but write a notice about it in the discord channel #testnet-restarts first.

Updating your node basically just means pulling an updated docker build from the Factom repo and restarting your node:

    docker pull factomd
    docker kill factomd
    
Now run the startup command again.
