# HTTP/3 in Docker Compose via Traefik

This repository contains a docker compose environment to test HTTP/3 compatibility with traefik.

To verify the setup you can run the following command

```shell
docker compose up --exit-code-from test
```

Which should result in the following output:

```
$ docker compose up --exit-code-from test
[+] Running 3/0
 ✔ Container http3-docker-compose-example-whoami-1   Created                                                                                                                  0.0s 
 ✔ Container http3-docker-compose-example-traefik-1  Created                                                                                                                  0.0s 
 ✔ Container http3-docker-compose-example-test-1     Recreated                                                                                                                0.0s 
Attaching to http3-docker-compose-example-test-1, http3-docker-compose-example-traefik-1, http3-docker-compose-example-whoami-1
http3-docker-compose-example-whoami-1   | 2023/10/26 10:14:04 Starting up on port 80
http3-docker-compose-example-test-1     |   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
http3-docker-compose-example-test-1     |                                  Dload  Upload   Total   Spent    Left  Speed
http3-docker-compose-example-test-1     | HTTP/3 200 
http3-docker-compose-example-test-1     | content-type: text/plain; charset=utf-8
http3-docker-compose-example-test-1     | date: Thu, 26 Oct 2023 10:14:07 GMT
http3-docker-compose-example-test-1     | content-length: 336
http3-docker-compose-example-test-1     | 
  0   336    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
http3-docker-compose-example-test-1 exited with code 0
(...)
```


## Test with your browser

If you want to test the HTTP/3 compatibility in your browser, you must make sure that the browser trusts the certificate.

One way to do this is to use mkcert:

1. install mkcert: https://github.com/FiloSottile/mkcert#installation
2. run `mkcert -install`
3. run `mkcert whoami.localhost` inside this repository (this should overwrite the default certificates whoami.localhost.pem and whoami.localhost-key.pem)
4. run `docker-compose up`

You should now be able to access https://whoami.localhost/ with your browser.
E.g. with Firefox you can check the protocol in the developer tools (F12) under the network tab (sometimes you need to restart firefox and reload the site a few times before it actually uses http/3).

> Be aware: Even if the setup is correct, the browser may not use HTTP/3. See https://www.smashingmagazine.com/2021/09/http3-practical-deployment-options-part3/#alt-svc for more information.
> 
> You can force chrome to use HTTP/3 by starting chrome with the `--origin-to-force-quic-on`, e.g. `google-chrome --origin-to-force-quic-on=whoami.localhost:443` (make sure to close all chrome windows, before restarting chrome via the command line).
