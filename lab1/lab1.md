## Lab 1 - Getting started & DAST (25-30 min)

During this workshop, the [Juice Shop application](https://owasp.org/www-project-juice-shop/) from OWASP (Open Source Foundation for Application Security) will be analyzed and tested in various ways, in order to find and track vulnerabilities.

The tools used in lab1 will be the popular DAST (Dynamic Application Security Scanning) tool, [OWASP ZAP](https://www.zaproxy.org/) as well as the community edition of Burp Suite.

-------

#### Task - Getting to know the Juice Shop & Burp

The Juice Shop is the self-proclaimed - *most modern and sophisticated insecure web application*. It is developed by OWASP to give developers a chance to get hands on experience exploiting different vulnerabilities; it is commonly used in workshops such as this one.

**Start the Juice Shop application**

To begin with, start the Juice Shop using Docker.

```docker run -d -p 3000:3000 bkimminich/juice-shop```

Leave the app running! Make sure it is accessible on port 3000 of the IP your Docker host is using, usually http://localhost:3000.

**Start Burp Suite**

Use a temporary project and the Burp defaults.

**Open the Juice Shop in Burp**

1. Select *Proxy tab > Open browser*.
2. Open the Juice Shop in the Burp browser (Chromium).
3. Visit the *HTTP history* tab in Burp, it is on the Proxy page.
4. Make sure some HTTP data from the Juice Shop has been captured.

**Create a Juice Shop account**

*Account (top right) -> Login -> Not yet a customer -> Create account*

Do not forget the credentials by e.g. using the ones outlined below:

> email: admin@test.com password: password

**Login to the Juice Shop**

Use your credentials to log in.

**Add a product to the basket**

**Look at the recorded traffic**

In Burp, go to: *Proxy > HTTP history*

As you may have seen, the traffic was recorded by Burp. Burp suite has the capability to intercept and modify request as well as resending request with alternate properties. Such capabilities will be used later on, the objective of this task was to get a feel for the Juice Shop and Burp.

-----

#### Task - Another one bites the DAST

A DAST scan will typically scan a running application for vulnerabilities, contrary to SAST, which statically checks the code.

Burp is able to scan applications for vulnerabilities. This may be done from the UI, but in order to emulate how this could be integrated into a CI/CD pipeline - we'll make use of ZAP, running in a container.

**Do a passive security scan**

A passive scan will not actually attack the application, contrary to an active scan. It will just run a passive analysis. You may run a passive scan using:

```docker run -v <PATH_TO_LAB_1>:/zap/wrk/:rw --network host -t ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -r passive.html -j -a -t http://<INSERT_APP_IP>:3000```

> It might take some time...

> Depending on your Docker setup, you might have to use the root user: -u 0 

> stdout will display a report, so will the mounted directory (passive.html).

**Reflect on the errors seen and on how effective the scan was.**

**Briefly, discuss among you, what did you scan?**

**Perform an API scan**

ZAP may also ingest GraphQL, OpenAPI and SOAP specifications to do an API scanning. Run the following to scan a very limited part of the Juice Shop API, as defined in the *lab1/api.yaml* API specification.

```docker run -v <PATH_TO_LAB_1>:/zap/wrk/:rw -u 0 --network host -t ghcr.io/zaproxy/zaproxy:stable zap-api-scan.py -f openapi -r api.html -t ./api.yml```

> The Scan is using the URL in the api.yml (localhost:3000). In case of your Docker setup, you will have to change this.

**Investigate the result**

The API scan will output a report, `api.html`. Investigate the report.

What did ZAP find?

**Task - Exploit the SQL Injection vulnerability found in the API**

*Objective: Login as an admin*

This lab is not about the vulnerability itself, it is about finding it and understanding the impact. Therefore, hints as well as the full solution is provided in this folder.

Two hints to start you of:
- You must logout first.
- The first database user is the admin...

>*OWASP Top Ten*
[3 - Injection](https://owasp.org/Top10/A03_2021-Injection/)

**Once you are done, go to the score board**

Great work!

Visit: ```localhost:3000/#/score-board```

The Juice Shop has a built in score board to keep track of the vulnerabilities you have exploited.

>As you see, there are a many potential challenges to solve!!

-------

#### Task - Payback time (Optional)

**Solve the 3 star task - Payback time**

*Objective: Place an order that makes you rich*

Using Burp in this task is highly recommended.

In order to repeat a request:

Right click on it in: *HTTP history > Send to repeater > Open repeater and modify > Send*.

> *OWASP Top Ten*
> [4 - Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/)

#### Task - View basket (Optional)

**Solve the 2 star task - View Basket**

*Objective: View another user's shopping basket.*

>*OWASP Top Ten*
[1 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

> A walkthrough is available in the UI on the scoreboard.

**Final question**

How could the 2 vulnerabilities above have been mitigated?

#### Extra tasks

Continue with tasks you find interesting

**EXCEPT**
- 5 star: Unsigned JWT
