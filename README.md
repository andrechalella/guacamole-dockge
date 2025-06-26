# Guacamole with Docker Compose

```
docker-compose file for Apache Guacamole
created by PCFreak 2017-06-28
forked by andrechalella 2025-06-23 in version 0.62
```

Changes by andrechalella:
- Make docker-compose.yml truly standalone for immediate deployment by
  pasting the contents in Dockge's text-editor UI.
- To this end, I implemented Apache's suggested DB initialization strategy
   (shared volume with the packaged schema files).
- Remove Nginx.

## HOW TO USE

The docker-compose.yml file is made to be standalone, i.e there is no preparation
needed. Simply paste the contents into Dockge's "compose.yaml" UI, or drop the
file into a subdirectory and run "docker compose up", and everything should
download and start, with proper initialization the first time.

## ACCESSING GUACAMOLE WEB INTERFACE

By default, the docker-compose.yml file does not expose/publish ports. The
containers are connected solely to the "guacamole-network" Docker network.

If you want to access Guacamole UI directly, normally you can find the Guacamole
container's IP with `docker inspect <container>`. See "Guacamole URL" note below.

If you want other containers (e.g Nginx Proxy Manager) to access Guacamole via its
container hostname (Docker DNS), remember to add your Docker custom network to the
"networks" section of the "guacamole" service (also to the "networks" top-level element).

### Guacamole URL

- Guacamole is accessible via the 8080 TCP port (Apache Tomcat HTTP server)
- Guacamole's URL path is "/guacamole/"
- So for instance `curl http://<container-ip>:8080/guacamole/`
- If using NPM:
  - In `Edit Proxy Host -> Details`, DON'T put the path in "Forward Hostname/IP".
  - In `Edit Proxy Host -> Custom locations`:
    - Click on `Add location`
    - `Define location`: insert a lone slash, i.e `/`
    - `Forward Hostname / IP`: write the full address with path, i.e `<hostname|IP>/guacamole/`
    - `Forward Port`: `8080`
