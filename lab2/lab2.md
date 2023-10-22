
# Lab 2 - Static Analysis

## Part 1 - SAST

For this lab we will explore tools using static code analysis to find vulnerabilities in code.

#### SAST - Running static analysis

Let's do a static analysis of the Juice Shop application.

**Clone the Juice shop repository**

```git clone https://github.com/juice-shop/juice-shop.git --depth 1```

> Note: Do NOT run npm install. I've tried and it made the scan blow up even when node_modules were ignored...

**Analyze the Juice Shop application using Bearer**

Time to start analyzing the application using Bearer. You will have to modify the volume in accordance with your setup.

```docker run --rm -v <INSERT_JUICE_SHOP_PATH>:/tmp/scan bearer/bearer:latest-amd64 scan --output /tmp/scan/bearer.html --format html /tmp/scan```

or

```bearer scan --output /tmp/scan/bearer.html --format html /tmp/scan <juice-shop-path>```

**Look at the output**

Did the tool find any vulnerabilities?

Is there an overlap with the earlier DAST scans?

#### Task - Access log

*Objective: Access the logs of the application*

Use the output of the SAST scan to access the application logs.

>*OWASP Top Ten*
[1 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

**Question**

What are the implications of leaving such an error unfixed?

------

**Reflect upon the coverage of the SAST test**

Why was this error not caught?

## Part 2 - Dependency... (hell)

#### Task - Investigate the Juice Shop´s dependencies

**Use Trivy to analyze the Juice Shop Docker image**

```trivy image bkimminich/juice-shop```

or

```docker run --rm -v C:\Users\<INSERT_PATH_TO_LAB>:/root/.cache/ aquasec/trivy:0.19.2 aquasec/trivy:0.19.2 bkimminich/juice-shop```

**Look through the output from Trivy**

There will definitely be a couple of errors in the dependencies, but let´s hold on further investigation a bit.

**Generate a BOM**

```trivy image --format cyclonedx --output bom.json bkimminich/juice-shop```

or

```docker run --rm -v C:\Users\<INSERT_PATH_TO_LAB>:/root/.cache/ aquasec/trivy:0.19.2 aquasec/trivy:0.19.2 image --format cyclonedx --output bom.json bkimminich/juice-shop```

This will create a bom at `bom.json`

**Start dependency-track**

Dependency track is an OWASP tool that keeps track of dependencies and vulnerabilities over time. It ingests the dependencies of projects using a BOM.

```docker-compose -f dependency-track.yml up```

**Login into dependency track**

- Visit localhost:8080
- Login using: admin / admin
- Change the password to something else

**Create a new project**

- Go to project (left menu bar)
- Create a new project:
  name: *Juice Shop*
  classifier: *Application*

**Import the BOM**

Go to: project (Juice Shop) -> components -> import BOM

Import the BOM into dependency track, this could take a little while. Just ignore any errors as long as you successfully ingested about 1600 components...

**Check if you are targeted by a new vulnerability**

A new security issue has been reported for the *jsonwebtoken* package of version < 9. Check if you are affected!

Go to: components (left menu bar) -> search (jsonwebtoken)

**Find the vulnerability**

What is the issue with the 0.1.0 version of the package?

View the 0.1.0 version component and then the vulnerabilities.

**Exploit the vulnerability**

*Objective: Authenticate but as the bogus user: ```jwtn3d@juice-sh.op```*

In hints, there is a step-by-step guide to follow, but for the more seasoned hacker, try exploiting without any hints.

> *OWASP Top Ten*
[1 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
[2 - Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)
[6 - Outdated Components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/)
