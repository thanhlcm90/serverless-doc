# CORS in Serverless

## Understanding CORS

There are two things we need to do to support CORS in our Serverless API.

1. Preflight OPTIONS requests

2. Respond with CORS headers

There’s a bit more to CORS than what we have covered here. So make sure to check out the [Wikipedia article for further details](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

If we don’t set the above up, then we’ll see something like this in our HTTP responses.

`No 'Access-Control-Allow-Origin' header is present on the requested resource`

And our browser won’t show us the HTTP response. This can make debugging our API extremely hard.

## Setup CORS in API Gateway

Go to the API Gateway detail, and click to `Enable CORS` in `Action` menu

![Alt text](images/cors1.png?raw=true)

Check the option Gateway Responses for Datacom POC API API `DEFAULT 4XX` and `DEFAULT 5XX` to enable CORS for error response.

![Alt text](images/cors2.png?raw=true)

Finally, click `Enable CORS and replace existing CORS headers`