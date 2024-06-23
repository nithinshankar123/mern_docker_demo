Commands to use to containerize this file .
docker ps(To check if there are any containers in my docker)
docker images (To check if there are any images in my docker)
mkdir docker(creating a docker directory)
cd docker(going into the docker)
git clone https://github.com/nithinshankar123/mern_docker_demo.git(To clone the repository from git to docker directory)
cd mern_docker_demo(going to the project files)
ls -ltr(seeing the full information of the files and list)
docker network create library-mern-api(creates a new Docker network named library-mern-api)
docker volume create mongodb-data(reates a Docker volume named mongodb-data for persistent storage)
docker run -dit -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password -e PWD=/ -v mongodb-data:/data/db --name mern_library_nginx_mongodb_1 --net library-mern-api mongo
   Runs a Docker container in detached mode (-d) with a pseudo-TTY (-it).
   Publishes container's port 27017 to the host (-p 27017:27017).
   Sets environment variables for MongoDB root user credentials (-e MONGO_INITDB_ROOT_USERNAME=admin and -e MONGO_INITDB_ROOT_PASSWORD=password).
   Sets the working directory inside the container (-e PWD=/).
   Mounts a Docker volume named mongodb-data to persist MongoDB data (-v mongodb-data:/data/db).
   Names the container as mern_library_nginx_mongodb_1 (--name mern_library_nginx_mongodb_1).
   Connects the container to an existing Docker network named library-mern-api (--net library-mern-api).
   Uses the MongoDB image (mongo) to create and run the container.(it pulls the image from docker hub if it was not there

docker run -dit -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net library-mern-api --name mern_library_nginx_mongo-express_1 -e ME_CONFIG_MONGODB_SERVER=mern_library_nginx_mongodb_1 -e ME_CONFIG_BASICAUTH_USERNAME=admin -e ME_CONFIG_BASICAUTH_PASSWORD=admin123456 mongo-express
     docker run -dit: Starts a Docker container in detached mode with interactive terminal.
     -p 8081:8081: Publishes container's port 8081 to the host.
     -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin: Sets the MongoDB admin username environment variable for mongo-express.
     -e ME_CONFIG_MONGODB_ADMINPASSWORD=password: Sets the MongoDB admin password environment variable for mongo-express.
     --net library-mern-api: Connects the container to the Docker network library-mern-api.
     --name mern_library_nginx_mongo-express_1: Names the container as mern_library_nginx_mongo-express_1.
    -e ME_CONFIG_MONGODB_SERVER=mern_library_nginx_mongodb_1: Specifies the MongoDB server hostname (linked container) for mongo-express.
    -e ME_CONFIG_BASICAUTH_USERNAME=admin: Sets the basic authentication username for mongo-express.
    -e ME_CONFIG_BASICAUTH_PASSWORD=admin123456: Sets the basic authentication password for mongo-express.
      mongo-express: The name of the Docker image used to create the container.
  ifconfig (to check the ipaddress to connect to my port number to use mongoexpress document in chrome)

docker images(showes images we are using)
REPOSITORY      TAG       IMAGE ID       CREATED        SIZE
mongo           latest    95b3ba6bed35   4 weeks ago    797MB
mongo-express   latest    870141b735e7   3 months ago   182MB

cd server/(goes into server repository in docker)
docker build -t mern_library_nginx_library-api .(used to build a Docker image with the tag -t mern_library_nginx_library-api from the Dockerfile located in the current directory )

cd ..(go to docker location)
cd client/
docker build -t mern_library_nginx_client .
 docker images
REPOSITORY                       TAG       IMAGE ID       CREATED          SIZE
mern_library_nginx_client        latest    55eb9c26daf7   32 seconds ago   710MB
mern_library_nginx_library-api   latest    8a8ea39d2cb2   5 minutes ago    187MB
mongo                            latest    95b3ba6bed35   4 weeks ago      797MB
mongo-express                    latest    870141b735e7   3 months ago     182MB
cd ..
cd nginx

docker build -t mern_library_nginx_nginx .
cd ..
cd server

docker run -dit -p 5000:5000 -e MONGO_URI=mongodb://admin:password@mern_library_nginx_mongodb_1 -e NODE_ENV=development -e PWD=/app -v "$(pwd)":/app -v "$(pwd)"/node_modules:/app/node_modules --net library-mern-api --name library_mern_nginx mern_library_nginx_library-api

docker run: Starts a Docker container.
-dit: Options used to run the container in detached mode (-d), allocate a pseudo-TTY (-t), and keep it running in the background (-i).
-p 5000:5000: Publishes the container's port 5000 to the host's port 5000, allowing external access to the application running inside the container.
-e MONGO_URI=mongodb://admin
@mern_library_nginx_mongodb_1: Sets the environment variable MONGO_URI inside the container to connect to MongoDB. Here, admin is the MongoDB username and password is the password, connecting to the MongoDB container named mern_library_nginx_mongodb_1.
-e NODE_ENV=development: Sets the NODE_ENV environment variable to development, which is a common practice to specify the environment the application is running in.
-e PWD=/app: Sets the working directory (PWD) inside the container to /app.
-v "$(pwd)":/app: Mounts the current host directory into the container at /app. This allows the container to access and potentially modify files in the current directory on the host machine.
-v "$(pwd)"/node_modules:/app/node_modules: Mounts the node_modules directory from the host into the container's /app/node_modules. This is done to optimize performance by avoiding unnecessary package installations within the container.
--net library-mern-api: Connects the container to the Docker network library-mern-api, enabling network communication between containers on the same network.
--name library_mern_nginx: Names the container as library_mern_nginx.
mern_library_nginx_library-api: Specifies the Docker image (mern_library_nginx_library-api) to use for creating and running the container.

cd ..
cd client
docker run -d -v "$(pwd)":/app -v "$(pwd)"/node_modules:/app/node_modules --net library-mern-api  --name library_mern_frontend mern_library_nginx_client

docker run: Initiates the creation and running of a Docker container.
-d: Runs the container in detached mode, meaning it runs in the background and prints the container ID.
-v "$(pwd)":/app: Mounts the current directory (where this command is executed) into the container at /app. This is typically where the frontend application code resides.
-v "$(pwd)"/node_modules:/app/node_modules: Mounts the node_modules directory from the host into the container's /app/node_modules. This is done to optimize performance by avoiding unnecessary package installations within the container.
--net library-mern-api: Connects the container to the Docker network named library-mern-api, allowing communication with other containers on the same network.
--name library_mern_frontend: Names the container as library_mern_frontend.
mern_library_nginx_client: Specifies the Docker image (mern_library_nginx_client) to use for creating and running the container. This image likely contains the frontend application code and necessary dependencies (like Node.js and React).

 docker run -d -p 8080:80 --name mern_library_nginx_nginx_1 --net library-mern-api mern_library_nginx_nginx

 docker run: Starts a new Docker container.
-d: Runs the container in detached mode, meaning it runs in the background.
-p 8080:80: Publishes port 80 from the container to port 8080 on the host. This means you can access the web server running inside the container via http://localhost:8080.
--name mern_library_nginx_nginx_1: Names the container as mern_library_nginx_nginx_1.
--net library-mern-api: Connects the container to the Docker network named library-mern-api. This allows the nginx container to communicate with other containers on the same network, such as backend services or a database.
mern_library_nginx_nginx: Specifies the Docker image to use for creating and running the container. In this case, it seems to be an nginx-based image (mern_library_nginx_nginx).


