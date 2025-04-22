# Devops-Lab6

# Create Docker image of fastapi api server using github actions

## podman install steps

install `podman-cli` from [https://podman.io/](https://podman.io/)

then execute
```bash
podman machine init
podman machine start

alias docker=podman

docker --version
```# fastapi-dockerize

### python server
`main.py`
```python
from fastapi import FastAPI
app = FastAPI()
import uvicorn

@app.get("/")
def read_root():
    return dict(name = "Prateek", Location = "Deheradun")

@app.get("/{data}")
def read_root(data):
    return dict(hi = data, Location = "Deheradun")

if __name__ == "__main__":
    uvicorn.run("main:app", host="0.0.0.0", port=80, reload=True)
```

### python requirements 
`requirements.txt`
```txt
fastapi
uvicorn
```

### Dockerfile for image building/containerization of app
`Dockerfile`
```Dockerfile
FROM ubuntu

RUN apt update -y
RUN apt install python3 python3-pip pipenv -y

WORKDIR /app
COPY . /app/
RUN pipenv install -r requirements.txt

EXPOSE 80


# CMD pipenv run uvicorn main:app --host 0.0.0.0 --port 80
CMD pipenv run python3 ./main.py
```
### build image using docker

`docker build -t imagename:v1 .`

check docker image
`docker images`

test docker image and run docker container
`docker run imagename`
