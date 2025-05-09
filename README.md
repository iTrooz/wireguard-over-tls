Example of how to connect to a Wireguard server (peer) over TLS

Technologies used:
- [wstunnel](https://github.com/erebe/wstunnel) (TLS layer)

# Installation

## Prerequisites
- Wireguard server, and client already configured/working.
- Linux client (Other OS are not supported)

## Server

### wstunnel
On the server, you need to run `docker compose up -d` inside the `server/wstunnel/` folder. Make sure the value `127.0.0.1:51820` corresponds to the Wireguard server address and port on your server.  
Note: nothing prevents you from running the server directly outside docker.

### nginx
If you want to use a reverse proxy to handle TLS, take look at [this nginx config file](server/nginx/vhost.conf)

### kubernetes
This repository handles the case where the Wireguard server is running on a node of a Kubernetes cluster. In this case, if you want to still use port 443, you have to go through the cluster. A helm chart is provided at `server/kube/` to redirect the traffic from a given hostname to the wstunnel server. Modify `values.yaml` and apply with `helm upgrade -i wg ./`

## Client
First, you need to install `wstunnel` on your client: `cargo install --git https://github.com/erebe/wstunnel` (or check their releases).

Then, you need to edit your wg-quick configuration, according to [the following example](client/wgcon-tls.conf). (Do not forget to change the wstunnel endpoint in `PostUp` !)
