## Lab 1 - Getting started & DAST (25-30 min)

During this workshop, the [Juice Shop application](https://owasp.org/www-project-juice-shop/) from OWASP will be analyzed and tested in various ways, in order to find and track vulnerabilities.

For this lab we will explore one of the most used DAST tools, [ZAP](https://www.zaproxy.org/), developed by OWASP. We will also utilize the community edition of Burp Suite.

-------

#### Task - Getting to know the Juice Shop & Burp

The Juice Shop is probably the worlds most stable, but extremely insecure, application. It is developed by OWASP to give developers a chance to test exploiting different security issues and is commonly used in workshops such as this one.

**Start the Juice Shop application**

To begin with, start the Juice Shop using Docker.

```docker run -d -p 3000:3000 bkimminich/juice-shop```

Leave the app running! Make sure it is accesible on port 3000 of the IP your Docker host is using (usually http://localhost:3000).

**Start Burp Suite**

Use a temporary project and the Burp defaults.

**Open the Juice Shop in Burp**

1. Go to proxy > open browser
2. Open the Juice Shop in the Burp browser (Chromium)
3. Visit the *HTTP history* tab inside the proxy tab.
4. Make sure you can see some HTTP data from the Juice Shop.

**Create a Juice Shop account**

Account (top right) -> Login -> Not yet a customer -> Create account

To not forget the credentials, I suggest you use:

*email*: admin@test.com *password*: password

**Login to the Juice Shop**

Use your credentials and log on to the Juice Shop.

**Add a product to the basket**

**Look at the recorded traffic**

In Burp, go to: Proxy > HTTP history

As you may have seen, the traffic was recorded by Burp. Burp suite has the capability to intercept and modify request as well as resending request with alternate properties. Such capabilities will be used later on, the objective of this task was to get a feel for the Juice Shop and Burp.

-----

#### Task - Another one bites the DAST

A DAST scan will typically scan a running application for vulnerabilities, contrary to SAST, which statically checks the code.

Burp is able to scan applications for vulnerabilities. This may be done from the UI, but in order to emulate how this could be integrated into a CI/CD pipeline - we'll make use ZAP from OWASP, running in Docker.

**Do a passive security scan**

A passive scan will not actually attack the application, contrary to an active scan. It will just run a passive analysis. You may run a passive scan using:

```docker run -v <PATH_TO_LAB_1>:/zap/wrk/:rw --network host -t ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -r passive.html -j -a -t http://<INSERT_APP_IP>:3000```

> It might take some time...

> Depending on your Docker setup, you might have to use the root user: -u 0 

> stdout will contain a report, so will the mounted directory (passive.html).

**Reflect on the errors seen and on how effective the scan was.**

**Perform an API scan**

ZAP may also ingest GraphQL, OpenAPI and SOAP specifications and perform a scan of an API. Run the following to scan a very limited part of the Juice Shop API, as defined in the lab1/api.yaml API specification.

```docker run -v <PATH_TO_LAB_1>:/zap/wrk/:rw -u 0 --network host -t ghcr.io/zaproxy/zaproxy:stable zap-api-scan.py -f openapi -r api.html -t ./api.yml```

> The Scan is using the URL in the api.yml (localhost:3000). In case of your Docker setup, you will have to change this.

**Investigate the result**

The API scan will output a report `api.html`, investigate the report.

What did ZAP find?

**Task - Exploit SQL Injection vulnerability found in the API**

*Objective: Login as an admin*

http://localhost:3000/#/login

You must of course logout first. More hints as well as the full solution is provided in this folder.

As a seasoned hacker this should be a piece of cake, note that the first database user is the admin...

>*OWASP Top Ten*
[3 - Injection](https://owasp.org/Top10/A03_2021-Injection/)

**Go to the score board**

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

Right click on it in the HTTP history > Send to repeater > Open repeater and modify > Send

> *OWASP Top Ten*
> [4 - Insecure Design](https://owasp.org/Top10/A04_2021-Insecure_Design/)

#### Task - View basket (Optional)

**Solve the 2 star task - View Basket**

*Objective: View another user's shopping basket.*

>*OWASP Top Ten*
[1 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

> Walkthrough is available in the UI on the scoreboard.

**Final question**

How could the 2 vulnerabilities above have been mitigated?

#### Extra tasks

Continue with tasks you find interesting

**EXCEPT**
- 5 star: Unsigned JWT
