<!-- !> This documentation is obsolete. Refer to [SHIP-HATS documentation](https://docs.developer.tech.gov.sg/docs/ship-hats-docs/) for more details. -->

# Configuration of Common Developer CLI tools with Cloudflare WARP Guide

This article tells you how to configure the following common applications.

> **Note**:
>- Tools listed here are the common applications/tools used by software developers in the Singapore Government agencies.
>- To configure other applications or tools, refer to the [Cloudflare documentation](https://developers.cloudflare.com/cloudflare-one/connections/connect-devices/warp/install-cloudflare-cert).

- [Node.js and NPM](#nodejs-and-npm)
- [Docker](#docker)
- [AWS CLI](#aws-cli)
- [Golang](#golang)

> **Note**:
>- If you are experiencing issues while using any CLI tools and applications to access SGTS services, create a support request. For more information, refer to [create support request](https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/support/raise-service-request).
>- All other issues that are not related to SGTS services, contact [Cloudflare Community Support Forums](https://support.cloudflare.com/hc/en-us)

## Node.js and NPM


Node.js and NPM use a hardcoded certificate store and requires additional configuration to trust the Cloudflare Certificate.

**macOS users**

If you are using macOS, Zsh is likely to be your default terminal. If you are using zsh, please run the following commands:

```bash
mkdir -p "${HOME}/.config/.cloudflare"
curl -sSLj -o "${HOME}/.config/.cloudflare/Cloudflare_CA.pem" "https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA.pem"
echo 'export NODE_EXTRA_CA_CERTS="${HOME}/.config/.cloudflare/Cloudflare_CA.pem"' | tee -a "${HOME}/.zshrc"
source "${HOME}/.zshrc"
```
**Linux users**

If you are using Linux, Bash is likely to be your default terminal. If you are using bash, please run the following commands:

```bash
mkdir -p "${HOME}/.config/.cloudflare"
curl -sSLj -o "${HOME}/.config/.cloudflare/Cloudflare_CA.pem" "https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA.pem"
echo 'export NODE_EXTRA_CA_CERTS="${HOME}/.config/.cloudflare/Cloudflare_CA.pem"' | tee -a "${HOME}/.bashrc"
source "${HOME}/.bashrc"
```


## Docker

<!--Docker for Desktop makes use of virtual machines to host the Docker engine. The Docker engine must be configured to trust the Cloudflare Certificate.

Please follow the Documentation for Docker for Desktop for [Mac](https://docs.docker.com/desktop/mac/#add-tls-certificates) or [Windows](https://docs.docker.com/desktop/windows/#adding-tls-certificates) to install the Cloudflare Cert.-->

Docker for Desktop on macOS and Windows uses virtual machines to host the Docker engine. The Docker engine must be configured to trust the Cloudflare Certificate.

Following are the instructions for the smooth operation of the Docker engine behind the Cloudflare WARP, but note this is **not recommended** for building a production-ready Docker images.

> **Tip**:
> You may consider building your Docker images using [SHIP-HATS CI/CD]( https://www.ship.gov.sg/).

- [Pull Docker images from a Docker image repository with Cloudflare Warp](#pull-docker-images-from-a-docker-image-repository-with-cloudflare-warp)
- [Connect operating system in your Docker container to Internet with Cloudflare Warp](#connect-operating-system-in-your-docker-container-to-internet-with-cloudflare-warp)

### Pull Docker images from a Docker image repository with Cloudflare Warp

To pull Docker images from a Docker image repository with Cloudflare Warp turned on, you must configure the Docker engine on your host machine to trust the Cloudflare Certificate Authority (CA) certificate.

**To configure Docker engine on your host machine to trust Cloudflare Certificate Authority (CA) certificate**

1.	Locate the Docker engine configuration directory on your host machine. This is usually the `.docker` directory in your user home directory. Create the `.docker` directory if it does not exist.
2.	Locate the certificate directory for your Docker image repository. This is located in the `certs.d` directory in the `.docker` directory. Create the directory for your Docker Image repository if it does not exist in the `certs.d` directory.

For example, `mkdir -p ~/.docker/certs.d/registry-in.ship.gov.sg`.

3. Copy the Cloudflare CA certificate from https://docs.developer.tech.gov.sg/docs/security-suite-for-engineering-endpoint-devices/support/cloudflare-cert-update-guide and save it in the Docker Image repository certificate directory as ca-certificates.crt.
For example:

```
curl -sSLj -o ~/.docker/certs.d/registry-in.ship.gov.sg/ca-certificates.crt https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA.pem
```

### Connect operating system in your Docker container to Internet with Cloudflare Warp

To connect the operating system in your Docker container to the Internet with Cloudflare Warp turned on, you need to configure the operating system on the Docker container to trust the Cloudflare CA certificate.

**To configure operating system to trust Cloudflare certificate**

The following Dockerfile snippet shows how to configure the operating system to trust the Cloudflare Certificate.  

**Prerequisite**

- Turn off Cloudflare WARP and run the apt-get commands. This is needed to run the above Dockerfile correctly.

> **Note**:
> Source of the following snippet is ubuntu.

```
RUN \
    apt-get update && \
    apt-get install -y ca-certificates && \
    curl -sSLj -o "/etc/ssl/certs/Cloudflare_CA.pem" "https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA.pem" && \
    update-ca-certificates
```

## AWS CLI

AWS CLI uses its own certificate store. It must be configured to trust the Cloudflare Certificate.

For Linux & macOS users:
```bash
mkdir -p "${HOME}/.config/.cloudflare"
curl -sSLj -o "${HOME}/.config/.cloudflare/Cloudflare_CA.pem" "https://seed-general-public-files.s3.ap-southeast-1.amazonaws.com/seed-cloudflare-root-certs/Cloudflare_CA.pem"

# If you are using macOS, Zsh is likely to be your default terminal. If you are using Zsh, please run the following commands:
echo 'export AWS_CA_BUNDLE="${HOME}/.config/.cloudflare/Cloudflare_CA.pem"' | "tee -a ${HOME}/.zshrc"
source "${HOME}/.zshrc"

# If you are using Linux, Bash is likely to be your default terminal. If you are using Bash, please run the following commands:
echo 'export AWS_CA_BUNDLE="${HOME}/.config/.cloudflare/Cloudflare_CA.pem"' | "tee -a ${HOME}/.bashrc"
source "${HOME}/.bashrc"
```



## AWS SDK


### Javascript
```js
import { readFileSync } from “fs”;
import { Agent } from “https”;
import { config } from “aws-sdk”:

const certs = [ readFileSync(“path-to-cert”) ];

config.update({
        httpOptions: {
            agent: new Agent({
                ca: certs,
            }),
        },
    });
```


## Golang
Golang on macOS does not use the Big Sur DNS resolver by default, resulting in DNS resolution by golang binaries not being able to work with VPN clients such as OpenVPN or Cloudflare WARP. Please ensure that you build your golang binaries with cgo enabled.
