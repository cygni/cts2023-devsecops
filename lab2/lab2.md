# Lab 2 - Static Analysis (20-30 min)

## Part 1 - SAST (Static Application Security Testing)

This part of the laboration will utilize static analysis to find vulnerabilities in the source code.

#### SAST - Running static analysis

Bearer is the designated tool to run a SAST scan during this lab.

**Clone the Juice Shop repository**

```git clone https://github.com/juice-shop/juice-shop.git --depth 1```

> Note: Do NOT run npm install.

**Analyze the Juice Shop source code using Bearer**

Run the following command and modify the path in accordance with your setup.

```docker run --rm -v <INSERT_JUICE_SHOP_CODE_PATH>:/tmp/scan bearer/bearer:latest-amd64 scan --output /tmp/scan/bearer.html --format html /tmp/scan```

or

```bearer scan --output ./juice-shop/bearer.html --format html <INSERT_JUICE_SHOP_CODE_PATH>```

The bearer.html report will be available in the juice-shop folder.

**Look at the output**

Did the tool find any vulnerabilities?

Is there an overlap with the earlier DAST scans?

#### Task - Access log

*Objective: Access the logs of the Juice shop*

Use the output of the SAST scan to access the application logs.

>*OWASP Top Ten*
[1 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

**Question**

What are the implications of leaving such an error unfixed?

## Part 2 - Dependency... (hell)

#### Task - Investigate the Juice Shop´s dependencies

**Use Trivy to analyze the Juice Shop Docker image**

Trivy is one of many tools capable to analyze a OCI compliant images. Run a security scan of the Juice Shop image using:

```trivy image --format json --output ./lab2/vuln.json bkimminich/juice-shop```

or

```docker run --rm -v C:\Users\<INSERT_PATH_TO_LAB_2>:/root/.cache/ -v C:\Users\<INSERT_PATH_TO_LAB_2>:/tmp/output aquasec/trivy:0.46.0 image --format json --output /tmp/output/vuln.json bkimminich/juice-shop```

**Look through the output from Trivy**

There will definitely be a couple of errors in the dependencies, but let´s hold further investigation a bit.

**Generate a BOM**

```trivy image --format cyclonedx --output bom.json bkimminich/juice-shop```

or

```docker run --rm -v C:\Users\<INSERT_PATH_TO_LAB_2>:/root/.cache/ -v C:\Users\<INSERT_PATH_TO_LAB_2>:/tmp/output aquasec/trivy:0.46.0 image --format cyclonedx --output /tmp/output/bom.json bkimminich/juice-shop```

This will create a bom at `bom.json`

**Start dependency-track**

Dependency track is an OWASP tool that keeps track of dependencies and vulnerabilities over time. It ingests the dependencies of projects using a BOM.

Start it using:

```docker-compose -f dependency-track.yml up```

**Login to dependency track**

- Visit *localhost:8080*
- Login using: admin / admin
- Change the password to something else

**Create a new project**

- Go to project (left menu bar)
- Create a new project:
  name: *Juice Shop*
  classifier: *Application*

**Import the BOM**

Go to: *Project (Juice Shop) -> Components -> Upload BOM*

Import the BOM into dependency track; this may take a while. Just ignore any errors as long as about 1600 components were successfully ingested.

> There is a refresh button

**A new vulnerability is all over the news, check if you are targeted!!**

A new security issue has been reported for the *jsonwebtoken* package of version < 9. Check if you are affected!

Go to: *Components (left menu bar) -> Search (jsonwebtoken)*

**Find the vulnerability**

What is the issue with the 0.1.0 version of the package?

View the 0.1.0 version component and then the vulnerabilities.

**Exploit the vulnerability CVE-2022-23540**

*Objective: Authenticate in the Juice Shop, but as the bogus user: ```jwtn3d@juice-sh.op``` when calling the API.*

In solutions, there is a step-by-step guide to follow as this is a more challenging exploit.

For the more seasoned hacker, try exploiting without the solution.

> *OWASP Top Ten*
[1 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
[2 - Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/)
[6 - Outdated Components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/)
