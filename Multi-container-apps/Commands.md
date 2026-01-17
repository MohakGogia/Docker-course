# 1. DB: Run mongodb docker container
`docker run --name mongodb --rm -d -p 27017:27017 mongo`

### Read logs of container
`docker logs --CONTAINER_NAME--`


# 2. Backend: Build backend image from backend dir
`docker build -t goals-node .`

### Run backend container
`docker run --name goals-backend --rm -d -p 80:80 goals-node`



# 3. Frontend: 
### NOTE: FOR dockerizing react application, the container should run in interactive mode to make it continuously listen to it, and don't let it stop

### Build frontend image from frontend dir
`docker build -t goals-react .`

### Run frontend container
`docker run --name goals-frontend --rm -d -p 3000:3000 -it goals-react`




# 4. Shared network, volumes, live updates of code and authorized access to DB

### Create network
`docker network create goals-net`


### Common network, data persistance, live code updates, secure access to DB, env variable in existence for all 3 apps

Named volume to map it to a path in the container (default is /data/db where mongo stores the data)
env variables are given to set authorized access to the db
NOTE: For auth issues to db, delete old volume and recreate the container: docker volume rm data

`docker run --name mongodb -v data:/data/db --rm -d --network goals-net -e MONGO_INITDB_ROOT_USERNAME=moq -e MONGO_INITDB_ROOT_PASSWORD=secret mongo`


After adjusting the code for adding the container name of mongodb
It still publishes port 80 for frontend app to have it listen for incoming request as frontend code runs on browser so we can't give docker container names url there, 
it will still reach out to localhost post 80 for the request, and still in the same network to communicate directly with the mongo container
Logs at the end so docker container logs survive and dont get overriden hence shorter path towards the end to avoid overriding

`docker run --name goals-backend -v /Users/mohak.gogia/Projects/Personal/Docker-course/Multi-container-apps/backend:/app -v logs:/app/logs -v /app/node_modules -e MONGODB_PASSWORD=secret --rm -d -p 80:80 --network goals-net goals-node`


Bind mount for src folder to have live code updates in the running containerized app
`docker run --name goals-frontend -v /Users/mohak.gogia/Projects/Personal/Docker-course/Multi-container-apps/frontend/src:/app/src --rm -d -p 3000:3000 -it goals-react`