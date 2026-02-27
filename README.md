use image: hanvitha/myapp-book-store

🔹 1. How do you reduce the size of a Docker image?

Answer:

To reduce Docker image size, I follow these best practices:
Use lightweight base images
Example: node:alpine, python:alpine instead of full OS images.
Use multi-stage builds
Build dependencies in one stage and copy only required artifacts to final image.
Combine RUN commands
Reduce layers:
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
Remove unnecessary packages & cache
Clean apt cache
Remove build tools after installation

Use .dockerignore
Avoid copying:
node_modules
.git
logs
local config files
Use specific tags instead of latest
Ensures smaller and predictable images.

🔹 2. What is Docker Compose and when would you use it?

Answer:
Docker Compose is a tool used to define and manage multi-container Docker applications using a docker-compose.yml file.

It allows us to:
Define services
Configure networks
Set up volumes
Manage environment variables

When I use it:
Running app + database locally
Microservices architecture
Development & testing environments
Simplifying multi-container setup with single command:

docker-compose up -d

Example:
If I have a Node.js app + MongoDB + Redis, I define all in one YAML file.

🔹 3. How would you containerize a Node.js or Python application?
✅ Node.js Example:

Step 1: Create Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json .
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]

Step 2: Build Image
docker build -t node-app .
Step 3: Run Container
docker run -p 3000:3000 node-app
✅ Python Example:
FROM python:3.11-alpine
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]
🔹 4. How do you handle secrets in Docker?

Answer:
I never hardcode secrets in:
Dockerfile
Source code
Git repository
Secure approaches:
Environment Variables
Pass at runtime:
docker run -e DB_PASSWORD=secret
Docker Secrets (Swarm Mode)
In AWS environment:
Use AWS Secrets Manager
Or SSM Parameter Store
Attach IAM Role to ECS task
Use .env file (only for local dev, not production)
Best practice: Use IAM roles instead of storing credentials.

🔹 5. What happens if a container crashes? How can you ensure it restarts automatically?

If a container crashes:
The process inside container stops
Container status becomes “Exited”
To restart automatically:
Docker Restart Policies
docker run --restart=always image-name
Options:
no
on-failure
always
unless-stopped
In production:
Use Amazon ECS service with desired count
Health checks automatically replace failed containers
In Kubernetes:
Pods restart automatically
ReplicaSet maintains desired state

🔹 6. How would you debug a running container?

Answer:
Here are the steps I follow:
Check container logs
docker logs container_id
Exec into container
docker exec -it container_id sh
Inspect container
docker inspect container_id
Check resource usage
docker stats
If running in AWS:
Check CloudWatch logs
Verify ECS task events

🔹 7. What is the difference between Docker Swarm and Kubernetes?
Docker Swarm	          Kubernetes
Simple setup	          Complex but powerful
Docker-native	          Cloud-native standard
Limited ecosystem   	   Large ecosystem
Basic scaling	           Advanced auto-scaling
Less commonly used now	 Industry standard

Real-world perspective:
Swarm is easier for small setups
Kubernetes is preferred in enterprises
In AWS, we use Amazon EKS for managed Kubernetes

🔥 Interview Tip for 2-Year DevOps Engineer

When answering:
Give practical experience
Mention AWS integration
Talk about production scenarios
Mention monitoring & security
