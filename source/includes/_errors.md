# Errors

<aside class="notice">
We tried to design the system as intuitive as possible and hope that every single interaction will go easy and smooth, but in case something went wrong, you'll have to deal with errors.
</aside>

The bookit api uses the following error codes:


Error Code | Name | Meaning
---------- | ------- | ------
400 | Bad Request | your request is invalid.
401 | Unauthorized | your authorization token is wrong.
403 | Forbidden | the resource requested is hidden for you.
404 | Not Found | the specified resource could not be found.
409 | Conflict | indicates that the request could not be processed because of conflict in the current state of the resource.
422 | Unprocessable Entity | your request structure is right, but something wrong with info you're passing to the service.
429 | Too Many Requests | you're requesting too many resources, slow down.
500 | Internal Server Error | we had a problem with our server, try again later.