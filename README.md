#### JSend

All GoMoney services implement [the JSend specification](https://github.com/omniti-labs/jsend) for serving API responses. In a nutshell, this middleware attaches a JSend object to each request with `success`, `fail`, and `error` methods that can then be invoked later on. This means our responses usually follow the same general formats.

_Successes_

```json
{
  "status" : "success",
  "data" : {
     "relevant data here : ""
  }
}
```

_Fails_ - (note: we don't really ever use this)

```json
{
  "status" : "fail",
  "data" : {
    "relevant info of why the API call failed" : ""
  }
}
```

_Errors_ 

```json
{
  "status" : "error",
  "message" : "relevant message about the erorr here",
  "code" : "error code, often just the http status code",
  "error_code": "3-digit-internal-error-code",
  "data" : {
    "any other relevant info about the error here i.e. conditions, stack traces etc" : ""
  }
}
```

The `status` key - a simple key to let you know upfront if the request was successful or not. This is merely an added convenience as the HTTP status code attached to the server response can also be used to determine the result of an API call. This is the only key that is universal across requests.

The `message` key - In the event of an error, the message key will contain a description of the error. This key is only available in error responses

The `data` key - is where you want to look at for the result of your request. It can either be an object, or an array depending on the request made. In the event of an error, this usually filled with additional information on whatever error occurred for help with debugging.

The `code` key - This key is only available in the event of an error. This simply mirrors whatever HTTP status code accompanied the request as another added convenience. This code could also just be read from the request itself.

The `error_code` key - This key is only available in the event of an error. This is a 3 digit internal error code mapped to *exact internal errors*. This is usually an int from `100` - `999`. An `InsufficientFundsError` could be maped to `303` and when the client sees a `error_code` that's `303` it can display an exact screen or message.

> Note: because we follow the JSend specification, this means the content type for responses will always be `application/json`!


