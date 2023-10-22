#### Task - Access log

Search for: */support/logs*. There seems to be a problem with: *Missing access restriction to directory listing detected*

#### Find the vulnerability

A JWT is very often used to do authentication and authorization when communicating with an API. By modifying the JWT, setting the alg to "none" we may bypass access, as detailed in the CVE.

- Track the traffic to the Juice Shop and look at the JWT sent in the Authorization header.
- Copy the token, i.e. the header without the `Bearer ` and paste it into the editor at: https://jwt.io
- Look again at the vulnerability, it seems that by setting the "alg" to "none" we may bypass JWT validation.
- Replace the "RS256" in "alg" with "none" and replace the email of the user with: ```jwtn3d@juice-sh.op```.
- Convert the header and payload separately into base64 using e.g. https://base64.guru/converter
- Form a new token on the format: ```<header-base64>.<payload-bas64>.```. MAKE SURE TO REMOVE THE PADDING "=" at the end of each base64-encoded string as it is not part of the JWT specification.
- Send a request to localhost:3000/rest/whoami using the new token ```Bearer <token>```
- Look at the Juice Shop score board.
