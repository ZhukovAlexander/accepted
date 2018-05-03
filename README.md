# accepted
Simple job queue with an easy to use HTTP interface. 

## API
### Enqueue jobs
Simple job enqueue:
```bash
curl -X POST http://localhost:8080/jobs/ -d '{"queue": "myqueue", "data": {"param": "foo"}}'
```
returns `HTTP 202 Accepted` response

Enqueu job with a callback url:
```bash
curl -X POST -H "X-ACCEPTED-CALLBACK-URL=https://example.com/" \
    http://localhost:8080/jobs/ -d '{"queue": "myqueue", "data": {"param": "foo"}}'
```


```bash
X-ACCEPTED-RESULT-TTL #FDF // FDF
X-ACCEPTED-CALLBACK-URL #
X-ACCEPTED-PRIORITY #
```

### Dequeue job for processing

```bash
curl -X POST http://localhost:8080/want_job/?block=10&timeout=30
```
