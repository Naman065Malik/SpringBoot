# HTTP Status Codes

- **Information responses (100 - 199)**
- **Successful responses (200 - 299)**
    - 200 OK
        
        The request succeeded. Everything is Fine.
        
    - 201 Created
        
        The request succeeded, and a new resource was created as a result. This is typically the response send after `POST` requests, or some `PUT` requests.
        
    - 202 Accepted
        
        The request has been received but not yet acted upon. It is intended for cases where another process or server handles the request, or for bath processing (Ex.- Sending notifications through email).
        
    - 204 No Content
        
        There is no content to send for this request, but the headers may be useful.
        
- **Redirection messages (300 - 399)**
    - 301 Moved Permanently
        
        The URL of the requested resource has been changed permanently. The new URL is given in the response.
        
    - 302 Found
        
        This response code means that URL of requested resource has been changed temporarily.
        
- **Client error responses (400 - 499)**
    - 400 Bad Request
        
        The Server cannot or will not process the request due to something that is perceived to be client error. (Ex- invalid request, malformed request syntax.)
        
    - 401 Unauthorized
        
        This response means `unauthenticated` . That is, the client must authenticate itself to get response.
        
    - 403 Forbidden
        
        The client does not have access to the content. Unlike `401 Unauthorized` , the client’s identity is known to the server.
        
    - 404 Not Found
        
        The server cannot find the requested resource.
        
- **Server error responses (500 - 599)**
    - 500 Internal Server Error
        
        The server has encountered a situation it does not known how to handle.
        
    - 502 Bad Gateway
        
        This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response.
        
    - 503 Service Unavailable
        
        The server is not ready to handle the request due to maintenance or server overload.
        

You can learn more about these Status Codes on official website of mdn ([HTTP response status codes - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status))