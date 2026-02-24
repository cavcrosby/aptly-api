# aptly-api

This is a container image for Aptly's HTTP REST API.
[Aptly](https://www.aptly.info/) is a swiss army knife for Debian repository
management. Aptly can be used as either via CLI or an
[API](https://www.aptly.info/doc/api/swagger).

## Supported tags and respective `Dockerfile` links

- [`1.6`, `latest`](https://github.com/cavcrosby/aptly-api/blob/main/src/Dockerfile)

## How to use this image

Something of importance to note is that by default, the API stores its database,
downloaded packages, and published repositories all within the
`/var/lib/aptly-api` directory. Hence, it's advisable that a volume be mounted
to this directory. Said volume can be used by multiple instances of the API as
it runs with the `-no-lock` option.

```shell
docker run --detach --volume "aptly-api:/var/lib/aptly-api" --publish "8080:8080" "cavcrosby/aptly-api"
```

In addition, a volume containing GPG keys (either ASCII armored or binary) can
be mounted at the `/etc/aptly-api/secrets/gpg` path to have those added to the
container's keyring to be used by the API.

### Using a different listening port

```shell
docker run --detach --volume "aptly-api:/var/lib/aptly-api" --publish "8080:8081" --env "PORT=8081" "cavcrosby/aptly-api"
```

### Using a different configuration file

```shell
docker run --detach --volume "aptly-api:/var/lib/aptly-api" --volume "${PWD}/aptly.conf:/tmp/aptly.conf" --publish "8080:8080" --env "CONFIG_PATH=/tmp/aptly.conf" "cavcrosby/aptly-api"
```

## License

See LICENSE.
