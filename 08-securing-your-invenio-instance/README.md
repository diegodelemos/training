## Tutorial 08 - Securing your Invenio instance

In this session, we will discover the key points which guarantee proper security on an Invenio instance. We will see from securing the web application to procedures on how to encrypt user data stored on the DB. We will explore with small exercises how vulnerabilities can be exploited in the event of missconfigurations..

Jump to: [Step 1](#step-1) | [Step 2](#step-2) | [Step 3](#step-3) | [Step 4](#step-4)

Any extra long description

## Step 1

Start from a clean and working instance:

```bash
$ cd 08-securing-your-invenio-instance/
$ ./init.sh
```

## Step 2: allowed hosts

```console
$ http --verify false https://127.0.0.1:5000/api/records/ Host:test
HTTP/1.0 400 BAD REQUEST
Content-Length: 56
Content-Security-Policy: default-src 'self'; object-src 'none'
Content-Type: application/json
Date: Tue, 12 Mar 2019 22:18:29 GMT
Referrer-Policy: strict-origin-when-cross-origin
Server: Werkzeug/0.14.1 Python/3.6.7
Strict-Transport-Security: max-age=31556926; includeSubDomains
X-Content-Security-Policy: default-src 'self'; object-src 'none'
X-Content-Type-Options: nosniff
X-Frame-Options: sameorigin
X-XSS-Protection: 1; mode=block

{
    "message": "Host \"test\" is not trusted",
    "status": 400
}
```

But if we use a host specified in `APP_ALLOWED_HOSTS`:
```console
http --verify false https://127.0.0.1:5000/api/records/ Host:the-invenio.com
HTTP/1.0 200 OK
Content-Length: 294
Content-Security-Policy: default-src 'self'; object-src 'none'
Content-Type: application/json
Date: Tue, 12 Mar 2019 22:19:58 GMT
Link: <https://the-invenio.com/api/records/?page=1&sort=mostrecent&size=10>; rel="self"
Referrer-Policy: strict-origin-when-cross-origin
Retry-After: 3600
Server: Werkzeug/0.14.1 Python/3.6.7
Strict-Transport-Security: max-age=31556926; includeSubDomains
X-Content-Security-Policy: default-src 'self'; object-src 'none'
X-Content-Type-Options: nosniff
X-Frame-Options: sameorigin
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4999
X-RateLimit-Reset: 1552432799
X-XSS-Protection: 1; mode=block

{
    "aggregations": {
        "keywords": {
            "buckets": [],
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0
        },
        "type": {
            "buckets": [],
            "doc_count_error_upper_bound": 0,
            "sum_other_doc_count": 0
        }
    },
    "hits": {
        "hits": [],
        "total": 0
    },
    "links": {
        "self": "https://the-invenio.com/api/records/?page=1&sort=mostrecent&size=10"
    }
}
```

## Step 3: change secret key

```diff
--- a/the_invenio/config.py
+++ b/the_invenio/config.py
@@ -128,7 +128,7 @@ JSONSCHEMAS_HOST = 'the-invenio.com'

 #: Secret key - each installation (dev, production, ...) needs a separate key.
 #: It should be changed before deploying.
-SECRET_KEY = 'CHANGE_ME'
+SECRET_KEY = '<freshly-generated-key>'
 #: Max upload size for form data via application/mulitpart-formdata.
 MAX_CONTENT_LENGTH = 100 * 1024 * 1024  # 100 MiB
 #: Sets cookie with the secure flag by default
```

## What did we learn

* Develop on Invenio
* Be happy!
* Understand something