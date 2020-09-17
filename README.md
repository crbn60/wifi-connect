# balenablocks/wifi-connect

The wifi-connect block is a docker image that runs [wifi-connect](https://github.com/balena-io/wifi-connect) which is a utility for dynamically setting the WiFi configuration on a Linux device via a captive portal.

## Usage

To use this image, create a service in your `docker-compose.yml` file as shown below:

```yml
version: "2.1"

services:
  wifi-connect:
    image: balenablocks/wifi-connect:<device arch name>
    restart: always
    network_mode: host
    privileged: true
    labels:
      io.balena.features.dbus: "1"
      io.balena.features.firmware: "1"
```

You can also set your `docker-compose.yml` to build a `Dockerfile.template` file, and use the build variable `%%BALENA_ARCH%%` so that the correct image is automatically built for your device arch (see [supported architectures](#Supported-architectures)):

_docker-compose.yml:_

```yaml
version: "2.1"

services:
  wifi-connect:
    build: ./
    restart: always
    network_mode: host
    privileged: true
    labels:
      io.balena.features.dbus: "1"
      io.balena.features.firmware: "1"
```

_Dockerfile.template_

```dockerfile
FROM balenablocks/wifi-connect:%%BALENA_ARCH%%
```

## Customisation

- Set the SSID by setting an environment variable `SSID` to the value you want
- Password protect the Access Point by setting `PASS` to the required value

You can refer to the [docs](https://www.balena.io/docs/learn/manage/serv-vars/#environment-and-service-variables) on how to set environment or service variables

Alternatively, you can set them in the `docker-compose.yml` or `Dockerfile.template` files.

## Changing the logic in the start.sh file

When the container run, it executes the start.sh script. If the default logic does not suit your use case, you can adapt the script and replace the file in the image. You will have to set up the `Dockerfile.template` file as illustrated above and use the `COPY` directive and replace the file at `/start.sh`.

_**Note: This is a minimal image containing only the required dependencies and busybox. You will have to make use of the minimal toolset that busybox provides. [Available commands](https://manpages.debian.org/stretch/busybox/busybox.1.en.html#COMMANDS)**_.