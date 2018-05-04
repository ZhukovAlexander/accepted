API
===


- [Job API](#section-api-jobs)
  - [`POST /jobs`](#api-post-jobs)
  - [`GET /jobs/<uuid>`](#api-get-job)
  - [`POST /want_job`](#api-jobs-dequeue)
  - [`PUT /jobs/<uuid>`](#api-jobs-update)
  - [`DELETE /jobs/<uuid>`]($api-jobs-delete)


## <a name="api-jobs">Job API</a>

### <a name="api-post-jobs">`POST /jobs`</a>

Enqueue a job.

```http
POST /jobs HTTP/1.1
{
  "queue": "myQueue",
  "params": {
    "param1": "foo" 
  }
}
```

```http
HTTP/1.1 202 Accepted
Location: /jobs/<uuid>
```

|Request headers                   |Meaning                              |Note          |
|:---------------------------------|:------------------------------------|:-------------|
|`X-Accepted-Priority`             |Prirority of the job from 0 to 3.     |     |
|`X-Accepted-Result-TTL`           | Number of seconds to retain the job result for||
|`X-Accepted-Callback-URL`         | Make a POST request to the url with the job result as a payload||
|`X-Accepted-Job-Timeout`| Number of seconds after which job is considered failed||
|`X-Accepted-Delay`| Number of seconds to delay the job execution for||

|Response headers|Meaning|Note
|:---------------------------------|:-------------------------------------|:------------|
|`Location`                        |Job handle                            |             |

### <a name="api-post-jobs">`GET /jobs/<uuid>`</a>

Get job info.

```http
GET /jobs/<uuid> HTTP/1.1
```

```http
HTTP/1.1 200 OK
{ 
  "id": "bc2d984f-6179-4e0b-8116-1627679e5acd",
  "status": "running",
  "priority": 1,
  "queue": "myQueue",
  "params": {
    "param1": "foo" 
  }
}
```

|Request headers                   |Meaning                              |Note          |
|:---------------------------------|:------------------------------------|:-------------|
|`X-Accepted-Wait-Result`          |Number of seconds to wait for the job result    |     |

### <a name="api-jobs-deque">`POST /want_job`</a>

Grab a job for processing.

```http
POST /want_job/ HTTP/1.1
```

```http
HTTP/1.1 200 OK
{
  "id": "bc2d984f-6179-4e0b-8116-1627679e5acd",
  "queue": "myQueue",
  "priority": 1,
  "params": {
    "param1": "foo" 
  }
}
```

If there are no jobs, server returns a 204 response
```http
HTTP/1.1 204 No Content
```
|Request headers                   |Meaning                              |Note          |
|:---------------------------------|:------------------------------------|:-------------|
|`X-Accepted-Block-For`            |Number of seconds to wait for the job to arrive    |     |
|`X-Accepted-Timeout`              |Number of seconds to re-enqueue after if job is not complete ||

###  <a name="api-jobs-update">`PUT /jobs/<uuid>`</a>

Update the job
```http
PUT /jobs/<uuid>
{
  "status": "done",
  "result": "bar"
}
```

```http
HTTP/1.1 200 OK
{
  "id": "bc2d984f-6179-4e0b-8116-1627679e5acd",
  "queue": "myQueue",
  "status": "done",
  "priority": 1,
  "params": {
    "param1": "foo"
  },
  "result": "bar",
}
```
###  <a name="api-jobs-delete">`DELETE /jobs/<uuid>`</a>

Delete or cancel the job
```http
DELETE /jobs/<uuid>
```

Returns the deleted job in the response
```http
HTTP/1.1 200 OK
{
  "id": "bc2d984f-6179-4e0b-8116-1627679e5acd",
  "queue": "myQueue",
  "status": "done",
  "priority": 1,
  "params": {
    "param1": "foo"
  },
  "result": "bar",
}
```
