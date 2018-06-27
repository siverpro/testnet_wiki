# Create and setup server identities (Factom blockchain ID)
-------------------------------

To be able to join the test net as an authority server you will need a «personal» server identity.

## We will set the following up in the guide: 
- Test Credits
- Access to a full Factomd node, factom-walletd, and factom-cli
    https://github.com/FactomProject/FactomDocs/blob/master/installFromSourceDirections.md
    Note: You can skip the factomd package to use your Docker factomd.
- GoLang

## Run a copy of factomd locally synced to the blockchain.
- Your Docker swarm container can be used.

## Download and build server identity program. The following are directions for linux.

### Install git, golang 1.10, and glide. Set the proper $GOPATH environment variable.

This is already outlined in the "install from source  directions" linked above.

### Create and fund a new Test Credit address

- Start `factom-walletd`
- Create a new TC address `factom-cli -s=localhost:port newecaddress`
- Visit the Faucet and input your generated address.
- Verify that it has been funded: `factom-cli -s=localhost:port balance ECXXXXXXXXXXXXXX`
- Export your address: `factom-cli -s=localhost:port exportaddresses`

Take note of your private TC address for the next step, beginning with "Esxxxxxxxxxxxxxxxx"

### Clone the serveridentity program:

- `mkdir -p $GOPATH/src/github.com/FactomProject/`
- `cd $GOPATH/src/github.com/FactomProject/`
- `git clone https://github.com/FactomProject/serveridentity.git`
- `cd serveridentity`
- `glide install`
- `go install`
- `cd signwithed25519`
- `go install`
-  Run `./serveridentity full elements Esxxxxxxxxxxxxxxxxxxxxx -n=important -f` from the step above
- It will create the files important.conf and important.sh
- Record the private keys printed out to the screen on paper or long term storage.  These are used to control your identity in the future. Level 4 is the highest security and level 1 will be used to do more operations.
- Save important.sh, important.conf. This will be your `factomd.conf` file on the server
- Make sure factomd is running
- Run factom-walletd in a terminal window (use -s=courtesy-node.factom.com:80 if you don’t have a full node)
- The factom-cli commands in important.sh need to be run. Change all lines with `factom-cli …` now read, `factom-cli -s=localhost:port ...` or `factom-cli -s=courtesy-node.factom.com:80 ...` if you use the courtesy node mentioned in the line above. 
- Import the EC address to your wallet: `factom-cli importaddress Es2bTzUy71xg24XVQNV7xcsJwDxRofeV84NEiRJ49RV7HmcaUEUV`
- Check the balance `factom-cli listaddresses`
- Run the important.sh script.
- Check the explorer that the new identity chains were created 10 minutes later.
