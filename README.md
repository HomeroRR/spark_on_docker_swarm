# spark_on_docker_swarm
Demo of Spark on Docker Swarm with Jupyter notebook and Postgresql

## Requirements
- Docker
- System running Linux/MacOS

## Set-up

1. Clone this project from GitHub:
    ```bash    
    git clone https://github.com/HomeroRR/spark_on_docker_swarm.git
    ```
2. Declare POSTGRES_CONTAINER_PASSWORD in /workspace/.env: 
    ```bash    
    export POSTGRES_CONTAINER_PASSWORD=YOURSECRETKEYGOESHERE
    ```
    and load as an enviroment variable with 
    ```bash 
    source workspace/.env
    ```
3. Create `$HOME/data/postgres` directory for PostgreSQL files: 
    ```bash 
    mkdir -p ~/data/postgres
    ```

4. Make sure Docker Swarm is runnig by : 
    ```bash    
    docker swarm init
    ```
5. Deploy Docker Stack: 
    ```bash    
    docker stack deploy -c stack.yml jupyter
    ```
6. Retrieve the token to log into Jupyter: 
    ```bash
    docker logs $(docker ps | grep jupyter_spark | awk '{print $NF}')
    ```
7. From the Jupyter terminal, run the install script: 
   ```bash
   sh bootstrap_jupyter.sh
   ```
### Optionally to run a new jupyter notebook from inside the container
1. Get the container ID with `docker ps`
2. Attach a shell to the container: 
    ```bash    
    docker exec -it YOUR_ID_GOES_HERE bash
    ```
3. Run jupyter notebook: 
    ```bash    
    jupyter notebook --ip 0.0.0.0 --no-browser --allow-root
    ```

## Tear-down
1. Stop containers: 
    ```bash    
    docker stack rm jupyter
    ```
2. Stop swarm init: 
    ```bash    
    docker swarm leave
    ```
2. Remove repository: 
    ```bash    
    rm -rf spark_on_docker_swarm
    ```
## References

- https://github.com/garystafford/pyspark-setup-demo