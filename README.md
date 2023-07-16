# Deploy reactpy with fastapi backend to GCP

It uses gunicorn as server with uvicorn as workers.

See app.yaml

```
entrypoint: gunicorn -b :$PORT -w 2 -k uvicorn.workers.UvicornWorker main:app
```



