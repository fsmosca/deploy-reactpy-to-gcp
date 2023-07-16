# Deploy reactpy with fastapi backend to GCP

It uses gunicorn as server with uvicorn as workers.

See app.yaml

```
entrypoint: gunicorn -b :$PORT -w 2 -k uvicorn.workers.UvicornWorker main:app
```

You can deploy this in GCP, similar to the one with flask backend [rflask](https://github.com/fsmosca/rflask).

Deploy result.

![image](https://github.com/fsmosca/gcp-reactpy-fastapi/assets/22366935/8e79191b-0196-4570-abbe-da11dc8d8588)

## Files

app.yaml

```python
# Uses gunicorn with uvicorn workers.

runtime: python

# F1 = 2 workers (default), F2 = 4 workers
instance_class: F1

env: flex

# -w 2 sets uvicorn number of workers
# The number of workers you specify should match the
# instance class of your App Engine app.
entrypoint: gunicorn -b :$PORT -w 2 -k uvicorn.workers.UvicornWorker main:app

runtime_config:
    operating_system: "ubuntu22"
    runtime_version: "3.11"
```

main.py

```python
from reactpy import component, html
from reactpy.backend.fastapi import configure
from fastapi import FastAPI


@component
def HelloWorld():
    return html.h1("Hello, world from reactpy on fastapi backend.")


app = FastAPI()
configure(app, HelloWorld)
```

requirements.txt

```
reactpy
fastapi
uvicorn[standard]
gunicorn
```

## References

* https://reactpy.dev/docs/guides/getting-started/running-reactpy.html#running-reactpy-in-production
* https://cloud.google.com/appengine/docs/flexible/python/runtime
