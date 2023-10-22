#### Task - Access log

Found anything related to logs in the SAST output? It is not necessarily displayed as a HIGH impact vulnerability.


- Track the traffic to the website and look at the JWT sent in the authorization header.
- Copy the token, i.e. the header without the `Bearer ` part into the editor at: https://jwt.io
- Look again at the vulnerability, it seems that by setting the "alg" to "none" we may bypass JWT validation.
- Change the replace the "RS256" in "alg" with "none" and replace the email of the user with: ```jwtn3d@juice-sh.op```.
- Convert the header and payload separately into base64 using e.g. https://base64.guru/converter
- Form a new token on the format: ```<header-base64>.<payload-bas64>.```. MAKE SURE TO REMOVE THE PADDING "=" at the end of each base64-encoded string as it is not part of the JWT spec.
- Send a request to localhost:3000/rest/whoami using the new token ```Bearer <token>```
