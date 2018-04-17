# About

The `docker-registry-frontend` is a browser-based solution for browsing and modifying a private Docker registry.

# Usage

This application is available in the form of a Docker image that you can run as a container by executing this command:

    docker run -d -e ENV_DOCKER_REGISTRY_HOST=reg.ajie.tech -e ENV_DOCKER_REGISTRY_PORT=5000 -p 8080:80 jiehoes/my-pdr

This command starts the container and forwards the container's private port `80` to your host's port `8080`. Make sure you specify the correct url to your registry.

When the application runs you can open your browser and navigate to [http://reg.ajie.tech:8080][1].

## Docker registry using SSL encryption

If the Docker registry is only reachable via HTTPs (e.g. if it sits behind a proxy) , you can run the following command:

    docker run -d -e ENV_DOCKER_REGISTRY_HOST=reg.ajie.tech -e ENV_DOCKER_REGISTRY_PORT=ENTER-5000 -e ENV_DOCKER_REGISTRY_USE_SSL=1 -p 8080:80 jiehoes/my-pdr

## SSL encryption

If you want to run the application with SSL enabled, you can do the following:

    docker run -d -e ENV_DOCKER_REGISTRY_HOST=reg.ajie.tech -e ENV_DOCKER_REGISTRY_PORT=5000 -e ENV_USE_SSL=yes -v $PWD/server.crt:/etc/apache2/server.crt:ro -v $PWD/server.key:/etc/apache2/server.key:ro -p 443:443 jiehoes/my-pdr

Note that the application still serves the port `80` but it is simply not exposed ;). Enable it at your own will. When the application runs with SSL you can open your browser and navigate to [https://reg.ajie.tech][2].

## Use the application as the registry

If you are running the Docker registry on the same host as the application but only accessible to the application (eg. listening on `127.0.0.1`) then you can use the application as the registry itself.

Normally this would then give bad advice on how to access a tag:

    docker pull localhost:5000/yourname/imagename:latest

We can override what hostname and port to put here:

    docker run -d -e ENV_DOCKER_REGISTRY_HOST=localhost -e ENV_DOCKER_REGISTRY_PORT=5000 -e ENV_REGISTRY_PROXY_FQDN=reg.ajie.tech -e ENV_REGISTRY_PROXY_PORT=443 -e ENV_USE_SSL=yes -v $PWD/server.crt:/etc/apache2/server.crt:ro -v $PWD/server.key:/etc/apache2/server.key:ro -p 443:443 jiehoes/my-pdr

A value of `80` or `443` for `ENV_REGISTRY_PROXY_PORT` will not actually be shown as Docker will check `443` and then `80` by default.

## Kerberos authentication

If you want to use Kerberos to protect access to the registry frontend, you can
do the following:

    docker run -d -e ENV_DOCKER_REGISTRY_HOST=reg.ajie.tech -e ENV_DOCKER_REGISTRY_PORT=5000 -e ENV_AUTH_USE_KERBEROS=yes -e ENV_AUTH_NAME="Kerberos login" -e ENV_AUTH_KRB5_KEYTAB=/etc/apache2/krb5.keytab -v $PWD/krb5.keytab:/etc/apache2/krb5.keytab:ro -e ENV_AUTH_KRB_REALMS="ENTER.YOUR.REALMS.HERE" -e ENV_AUTH_KRB_SERVICE_NAME=HTTP -p 80:80 jiehoes/my-pdr

You can of course combine SSL and Kerberos.

# Browse mode

If you want to start applicaton with browse mode which means no repos/tags management feature in the UI, You can specify `ENV_MODE_BROWSE_ONLY` flag as follows:

    docker run -d -e ENV_DOCKER_REGISTRY_HOST=reg.ajie.tech -e ENV_DOCKER_REGISTRY_PORT=5000 -e ENV_MODE_BROWSE_ONLY=true -p 8080:80 jiehoes/my-pdr

You can set `true` or `false` to this flag.

**NOTE** For now `ENV_MODE_BROWSE_ONLY` will be overwritten to `true`.

# Default repositories per page

By default 20 repositories will be listed per page. To adjust this number, to
let's say 50 pass `-e ENV_DEFAULT_REPOSITORIES_PER_PAGE=50` to your `docker run`
command.

# Default tags per page

By default 10 tags will be listed per page. To adjust this number, to
let's say 5 pass `-e ENV_DEFAULT_TAGS_PER_PAGE=5` to your `docker run`
command. Note that providing a big number will result in a heavy load on browsers.

# Contributions are welcome!

If you like the application, I invite you to contribute and report bugs or feature request on the project's github page: [https://github.com/kwk/docker-registry-frontend][3].
To learn how to develop for the docker-registry-frontend, see [here](develop/README.md).



  [1]: http://localhost:8080
  [2]: https://localhost
  [3]: https://github.com/kwk/docker-registry-frontend
  [features]: https://github.com/kwk/docker-registry-frontend/wiki/Features
  [v1branch]: https://github.com/kwk/docker-registry-frontend/tree/v1-deprecated
