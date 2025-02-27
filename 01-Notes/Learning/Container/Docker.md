# Docker Cheat Sheet for Interview or Learning (Excel-Compatible)

  

Below is a comprehensive Docker cheat sheet designed for interview preparation or learning. It includes essential commands and key concepts, formatted so you can easily copy and paste it into Excel for study or reference.

  

## Instructions for Excel

  

1. Copy the entire content below.
  
2. Paste it into a text editor and save it as a .txt file, or paste it directly into Excel.
  
4. In Excel, select the pasted column, go to **Data > Text to Columns**, choose **Delimited**, and set | as the delimiter. Click **Finish**.
  
6. This will split the content into three columns: **Category**, **Item**, and **Description**.
  

## Docker Cheat Sheet

  

text

  

WrapCopy

  

`Basic Commands|docker --version|Check Docker version Basic Commands|docker info|Get detailed information about Docker   Image Commands|docker build -t <image_name> .|Build an image from Dockerfile in current directory   Image Commands|docker images|List all images on the system   Image Commands|docker rmi <image_id>|Remove an image   Image Commands|docker pull <image_name>|Pull an image from registry   Image Commands|docker push <image_name>|Push an image to registry   Container Commands|docker run -d --name <container_name> <image_name>|Run a container in detached mode   Container Commands|docker ps|List running containers   Container Commands|docker ps -a|List all containers including stopped ones   Container Commands|docker stop <container_id>|Stop a running container   Container Commands|docker start <container_id>|Start a stopped container   Container Commands|docker rm <container_id>|Remove a container   Container Commands|docker exec -it <container_id> /bin/bash|Open a bash shell in a running container   Container Commands|docker logs <container_id>|View logs of a container   Container Commands|docker inspect <container_id>|Get detailed information about a container   Network Commands|docker network create <network_name>|Create a new network   Network Commands|docker network ls|List all networks   Network Commands|docker network inspect <network_name>|Inspect a network   Network Commands|docker run --network <network_name> <image_name>|Run a container on a specific network   Volume Commands|docker volume create <volume_name>|Create a new volume   Volume Commands|docker volume ls|List all volumes   Volume Commands|docker volume rm <volume_name>|Remove a volume   Volume Commands|docker run -v <volume_name>:<container_path> <image_name>|Mount a volume to a container   Cleaning Up|docker system prune|Remove all unused containers networks images and volumes   Cleaning Up|docker image prune|Remove unused images   Cleaning Up|docker container prune|Remove stopped containers   Docker Compose|docker-compose up|Start services defined in docker-compose.yml   Docker Compose|docker-compose down|Stop and remove services defined in docker-compose.yml   Docker Compose|docker-compose build|Build or rebuild services   Docker Swarm|docker swarm init|Initialize a swarm   Docker Swarm|docker service create|Create a new service   Docker Swarm|docker service ls|List services in the swarm   Docker Swarm|docker node ls|List nodes in the swarm   Concepts|Dockerfile|A script to build Docker images   Concepts|Image|A read-only template for creating containers   Concepts|Container|A runnable instance of an image   Concepts|Volume|Persistent data storage for containers   Concepts|Network|Allows communication between containers   Concepts|Docker Compose|Tool for defining and running multi-container applications   Concepts|Docker Swarm|Native clustering and orchestration for Docker`

  

## Additional Notes

  

- **Categories**: The cheat sheet is organized into categories such as Basic Commands, Image Commands, Container Commands, and more, making it easier to navigate in Excel.
  
- **Concepts**: Key Docker concepts are included at the end to help you understand the terminology and architecture, which is crucial for interviews.
  
- **Excel Tips**:  
    - After splitting into columns, you can use Excel's sorting or filtering features to focus on specific categories.
      
    - Adjust column widths for better readability.
      
    
  
- **Interview Preparation**:  
    - Be familiar with the commands and their options (e.g., -d for detached mode, -it for interactive terminal).
      
    - Understand the difference between images and containers, and the purpose of volumes and networks.
      
    - For advanced interviews, be prepared to discuss Docker Compose and Docker Swarm, including their use cases.
      
    
  

This cheat sheet covers essential Docker commands and concepts, making it ideal for both learning and interview preparation.

