


# Docker Basics for Beginners

**The concepts you actually need first.**

A beginner-friendly introduction to Docker covering the core concepts: images, containers, Dockerfiles, volumes, networks, and Docker Compose. 

Explains why Docker exists (solving the 'works on my machine' problem), how containers differ from VMs, and walks through key Dockerfile instructions. 

Includes a hands-on 10-minute exercise running an nginx container and containerizing a simple Flask app, with a Docker Compose example for multi-container setups.

> A no-fluff introduction to Docker -what it is, the core building blocks, and a 10-minute hands-on example to make it click.

## Why Docker Exists

“It works on my machine” is the problem Docker was built to kill. An app depends on a specific language version, specific libraries, specific OS settings — and any mismatch between your laptop and the server breaks something.

Docker solves this by packaging an application and everything it needs to run (runtime, libraries, dependencies, config) into a single unit called a container. That container runs identically on your laptop, a teammate’s laptop, a CI server, or a production VM, because it’s not relying on whatever happens to be installed on the host — it carries its own environment with it.

This is different from a virtual machine. A VM virtualizes an entire operating system, which is heavy and slow to start. A container shares the host machine’s OS kernel and only packages the application layer on top — which is why containers start in milliseconds and use a fraction of the resources a VM does.

##The Core Building Blocks

### 1. Image — the blueprint

A **Docker image** is a read-only template: your application code, a runtime (like *Python* or *Node*), libraries, and configuration, all bundled into layers. It’s not running anything — it’s the packaged, shareable artifact, like a class before you instantiate an object.

Images are built from a **Dockerfile** — a text file of instructions for how to assemble the image, layer by layer.

```
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

Each instruction (`FROM`, `COPY`, `RUN`) creates a new layer. 
Docker caches layers, so if you only change your application code and not requirements.txt, rebuilding skips reinstalling dependencies — this is why dependency installation usually comes before copying the rest of the code, not after.

### 2. Container — the running instance

A container is a running instance of an image — the object created from that class. You can run many containers from the same image, each isolated from the others, each with its own filesystem, process space, and network interface (by default).

```
docker run -d -p 5000:5000 --name my-app my-flask-image
```

This starts a container named `my-app` from `my-flask-image`, running in the background (`-d`), with port 5000 inside the container mapped to port 5000 on your host (`-p`).

### 3. Dockerfile instructions worth knowing

Instruction: What it does `FROM` Base image to build on top of `WORKDIR` Sets the working directory inside the container `COPY` Copies files from your machine into the image `RUN` Executes a command at *build* time (e.g. installing packages) `CMD` The default command run at *container* start time `EXPOSE` Documents which port the app listens on (doesn't actually publish it — `-p` does that) `ENV` Sets environment variables inside the container

The distinction between `RUN` and `CMD` trips up a lot of beginners: `RUN` happens once, while the image is being built. 
`CMD` happens every time a container starts from that image.
Containers are disposable. 
Stop one, delete it, start a new one from the same image — nothing is lost, because the image itself never changes.
    
### 4. Volumes — data that survives

A container’s filesystem is ephemeral by default — delete the container, lose everything written inside it. For data that needs to persist (a database’s files, uploaded user content), Docker uses **volumes**: storage that exists outside the container’s own filesystem and survives container removal.

```
docker run -d -v pgdata:/var/lib/postgresql/data postgres:15
```

Here, `pgdata` is a named volume managed by Docker, mounted into the container at `/var/lib/postgresql/data`. 
Stop and remove the container, recreate it pointing at the same volume, and the data is still there.

### 5. Networks — containers talking to each other

By default, containers are isolated from each other. 
A **Docker network** lets containers discover and talk to one another by name instead of by IP address — which matters because container IPs change every time they’re recreated.

```
docker network create my-app-net
docker run -d --network my-app-net --name db postgres:15
docker run -d --network my-app-net --name web my-flask-image
```

Now the `web` container can reach the database simply by using the hostname `db` — Docker's built-in DNS resolves it.

### 6. Docker Compose — running multiple containers together

Most real applications are more than one container — a web server, a database, maybe a cache. 
Typing out `docker run` with all its flags for each one gets unwieldy fast. 
**Docker Compose** lets you define your entire multi-container setup in one YAML file and bring it all up with a single command.

```
version: "3.9"
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - db
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: mypassword
    volumes:
      - pgdata:/var/lib/postgresql/data
volumes:
  pgdata:
```

`docker-compose up` builds the `web` image, starts both services, creates a network between them automatically, and attaches the named volume — all the pieces above (image, container, volume, network) wired together in one file.

## How It All Fits Together

```
Dockerfile  →  build  →  Image
                            │
                       docker run
                            │
                          Container ── attached to ── Network (talks to other containers by name)
                            │
                       mounted volume ── Volume (data survives container removal)

Multiple services + networks + volumes, declared together → docker-compose.yml
```


## Hands-On: Run and Containerize a Simple App in 10 Minutes

1. Run an existing image (no Dockerfile needed yet):

```
docker run -d -p 8080:80 --name my-nginx nginx:latest
```

Visit `http://localhost:8080` — you'll see the default nginx welcome page. 
You just ran a container without writing a single line of config.

2. Check what's running:

```
docker ps
```

3. Look at the logs:

```
docker logs my-nginx
```

4. Get a shell inside the running container:

```
docker exec -it my-nginx bash
```

This drops you inside the container's filesystem — useful for debugging exactly what your app sees at runtime, as opposed to what you assume it sees.

5. Stop and clean up:

```
docker stop my-nginx
docker rm my-nginx
```

6. **Now containerize your own app**. Save this as Dockerfile in a small Flask project's root:

```
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```


Build and run it:

```
docker build -t my-flask-app .
docker run -d -p 5000:5000 --name flask-test my-flask-app
curl http://localhost:5000
```

That's the entire loop: write a Dockerfile once, build an image, run as many containers from it as you need, anywhere Docker is installed.




