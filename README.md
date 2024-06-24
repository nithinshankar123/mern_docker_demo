Here's the text formatted into Markdown for a README file, with commands highlighted and explanations styled for clarity:

```markdown
# Containerizing the MERN Stack Application

## Prerequisites
Ensure you have Docker installed on your machine.

## Steps

1. **Check for existing Docker containers and images:**
   ```sh
   docker ps
   docker images
   ```

2. **Set up the Docker directory:**
   ```sh
   mkdir docker
   cd docker
   ```

3. **Clone the repository:**
   ```sh
   git clone https://github.com/nithinshankar123/mern_docker_demo.git
   cd mern_docker_demo
   ls -ltr
   ```

4. **Create a Docker network and volume:**
   ```sh
   docker network create library-mern-api
   docker volume create mongodb-data
   ```

5. **Run MongoDB container:**
   ```sh
   docker run -dit -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password -e PWD=/ -v mongodb-data:/data/db --name mern_library_nginx_mongodb_1 --net library-mern-api mongo
   ```
   - **Options:**
     - `-dit`: Run container in detached mode with a pseudo-TTY.
     - `-p 27017:27017`: Publish container's port 27017 to the host.
     - `-e MONGO_INITDB_ROOT_USERNAME=admin`: Set MongoDB root username.
     - `-e MONGO_INITDB_ROOT_PASSWORD=password`: Set MongoDB root password.
     - `-e PWD=/`: Set the working directory inside the container.
     - `-v mongodb-data:/data/db`: Mount Docker volume for persistent storage.
     - `--name mern_library_nginx_mongodb_1`: Name the container.
     - `--net library-mern-api`: Connect to Docker network.

6. **Run Mongo-Express container:**
   ```sh
   docker run -dit -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net library-mern-api --name mern_library_nginx_mongo-express_1 -e ME_CONFIG_MONGODB_SERVER=
