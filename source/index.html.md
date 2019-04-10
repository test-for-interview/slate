---
title: Canotic Job API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL
  - shell_session: CLI
  - python: Python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Canotic Job API. You can use our API to ....

We offer library code snippets for CLI, cURL and Python. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```shell
-u "CANOTIC_API_KEY:" \
```

```shell_session
import kittn

api = kittn.authorize('meowmeowmeow')
```

```python
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: myAPIkey"
```

> Make sure to replace `myAPIkey` with your API key.

Canotic's Job API uses API keys to authenticate requets. You can view and manage your API keys in our [developer portal](http://canotic.com/developers).

You must include the API key in all API requests to the server in a header that looks like the following:

`Authorization: myAPIkey`

<aside class="notice">
You must replace <code>myAPIkey</code> with your personal API key.
</aside>

# Jobs

## Create a job

```shell
curl "API_ENDPOINT:/apps/APP_ID:" \
  -u "CANOTIC_API_KEY:" \
  -d callback_url="http://www.example.com/callback" \
  -d inputs=[{"data_url":"https://your-s3-bucket.s3.amazonaws.com/self-driving-car.png"}] \
  -d inputs_file="https//bucket.s3.amazonaws.com/data/data.json"
```

```shell_session
canotic create_job --app_id "APP_ID:" --callback_url "http://www.example.com/callback" --inputs [{"data_url":"https://your-s3-bucket.s3.amazonaws.com/self-driving-car.png"}] --inputs_file "https//bucket.s3.amazonaws.com/data/data.json"
```

```python
import canotic as ca
    
    client = ca.client("CANOTIC_API_KEY:")
    
    client.create_job(
        api_id="APP_ID:"
        callback_url='http://www.example.com/callback',
        inputs=[{"data_url":"http://i.imgur.com/XOJbalC.jpg"}],
        inputs_file="https//bucket.s3.amazonaws.com/data/data.json",
    )
```

> Response:

```json
{
      "jobs": [
        {
          "job_id": "5774cc78b01249ab09f089dd",
          "created_at": "2016-06-23T09:09:34.752Z",
          "callback_url": "http://www.example.com/callback",
          "app_id": "60D31EB37595DD44584BE5EF363283E3",
          "status": "pending",
          "input": {
            "data_url": "https://your-s3-bucket.s3.amazonaws.com/self-driving-car.png"
          },
          "metadata": {}
        }
      ],
      "total": 1,
      "limit": 100,
      "offset": 0,
      "has_more": false
    }
```

This endpoint creates a new job.

### HTTP Request

`GET http://canotic.com/api/jobs/`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
app_id | string | The app ID for which you want to create a new job
callback_url | string | Where we will POST the job results. See the [callbacks section](#callbacks) for further details.
inputs_file | object | A JSON file containing all your inputs

## Get a job

```shell
curl "API_ENDPOINT:/jobs/JOB_ID:" \
      -u "CANOTIC_API_KEY:"
```

```shell_session
canotic fetch_job --job_id "JOB_ID:"
```

```python
import canotic as ca
    
    client = ca.client("CANOTIC_API_KEY:")
    job_id = "JOB_ID:"
    
    client.fetch_job(job_id)
```

> Response:

```json
{
      "job_id": "576ba74eec471ff9b01557cc",
      "completed_at": "2016-06-23T09:10:02.798Z",
      "created_at": "2016-06-23T09:09:34.752Z",
      "callback_url": "http://www.example.com/callback",
      "app_id": "60D31EB37595DD44584BE5EF363283E3",
      "status": "completed",
      "callback_succeeded": true,
      "input": {
        "data_url": "https://your-s3-bucket.s3.amazonaws.com/cute_dog.png"
      },
      "output": {
        "category": "dog"
      },
      "metadata": {}
    }
```

This endpoint retrieves a specific job.

### HTTP Request

`GET http://canotic.com/api/jobs/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
job_id | The ID of the job you want to retrieve

## Cancel a job

```shell
curl "API_ENDPOINT:/jobs/JOB_ID:/cancel" \
      -u "CANOTIC_API_KEY:"
```

```shell_session
canotic cancel_job --job_id "JOB_ID:"
```

```python
import canotic as ca
    
    client = ca.client("CANOTIC_API_KEY:")
    job_id = "JOB_ID:"
    
    client.cancel_job(job_id)
```

> Response:

```json
{
      "job_id": "576ba74eec471ff9b01557cc",
      "created_at": "2016-06-23T09:09:34.752Z",
      "callback_url": "http://www.example.com/callback",
      "app_id": "60D31EB37595DD44584BE5EF363283E3",
      "status": "canceled",
      "input": {
        "data_url": "https://your-s3-bucket.s3.amazonaws.com/cute_dog.png"
      },
      "metadata": {}
    }
```

This endpoint cancels a specific job.

### HTTP Request

`DELETE http://canotic.com/api/jobs/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

## Restart a job

```shell
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```shell_session
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```python
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> Response:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific job.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://canotic.com/api/jobs/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
job_id | The ID of the job you want to restart

## Get all jobs

```shell
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```shell_session
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```python
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> Response:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves all jobs.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://canotic.com/api/jobs/<ID>`

### URL Parameter (optional)

Parameter | Description
--------- | -----------
app_id | The ID of the app you want to retrieve all the jobs from

# Callbacks

On your jobs, you will be required to supply a `callback_url`, a fully qualified URL that we will POST with the results of the job when completed. The data will be served as a JSON body (`application/json`). Alternately, you can set a default callback URL in your profile, which will be used for jobs that do not specify one.

Additionally, in order to simplify testing and add support for email automation pipelines, you may provide an **email address** as the `callback_url`. In this case, each completed job will result in an email sent with the body as the job's JSON payload.

## POST data

|  Attribute | Type | Description |
|---|---|---|
| job_id | string | A unique identifier for a job |
| status | string | The status of the job when it was completed. Normally completed, but can also be error in the case that a job failed processing. |
| output | object | An object containing the output of the job (based on the particular app type). For dog v cat, for example, this will just be, e.g., "category"="dog".  |
| job | object | The full job object for reference and convenience. |
