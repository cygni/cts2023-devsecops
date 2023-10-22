## Prerequisites

Most tools will run in Docker, in that case, prepare by pulling the image.

- Docker
- Docker Compose
- Burp Suite CE
- Bearer SAST
- ZAP Docker image
- Trivy
- A basic understanding of SQL Injections and JWTs.

**Windows users, use Powershell!** This since Git bash will not always handle paths correctly.

#### Setup - Burp Suite CE

Download Burp Suite from https://portswigger.net/burp/releases/professional-community-2023-10-2-3?requestededition=community&requestedplatform=

#### Setup - Bearer (SAST)

There are many paid tools to do SAST, we will use one, quite recently released, open source SAST tool, [Bearer](https://docs.bearer.com/)

*Welcome to the Bearer CLI documentation. Bearer CLI is a static application security testing (SAST) tool that scans your source code and analyzes your data flows to discover, filter and prioritize security and privacy risks.*

**Installation**

[Docs - installation](https://docs.bearer.com/reference/installation/)

**Docker**

```docker run --rm -v /path/to/repo:/tmp/scan bearer/bearer:latest-amd64 scan /tmp/scan```

**Install script**

```curl -sfL https://raw.githubusercontent.com/Bearer/bearer/main/contrib/install.sh | sh```

**Homebrew**

```brew install Bearer/tap/bearer```

**Debian / Ubuntu**

```
sudo apt-get install apt-transport-https
echo "deb [trusted=yes] https://apt.fury.io/bearer/ /" | sudo tee -a /etc/apt/sources.list.d/fury.list
sudo apt-get update
sudo apt-get install bearer
```

#### Setup - Trivy

It is recommended to not use Docker when running Trivy. This, since the setup with the Trivy container is, in comparison, more complicated.

> Windows users, you are required to use Docker in this case.

[Docs - installation](https://aquasecurity.github.io/trivy/v0.46/getting-started/installation/)

**Docker**

[See docker part in installation docs](https://aquasecurity.github.io/trivy/v0.46/getting-started/installation/)

**Install script**

```curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin v0.46.0```

**Homebrew**

```brew install aquasecurity/trivy/trivy```

#### Setup - ZAP scanning

```docker pull ghcr.io/zaproxy/zaproxy```
