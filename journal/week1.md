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

run this next
```
docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
```
or this
```
docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
```
ctr * c
cd into front end directory
install npm
```
npm i
```
create a new docker file in the front end directory and insert the following

```
FROM node:16.18

ENV PORT=3000

COPY . /frontend-react-js
WORKDIR /frontend-react-js
RUN npm install
EXPOSE ${PORT}
CMD ["npm", "start"]

```
go back a directory cd ..

create a docker-compose.yml file

paste the contents into it

```
version: "3.8"
services:
  backend-flask:
    environment:
      FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
      BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./backend-flask
    ports:
      - "4567:4567"
    volumes:
      - ./backend-flask:/backend-flask
  frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js

# the name flag is a hack to change the default prepend folder
# name when outputting the image names
networks: 
  internal-network:
    driver: bridge
    name: cruddur
```
run this or rightclick the file and docker compose up

```
docker-compose up
```
click url and confirm the port and site is working

