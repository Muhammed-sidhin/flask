# Flask App with MySQL and Traefik

This project contains a Flask application with a MySQL database and Traefik for load balancing, containerized using Docker Compose and deployed in Docker Swarm.

## Project Structure

- `app/`: Contains the Flask application code and templates.
- `db/`: Contains the initial SQL script for database setup.
- `traefik/`: Contains the Docker Compose file for Traefik.
- `docker-compose.yml`: Docker Compose file for the entire project.

## Prerequisites

- Docker: sudo apt install docker.io -y
- Docker Compose: sudo apt install docker-compose -y

## Usage

1. **Build the Flask App with MySQL:**

    ```bash
    cd app
    touch app.py dashboard.html Dockerfile index.html requirements.txt
    docker build . -t flask-application
    ```
2. **Push the changes to DockerHub**

   ```bash
    docker login
    docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
    docker push TARGET_IMAGE[:TAG]
   ```
3. **Initialize the MySQL Database:**

    ```bash
    cd db
    vim init.sql
    ```

4. **Build the Traefik :**

    ```bash
    cd traefik
    vim docker-compose
    ```
5. **Initialize docker swarm :**

    ```bash
    ifconfig
    copy privateIP
    docker swarm init --advertise-add privateIP
    docker node ls
    ```
6. **Create docker Networks :**

    ```bash
    docker network create --driver=overlay frontend
    docker network create --driver=overlay backend
    docker network create --driver=overlay traefik-public
    ```
7. **Deploy the flask-app with mysql:**

    ```bash
    docker stack deploy -c docker-compose.yml flask-app
    docker stack ls
    docker stack ps flask-app
    docker stack services flask-app
    ```
8. **Deploy the traefik service:**

    ```bash
    cd traefik
    docker stack deploy -c docker-compose.yml traefik-stack
    docker stack ls
    docker stack ps traefik-stack
    docker stack services traefik-stack
    ```
9. **Access the Flask App:**

   The Flask app is accessible at (http://localhost:5000).

10. **Access the Traefik Dashboard:**

   The Traefik dashboard is accessible at (http://localhost:8080).

## Cleanup

To stop and remove the services, run:

```bash
docker stack rm stackName
docker stack rm traefik-stack
docker stack rm flask-app



