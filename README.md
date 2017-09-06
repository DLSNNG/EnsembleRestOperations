# Instructions

## Running the Example BPL

1. Fork Repo and place REST Example Production.xml file on your Cache Server
2. Follow instructions from [this post](https://community.intersystems.com/post/how-export-and-import-ensemble-components-and-productions) to import the classes
3. Enable RestExampleOperation, RestExampleBPL, and RestExampleService

Every 30 seconds (the poll interval we set in our Service's configuration):

* The Service should loop from 1-10 and pass a new Ens.StringRequest to our BPL, with the StringValue set to the current iteration (e.g. 1 for the first, 2 for the second, etc.)
* The BPL should call the Operation, passing in a DLS.REST.Request message
* The Operation should read the FormVariables property of this message and perform a GET request using the FormVariables as query params.
* The Operation should return a DLS.REST.Response message with the server's response as the ServerResponse property.
* The BPL should store the server's response to the Context, convert it to an %Object, then trace all the entries that were returned.

## How to use the DLS.REST.Operation with your own BPL
1. Add an instance of DLS.REST.OperationV2 to your production for each rest request you would like to make.
2. Set the Server and URL to the appropriate endpoint for each Operation.
3. Create a BPL that can call each REST Operation. If any Operations depend on the response of another, make sure you set those as synchronous so they run in the correct order. Store any relevant server responses to the BPL context and use a code block to perform any custom handling.
4. Create the Service that will trigger your BPL and set it's Target Config Names to include that BPL.

**_Note_**: If your DLS.REST.Request requires user credentials that you would rather not expose as plaintext, extend the DLS.REST.OperationV2 class and override the PopulatePayload() or PopulateFormVariables() method. This will allow you to hook into the ..Adapter.%CredentialsObj.Username and ..Adapter.%CredentialsObj.Password for the credentials set in your Operation's settings tab.
