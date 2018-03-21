## Why does my node take so long time after boot to start progressing it's block height ?
In the control panel you can see the [More Detailed Node Info Page](https://docs.factom.com/#more-detailed-node-information-page). 

At the top left corner in the more detailed node information page, looking at the underscores to the right of FNode0, there will be a W (Wait mode) for 10 minutes, then an I (Ignore mode) for another 10 minutes. 

**Wait mode** is the mode the servers stay at while booting to give the other nodes a chance to finish booting. It is collecting messages, but not sending acknowlegements. 

**Ignore mode** is where the server is sending out acknowledgements and EOMs (End Of Messages), but is not requesting missing messages. The intent is to only see fresh messages, but it is fragile because it can't get any messages that it had 
missed, which is one of the things that helps keep the network robust.

After the Wait- and Ignore modes go away, your node will request new blocks and process list items from its peers, and will catch up.
