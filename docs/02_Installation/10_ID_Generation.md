Creating a Factom blockchain ID
-------------------------------

To be able to join the test net as an authority server or an audit server you will
need a «personal» server identity.

## Run a copy of factomd locally synced to the blockchain.
Note: You can edit some commands later if you want to use a different node, like the courtesy node, but that is less reliable.

## Download and build server identity program. The following are directions for linux.

### Install git, golang 1.10, and glide. Set the proper $GOPATH environment variable.

### Clone the serveridentity program:

- `mkdir -p $GOPATH/src/github.com/FactomProject/`
- `cd $GOPATH/src/github.com/FactomProject/`
- `git clone https://github.com/FactomProject/serveridentity.git`
- `cd serveridentity`
- `glide install`
- `go install`
- `cd signwithed25519`
- `go install`
- Copy $GOPATH/bin/serveridentity to an offline computer with a thumbdrive
- On the offline computer run `./serveridentity full elements Es2bTzUy71xg24XVQNV7xcsJwDxRofeV84NEiRJ49RV7HmcaUEUV -n=important -f`
- It will create the files important.conf and important.sh
- Record the private keys printed out to the screen on paper or long term storage.  These are used to control your identity in the future. Level 4 is the highest security and level 1 will be used to do more operations.
- Copy the file important.sh, important.conf back to a thumbdrive to run on an online computer. This will be your `factomd.conf`
- Make sure factomd is running
- Run factom-walletd in a terminal window (use -s=courtesy-node.factom.com:80 if you don’t have a full node)
- The factom-cli commands in important.sh need to be directed to the courtesy-node. Change all lines with `factom-cli …` now read, `factom-cli -s=courtesy-node.factom.com:80 ...` (or whatever factomd you are using)
- Import the EC address to your wallet: `factom-cli importaddress Es2bTzUy71xg24XVQNV7xcsJwDxRofeV84NEiRJ49RV7HmcaUEUV`
- Check the balance `factom-cli listaddresses`
- Run the important.sh script.
- Check the explorer that the new identity chains were created 10 minutes later
