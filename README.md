# dockerautoproxy

## What?

Uses the [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) Docker image to automate reverse proxy setup in Docker Swarm.

## How?

### Role Usage

The image itself is designed to dynamically configure, so there isn't much to do ahead of time. See [`defaults/main.yml`](defaults/main.yml).

The `alpine` image is used by default, change `nginx_version` to `latest` to use a `debian:jesse` based image.

Creates a network called `nginx_proxy_net` which your proxied containers must join.

### Nginx Proxy Usage

Containers which need proxying need to do two things: Expose the port to be proxied, and a specify a virtual host. 

Ports can be specified in the Dockerfile using `EXPOSE`, on the command line using `--expose`, or in the Docker Compose file using `expose`.

Virtual hosts must be specified in an env var called `VIRTUAL_HOST`.

#### Advanced Usage

##### Custom nginx configs 

Can be loaded by mounting a host path containing nginx config files using the `dockerautoproxy_config_path` variable. The path will be created if it doesn't already exist on the host. 

##### SSL certificates

SSL certificates can be generated automatically or specified manually. By default, they're stored in a data volume shared between the containers. You can use `dockerautoproxy_certs_path` to mount a path on the host to store them at.

To automatically generate certificates, set the Ansible var to `dockerautoproxy_ssl_companion` to `true`. This will launch a second container which will handle SSL certificate generation with Let's Encrypt. To enable a service for this, set the container's env vars `LETSENCRYPT_HOST` and `LETSENCRYPT_EMAIL`. For example:

```
$ docker run -d \
    --name example-app \
    -e "VIRTUAL_HOST=example.com,www.example.com,mail.example.com" \
    -e "LETSENCRYPT_HOST=example.com,www.example.com,mail.example.com" \
    -e "LETSENCRYPT_EMAIL=foo@bar.com" \
    tutum/apache-php
```
