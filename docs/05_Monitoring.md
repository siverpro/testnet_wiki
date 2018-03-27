[TOC]
## Individual Node Monitoring Service
The Factoid Authority (TFA) has set up individual node monitoring which can be used by everyone for free. This includes push notifications for stalled or offline nodes. 
* A web interface showing current active nodes is available at [http://factoid.org/](http://factoid.org/ "http://factoid.org/").
* For Push-notifications for offline and/or stalled node the Telegram-service is utilized. Join The Factoid Authority's channel here (link: [https://t.me/joinchat/AAAAAE2-0U-d39NlScamUQ](https://t.me/joinchat/AAAAAE2-0U-d39NlScamUQ "https://t.me/joinchat/AAAAAE2-0U-d39NlScamUQ")), and ensure your telegram username is the same as your Discord handle. If you mute the channel you will still get push notifications for your own node as your username is correct.
* The TFA Telegram monitoring-bot is set to override the mute-function in Telegram if your personal server goes offline; so by muting the channel you will only get your own alerts and not everyone else’s.
## Testnet load
A working group in the testnet organizational structure is responsible for developing monitoring tools which polls and displays current and historical usage of the Factom Community Testnet blockchain. Various statistics and graphs for the testnet is displayed here: ([http://testnet.factom-monitoring.com](http://testnet.factom-monitoring.com)). The current blockchain WPS (Writes Per Second) is shown at the top right corner, "WPS" at the moment is an arbitrary metric that we came up with, the current definition of it is the number of entries in the latest EntryBlock divided by the block period (10min) which corresponds to an average number of entries revealed per second.  The WPS is output by a script polling the number of writes to the last block, and then dividing that number with 600 (number of seconds in the block).

------

## Local tools

Factomd comes with a few ways to monitor your node's health.
The most obvious tool is the control panel found at localhost:8080.
Information about the control panel can be found here
https://docs.factom.com/#factoid-live

Factomd also comes with more monitoring tools that are included in this docker setup called Prometheus and Grafana.
We will focus on Grafana, as this is the visualization tool that is most usful.
To see Grafana, visit http://localhost:3001.

## Setting up Grafana

1. First ensure you have Grafana open by visiting http://localhost:3001
2. The username is "admin" and password "admin". Be sure not to open this port to the world or anyone can log in!
3. You will be greeted by a page that has a button saying "Add data source". Click that
    - Name: Prometheus
    - Type: Prometheus
    - Ensure Default is checked
    - URL: http://prometheus:9090
4. Once all the fields are put in, click "Save and Test". You should be greeted by "Data source is working".
If you encounter an error, here are some debug steps before asking for assistance.
    - Ensure Prometheus is running `docker ps | grep prometheus`
    - If there are no results, then you did not successfully run `docker-compose up -d`. Try running `docker-compose down` then `docker-compose up -d`
5. Now we have Prometheus as a datasource, let's get some graphs up. In the top left is the Grafana logo, click it, then dashboard, then import
    - Top Left > Dashboard > Import
6. A preconfigured dashboard can be found here: https://grafana.com/dashboards/4482
    - Input `4482` into the first input, then click Load
    - Make sure to select your prometheus source we added earlier from the dropdown menu next to Prometheus
    - Click import and your dashboard is now viewable.
7. Feel free to mess around and change things to your liking.



