Directory Structure:

docker-Assignment/
├── docker-compose.yml
├── nginx/
│   ├── Dockerfile
│   └── nginx.conf
├── service_1/ (Go app)
│   ├── Dockerfile, go.mod, main.go
└── service_2/ (Python app)
    ├── Dockerfile, requirements.txt, app.py

Build and Run:

    docker-compose up --build

🔁 Routing Logic

Configured in nginx.conf:

location /service1 {
    proxy_pass http://service_1:8081;
}
location /service2 {
    proxy_pass http://service_2:8082;
}

    Requests to http://localhost/service1 are routed to service_1 (Go app).

    Requests to http://localhost/service2 are routed to service_2 (Python app).

❌ Current Issue

    service_2 is not defined in docker-compose.yml.

    Both containers exit immediately because:

        service_1 (Go) prints "Hello, World!" and exits.

        service_2 doesn’t exist yet in Docker Compose.

🛠️ Fix Suggestions

    Add service_2 block in docker-compose.yml:

service_2:
  build: ./service_2
  ports:
    - "8082:8082"
  networks:
    - app-network

Add Python code (app.py) in service_2/:

from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Python service!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8082)

Add requirements.txt:

flask

######################################################################################################## Sample work ###########################################


──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED          SIZE
docker-assignment-nginx       latest    ed49ca895bd8   2 minutes ago    279MB
docker-assignment-service_1   latest    a3c540c85766   10 minutes ago   776MB

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED          SIZE
docker-assignment-nginx       latest    ed49ca895bd8   3 minutes ago    279MB
docker-assignment-service_1   latest    a3c540c85766   11 minutes ago   776MB

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cd ..

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ nano docker-compose.yml

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ docker-compose up --build
WARN[0001] /home/legil/docker-Assignment/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] Running 1/1
 ✔ nginx Pulled                                                                                                                                                    9.3s
Compose can now delegate builds to bake for better performance.
 To do so, set COMPOSE_BAKE=true.
[+] Building 15.6s (12/12) FINISHED                                                                                                                      docker:default
 => [service_1 internal] load build definition from Dockerfile                                                                                                     0.5s
 => => transferring dockerfile: 516B                                                                                                                               0.0s
 => [service_1 internal] load metadata for docker.io/library/golang:1.18-alpine                                                                                    5.8s
 => [service_1 auth] library/golang:pull token for registry-1.docker.io                                                                                            0.0s
 => [service_1 internal] load .dockerignore                                                                                                                        0.3s
 => => transferring context: 2B                                                                                                                                    0.0s
 => [service_1 1/5] FROM docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                              0.7s
 => => resolve docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                                        0.6s
 => [service_1 internal] load build context                                                                                                                        0.4s
 => => transferring context: 133B                                                                                                                                  0.0s
 => CACHED [service_1 2/5] WORKDIR /app                                                                                                                            0.0s
 => CACHED [service_1 3/5] COPY . .                                                                                                                                0.0s
 => CACHED [service_1 4/5] RUN go mod tidy                                                                                                                         0.0s
 => CACHED [service_1 5/5] RUN go build -o service1 .                                                                                                              0.0s
 => [service_1] exporting to image                                                                                                                                 2.8s
 => => exporting layers                                                                                                                                            0.0s
 => => exporting manifest sha256:c8cfb637675f22a0c91ac9bb5f82af0ec91d1719588025a8a1b2a63debad6188                                                                  0.1s
 => => exporting config sha256:0842210c78c1813253dba63d9bd7e271c56342d3d82bf16c50947292fbc79fd7                                                                    0.1s
 => => exporting attestation manifest sha256:e52de61171711fef373f6f4ca27fe2d2885a0a3e418ab53d42b667cfb92f7536                                                      1.0s
 => => exporting manifest list sha256:2daf027152a58649e5889031a934f62768cd126b2ae12d370d6968b66d6ea0a1                                                             0.5s
 => => naming to docker.io/library/docker-assignment-service_1:latest                                                                                              0.1s
 => => unpacking to docker.io/library/docker-assignment-service_1:latest                                                                                           0.2s
 => [service_1] resolving provenance for metadata file                                                                                                             0.1s
[+] Running 4/4
 ✔ service_1                                Built                                                                                                                  0.0s
 ✔ Network docker-assignment_app-network    Created                                                                                                                1.0s
 ✔ Container docker-assignment-service_1-1  Created                                                                                                                9.2s
 ✔ Container docker-assignment-nginx-1      Created                                                                                                                6.7s
Attaching to nginx-1, service_1-1
service_1-1  | Hello, World!
service_1-1 exited with code 0
nginx-1      | /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
nginx-1      | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
nginx-1      | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
nginx-1      | 10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
nginx-1      | 10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
nginx-1      | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
nginx-1      | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
nginx-1      | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
nginx-1      | /docker-entrypoint.sh: Configuration complete; ready for start up
nginx-1      | 2025/06/24 10:26:16 [notice] 1#1: using the "epoll" event method
nginx-1      | 2025/06/24 10:26:16 [notice] 1#1: nginx/1.27.5
nginx-1      | 2025/06/24 10:26:16 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
nginx-1      | 2025/06/24 10:26:16 [notice] 1#1: OS: Linux 5.15.167.4-microsoft-standard-WSL2
nginx-1      | 2025/06/24 10:26:16 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
nginx-1      | 2025/06/24 10:26:16 [notice] 1#1: start worker processes
nginx-1      | 2025/06/24 10:26:16 [notice] 1#1: start worker process 29
nginx-1      | 2025/06/24 10:26:16 [notice] 1#1: start worker process 30

Gracefully stopping... (press Ctrl+C again to force)
[+] Stopping 2/2
 ✔ Container docker-assignment-nginx-1      Stopped                                                                                                                2.1s
 ✔ Container docker-assignment-service_1-1  Stopped                                                                                                                0.2s

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ docker ps -a
CONTAINER ID   IMAGE                         COMMAND                  CREATED         STATUS                      PORTS     NAMES
b8d18609d60c   nginx:latest                  "/docker-entrypoint.…"   2 minutes ago   Exited (0) 15 seconds ago             docker-assignment-nginx-1
ced2458eca9f   docker-assignment-service_1   "./service1"             2 minutes ago   Exited (0) 2 minutes ago              docker-assignment-service_1-1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ ls
docker-compose.yml  nginx  service_1  service_2

























































