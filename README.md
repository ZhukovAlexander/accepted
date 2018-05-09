# accepted
Simple job queue with an easy to use HTTP interface. At the end of a day,
it's all about queues, so let's make more of them.

## API
### Enqueue jobs

Simple job enqueue:
```bash
curl -X POST http://localhost:8080/jobs/ -d '{"queue": "myqueue", "data": {"param": "foo"}}'
```
returns `HTTP 202 Accepted` response

Request headers:
```bash
X-ACCEPTED-RESULT-TTL # how long to keep the job result in seconds, defaults to forever
X-ACCEPTED-CALLBACK-URL # if present, server will make a POST request to the URL 
                        # with the job result as a payload
X-ACCEPTED-PRIORITY # job priority level from 0 to 3 with lower levels processed first
```

Response headers:
```bash
Location # a job url in form of http://hostname:port/jobs/{job_id}
```
Enqueu job with a callback url:
```bash
curl -X POST -H "X-ACCEPTED-CALLBACK-URL=https://example.com/" \
    http://localhost:8080/jobs/ -d '{"queue": "myqueue", "data": {"param": "foo"}}'
```




### Dequeue job for processing

```bash
curl -X POST http://localhost:8080/want_job/?block=10&timeout=30
```
