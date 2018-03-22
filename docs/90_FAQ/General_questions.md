## Why does my node need to have a fixed IP address?

The requirement for a fixed IP address is not a requirement for the Factom protocol itself. It is a requirement for the testnet.

If two machines with the same identity but different IP addresses would enter the Factom network, one would kill itself to avoid causing issues.

## How come that 4 level of keys are issued when doing key generation?

Having 4 levels of keys allows you to change the lower level keys while retaining the higher-level ones. If your block signing key is compromised, you can use your level 4 key to change it. If your level 4 key gets compromised you can use your level 3 key, etc.

With this scheme in place, you can cold storage your level 1 key somewhere very secure but inaccessible, your level 2 key somewhere less secure but more accessible and so on, to the point where your block singing key is available to sign every minute, however is most susceptible to being stolen.
