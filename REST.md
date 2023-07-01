# Service Rest API

Here's a summary of the REST APIs exposed by the service

## Python SDK APIs - ApiKey

| Endpoint | Method | Description |
| --- | --- | --- |
| ```/v1/api/system/preparePublish``` | PUT | Begin a publish process|
| ```/v1/api/system/completePublish``` | PUT | Complete a publish process |

## User Protected APIs - FeatherToken

*These endpoints require the X-FEATHER-TOKEN header, with the token returned by /login*

| Endpoint | Method | Description |
|---|---|---|
| ```/v1/client/apikey``` | [GET, PUT, DELETE] | Create, Get or Delete an API key for current user |

## Open APIs - Anonymous

*No authentication needed to call these APIs.*

| Endpoint | Method | Description |
| --- | --- | --- |
| ```/v1/public/systems``` | GET | List all systems available |
| ```/v1/public/system``` | GET | Get detailed information about a system by looking it up by username and system name |
| ```/v1/public/{userId}/systems``` | GET | List of all systems for a user |
| ```/v1/public/system/{systemId}``` | GET | Get detailed information about a system |
| ```/v1/public/system/{systemId}/step/{stepIndex}``` | PUT | Execute a step or system |
  
   
# SDK Rest API

Here's a summary of the REST APIs exposed by the python SDK for the local publishing flow:

| Endpoint | Method | Description |
|---|---|---|
| ```/v1/system/info``` | GET | Get information on the currently loaded system, including *contextId* |
| ```/v1/system/{contextId}/step/{idx}/info``` | GET | Get information on the step at index **idx** |
| ```/v1/system/{contextId}/step/{idx}``` | PUT | Execute step at index **idx** |
| ```/v1/system/{contextId}/publish``` | PUT | Publish the system to Feather. API_KEY needs to be provided |

### What is ContextId?

During local execution *only*, the system is assigned a unique **contextId**. This ID is returned during the /system/info call, and needs to be used for subsequent API calls. An API call will return **410 (Gone)** If the wrong context ID is used. The local page should detect this, and redirect to the first/main page.

### Step evaluation order

The SDK needs to evaluate the steps in the correct order, in order to extract the metadata from the python code. Step Info call response contains an **is_valid** field, which must be true for the page to be able to trust the info. So the typical API flow for a system with 2 steps should be:

    GET /v1/system/info                         -> Find out num steps, name, and contextId
    GET /v1/system/{contextId}/step/0/info      -> Get info and schema on step 1. From this the UI can be built for step 1
    PUT /v1/system/{contextId}/step/0           -> Execute Step 1 (index 0)
    GET /v1/system/{contextId}/step/1/info      -> Get info and schema on step 2, and build the UI for step 2
    PUT /v1/system/{contextId}/step/1           -> Finally execute step 2 and get the final system outputs. 
                                                   Display the publish page and go on to publishing


