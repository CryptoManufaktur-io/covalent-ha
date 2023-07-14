# Overview

Highly available Covalent Refiner deployment

This repo documents deploying the Covalent Refiner in container orchestration. This specific example uses docker swarm mode. A k8s deployment would need additional adjustments.

The basic architecture is ipfs-pinner, evm-server and rudder services in container orchestration, with an haproxy to access two or more Moonbase or Moonbeam RPC servers. ipfs-pinner requires a small amount of stateful storage, which can be EFS for example. Moonbeam RPC servers run in baremetal and are secured by TLS.

## Moonbase/beam RPC

haproxy offers active/passive load balancing to two or more Moonbase / Moonbeam RPC servers. Adjust `moonbase-haproxy.cfg` and `rudder.yml` with the FQDN of your servers and the FQDN for the load balancer URL, respectively. check-ecsync.sh is an external check that verifies that sync is completed and the server has at least 5 peers.

You can use [moonbeam-docker](https://github.com/CryptoManufaktur-io/moonbeam-docker) to run the RPC servers. It supports [TLS encryption via traefik](https://eth-docker.net/Usage/ReverseProxy), and ufw can be used to [restrict incoming https](https://eth-docker.net/Support/Cloud) to trusted source IPs, those of the container orchestration nodes.

## Stateful storage

In this example, I am using Amazon EFS. Adjust the EFS URLs in `rudder.yml` to what you have deployed.

## Secret keys

The Web3 IPFS storage API key and the EOA secret key go into rudder.yml directly, as environment variables.
