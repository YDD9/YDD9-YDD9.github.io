
# [volumes](https://docs.docker.com/compose/compose-file/#volumes)</br>
Mount host paths or named volumes, specified as sub-options to a service.

You can mount a host path as part of a definition for a single service, and there is no need to define it in the top level volumes key.

But, if you want to reuse a volume across multiple services, then define a named volume in the top-level volumes key. Use named volumes with services, swarms, and stack files.

Note: The top-level volumes key defines a named volume and references it from each service’s volumes list. This replaces volumes_from in earlier versions of the Compose file format. See Use volumes and Volume Plugins for general information on volumes.

This example shows a named volume (mydata) being used by the web service, and a bind mount defined for a single service (first path under db service volumes). The db service also uses a named volume called dbdata (second path under db service volumes), but defines it using the old string format for mounting a named volume. Named volumes must be listed under the top-level volumes key, as shown.
```
version: "3.2"
services:
  web:
    image: nginx:alpine
    volumes:
      - type: volume
        source: mydata
        target: /data
        volume:
          nocopy: true
      - type: bind
        source: ./static
        target: /opt/app/static

  db:
    image: postgres:latest
    volumes:
      - "/var/run/postgres/postgres.sock:/var/run/postgres/postgres.sock"
      - "dbdata:/var/lib/postgresql/data"

volumes:
  mydata:
  dbdata:
```
Note: See Use volumes and Volume Plugins for general information on volumes.</br>
Syntax: volumes is simply a string, don't put space after column, simply `- "/var/run/postgres/postgres.sock:/var/run/postgres/postgres.sock"`
or `- "dbdata:/var/lib/postgresql/data"`

# [Choose between named volumes, mount volues and volume as a container](https://stackoverflow.com/questions/36312699/chown-docker-volumes-on-host-possibly-through-docker-compose/36321403#36321403)

It's best to not try to access files inside /var/lib/docker directly where named volumes stored. Those directories are meant to be managed by the docker daemon, and not to be messed with.

To access the data inside a volume, there's a number of options;

* use a bind-mounted directory (you considered that, but didn't fit your use case).</br>
* use a "service" container that uses the same volume and makes it accessible through that container, for example a container running ssh (to use scp) or a SAMBA container (such as svendowideit/samba)</br>
* use a volume-driver plugin. there's various plugins around that offer all kind of options. For example, the local persist plugin is a really simple plug-in that allows you to specify where docker should store the volume data (so outside of /var/lib/docker)</br>
