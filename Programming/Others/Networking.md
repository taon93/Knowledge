### HTTP protocol: 
There are 3 versions of the protocol that are widely used: 1.1, 2 and 3. 
- 1.1 - improvement over 1.0 - except protocol unification and efficiency and ambiguity fixes it enabled sending pipelined
requests and keep alive connections
- 2 - efficiency upgrade, enabled to stream multiplexed requests
- 3 - HTTP over QUICK - changed sublayer protocol from TCP to QUICK (which is based on UDP)
#### Main HTTP methods (verbs): 
- GET: Used to request website or data from the server, usually don't have a body and is used to retrieve data.
- HEAD: used like GET - send only header part of the message - without body.
- POST: Used to post data on the server, create request
- PUT: Similar to the POST method but used also for updating data: whereas POST should be used only for creation - in real applications
POST is used most of the time, regardless of creation or update.
- DELETE: Delete specified resource
- TRACE: server responds with the same message as received from TRACE, used to check if messages are altered by any proxies or 
to check if there are no connectivity issues.
- OPTIONS: responds with the set of methods supported by the endpoint
- PATCH: alters specified resources (update function)
#### Server response classes: 
- 100: information
- 200: success
- 300: redirections (resource is no longer there) 
- 400: client error
- 500: server error