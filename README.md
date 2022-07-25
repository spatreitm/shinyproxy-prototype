# shinyproxy-prototype


## How to run from scratch

1. Git Clone the repository
2. Create a docker network that ShinyProxy will use to communicate with the Shiny containers.
```
sudo docker network create sp-example-net
```
3. Open a terminal, go to the directory where Dockerfile and application.yml is present and run the following command to build the ShinyProxy image:
```
sudo docker build . -t shinyproxy-proto
```
4. Run the following command to launch the ShinyProxy container:
```
sudo docker run -v /var/run/docker.sock:/var/run/docker.sock:ro --group-add $(getent group docker | cut -d: -f3) --net sp-example-net -p 8080:8080 shinyproxy-proto
```
Note: inside the Docker container, ShinyProxy runs as a non-root user, therefore it does not have access to the Docker socket by default. By adding the --group-add $(getent group docker | cut -d: -f3) option we ensure that the user is part of the docker group and thus has access to the Docker daemon.

Note: by adding the -d option to the command (just after docker run), the Docker container will run in the background.

## How to run the image that is on github container registry

1. Run the following command to pull and launch the ShinyProxy container:

```
sudo docker run -v /var/run/docker.sock:/var/run/docker.sock:ro --group-add $(getent group docker | cut -d: -f3) --net sp-example-net -p 8080:8080 ghcr.io/spatreitm/shinyproxy_proto:latest
```
