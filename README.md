# dockerautoproxy

## What?

Uses the [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) Docker image to automate reverse proxy setup in Docker Swarm.

## How?

### Role Usage

The image itself is designed to dynamically configure, so there isn't much to do ahead of time. See [`defaults/main.yml`](defaults/main.yml).

The `alpine` image is used by default, change `nginx_version` to `latest` to use a `debian:jesse` based image.

### Nginx Proxy Usage

Containers which need proxying need to do two things: Expose the port to be proxied, and a specify a virtual host. 

Ports can be specified in the Dockerfile using `EXPOSE`, on the command line using `--expose`, or in the Docker Compose file using `expose`.

Virtual hosts must be specified in an env var called `VIRTUAL_HOST`.

#### Advanced Usage

* Custom nginx configs can be provided by mounting the config file as a volume at `/etc/nginx/vhost.d/{VIRTUAL_HOST}_location`.
* SSL certificates can be provided, or a companion container can be used to [automatically provision certs from LetsEncrypt](https://github.com/jwilder/nginx-proxy#ssl-support-using-letsencrypt).
