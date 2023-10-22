## Lab 1 - Getting started, SAST & DAST

During this workshop, the [Juice Shop application](https://owasp.org/www-project-juice-shop/) from OWASP will be analyzed and tested in various ways, in order to find and track vulnerabilities.

For this lab we will explore one of the most used DAST tools, [ZAP](https://www.zaproxy.org/), developed by OWASP. We will also utilize the community edition of BURP Suite.

-------

#### Task - Getting to know the Juice Shop

**Start the Juice Shop application**

```docker run -d -p 3000:3000 bkimminich/juice-shop```

Leave the app running!

**Start BURP Suite**

Use a temporary project and the BURP defaults.

**Open the Juice Shop in BURP**

1. Go to proxy > open browser
2. Open the Juice Shop in the BURP browser (Chromium)
3. Visit *target* tab and find the panel to the left containing the Juice shop application.

> It is dependent on your Docker setup where the Juice Shop starts, most likely it will be visible at: ```127.0.0.1:3000```

**Create a Juice Shop account**

Account (top right) -> Login -> Not yet a customer -> Create account

**Login to the Juice Shop**

Use your credentials and log on to the Juice Shop.

**Add money to your wallet**

Account -> Orders & Payment -> Digital wallet

Use a random 16-digit card number like: 4545454545454545 and e.g. 200 as the amount to fill up.

You will have to add a card.

**Purchase a product**

Add any product (home page) -> Go to basket (top right) -> Input address -> Choose delivery -> Select pay using wallet -> Place order and pay.

**Look at the recorded traffic**

Go to proxy > HTTP history

As you may have seen, the traffic was recorded by BURP, which facilities further investigation as well as manipulation. For example, BURP suite has the capability to intercept and modify request as well as to resend request with alternate properties.

Such capabilities will be used later on, the objective in this task was to get a feel for the Juice Shop and Burp.

-----

#### Task - Another one bites the DAST

A DAST scan will typically scan a running application for vulnerabilities, contrary to SAST, which statically checks the code.

BURP is able to scan applications for vulnerabilities. This may be done from the UI, but in order to emulate how this could be integrated into a CI/CD pipeline - we'll make use ZAP from OWASP, running in Docker.

**Do a passive security scan**

A passive scan will not actually attack the application, contrary to an active scan. It will just run a passive analysis. You may run a passive scan using:

```docker run -u 0 --network host -t ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -r passive.html -j -a -t http://<INSERT_APP_IP>:3000```

**Reflect on the errors seen and on how effective the scan was.**

**Perform an API scan**

ZAP may also ingest GraphQL, OpenAPI and SOAP specifications and perform a scan of an API. Run the following to scan a very limited part of the Juice Shop API.

```docker run -v <PATH_TO_LAB_1>:/zap/wrk/:rw -u 0 --network host -t ghcr.io/zaproxy/zaproxy:stable zap-api-scan.py -f ./lab1/api.yml -r api.html```

**Investigate the result**

The API scan will output a report `api.html`, investigate the report.

What did ZAP find?

**Task - Exploit the vulnerability found in the API**

*Objective: Login as an admin*

Hints and solution is provided in this folder.

As a seasoned hacker this should be a piece of cake, note that the first database user is the admin...

>*OWASP Top Ten*
[3 - Injection](https://owasp.org/Top10/A03_2021-Injection/)

**Go to the score board**

Visit: ```localhost:3000/#/score-board```

The Juice Shop has a built in score board to keep track of the vulnerabilities you have exploited.

>As you see, there are a many potential challenges to solve!!

-------

#### Task - Payback time

**Solve the 3 star task - Payback time**

*Objective: Place an order that makes you rich*

It's highly recommended that you use BURP. You could look at the HTTP history.

In order to repeat a request:

Right click on it in the HTTP history > send to repeater > Open repeater and modify > Send

> *OWASP Top Ten*
> [4 - Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/)


#### Task - View basket

**Solve the 2 star task - View Basket**

*Objective: View another user's shopping basket.*

>*OWASP Top Ten*
[1 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

**Final question**

How could these vulnerabilities have been mitigated?

#### Extra tasks

Continue with tasks you find interesting

**EXCEPT**
- 5 star: Unsigned JWT
