## Prerequisites

- Burp Suite CE
- Bearer SAST
- Trivvy
- Docker
- Docker Compose

#### Setup - Burp Suite CE

Download Burp Suite from https://portswigger.net/burp/releases/professional-community-2023-10-2-3?requestededition=community&requestedplatform=

#### Setup - Bearer (SAST)

There are many paid tools to do SAST, we will use one, quite recently released, open source SAST tool, [Bearer](https://docs.bearer.com/)

*Welcome to the Bearer CLI documentation. Bearer CLI is a static application security testing (SAST) tool that scans your source code and analyzes your data flows to discover, filter and prioritize security and privacy risks.*

**Installation**

[Docs - installation](https://aquasecurity.github.io/trivy/v0.18.3/installation/=)

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

#### Setup - Trivvy

It is recommended to not use Docker when running Trivy. This, since the setup with the Trivy container is, in comparison, more complicated since Docker-in-Docker is needed.

> Windows users, you are required to use Docker in this case.

[Docs - installation](https://aquasecurity.github.io/trivy/v0.18.3/installation)

**Docker**

[See docker part in installation docs](https://aquasecurity.github.io/trivy/v0.18.3/installation)

**Install script**

```curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin v0.18.3```

**Homebrew**

```brew install aquasecurity/trivy/trivy```

**Debian / Ubuntu**

```
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```
