# Cookie value extractor plugin for traefik

[![Build Status](https://github.com/bonovoxly/extract-cookie-regex/workflows/Main/badge.svg?branch=master)](https://github.com/bonovoxly/extract-cookie-regex/actions)

**Shoutout to <https://github.com/v-electrolux/extrac-cookie> for the original code...*


Takes specified cookie value (from Cookie header)
and put it in a header you want. You can declare prefix for value,
which will be added to cookie value.
If there is no such cookie, the header value will not be set 

## Configuration

### Fields meaning
- `cookieName`: name of cookie, its value will be extracted. 
   No default, mandatory field
- `headerNameForCookieValue`: name of header, in which will be put cookie value.
  Default is 'Authorization'
- `cookieValuePrefix`: string prefix, that will be added to cookie value.
  Default is 'Bearer '
- `logLevel`: `warn`, `info` or `debug`. Default is `info`

### Static config examples

- cli as local plugin
```
--experimental.localplugins.extract-cookie-regex=true
--experimental.localplugins.extract-cookie-regex.modulename=github.com/bonovoxly/extract-cookie-regex
```

- envs as local plugin
```
TRAEFIK_EXPERIMENTAL_LOCALPLUGINS_extract-cookie-regex=true
TRAEFIK_EXPERIMENTAL_LOCALPLUGINS_extract-cookie-regex_MODULENAME=github.com/bonovoxly/extract-cookie-regex
```

- yaml as local plugin
```yaml
experimental:
  localplugins:
     extract-cookie-regex:
      modulename: github.com/bonovoxly/extract-cookie-regex
```

- toml as local plugin
```toml
[experimental.localplugins.extract-cookie-regex]
    modulename = "github.com/bonovoxly/extract-cookie-regex"
```

### Dynamic config examples

- docker labels
```
traefik.http.middlewares.extractCookieRegexMiddleware.plugin.extract-cookie-regex.cookieName=test_access_token
traefik.http.middlewares.extractCookieRegexMiddleware.plugin.extract-cookie-regex.headerNameForCookieValue=Authorization
traefik.http.middlewares.extractCookieRegexMiddleware.plugin.extract-cookie-regex.cookieValuePrefix=Bearer 
traefik.http.middlewares.extractCookieRegexMiddleware.plugin.extract-cookie-regex.logLevel=warn
traefik.http.routers.extractCookieRegexRouter.middlewares=extractCookieRegexMiddleware
```

- yaml
```yml
http:

  routers:
    extractCookieRegexRouter:
      rule: host(`demo.localhost`)
      service: backend
      entryPoints:
        - web
      middlewares:
        - extractCookieRegexMiddleware

  services:
    backend:
      loadBalancer:
        servers:
          - url: 127.0.0.1:5000

  middlewares:
    extractCookieRegexMiddleware:
      plugin:
        extract-cookie-regex:
          cookieName: test_access_token
          headerNameForCookieValue: Authorization
          cookieValuePrefix: 'Bearer '
          logLevel: warn
```
