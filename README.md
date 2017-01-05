## Docker Compose Virtual Hosts

### Commands
```bash
make docker.up # Build and run a local development server on docker.
make docker.down # Destroy all volumes and containers on docker.
make docker.iter # Equivalent to docker.down then docker.up.
cat Makefile # Get a list of other targets in the Makefile.
make<tab><tab> # If your shell supports Makefile tab-completion.
```

### Description

This will pull [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) so you can use URL like  http://my-application.dev/ to access the local development server. You can run many applications this way by using virtual hosts so you don't have to configure separate ports for every application you have running. You can run two apache containers, both listening on port 80, and nginx-proxy will route web traffic based on the VIRTUAL_HOST environment variable.

You can run related micro-services locally before pushing them to production. I do a lot of local development using [docker](https://www.docker.com/) and [ngrok](https://ngrok.com/) so this helps a lot. I don't have to fiddle with ports anymore. I just type the URL and go!

[jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) is where all the fun is at. This setup just gets me up and running quicker. Take a gander at that repository to learn how it works.

### Caveats

docker-compose likes to create a separate network for every project it comes across. Containers can't see or communicate with each other if this happens. The nginx-proxy won't be able to proxy anything because it's on a different network.

The host network won't work because you will have overlapping ports on your real IP address ie. 192.168.1.100, 10.10.10.100, etc. I tried the default bridge network that docker run uses but that didn't work either.

To fix this you have to create a single network for docker-compose to latch onto. I have other "services" like this so naturally I named the network "services". Feel free to name the network whatever you like.

A script is provided that creates a new network named "services". If you change it, then make sure you change the external network in the docker-compose.yml file.
