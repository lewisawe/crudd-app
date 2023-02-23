# Week 1 — App Containerization

## steps to containerization
CD into backend flask
### 1. Create a DockerFile in the Backend folder

Add into the Dockefile the following code:

```
FROM python:3.10-slim-buster

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

ENV FLASK_ENV=development

EXPOSE ${PORT}
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]

```

run pip install in the backend folder

```
pip3 install -r requirements.txt
```
Run this command and unlock the port
```
python3 -m flask run --host=0.0.0.0 --port=4567
```
stop the running instance with ctr*C
The run the command
```
export FRONTEND_URL="*"
export BACKEND_URL="*"
python3 -m flask run --host=0.0.0.0 --port=4567

```

click the link and append at the end of the url the following:
```
/api/activities/home
```
You should get back a json

unset the backend and frontend

```
unset FRONTEND_URL
unset BACKEND_URL

```

Try to env grep to confirm if they are unset

```
env | grep url
```
cd .. back to root directory
Build the image:
```
docker build -t  backend-flask ./backend-flask
```
Check Docker images:
```
docker images
```
Run the docker image:

```
docker container run --rm -p 4567:4567 -d backend-flask
```
You expect to get a 404 error as env variables are not set
ctr*C to stop then run this:

```
FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
```
ctr*C to stop the container or stop the container at the docker extension, you right click the running container and stop it



