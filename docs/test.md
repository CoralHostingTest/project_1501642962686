# The Apex API


**APEX API** provides endpoints to onboard, update, delete a project and
retrieve project(s) information. It also provides other operations to sync
with github or receive github events. <br/> <br/> ***Terminologies*** <br/> *
**Project**: A project is an API that is registered with APEX through the
onboarding process. <br/>

This API has the following attributes:

| Attribute | Value                 |
|-----------|-----------------------|
| namespace | com.yahoo.apex.parsec |
| version   | 1                     |


## Resources

### [ProjectResponse](#ProjectResponse)

#### POST /projects/

Create/Onboard a project. <br/> Please refer to  [Sample
Request/Responses](#TBD) sections for examples.


#### Request parameters:

| Name            | Type                              | Source | Options | Description                 |
|-----------------|-----------------------------------|--------|---------|-----------------------------|
| creationRequest | [ProjectRequest](#projectrequest) | body   |         | Required request parameter. |


#### Responses:

Expected:

| Code         | Type                                |
|--------------|-------------------------------------|
| 202 Accepted | [ProjectResponse](#projectresponse) |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 403 Forbidden             | ResourceError |         |
| 409 Conflict              | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


#### PUT /projects/{id}

Edit/Modify a project. <br/> Please refer to  [Sample Request/Responses](#TBD)
sections for examples.


#### Request parameters:

| Name    | Type                              | Source | Options | Description |
|---------|-----------------------------------|--------|---------|-------------|
| id      | UUID                              | path   |         |             |
| project | [ProjectRequest](#projectrequest) | body   |         |             |


#### Responses:

Expected:

| Code   | Type            |
|--------|-----------------|
| 200 OK | ProjectResponse |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 403 Forbidden             | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 409 Conflict              | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


#### GET /projects/{id}

Get the detail of the project with the given project id. <br/> Please refer to
[Sample Request/Responses](#TBD) sections for examples.


#### Request parameters:

| Name | Type | Source | Options | Description |
|------|------|--------|---------|-------------|
| id   | UUID | path   |         |             |


#### Responses:

Expected:

| Code   | Type            |
|--------|-----------------|
| 200 OK | ProjectResponse |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


### [ProjectResponseList](#ProjectResponseList)

#### GET /projects

Get all projects. <br/> Please refer to [Sample Request/Responses](#TBD)
sections for examples.


#### Responses:

Expected:

| Code   | Type                |
|--------|---------------------|
| 200 OK | ProjectResponseList |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


### [NullResponse](#NullResponse)

#### DELETE /projects/{id}

Delete the project with the given project id. <br/> Please refer to [Sample
Request/Responses](#TBD) sections for examples.


#### Request parameters:

| Name | Type | Source | Options | Description |
|------|------|--------|---------|-------------|
| id   | UUID | path   |         |             |


#### Responses:

Expected:

| Code           | Type |
|----------------|------|
| 204 No Content |      |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


#### POST /projects/{id}/events

For github Webhook callback <br/>


#### Request parameters:

| Name  | Type                                  | Source | Options | Description |
|-------|---------------------------------------|--------|---------|-------------|
| id    | UUID                                  | path   |         |             |
| event | [PushEventRequest](#pusheventrequest) | body   |         |             |


#### Responses:

Expected:

| Code           | Type |
|----------------|------|
| 204 No Content |      |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


#### POST /projects/{id}/sync

Force Apex to sync with the project data on git. <br/> Please refer to [Sample
Request/Responses](#TBD) sections for examples.


#### Request parameters:

| Name | Type | Source | Options | Description |
|------|------|--------|---------|-------------|
| id   | UUID | path   |         |             |


#### Responses:

Expected:

| Code           | Type |
|----------------|------|
| 204 No Content |      |


Exception:

| Code                      | Type          | Comment |
|---------------------------|---------------|---------|
| 400 Bad Request           | ResourceError |         |
| 401 Unauthorized          | ResourceError |         |
| 404 Not Found             | ResourceError |         |
| 409 Conflict              | ResourceError |         |
| 500 Internal Server Error | ResourceError |         |


## Types

### <a name="DateTime"></a> DateTime

DateTime string that follows ISO 8601 UTC format, e.g.2013-03-06T11:00:00Z

`DateTime` is an alias of type `String`

### <a name="ProjectRequest"></a> ProjectRequest

The request parameter used in project onboarding/creation and update.
<br/><br/> - Each and every field are required for creation (not null), and
can not be blank. <br/> - The fields are allowed to be null on update but not
blank. If the field value is null for update, the field will be kept
unchanged. <br/> - Please refer to  [Sample Request/Responses](#TBD) sections
for examples. <br/> - **Implementation note**: Regex is used to verify the
input is not blank and escape '\\' twice for rdl & java

`ProjectRequest` is a `Struct` type with the following fields:

| Name        | Type   | Options | Description                                                                                                                                                                  | Notes |
|-------------|--------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------|
| repoUrl     | String |         | The ssh url to the repository where the RDL source and documents resides. <br/> Should be in the format of `git@git.corp.yahoo.com:${orgname}/${reponame}.git#${branchName}` |       |
| name        | String |         | The name of the api.                                                                                                                                                         |       |
| custodian   | String |         | The custodian/contact of the api, should be a `@yahoo-inc` address. An ilist is preferred.                                                                                   |       |
| description | String |         | Long description of the api.                                                                                                                                                 |       |


### <a name="ProjectResponse"></a> ProjectResponse

The response object used in GET operation

`ProjectResponse` is a `Struct` type with the following fields:

| Name               | Type                  | Options | Description                                                                                                              | Notes |
|--------------------|-----------------------|---------|--------------------------------------------------------------------------------------------------------------------------|-------|
| id                 | UUID                  |         | System generated unique id.                                                                                              |       |
| name               | String                |         | The name of the project. As specified on project creation.                                                               |       |
| custodian          | String                |         | The custodian of the project. As specified on project creation.                                                          |       |
| description        | String                |         | The description of the project. As specified on project creation.                                                        |       |
| repoUrl            | String                |         | The repository of the project. As specified on project creation.                                                         |       |
| repoOrg            | String                |         | The organization of the project repository, specified in `repoUrl` on project creation.                                  |       |
| repoName           | String                |         | The name of the project repository, specified in `repoUrl` on project creation.                                          |       |
| repoBranch         | String                |         | The branch of the project repository, specified in `repoUrl` on project creation.                                        |       |
| repoHeadSha        | String                |         | The sha of the project repository.                                                                                       |       |
| docUrl             | String                |         | The url to the document-hosting page, ex: https://git.corp.yahoo.com/pages/ApexHosting/project_cat/                      |       |
| reviewUrl          | String                |         | The url to the api review jive page, ex: https://yahoo.jiveon.com/thread/17141, as specified in the project config file. |       |
| splunkDashboardUrl | String                |         | The url to the api splunk dashboard, as specified in the project config file.                                            |       |
| testHostUrl        | String                |         | The test host url, as specified in the project config file.                                                              |       |
| webHookId          | Int64                 |         |                                                                                                                          |       |
| createdTime        | [DateTime](#datetime) |         | The date time when the project is created.                                                                               |       |
| updateTime         | [DateTime](#datetime) |         | The date time when the project is updated.                                                                               |       |


### <a name="ProjectResponseList"></a> ProjectResponseList

A collection of [ProjectResponse](#projectresponse)

`ProjectResponseList` is a `Struct` type with the following fields:

| Name                | Type                                             | Options | Description | Notes |
|---------------------|--------------------------------------------------|---------|-------------|-------|
| projectResponseList | Array&lt;[ProjectResponse](#projectresponse)&gt; |         |             |       |


### <a name="NullResponse"></a> NullResponse

Represents response when there is no content.

`NullResponse` is a `Struct` with no specified fields


### <a name="PushEventRequest"></a> PushEventRequest

Represents the github webhook payload. Currently we only need these 2 fields.
<br/> ref: https://developer.github.com/v3/activity/events/types/#pushevent

`PushEventRequest` is a `Struct` type with the following fields:

| Name  | Type   | Options | Description    | Notes |
|-------|--------|---------|----------------|-------|
| ref   | String |         |                |       |
| after | String |         | the latest SHA |       |


[Swagger URL](https://git.corp.yahoo.com/pages/ApexTest/Swagger-UI/parsec/swagger-ui/?url=https://git.corp.yahoo.com/pages/ApexTest/project_1501642962686/swagger-json/test_swagger.json)
