This page describes how to set up a monitoring site, [like this one](http://eth-netstats.herokuapp.com/), for your private network. It builds upon [this wiki article](https://github.com/ethereum/go-ethereum/wiki/Setting-up-private-network-or-local-cluster) and assumes you've created a local cluster using [this script (cluster)](https://gist.githubusercontent.com/kobigurk/a12dbb580507bbd4382d/raw/25f26c6cde0c130030310a51883ce8dffaa1e805/cluster).

The monitoring system consists of two components:

1. **eth-netstats** - the monitoring site which lists the nodes.
2. **eth-net-intelligence-api** - these are processes that communicate with the ethereum client using RPC and push the data to the monitoring site.

#Monitoring site
Clone the repo and install dependencies:

    git clone https://github.com/cubedro/eth-netstats
    cd eth-netstats
    npm install

Then choose a secret and start the app:

    WS_SECRET=<chosen_secret> npm start

You can now access the (empty) monitoring site at `http://localhost:3000`.

You can also choose a different port:
    
    PORT=<chosen_port> WS_SECRET=<chosen_secret> npm start

#Client-side information relays
These processes will relay the information from each of your cluster nodes to the monitoring site using websockets.

Clone the repo, install dependencies and make sure you have pm2 installed:

    git clone https://github.com/cubedro/eth-net-intelligence-api
    cd eth-net-intelligence-api
    npm install
    sudo npm install -g pm2

Now, use [this script (intelligence)](https://gist.githubusercontent.com/kobigurk/a12dbb580507bbd4382d/raw/4ea274229e10f52d353105395f312c05fdf248b2/intelligence) to create an `app.json` suitable for pm2.

Usage:

    bash intelligence <destination_app_json_path> <number_of_clusters> <name_prefix> <ws_server> <ws_secret>

- `destination_app_json_path` is the target directory for the resulting app.json.
- `number_of_clusters` is the number of nodes in the cluster.
- `name_prefix` is a prefix for the node names as will appear in the listing.
- `ws_server` is the eth-netstats server. Make sure you write the full URL, for example: http://localhost:3000.
- `ws_secret` is the eth-netstats secret.

For example:

    bash intelligence . 5 mynode http://localhost:3000 big-secret    

Run the script and copy the resulting `app.json` into the `eth-net-intelligence-api` directory. Afterwards, `cd` into `eth-net-intelligence-api` and run the relays using `pm2 start app.json`. To stop the relays, you can use `pm2 delete app.json`.

At this point, your monitoring site should monitor all your nodes!