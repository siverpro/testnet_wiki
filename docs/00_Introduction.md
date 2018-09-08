[TOC]
What is this Factom community test net?
---------------------------------------

To be able to understand how the Factom community test net (FCN) will be put together, one first need some basic knowledge about the basic design of the Factom blockchain.

The Factom blockchain consists of ordinary nodes (like the one that runs as part of the Federation Wallet), as well as «Federated Servers» and «Audit Servers». These two last categories is referred to as the «Authority Set».

Initially the Factom main net (the production environment) consisted of 18 Authority servers (9 Federated- and 9 Audit-servers), all run and controlled by Factom.

Factom’s Milestone 3 (M3) distributes the control of the Authority set to entities that will run the servers on a for-profit-basis. There will be 65 authority nodes eventually run by 65 different entities.

The FCN is a parallel network to Factom’s main net, and hosted on servers/nodes provided by the community.

The test net software/code will for all practical purposes be identical to the one used on the main net, but it will be configured to only interact with the test net blockchain.

A (large) amount of Testoids (TTS) will be issued to everyone participating in the test net. These are the counterpart of Factoids; and will be exchangeable for Test Credits (TC) that may be spent on the test net to write to the test net blockchain.

----
Why a community test net?
-------------------------

Currently Factom (and their customers) are running their own private test nets. For Factom these are very useful for testing new releases of the software, and their
customers may use their test nets to learn how to operate without spending real entry credits or broadcasting to the world that they are currently working their way into the
Factom ecosystem.

Factom’s current, private, test nets are hosted «in house» with Factom and are not
very well suited to test how the distribution of the Authority Set will affect the protocol
and how it operates.

The FCN will provide insights of this kind to Factom, as well as be a good
opportunity to get familiar with running and operating a Authority server for the people
who might want to do so after M3.

In addition to the above, the FCN will be free to use for developers, and this will
provide a good opportunity for those who want to interact with the protocol for testing
purposes.

----
How does one proceed to join this testnet then?
-------------------------------------------------

Well, first of all you will have to decide if you are going to run a server that will be part
of the Authority set, or an ordinary factomd node. The testnet will need both kinds to
function properly.

1. Joining the test net as part of the Authority set

    Joining as a part of the Authority set, as either a Federated server or an
    Audit server will require close to 100% uptime, and should be hosted on a
    computer dedicated to this effort; alternatively hosted in the cloud.

    People interested in participating in running an Authority node in the future, or
    who has a dedicated computer that satisfies the uptime requirement should chose
    this option.

    Those who want to run an Authority server will need to fill out [this application](https://docs.google.com/forms/d/e/1FAIpQLSd-t33chnGOyLZ6kJ-QC-L0EgOExzY7GQ8y9e0I0E4AIbdKBQ/viewform), in addition it is highly recommended to join the factom community discord found [here](https://discord.gg/YYM9w2V)

2. Participating as an ordinary node

    Most people would like to participate by running an ordinary factomd
    node, as this would not put any special requirements on uptime and stability. A
    node runs in the system background, and does not use much system resources
    or bandwidth.

    Even if you «just» run a ordinary factomd instance you will still be able to
    interact with the test net by using the command line. Here you can convert
    testoids to test credits to your heart’s content, as well as transfer (or hoard)
    testoids just as you do Factoids.

    There is no signup required to participate as an ordinary node on the test
    net; just follow the instructions in the ‘how-to-install’-chapter carefully and you
    will be on your way.

After figuring out what kind of server/node you want to run, you should verify that
the computer you will be using meets the [minimum system requirement](../01_Before_you_start/01_Requirements). As pointed out above, Authority servers will also require a stable internet
connection and have an uptime measured in weeks and months; not days.
