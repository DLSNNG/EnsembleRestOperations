# Instructions

## Running the Example BPL

1. Clone Repo and place REST Example Production.xml file on your Cache Server
2. Follow instructions from [this post](https://community.intersystems.com/post/how-export-and-import-ensemble-components-and-productions) to import the classes
3. Enable RestExampleOperation, RestExampleBPL, and RestExampleService

Every 30 seconds (the poll interval we set in our Service's configuration):

* The Service should loop from 1-10 and pass a new Ens.StringRequest to our BPL, with the StringValue set to the current iteration (e.g. 1 for the first, 2 for the second, etc.)
* The BPL should call the Operation, passing in a DLS.REST.Request message
* The Operation should read the FormVariables property of this message and perform a GET request using the FormVariables as query params.
* The BPL should store the server's response to the Context, convert it to an %Object, then trace all the entries that were returned.

## Usage
1. Add an instance of DLS.REST.OperationV2 to your production for each rest request you would like to make.
2. Set the Server and URL to the appropriate endpoint
3. Create a BPL that can call each REST Operation. If any operations depend on the response of another, make sure you set those as synchronous so the run in the correct order.
4. Create the Service that will trigger your BPL.
