

# Containerizing the MERN Stack Application

## Prerequisites
Ensure you have Docker installed on your machine.

## Steps

1. **Check for existing Docker containers:**
   ```sh
   docker ps
   ```
   *Use this command to list all running Docker containers. It helps ensure that there are no conflicting containers running on your machine.*

2. **Check for existing Docker images:**
   ```sh
   docker images
   ```
   *This command lists all Docker images available on your machine.*

3. **Create a directory for Docker:**
   ```sh
   mkdir docker
   ```
   *This command creates a new directory named `docker`.*

4. **Navigate into the Docker directory:**
   ```sh
   cd docker
   ```
   *Change the current directory to the newly created `docker` directory.*

5. **Clone the repository from GitHub:**
   ```sh
   git clone https://github.com/nithinshankar123/mern_docker_demo.git
   ```
   *Clone the repository containing the MERN stack application into the `docker` directory.*

6. **Navigate to the project files:**
   ```sh
   cd mern_docker_demo
   ```
   *Change the directory to `mern_docker_demo` to access the project files.*

7. **List files with detailed information:**
   ```sh
   ls -ltr
   ```
   *This command lists all files in the current directory with detailed information including permissions, number of links, owner, group, size, and modification date.*

8. **Create a Docker network:**
   ```sh
   docker network create library-mern-api
   ```
   *Create a new Docker network named `library-mern-api` to facilitate communication between containers.*

9. **Create a Docker volume:**
   ```sh
   docker volume create mongodb-data
   ```
   *Create a Docker volume named `mongodb-data` for persistent storage of MongoDB data.*

10. **Run MongoDB container:**
    ```sh
    docker run -dit -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password -e PWD=/ -v mongodb-data:/data/db --name mern_library_nginx_mongodb_1 --net library-mern-api mongo
    ```
    *Starts a MongoDB container in detached mode with port 27017 exposed. It sets up environment variables for MongoDB root user credentials, mounts a Docker volume for data persistence, names the container `mern_library_nginx_mongodb_1`, and connects it to the `library-mern-api` network.*

11. **Run Mongo Express container:**
    ```sh
    docker run -dit -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net library-mern-api --name mern_library_nginx_mongo-express_1 -e ME_CONFIG_MONGODB_SERVER=mern_library_nginx_mongodb_1 -e ME_CONFIG_BASICAUTH_USERNAME=admin -e ME_CONFIG_BASICAUTH_PASSWORD=admin123456 mongo-express
    ```
    *Starts a Mongo Express container in detached mode with port 8081 exposed. It sets up environment variables for MongoDB admin credentials and server, and basic authentication credentials, names the container `mern_library_nginx_mongo-express_1`, and connects it to the `library-mern-api` network.*

12. **Check IP address (optional):**
    ```sh
    ifconfig
    ```
    *Use this command to check the IP address to connect to the Mongo Express application in your browser.*

13. **Check Docker images:**
    ```sh
    docker images
    ```
    *This command lists all Docker images available on your machine, useful to ensure the required images are present.*

14. **Build the server Docker image:**
    ```sh
    cd server/
    docker build -t mern_library_nginx_library-api .
    ```
    *Navigate to the `server` directory and build a Docker image for the server with the tag `mern_library_nginx_library-api`.*

15. **Build the client Docker image:**
    ```sh
    cd ..
    cd client/
    docker build -t mern_library_nginx_client .
    ```
    *Navigate to the `client` directory and build a Docker image for the client with the tag `mern_library_nginx_client`.*

16. **Check Docker images again:**
    ```sh
    docker images
    ```
    *Verify that the new images for the server and client have been created.*

17. **Build the NGINX Docker image:**
    ```sh
    cd ..
    cd nginx
    docker build -t mern_library_nginx_nginx .
    ```
    *Navigate to the `nginx` directory and build a Docker image for NGINX with the tag `mern_library_nginx_nginx`.*

18. **Run the server container:**
    ```sh
    cd ..
    cd server
    docker run -dit -p 5000:5000 -e MONGO_URI=mongodb://admin:password@mern_library_nginx_mongodb_1 -e NODE_ENV=development -e PWD=/app -v "$(pwd)":/app -v "$(pwd)"/node_modules:/app/node_modules --net library-mern-api --name library_mern_nginx mern_library_nginx_library-api
    ```
    *Starts the server container in detached mode with port 5000 exposed. It sets environment variables for MongoDB URI and Node environment, mounts the current directory and node_modules, names the container `library_mern_nginx`, and connects it to the `library-mern-api` network.*

19. **Run the client container:**
    ```sh
    cd ..
    cd client
    docker run -d -v "$(pwd)":/app -v "$(pwd)"/node_modules:/app/node_modules --net library-mern-api --name library_mern_frontend mern_library_nginx_client
    ```
    *Starts the client container in detached mode. It mounts the current directory and node_modules, names the container `library_mern_frontend`, and connects it to the `library-mern-api` network.*

20. **Run the NGINX container:**
    ```sh
    docker run -d -p 8080:80 --name mern_library_nginx_nginx_1 --net library-mern-api mern_library_nginx_nginx
    ```
    *Starts the NGINX container in detached mode with port 80 exposed to port 8080 on the host. It names the container `mern_library_nginx_nginx_1` and connects it to the `library-mern-api` network.*
```
