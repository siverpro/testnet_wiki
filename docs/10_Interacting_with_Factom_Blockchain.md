## Interacting with the Factom community testnet from the command line

The testnet can be interacted with in much the same manner as the Factom main-net.

Testoids (equivalent to Factoids) can be transferred or converted to Test credits
(equivalent to Entry credits), chains and entries created, information on transfers and
account balances pulled and APIs interfaced.

A full list of CLI (command line interface) commands is available by executing:

    docker exec factomd_node factom-cli help

All factomd commands have to be prefaced by: `docker exec factomd_node factom-cli`

------

## Some useful commands

| Task | Command | Information |
|----- | ------- | ----------- |
| Add Chain | `factom-cli addchain [-fq] [-n NAME1 -n NAME2 -h HEXNAME3] [-CET] ECADDRESS <STDIN>` | Create a new Factom Chain. Read data for the First Entry from stdin. Use the Entry Credits from the specified address. -C ChainID. -E EntryHash. -T TxID. |
| Add Entry | `factom-cli addentry [-fq] [-n NAME1 -h HEXNAME2 ...|-c CHAINID] [-e EXTID1 -e EXTID2 -x HEXEXTID ...] [-CET] ECADDRESS <STDIN>` | Create a new Factom Entry. Read data for the Entry from stdin. Use the Entry Credits from the specified address. -C ChainID. -E EntryHash. -T TxID. |
| Check Balance | `factom-cli balance [-r] ADDRESS` | If this is an EC Address, returns number of Entry Credits. If this is a Factoid Address, returns the Factoid balance. |
| Convert Testoids to Test Credits | `factom-cli buyec [-fqrT] FCTADDRESS ECADDRESS ECAMOUNT` | Buy ECAMOUNT number of entry credits. -f force. -q quiet. -r Netki DNS resolve. -T TxID. |
| Export private addresses | `factom-cli exportaddresses` | List the private addresses stored in the wallet |
| Get data | `factom-cli get allentries | chainhead | dblock | eblock | entry | firstentry | head | heights | walletheight | pendingentries | pendingtransactions | raw | dbheight | abheight | fbheight | ecbheight` | Get data about Factom Chains, Entries, and Blocks |
| New TC address | `factom-cli newecaddress` | Generate a new Entry Credit Address in the wallet |
| New Testoid address | `factom-cli newfctaddress` | Generate a new Factoid Address in the wallet |
| Reciept query | `factom-cli receipt ENTRYHASH` | Returns a Receipt for a given Entry |
| Send Testoids | `factom-cli sendfct [-fqrT] FROMADDRESS TOADDRESS AMOUNT` | Send Factoids between 2 addresses. -f force. -q quiet. -r Netki DNS resolve. -T TxID. |
