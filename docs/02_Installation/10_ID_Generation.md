# Create and setup server identities (Factom blockchain ID)
-------------------------------

To be able to join the test net as an authority server you will need a «personal» server identity. The identity is generated using the serveridentity program. Entry Credits is required to create your identity to the blockchain.

## We will set the following up in the guide: 
- Test Credits
- Access to a full Factomd node, factom-walletd, and factom-cli
- GoLang

A good starting point is found in "Interacting with the Factom Blockchain" section of the Wiki. If you followed the guide, you now have a full Factomd node along with the commandline tools factom-cli and factom-walletd.

## Create and fund a new Test Credit address

**Using the commandline:**

- Start `factom-walletd`
- Create a new TC address `factom-cli -s=localhost:port newecaddress`
- Visit the Faucet and input your generated address.
- Verify that it has been funded: `factom-cli -s=localhost:port balance ECXXXXXXXXXXXXXX`
- Export your address: `factom-cli -s=localhost:port exportaddresses`

Take note of your private TC address for the next step, beginning with "Esxxxxxxxxxxxxxxxx"

**Using the Enterprise Wallet:**

- Create a new Entry Credit address in your Address Book.
- Visit the Faucet and input your generated address
- Verify that it has been funded by checking the balance in the wallet
- Click the pencil-icon on your EC wallet and click "Display Private Key", you will need this for the next step.
- Also create a new Factoid address to set up efficency and payout

## Download and build server identity program. The following are directions for linux.

### Install git, golang 1.10, and glide. Set the proper $GOPATH environment variable.

    sudo apt-get install git golang-go golang-glide
    
Edit `~/.profile` and add these lines to the bottom:

    export GOPATH=$HOME/go
    export PATH=$PATH:$GOPATH/bin
    
Open a new terminal for these changes to take effect. You might even need to relog.

### Clone the serveridentity program:

    mkdir -p $GOPATH/src/github.com/FactomProject/
    cd $GOPATH/src/github.com/FactomProject/
    git clone https://github.com/FactomProject/serveridentity.git
    cd serveridentity
    glide install
    go install
    cd signwithed25519
    go install

## Create the server identity

-  Run `./serveridentity full elements Esxxxxxxxxxxxxxxxxxxxxx -n=important -f` from the step above
- It will create the files important.conf and important.sh
- Record the private keys printed out to the screen on paper or long term storage.  These are used to control your identity in the future. Level 4 is the highest security and level 1 will be used to do more operations.
- Save important.sh, important.conf. This will be your `factomd.conf` file on the server
- Make sure factomd is running
- Run factom-walletd in a terminal window
- The factom-cli commands in important.sh need to be run. Change all lines with `factom-cli …` to now read `factom-cli -s=localhost:port ...`
- Import the EC address to your wallet: `factom-cli importaddress Esxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
- Check the balance `factom-cli listaddresses`
- Run the important.sh script.
- Check the explorer that the new identity chains were created 10 minutes later.

## Set efficency and payout address

https://luciap.ca/#/identity-management

https://github.com/PaulBernier/factom-identity-cli
