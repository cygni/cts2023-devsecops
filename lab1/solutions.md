#### Exploit the vulnerability found in the API

There is the possibility to do a SQL injection attack against the Juice Shop, as suggested by the scan output.

[SQL injection](https://www.w3schools.com/sql/sql_injection.asp)

To execute the attack, do the following:

**Go to the login page**

**Use the following inside the email field when logging in**

email: ```' or 1=1--```
password: ```<write-anything>```

#### Payback time

**Go over to ZAP again**

**Add an item to the cart and look at the request**

**Open the request in the repeater**

**Resent the request, although this time add -10 items**