### **一、基础操作**

1. ​**镜像管理**​
    - 拉取镜像：`docker pull <镜像名:标签>`（如`nginx:latest`）
        
        2
        
        4
        
    - 查看本地镜像：`docker images`
        
        4
        
        7
        
    - 删除镜像：`docker rmi <镜像ID>`
        
        4
        
2. ​**容器操作**​
    - 启动容器：`docker run -d --name <容器名> -p <宿主机端口>:<容器端口> <镜像名>`（如`docker run -d --name my-nginx -p 8080:80 nginx`）
        
        2
        
        4
        
        7
        
    - 查看运行容器：`docker ps`
        
        4
        
        7
        
    - 停止/删除容器：`docker stop <容器名>` / `docker rm <容器名>`
        
        4
        
        7
        
3. ​**数据持久化**​
    - 挂载卷：`docker run -v <宿主机路径>:<容器路径> <镜像名>`（如`-v /data:/app/data`）
        
        4
        
        14
        
    - 使用数据卷：`docker volume create <卷名>`，运行时挂载
        
        14
        

---

### ​**二、故障排查**

1. ​**容器启动失败**​
    - 检查日志：`docker logs <容器名>`
        
        8
        
        9
        
    - 验证镜像：`docker images`确认镜像存在且未损坏
        
        8
        
    - 检查端口冲突：`netstat -tuln | grep <端口>`
        
        10
        
2. ​**网络问题**​
    - 进入容器检查网络：`docker exec -it <容器名> /bin/bash`后执行`ping`或`curl`
        
        9
        
    - 检查Docker网络配置：`docker network inspect <网络名>`
        
        15
        
3. ​**权限问题**​
    - 将用户加入Docker组：`sudo usermod -aG docker $USER`
        
        10
        

---

### ​**三、监控与日志**

1. ​**实时资源监控**​
    - `docker stats`：查看容器CPU、内存等实时数据
        
        11
        
        13
        
    - `docker top <容器名>`：查看容器内进程
        
        11
        
2. ​**日志管理**​
    - 集中日志工具：ELK Stack（Elasticsearch+Logstash+Kibana）或Fluentd
        
        12
        
        13
        
    - 日志轮转：在Dockerfile中配置`RUN echo "max-size=10m" >> /etc/logrotate.conf`
        
        12
        
3. ​**健康检查**​
    - Dockerfile中添加`HEALTHCHECK`指令（如`CMD curl -f http://localhost/health || exit 1`）
        
        11
        
        12
        

---

### ​**四、高级功能**

1. ​**自定义网络**​
    - 创建自定义网桥：`docker network create --subnet=192.168.0.0/24 my_network`
        
        14
        
        15
        
    - 运行容器指定网络：`docker run --network=my_network <镜像名>`
        
        14
        
2. ​**Docker Compose**​
    - 定义多容器应用：使用`docker-compose.yml`管理服务、网络和卷
        
        16
        
    - 启动服务：`docker compose up -d`
        
        16
        
3. ​**镜像优化**​
    - 使用多阶段构建减少镜像体积（Dockerfile中`FROM ... AS builder`）
        
        14
        
    - 清理未使用镜像：`docker image prune -a`
        
        4
        

---

### ​**五、常用命令速查**

| 功能          | 命令示例                              | 说明         | 来源         |
| ----------- | --------------------------------- | ---------- | ---------- |
| 查看所有容器（含停止） | `docker ps -a`                    | 列出所有容器实例   | 4<br><br>7 |
| 进入容器交互式操作   | `docker exec -it <容器名> /bin/bash` | 进入容器终端     | 4<br><br>7 |
| 导出/导入镜像     | `docker save -o <文件名>.tar <镜像>`   | 导出镜像为tar包  | 16         |
| 动态调整资源限制    | `docker update --memory=2g <容器名>` | 限制容器内存为2GB | 14         |

### **Docker Cheat Sheet**

---

#### **Basic Usage**

