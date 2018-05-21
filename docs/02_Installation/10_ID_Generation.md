Creating a Factom blockchain ID
To be able to join the test net as an authority server or an audit server you will need a «personal» server identity. This is done by executing some special commands that tells your factom node to generate a new ID.

Important: After starting the node for the first time (start.sh) you should wait until it has fully synced with the rest of the net before you interact with the blockchain in the following steps. Verify via control panel at :8090 ("Node sync status 1: 100%"). This could take hours.

Generating the ID requires you to have two terminal windows open. One will be used for hosting the factom wallet while the other is used for inputting the appropriate commands

Image 1

In the first window launch the Factom wallet:

docker exec -it factomd_node factom-walletd
Then move along to the second terminal window. All further commands will be executed from this window.

If you have been issued a Test Credit (TC) address, import it now:

docker exec factomd_node factom-cli importaddress EsXXXXXXXX
If you already have a TC address, you may skip the following section.

New Test Credit Address
If you don't have a Test Credit address yet, you have to generate a new one, fund it by using the Faucet, confirm the funding and then continue installation.

Generate new TC address:

docker exec factomd_node factom-cli newecaddress
Take note of your address, then visit the Factoid Faucet to fund your address, either under the "Before you start"-section or this direct link.

Before you continue, please verify that your address has been funded:

docker exec factomd_node factom-cli balance ECXXXXXXXXXXXXXX
Note: Use the public address. The amount of credits in the TC-account will be displayed. Your wallet has to be fully synced before your credits will appear in your balance. You can verify your sync status from the Control Panel.

Run the following command to generate a new Testoid (TTS) address:

docker exec factomd_node factom-cli newfctaddress
Export the addresses you generated in the previous steps:

docker exec factomd_node factom-cli exportaddresses
Note: This will export the SECRET/PRIVATE addresses associated with the TC/TTS-addresses

Image 2

Execute the following command to generate an ID, public/private keys:

docker exec factomd_node serveridentity full elements EsXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX -n create -f
Note: The private TC address should be used.

Copy/write down the output from the previous command (take a mental note of where the public key and Root chain information is presented).

Image 3

The previous command also (by magic) created a script for publishing your new ID to the blockchain. Run this script by executing the following commands:

docker exec factomd_node chmod 766 /root/create.sh
and;

docker exec factomd_node bash /root/create.sh
Verify that your ID is written to the testnet blockchain by executing:

docker exec factomd_node factom-cli get allentries YOUR-ROOT-CHAIN
Example:

docker exec factomd_node factom-cli get allentries 888888c0754c3218a32d12004fd9d30590ccbc22954f8688e1a9d628f943be80
Executing this command should yield an output containing:

«EntryHash/ExtID/content», while «missing chain head» indicates that your ID failed to register in the blockchain.
Note: if this commands fails, what a minute and then try again.

With your ID registered in the testnet blockchain you may now modify your factom node to utilize this new identity by running this command:

docker exec factomd_node bash -c "sed -i '/Node Identity Information/q' /root/.factom/m2/factomd.conf && grep Identity -A 2 create.conf >> /root/.factom/m2/factomd.conf"
The last step is to reboot your node; which will make it rejoin the testnet with the correct identification:

docker exec factomd_node bash /root/bin/stop.sh
and

docker exec factomd_node bash /root/bin/start.sh
