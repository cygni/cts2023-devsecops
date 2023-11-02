## Prerequisites

- Docker
- Docker Compose
- Burp Suite CE
- Bearer SAST
- ZAP
- Trivy
- A basic understanding of [SQL Injections](https://en.wikipedia.org/wiki/SQL_injection) and [JWTs](https://en.wikipedia.org/wiki/JSON_Web_Token).

**Windows users, I recommend using Powershell** This since, for example, Git Bash doesn't always handle paths correctly, which may be problematic when using container volumes.

------------

### Setup - Burp Suite CE

Download Burp Suite from https://portswigger.net/burp/releases/professional-community-2023-10-2-3?requestededition=community&requestedplatform=

------------

### Setup - Bearer (SAST)

There are many paid tools to do SAST. In the workshop, the quite recently released open sourced SAST tool [Bearer](https://docs.bearer.com/) will be used.

**Installation**

[Docs - installation](https://docs.bearer.com/reference/installation/)

**Docker**

```docker pull bearer/bearer:latest-amd64```

**Install script**

```curl -sfL https://raw.githubusercontent.com/Bearer/bearer/main/contrib/install.sh | sh```

**Homebrew**

```brew install Bearer/tap/bearer```

------------

#### Setup - ZAP scanning

```docker pull ghcr.io/zaproxy/zaproxy```

------------

### Setup - Trivy

It is recommended to not use Docker when running Trivy. This since the setup with the Trivy container is, in comparison, more complicated since it requires Docker-in-Docker.

Windows users, you are required to use Docker in this case.

[Docs - installation](https://aquasecurity.github.io/trivy/v0.46/getting-started/installation/)

**Docker**

```docker pull aquasec/trivy:0.46.0```

**Install script**

```curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin v0.46.0```

**Homebrew**

```brew install aquasecurity/trivy/trivy```