|Command|Description|
|---|---|
|`docker run -d --name <name> <image>`|Run a container in detached mode.|
|`docker run -it <image> /bin/bash`|Run a container interactively.|
|`docker run -p <host_port>:<container_port> <image>`|Map host port to container port.|
|`docker run -v <host_dir>:<container_dir> <image>`|Mount a host directory as a volume.|
|`docker ps`|List running containers.|
|`docker ps -a`|List all containers (including stopped).|
|`docker start/stop/restart <container>`|Start/stop/restart a container.|
|`docker rm <container>`|Remove a stopped container.|
|`docker rmi <image>`|Remove an image.|
|`docker pull <image>`|Download an image from a registry.|
|`docker images`|List all local images.|
|`docker build -t <tag> .`|Build an image from a Dockerfile.|
|`docker push <user>/<image>`|Push an image to a registry.|

---

#### **Troubleshooting**

|Command|Description|
|---|---|
|`docker logs <container>`|View container logs.|
|`docker logs --tail=100 -f <container>`|Tail the last 100 log lines in real-time.|
|`docker inspect <container>`|Inspect container metadata (JSON).|
|`docker exec -it <container> /bin/bash`|Open a shell in a running container.|
|`docker top <container>`|View processes running inside a container.|
|`docker stats`|Live resource usage (CPU, memory, etc.).|
|`docker port <container>`|List port mappings for a container.|
|`docker system prune`|Remove unused containers, networks, and images.|
|`docker rm -f <container>`|Force-remove a running container.|

**Common Issues**:

- **Port conflicts**: Use `netstat -tulpn | grep <port>` to find conflicting processes.
    
- **Image pull errors**: Check authentication with `docker login`.
    

---

#### **Monitoring**

|Command|Description|
|---|---|
|`docker stats`|Live resource usage for all containers.|
|`docker system df`|Check Docker disk usage.|
|`docker top <container>`|View container processes.|
|`docker events`|Stream real-time Docker events.|
|`docker update --memory 512m <container>`|Update container memory limit.|

**Tools**:

- **cAdvisor**: Run `docker run -d --name=cadvisor -p 8080:8080 google/cadvisor` for container metrics.
    
- **Prometheus/Grafana**: For advanced monitoring.
    

---

#### **Log Management**

|Command|Description|
|---|---|
|`docker logs <container>`|View logs.|
|`docker logs --since=10m <container>`|Show logs from the last 10 minutes.|
|`docker logs -t <container>`|Include timestamps.|
|`docker compose logs`|View logs for Compose services.|
|`docker logs --details <container>`|Show extra details (e.g., environment variables).|

**Log Drivers**:

- Configure in `daemon.json`: `json-file`, `syslog`, `journald`, etc.
    
- Log location (default): `/var/lib/docker/containers/<container_id>/<container_id>-json.log`.
    

---

#### **Advanced Usage**

**Networking**:

|Command|Description|
|---|---|
|`docker network create <network>`|Create a custom network.|
|`docker network ls`|List networks.|
|`docker network inspect <network>`|Inspect network details.|

**Docker Compose**:

|Command|Description|
|---|---|
|`docker compose up -d`|Start services in detached mode.|
|`docker compose down`|Stop and remove containers/networks.|
|`docker compose build`|Rebuild images.|

**Volumes**:

|Command|Description|
|---|---|
|`docker volume create <name>`|Create a volume.|
|`docker volume ls`|List volumes.|
|`docker volume inspect <name>`|Inspect volume details.|

**Swarm**:

|Command|Description|
|---|---|
|`docker swarm init`|Initialize a Swarm.|
|`docker service create <image>`|Deploy a service.|
|`docker service ls`|List services.|

**Miscellaneous**:

- **Multi-stage builds**: Optimize image size in Dockerfiles.
    
- **Healthchecks**: Add `HEALTHCHECK` directives to Dockerfiles.
    
- **Buildx**: `docker buildx build --platform linux/amd64,linux/arm64` for multi-arch images.
    
- **Save/Load Images**: `docker save -o <file>.tar <image>` and `docker load -i <file>.tar`.
    
- **Security**: Run containers as non-root with `USER <username>` in Dockerfiles.
    

---

#### **Quick Tips**

- Use `docker <command> --help` for detailed documentation.
    
- Clean up unused data: `docker system prune -a --volumes`.
    
- Copy files: `docker cp <container>:<path> <host_path>`.
    
- Set environment variables: `-e VAR=value` in `docker run` or use `.env` files with Compose.
    
