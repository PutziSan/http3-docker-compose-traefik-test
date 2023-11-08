# HTTP/3 in Docker Compose via Traefik

This repository contains a docker compose environment to test HTTP/3 compatibility with traefik.

To verify the setup you can run the following command

```shell
docker compose up -d
docker run --network="host" ymuski/curl-http3 curl --insecure --head --http3-only --retry 3 --retry-all-errors --fail https://whoami.localhost
docker compose down
```

Which should result in the following output:

```
(...)
HTTP/3 200 
content-type: text/plain; charset=utf-8
(...)
```

If you executed the commands above in very fast order, you may get a 404 error at first. This is because the docker-compose environment needs a few seconds to start up the webserver. But curl is configured to automatically retry the request 3 times via the `--retry 3 --retry-all-errors --fail` flags.

## Test with your browser

If you want to test the HTTP/3 compatibility in your browser, you must make sure that the browser trusts the certificate.

One way to do this is to use mkcert:

1. install mkcert: https://github.com/FiloSottile/mkcert#installation
2. run `mkcert -install`
3. run `mkcert whoami.localhost` inside this repository (this should overwrite the default certificates whoami.localhost.pem and whoami.localhost-key.pem)
4. run `docker-compose up`

You should now be able to access https://whoami.localhost/ with your browser and verify that the HTTP/3 protocl was used via the network tab.

If you have trouble to enable HTTP/3 with your browser check [Validating HTTP/3 Protocol Support on a Local Site](https://www.putzisan.com/articles/validating-http3-local-site#method-1-verification-using-google-chrome) for more information.
