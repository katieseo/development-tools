# Web security

MDN [https://developer.mozilla.org/en-US/docs/Web/Security]

## Same-origin policy

Under the same-origin policy, web browsers do not permit a web page to access resources who origin differ than that of the current page. The origin is considered to be different when the scheme, hostname or port of the resource do not match that of the page. Overcoming the limitations of same-origin security policy is possible using a technique called Cross-origin resource sharing or simply CORS.

```
example) http://store.company.com/dir/page.html
 
http://store.company.com/dir2/other.html   ---> same origin: only the path differs
https://store.company.com/page.html        ---> failure: different protocol
http://store.company.com:81/dir/page.html  ---> failure: differnt port
http://news.company.com/dir/page.html.     ---> failure: differnt host
```

Same-origin policy [https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy]

CORS [https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS]


## Types of attacks

#### Click-jacking

#### Cross-site scripting (XSS)
Cross-site scripting (XSS) is a security exploit which allows an attacker to inject into a website malicious client-side code.

#### Cross-site request forgery (CSRF)

#### Man-in-the-middle (MitM)

#### Session hijacking
