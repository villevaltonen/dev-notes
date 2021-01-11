## CORS

- CORS is meant for securing HTTP-requests from different origins
  - For example, from `myapp.com` to `api.myapp.com`
- CORS does a pre-flight request (OPTIONS), which expects guidelines from server for requests
- The most common CORS-headers are:
  - `Access-Control-Allow-Origin`, e.g. `myapp.com` (supports wildcard, but not advised unless public API)
  - `Access-Control-Allow-Methods`, e.g. `GET, POST, PUT, DELETE, OPTIONS`
  - `Access-Control-Allow-Headers`, e.g. `Accept, Content-Type, Content-Length, Accept-Encoding, X-CSRF-Token, Authorization`

### CORS and cookies

- To be able to set HTTP-only cookies you need to fulfill these requirements:
  - SameSite should be none
  - Secure enabled (means only HTTPS is allowed)
