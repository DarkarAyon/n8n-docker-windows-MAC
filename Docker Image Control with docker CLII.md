# Find your container ID
  
    docker ps -a

# Stop the container with the `<container_id>`

    docker stop <container_id>

# Remove the container with the `<container_id>`

    docker rm <container_id>

# Start the container

    docker run --name=<container_name> [options] -d docker.n8n.io/n8nio/n8n
