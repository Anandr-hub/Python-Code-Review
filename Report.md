# Code Review

## Exception Handling

- All Handlers Should have an Error Handling 
```angular2
@route('/foo_call', method='GET')
def fun_foo():
  try:
      # Write Some Code
     return Response(response, status_code=201)
  except Exception as e:
     return Response(str(e),status_code=401)

```

- No Proper Error Codes in Response
```angular2

200 Successfull Request. ...
400 Bad Request. ...
401 Unauthorized. ...
403 Forbidden. ...
404 Not Found. ...
500 Internal Server Error. ...
502 Bad Gateway. ...
503 Service Unavailable. ...
504 Gateway Timeout.
```

- No Proper Authentication Methods Being Used
` CSRF protection, Oauth2, JBWT `

##  Feature: Add User 
UC@001
```
Scenerio:
  - No Expection Handling Added
  - Check if the Token is None, If none, Return without running query
  - Unneccesary Db call 
  - Over-complex code
```
UC@002
```angular2
The Regex Implemented Only allows alphabets after Numbers order
If will not work in case whn user_name is like : test12test
We should have more precise Regex for username
```

UC@003
```
Use Oauth2,JBWT, Digest and othe secure authentications Instead of Simple Basic Authentication  
Which is not secure and easy to exploit
```

UC@004
```angular2
Sessons can be use to commit SQL database, which is more precise and atomic

```
##  Feature: Verify User

UC@001 
```
Instead of writting Sql Connect and commit code again, we can build some context manager class, adn reuse it in every API where ever needed

@contextmanager
def session_scope():
    session = Session()
    try:
        yield session
        session.commit()
    except Exception:
        session.rollback()
        raise
    finally:
        session.close()

This can be reused simply by :

with session_scope() as f:
  # Perform Read Write Operation

```

UC@002
```angular2
Write Query in seperate file for better Readability and uniqueness to reuse query wherever needed

```
UC@003
```angular2
Instead of running two diff queries we can simply run join operation on account and token

We can catch the User ID's instead of querying again on evry call and that catch can be refreshed on USer creation
```

##  Feature: User Debug

UC@001
```
Use cache and refresh cache on adding any account.
Use status code
Excepton Handling 

```
##  Feature: Get Token 
UC@001
```
We should never store User Password in database, Instead Best way is to use Oauth, But if you wana go for Basic authentication, 
Save password in encrypted Fashion to avoid vulnerabilities
```
UC@002
```
- Use Class Based Views and Seperate Query Models to reuse it in code
```

UC@003
```
Set Expiry in headers in response pass token in headers instead of response

response.set_header('Set-Cookie', 'name=value')

Add Params as required

max_age: Maximum age in seconds. (default: None)
expires: A datetime object or UNIX timestamp. (default: None)
domain: The domain that is allowed to read the cookie. (default: current domain)
path: Limit the cookie to a given path (default: /)
secure: Limit the cookie to HTTPS connections (default: off)

```

UC@004
```
- Use classes and create a UTIL functions to do basic operations
- Handle Atomic calls using sessions
```


##  Feature: Status

UC@001
```
ping -c Requires Sudo Permissions. SO needs to handle that

- Also Invalid params are being passed with the Ping 

- You can easily get RemoteIP by Inbuild request functions
request.getRemoteAddr(); and many more ways

```

UC@002
```
Use subprocess.Popen() instead which is better and can run wait and other async params
```

UC@003
```
Create a Request & Response class which can be used with all API's to Response with Status code and handle errors
```
