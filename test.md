# SSAA Project

Easing authentication and authorization process for applications


## JS Client

```js
class JwtManager {
    token
    groups
 
    constructor(token)


    isAuthenticated
    isAuthorized(action)
    getToken()
    getPayload() {
        ntid
        exp
        meta => saml claims
    }
    login()
    logout()
}
```

```js
acl {
    action1: [ groups... ],
    action2: [ groups... ],
    action3: [ groups... ]
    // ...
}
```

### Notes

1. Storage of token is left to the application

2. token is also sent in cookies (this only works if it is same domain)

3. same domain also resolves cors issues

4. up to user to place token in fetch requests header (getToken()) - this is not needed if sent in cookies and same domain

5. Scalability of ACL, should it be used to only check for parts of code that need to be run, based on permissions ?

6. Pure vanilla javascript can be used in any framework

7. login (), to obtain the JWT via SAML ADFS

8. Logout (), optional... need not be used, but you may want to use it to direct user to another site or some other task

---

## Sidecar

```
User Application ---> Sidecar /auth&redirect_to="automatically set in JS Client library" ---> SSO

User Application ---> Sidecar /whatever... ---> API if verified & authorised
                      401 if not verified or authorised
```

### Notes

1. JWT to be passed on to API Server still needs to be decoded (verification not needed). Does this need to be improved?

2. ACL specified

```js
{
    '/some/:aa/:bb': {
        GET: [groups...]
        POST: [groups...]
        PATCH: [groups...]
    }
    // ...
}
```

- unmatched path will 401. should it be 404?
- unmatched verb will 401. shoud is be 404?
- unmatched group will be 401

3. Should it be more permissible instead? e.g. if unmatched just allow?

---

## JWT Server

Primary aim is to provide JWT Token with SAML ADFS Claims

Simply use GET /auth

Callback will be to /?token=<token>

Those end-points should be configurable? or fixed?

