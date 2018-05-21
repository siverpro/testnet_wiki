[TOC]
## Individual Node Monitoring Service
The Factoid Authority (TFA) and De Facto has set up individual node monitoring which can be used by everyone for free. This includes push notifications for stalled or offline nodes. 
* A web interface showing current active nodes is available at [http://fct.tools/](http://fct.tools/).
* For Push-notifications for offline and/or stalled node a Telegram-service is utilized. Details about how to join can be found at http://fct.tools

## Testnet load
A working group in the testnet organizational structure is responsible for developing monitoring tools which polls and displays current and historical usage of the Factom Community Testnet blockchain. Various statistics and graphs for the testnet is displayed here: ([http://testnet.factom-monitoring.com](http://testnet.factom-monitoring.com)). 

The current blockchain WPS (Writes Per Second) is shown at the top right corner, "WPS" at the moment is an arbitrary metric that we came up with, the current definition of it is the number of entries in the latest EntryBlock divided by the block period (10min) which corresponds to an average number of entries revealed per second.  The WPS is output by a script polling the number of writes to the last block, and then dividing that number with 600 (number of seconds in the block).

------

## Local tools

Factomd comes with a few ways to monitor your node's health. The most obvious tool is the control panel found at localhost:8080. Information about the control panel can be found here [Factomd control panel](https://docs.factom.com/#the-factomd-control-panel)

Factomd also used to come with more monitoring tools called Prometheus and Grafana.
If you want to include them in your package, you can run Grafana and Prometheus docker images:

    docker run -d --name=grafana -p 3001:3001 grafana/grafana
    docker run -d --name=prometheus -p 9090:9090 prom/prometheus
    
To see Grafana, open http://localhost:3001

## Setting up Grafana

1. First ensure you have Grafana open by visiting http://localhost:3001
2. The username is "admin" and password "admin". Be sure not to open this port to the world or anyone can log in!
3. You will be greeted by a page that has a button saying "Add data source". Click that
    - Name: Prometheus
    - Type: Prometheus
    - Ensure Default is checked
    - URL: http://prometheus:9090
4. Once all the fields are put in, click "Save and Test". You should be greeted by "Data source is working".
5. Now we have Prometheus as a datasource, let's get some graphs up. In the top left is the Grafana logo, click it, then dashboard, then import
    - Top Left > Dashboard > Import
6. A preconfigured dashboard can be found here: https://grafana.com/dashboards/4482
    - Input `4482` into the first input, then click Load
    - Make sure to select your prometheus source we added earlier from the dropdown menu next to Prometheus
    - Click import and your dashboard is now viewable.
7. Feel free to mess around and change things to your liking.
