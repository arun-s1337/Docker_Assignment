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

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ ls
docker-compose.yml  nginx  service_1  service_2

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
[+] Building 180.3s (12/16)                                                                                                                              docker:default
 => [nginx internal] load build definition from Dockerfile                                                                                                         2.4s
 => => transferring dockerfile: 227B                                                                                                                               0.1s
 => [service_2 internal] load build definition from Dockerfile                                                                                                     2.4s
 => => transferring dockerfile: 324B                                                                                                                               0.0s
 => [nginx internal] load metadata for docker.io/library/nginx:latest                                                                                             12.7s
 => [service_2 internal] load metadata for docker.io/library/python:3.9-alpine                                                                                    11.4s
 => [nginx auth] library/nginx:pull token for registry-1.docker.io                                                                                                 0.0s
 => [service_2 auth] library/python:pull token for registry-1.docker.io                                                                                            0.0s
 => [service_2 internal] load .dockerignore                                                                                                                        1.9s
 => => transferring context: 2B                                                                                                                                    0.0s
 => [nginx internal] load .dockerignore                                                                                                                            2.0s
 => => transferring context: 2B                                                                                                                                    0.0s
 => [service_2 1/4] FROM docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                             116.4s
 => => resolve docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                                         1.0s
 => => sha256:1155a024ecd595abfc1d6d517f2a4339b62ca11f392ac0fe27faebf5fe66b987 249B / 249B                                                                         1.7s
 => => sha256:7de06ff945306f4194448c4c54ada65369102e1cb92db3b093193aa7079a41c2 460.22kB / 460.22kB                                                                 2.4s
 => => sha256:0c75cc4c879f8b1be0ef5e7ac1a4efe9caf769575cc4875f2d45578723ae189c 14.88MB / 14.88MB                                                                 150.9s
 => => sha256:fe07684b16b82247c3539ed86a65ff37a76138ec25d380bd80c869a1a4c73236 3.80MB / 3.80MB                                                                     4.5s
 => => extracting sha256:fe07684b16b82247c3539ed86a65ff37a76138ec25d380bd80c869a1a4c73236                                                                         13.3s
 => => extracting sha256:7de06ff945306f4194448c4c54ada65369102e1cb92db3b093193aa7079a41c2                                                                         14.3s
 => => extracting sha256:0c75cc4c879f8b1be0ef5e7ac1a4efe9caf769575cc4875f2d45578723ae189c                                                                         65.2s
 => => extracting sha256:1155a024ecd595abfc1d6d517f2a4339b62ca11f392ac0fe27faebf5fe66b987                                                                          4.0s
 => [service_2 internal] load build context                                                                                                                        5.1s
 => => transferring context: 324B                                                                                                                                  0.1s
 => [nginx internal] load build context                                                                                                                            6.7s
 => => transferring context: 677B                                                                                                                                  0.1s
 => [nginx 1/2] FROM docker.io/library/nginx:latest@sha256:6784fb0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d                                      162.5s
 => => resolve docker.io/library/nginx:latest@sha256:6784fb0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d                                              2.1s
 => => sha256:32e44235e1d55a34be577baa12271f067c0bf79ba4e1ca4e6ec484424540132a 1.40kB / 1.40kB                                                                     9.2s
 => => sha256:d2a7ba8dbfee4f222961ad1449648ab46a6cea044e4cfb374450325ecee8f67f 1.21kB / 1.21kB                                                                     9.7s
 => => sha256:979e6233a40af9a51ee2c9894bd42ebf78a79c87cbed70cc6a3840a0952f3d57 405B / 405B                                                                         9.4s
 => => sha256:1bc5dc8b475d9de62c94cf69a139df17aaf7fe7bac76b5133d8048a058fb3c3b 956B / 956B                                                                         3.3s
 => => sha256:3b00567da96412a0298328fad6b96aadd3bc9ca97e6683534da152b0b9e8a977 44.15MB / 44.15MB                                                                  55.8s
 => => sha256:56b81cfa547d78176e719c9cd387e35fbd95ce6e8500efb26d9fbcbac4dbc4f6 628B / 628B                                                                         3.7s
 => => sha256:dad67da3f26bce15939543965e09c4059533b025f707aad72ed3d3f3a09c66f8 28.23MB / 28.23MB                                                                  46.3s
 => => extracting sha256:dad67da3f26bce15939543965e09c4059533b025f707aad72ed3d3f3a09c66f8                                                                         81.0s
 => [service_2 2/4] WORKDIR /app                                                                                                                                  44.1s
 => [service_2 3/4] COPY . .                                                                                                                                       2.6s
[+] Building 180.5s (12/16)                                                                                                                              docker:default
 => [nginx internal] load build definition from Dockerfile                                                                                                         2.4s
 => => transferring dockerfile: 227B                                                                                                                               0.1s
 => [service_2 internal] load build definition from Dockerfile                                                                                                     2.4s
 => => transferring dockerfile: 324B                                                                                                                               0.0s
 => [nginx internal] load metadata for docker.io/library/nginx:latest                                                                                             12.7s
 => [service_2 internal] load metadata for docker.io/library/python:3.9-alpine                                                                                    11.4s
 => [nginx auth] library/nginx:pull token for registry-1.docker.io                                                                                                 0.0s
 => [service_2 auth] library/python:pull token for registry-1.docker.io                                                                                            0.0s
 => [service_2 internal] load .dockerignore                                                                                                                        1.9s
 => => transferring context: 2B                                                                                                                                    0.0s
 => [nginx internal] load .dockerignore                                                                                                                            2.0s
 => => transferring context: 2B                                                                                                                                    0.0s
 => [service_2 1/4] FROM docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                             116.4s
 => => resolve docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                                         1.0s
 => => sha256:1155a024ecd595abfc1d6d517f2a4339b62ca11f392ac0fe27faebf5fe66b987 249B / 249B                                                                         1.7s
 => => sha256:7de06ff945306f4194448c4c54ada65369102e1cb92db3b093193aa7079a41c2 460.22kB / 460.22kB                                                                 2.4s
 => => sha256:0c75cc4c879f8b1be0ef5e7ac1a4efe9caf769575cc4875f2d45578723ae189c 14.88MB / 14.88MB                                                                 151.1s
 => => sha256:fe07684b16b82247c3539ed86a65ff37a76138ec25d380bd80c869a1a4c73236 3.80MB / 3.80MB                                                                     4.5s
 => => extracting sha256:fe07684b16b82247c3539ed86a65ff37a76138ec25d380bd80c869a1a4c73236                                                                         13.3s
 => => extracting sha256:7de06ff945306f4194448c4c54ada65369102e1cb92db3b093193aa7079a41c2                                                                         14.3s
 => => extracting sha256:0c75cc4c879f8b1be0ef5e7ac1a4efe9caf769575cc4875f2d45578723ae189c                                                                         65.2s
[+] Building 267.1s (17/17) FINISHED                                                                                                                     docker:default
 => [nginx internal] load build definition from Dockerfile                                                                                                         2.4s
 => => transferring dockerfile: 227B                                                                                                                               0.1s
 => [service_2 internal] load build definition from Dockerfile                                                                                                     2.4s
 => => transferring dockerfile: 324B                                                                                                                               0.0s
 => [nginx internal] load metadata for docker.io/library/nginx:latest                                                                                             12.7s
 => [service_2 internal] load metadata for docker.io/library/python:3.9-alpine                                                                                    11.4s
 => [nginx auth] library/nginx:pull token for registry-1.docker.io                                                                                                 0.0s
 => [service_2 auth] library/python:pull token for registry-1.docker.io                                                                                            0.0s
 => [service_2 internal] load .dockerignore                                                                                                                        1.9s
 => => transferring context: 2B                                                                                                                                    0.0s
 => [nginx internal] load .dockerignore                                                                                                                            2.0s
 => => transferring context: 2B                                                                                                                                    0.0s
 => [service_2 1/4] FROM docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                             116.4s
 => => resolve docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                                         1.0s
 => => sha256:1155a024ecd595abfc1d6d517f2a4339b62ca11f392ac0fe27faebf5fe66b987 249B / 249B                                                                         1.7s
 => => sha256:7de06ff945306f4194448c4c54ada65369102e1cb92db3b093193aa7079a41c2 460.22kB / 460.22kB                                                                 2.4s
 => => sha256:0c75cc4c879f8b1be0ef5e7ac1a4efe9caf769575cc4875f2d45578723ae189c 14.88MB / 14.88MB                                                                 237.7s
 => => sha256:fe07684b16b82247c3539ed86a65ff37a76138ec25d380bd80c869a1a4c73236 3.80MB / 3.80MB                                                                     4.5s
 => => extracting sha256:fe07684b16b82247c3539ed86a65ff37a76138ec25d380bd80c869a1a4c73236                                                                         13.3s
 => => extracting sha256:7de06ff945306f4194448c4c54ada65369102e1cb92db3b093193aa7079a41c2                                                                         14.3s
 => => extracting sha256:0c75cc4c879f8b1be0ef5e7ac1a4efe9caf769575cc4875f2d45578723ae189c                                                                         65.2s
 => => extracting sha256:1155a024ecd595abfc1d6d517f2a4339b62ca11f392ac0fe27faebf5fe66b987                                                                          4.0s
 => [service_2 internal] load build context                                                                                                                        5.1s
 => => transferring context: 324B                                                                                                                                  0.1s
 => [nginx internal] load build context                                                                                                                            6.7s
 => => transferring context: 677B                                                                                                                                  0.1s
 => [nginx 1/2] FROM docker.io/library/nginx:latest@sha256:6784fb0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d                                      210.7s
 => => resolve docker.io/library/nginx:latest@sha256:6784fb0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d                                              2.1s
 => => sha256:32e44235e1d55a34be577baa12271f067c0bf79ba4e1ca4e6ec484424540132a 1.40kB / 1.40kB                                                                     9.2s
 => => sha256:d2a7ba8dbfee4f222961ad1449648ab46a6cea044e4cfb374450325ecee8f67f 1.21kB / 1.21kB                                                                     9.7s
 => => sha256:979e6233a40af9a51ee2c9894bd42ebf78a79c87cbed70cc6a3840a0952f3d57 405B / 405B                                                                         9.4s
 => => sha256:1bc5dc8b475d9de62c94cf69a139df17aaf7fe7bac76b5133d8048a058fb3c3b 956B / 956B                                                                         3.3s
 => => sha256:3b00567da96412a0298328fad6b96aadd3bc9ca97e6683534da152b0b9e8a977 44.15MB / 44.15MB                                                                  55.8s
 => => sha256:56b81cfa547d78176e719c9cd387e35fbd95ce6e8500efb26d9fbcbac4dbc4f6 628B / 628B                                                                         3.7s
 => => sha256:dad67da3f26bce15939543965e09c4059533b025f707aad72ed3d3f3a09c66f8 28.23MB / 28.23MB                                                                  46.3s
 => => extracting sha256:dad67da3f26bce15939543965e09c4059533b025f707aad72ed3d3f3a09c66f8                                                                         81.4s
 => => extracting sha256:3b00567da96412a0298328fad6b96aadd3bc9ca97e6683534da152b0b9e8a977                                                                         42.2s
 => => extracting sha256:56b81cfa547d78176e719c9cd387e35fbd95ce6e8500efb26d9fbcbac4dbc4f6                                                                          0.9s
 => => extracting sha256:1bc5dc8b475d9de62c94cf69a139df17aaf7fe7bac76b5133d8048a058fb3c3b                                                                          1.0s
 => => extracting sha256:979e6233a40af9a51ee2c9894bd42ebf78a79c87cbed70cc6a3840a0952f3d57                                                                          1.7s
 => => extracting sha256:d2a7ba8dbfee4f222961ad1449648ab46a6cea044e4cfb374450325ecee8f67f                                                                          0.7s
 => => extracting sha256:32e44235e1d55a34be577baa12271f067c0bf79ba4e1ca4e6ec484424540132a                                                                          1.1s
 => [service_2 2/4] WORKDIR /app                                                                                                                                  44.1s
 => [service_2 3/4] COPY . .                                                                                                                                      16.1s
 => ERROR [service_2 4/4] RUN pip install -r requirements.txt                                                                                                     64.6s
 => [nginx 2/2] COPY nginx.conf /etc/nginx/nginx.conf                                                                                                              8.5s
 => [nginx] exporting to image                                                                                                                                    29.7s
 => => exporting layers                                                                                                                                            5.9s
 => => exporting manifest sha256:e53dbba9d7fde8e0d32cd0c3cdf69a7272093832ce387b584297125cb29a120b                                                                  1.3s
 => => exporting config sha256:9e4fdc5acd1ece634795eb7a71ad464d0039c3eed5a2e43d4962373bb226b78b                                                                    1.2s
 => => exporting attestation manifest sha256:f7d21de6fca37db5df44ef3544b449ff8cff579e8338e16da90f5eb1f142abb2                                                     14.5s
 => => exporting manifest list sha256:ed49ca895bd8d05f18ff7262184dddb41d5c6db02a7ee029f6d6ad9805c5505c                                                             2.5s
 => => naming to docker.io/library/docker-assignment-nginx:latest                                                                                                  0.7s
 => => unpacking to docker.io/library/docker-assignment-nginx:latest                                                                                               1.8s
------ sha256:fe07684b16b82247c3539ed86a65ff37a76138ec25d380bd80c869a1a4c73236 3.80MB / 3.80MB                                                                     4.5s
 > [service_2 4/4] RUN pip install -r requirements.txt:37a76138ec25d380bd80c869a1a4c73236                                                                         13.3s
49.78 ERROR: Could not open requirements file: [Errno 2] No such file or directory: 'requirements.txt'                                                            14.3s
52.76  extracting sha256:0c75cc4c879f8b1be0ef5e7ac1a4efe9caf769575cc4875f2d45578723ae189c                                                                         65.2s
52.76 [notice] A new release of pip is available: 23.0.1 -> 25.1.192ac0fe27faebf5fe66b987                                                                          4.0s
52.76 [notice] To update, run: pip install --upgrade pip                                                                                                           5.1s
------ transferring context: 324B                                                                                                                                  0.1s
failed to solve: process "/bin/sh -c pip install -r requirements.txt" did not complete successfully: exit code: 1                                                  6.7s
 => => transferring context: 677B                                                                                                                                  0.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]6784fb0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d                                      151.0s
└─$ => resolve docker.io/library/nginx:latest@sha256:6784fb0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d                                              2.1s
 => => sha256:32e44235e1d55a34be577baa12271f067c0bf79ba4e1ca4e6ec484424540132a 1.40kB / 1.40kB                                                                     9.2s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]b374450325ecee8f67f 1.21kB / 1.21kB                                                                     9.7s
└─$ => sha256:979e6233a40af9a51ee2c9894bd42ebf78a79c87cbed70cc6a3840a0952f3d57 405B / 405B                                                                         9.4s
 => => sha256:1bc5dc8b475d9de62c94cf69a139df17aaf7fe7bac76b5133d8048a058fb3c3b 956B / 956B                                                                         3.3s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]3534da152b0b9e8a977 44.15MB / 44.15MB                                                                  55.8s
└─$ => sha256:56b81cfa547d78176e719c9cd387e35fbd95ce6e8500efb26d9fbcbac4dbc4f6 628B / 628B                                                                         3.7s
 => => sha256:dad67da3f26bce15939543965e09c4059533b025f707aad72ed3d3f3a09c66f8 28.23MB / 28.23MB                                                                  46.3s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]33b025f707aad72ed3d3f3a09c66f8                                                                         69.5s
└─$ [service_2 2/4] WORKDIR /app                                                                                                                                  35.5s
 To do so, set COMPOSE_BAKE=true.
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                              docker:default
└─$ [service_2 internal] load build definition from Dockerfile                                                                                                     1.7s
 => => transferring dockerfile: 324B                                                                                                                               0.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]y/python:3.9-alpine                                                                                    10.3s
└─$ [service_2 auth] library/python:pull token for registry-1.docker.io                                                                                            0.0s
 => [service_2 internal] load .dockerignore                                                                                                                        1.5s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.1s
└─$ [service_2 1/4] FROM docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                              48.3s
 => => resolve docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                                         0.8s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]75f2d45578723ae189c 14.88MB / 14.88MB                                                                  12.8s
└─$ => sha256:1155a024ecd595abfc1d6d517f2a4339b62ca11f392ac0fe27faebf5fe66b987 249B / 249B                                                                         2.2s
 => => sha256:fe07684b16b82247c3539ed86a65ff37a76138ec25d380bd80c869a1a4c73236 3.80MB / 3.80MB                                                                     9.5s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]3b093193aa7079a41c2 460.22kB / 460.22kB                                                                 2.2s
└─$ => extracting sha256:fe07684b16b82247c3539ed86a65ff37a76138ec25d380bd80c869a1a4c73236                                                                          4.3s
 => => extracting sha256:7de06ff945306f4194448c4c54ada65369102e1cb92db3b093193aa7079a41c2                                                                          1.4s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]f769575cc4875f2d45578723ae189c                                                                         24.2s
└─$ => extracting sha256:1155a024ecd595abfc1d6d517f2a4339b62ca11f392ac0fe27faebf5fe66b987                                                                          1.8s
 => [service_2 internal] load build context                                                                                                                        3.0s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.1s
└─$ [service_2 2/4] WORKDIR /app                                                                                                                                  32.0s
 => [service_2 3/4] COPY . .                                                                                                                                       3.6s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]xt                                                                                                     64.9s
└─$ --
 > [service_2 4/4] RUN pip install -r requirements.txt:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1] such file or directory: 'requirements.txt'
└─$ 3
52.43 [notice] A new release of pip is available: 23.0.1 -> 25.1.1
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ --
failed to solve: process "/bin/sh -c pip install -r requirements.txt" did not complete successfully: exit code: 1
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ docker-compose up --build
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
└─$ ose can now delegate builds to bake for better performance.
 To do so, set COMPOSE_BAKE=true.
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                              docker:default
└─$ [nginx internal] load build definition from Dockerfile                                                                                                         1.0s
 => => transferring dockerfile: 227B                                                                                                                               0.2s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]ile                                                                                                     1.2s
└─$ => transferring dockerfile: 410B                                                                                                                               0.1s
 => [service_2 internal] load build definition from Dockerfile                                                                                                     1.3s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.1s
└─$ [nginx internal] load metadata for docker.io/library/nginx:latest                                                                                              1.5s
 => [service_1 internal] load metadata for docker.io/library/golang:1.18-alpine                                                                                    2.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]y/python:3.9-alpine                                                                                     4.3s
└─$ [nginx internal] load .dockerignore                                                                                                                            0.8s
 => => transferring context: 2B                                                                                                                                    0.0s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        1.1s
└─$ => transferring context: 677B                                                                                                                                  0.0s
 => CACHED [nginx 1/2] FROM docker.io/library/nginx:latest@sha256:6784fb0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d                                 1.6s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d                                              1.1s
└─$ [service_1 internal] load .dockerignore                                                                                                                        1.5s
 => => transferring context: 2B                                                                                                                                    0.0s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]ne@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                              3.2s
└─$ => resolve docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                                        2.9s
 => [service_1 internal] load build context                                                                                                                        1.8s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.1s
└─$ [nginx 2/2] COPY nginx.conf /etc/nginx/nginx.conf                                                                                                              5.3s
 => [service_2 internal] load .dockerignore                                                                                                                        1.8s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.0s
└─$ [service_2 1/4] FROM docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                               2.8s
 => => resolve docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                                         2.5s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.7s
└─$ => transferring context: 32B                                                                                                                                   0.1s
 => CACHED [service_1 2/5] WORKDIR /app                                                                                                                            0.2s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                       30.6s
└─$ => exporting layers                                                                                                                                            9.1s
 => => exporting manifest sha256:54a74eb8bacd1c575f729583764bd2dad5aae397576152ec2ce77f8195001080                                                                  1.2s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]5f8bc102b252cda0c4af4766e19b97339e4a                                                                    1.8s
└─$ => exporting attestation manifest sha256:8de8f2db72959231039d6187d35045b696432f723c87e2fa3617d6c17371d7f3                                                      7.9s
 => => exporting manifest list sha256:75fc508c6ac4baaf99ce900a7d0dae12e65c48181a72452c19a60e414228ba6f                                                             4.3s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]latest                                                                                                  0.5s
└─$ => unpacking to docker.io/library/docker-assignment-nginx:latest                                                                                               3.5s
 => [service_1 3/5] COPY . .                                                                                                                                       6.2s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.0s
└─$ CACHED [service_2 3/4] COPY . .                                                                                                                                0.0s
 => [service_2 4/4] RUN pip install -r requirements.txt                                                                                                           37.9s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                       23.8s
└─$ --
 > [service_1 4/5] RUN go mod tidy:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1] parent directory; see 'go help modules'
└─$ --
failed to solve: process "/bin/sh -c go mod tidy" did not complete successfully: exit code: 1
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ cd service_1
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cat Dockerfile
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$  golang:1.18-alpine

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ DIR /app

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$  . .

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ go mod tidy

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ SE 8081

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ go build -o service1 .

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ["./service1"]

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker ps -a
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]S     NAMES
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ AINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ go mod tidy
Command 'go' not found, but can be installed with:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]nt called 'main')
└─$  apt install golang-go (You will have to enable component called 'main')

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker-compose up --buildcd ~/docker-Assignment/service_1
unknown flag: --buildcd
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ go mod init service_1
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$  apt install gccgo-go  (You will have to enable component called 'main')
sudo apt install golang-go (You will have to enable component called 'main')
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ sudo apt update
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]5 kB]
└─$ 2 http://kali.download/kali kali-rolling/main amd64 Packages [21.0 MB]
Get:3 http://kali.download/kali kali-rolling/main amd64 Contents (deb) [51.4 MB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ packages can be upgraded. Run 'apt list --upgradable' to see them.

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ sudo apt install golang-go
Upgrading:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]dc++6  locales-all
└─$
Installing:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
Installing dependencies:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]4                   libasan8      libcrypt-dev   libhwasan0  libpkgconf3       linux-libc-dev
└─$ nutils-common            g++                      gcc-14-x86-64-linux-gnu  libatomic1    libctf-nobfd0  libisl23    libquadmath0      manpages
  binutils-x86-64-linux-gnu  g++-14                   gcc-x86-64-linux-gnu     libbinutils   libctf0        libitm1     libsframe1        manpages-dev
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]g-1.24-go           libc-dev-bin  libgcc-14-dev  liblsan0    libstdc++-14-dev  pkgconf
└─$ p-14                     g++-x86-64-linux-gnu     golang-1.24-src          libc6-dev     libgomp1       libmpc3     libtsan2          pkgconf-bin
  cpp-14-x86-64-linux-gnu    gcc                      golang-src               libcc1-0      libgprofng0    libmpfr6    libubsan1         rpcsvc-proto
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ested packages:
  binutils-doc   cpp-doc         g++-multilib     gcc-multilib  automake  bison    gcc-14-multilib       | brz       libc-devtools     man-browser
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]     libtool   gdb      gdb-x86-64-linux-gnu  mercurial   glibc-doc
└─$ nutils-gold  cpp-14-doc      gcc-14-doc       autoconf      flex      gcc-doc  bzr                   subversion  libstdc++-14-doc

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ grading: 7, Installing: 49, Removing: 0, Not Upgrading: 166
  Download size: 141 MB
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
Continue? [Y/n] y
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]c-l10n all 2.41-6 [738 kB]
└─$ 2 http://kali.download/kali kali-rolling/main amd64 locales-all amd64 2.41-6 [11.1 MB]
Get:3 http://kali.download/kali kali-rolling/main amd64 libc6 amd64 2.41-6 [2,843 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]c-bin amd64 2.41-6 [635 kB]
└─$ 6 http://mirror.kku.ac.th/kali kali-rolling/main amd64 libgcc-s1 amd64 14.2.0-19 [72.8 kB]
Get:5 http://kali.download/kali kali-rolling/main amd64 gcc-14-base amd64 14.2.0-19 [49.4 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]stdc++6 amd64 14.2.0-19 [714 kB]
└─$ 8 http://kali.download/kali kali-rolling/main amd64 manpages all 6.9.1-1 [1,393 kB]
Get:11 http://kali.download/kali kali-rolling/main amd64 libbinutils amd64 2.44-3 [534 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]bgprofng0 amd64 2.44-3 [808 kB]
└─$ 13 http://kali.download/kali kali-rolling/main amd64 libctf-nobfd0 amd64 2.44-3 [156 kB]
Get:15 http://kali.download/kali kali-rolling/main amd64 binutils-x86-64-linux-gnu amd64 2.44-3 [1,014 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]nutils amd64 2.44-3 [265 kB]
└─$ 17 http://kali.download/kali kali-rolling/main amd64 libisl23 amd64 0.27-1 [659 kB]
Get:28 http://mirror.kku.ac.th/kali kali-rolling/main amd64 libasan8 amd64 14.2.0-19 [2,725 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]bmpfr6 amd64 4.2.2-1 [729 kB]
└─$ 19 http://http.kali.org/kali kali-rolling/main amd64 libmpc3 amd64 1.3.1-1+b3 [52.2 kB]
Get:20 http://kali.download/kali kali-rolling/main amd64 cpp-14-x86-64-linux-gnu amd64 14.2.0-19 [11.0 MB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1] amd64 libsframe1 amd64 2.44-3 [78.4 kB]
└─$ 29 http://mirror.kku.ac.th/kali kali-rolling/main amd64 liblsan0 amd64 14.2.0-19 [1,204 kB]
Get:30 http://mirror.primelink.net.id/kali kali-rolling/main amd64 libtsan2 amd64 14.2.0-19 [2,460 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1] golang-1.24-go amd64 1.24.2-2 [28.6 MB]
└─$ 14 http://mirror.sg.gs/kali kali-rolling/main amd64 libctf0 amd64 2.44-3 [88.6 kB]
Get:22 http://kali.download/kali kali-rolling/main amd64 cpp-x86-64-linux-gnu amd64 4:14.2.0-1 [4,840 B]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]p amd64 4:14.2.0-1 [1,568 B]
└─$ 24 http://kali.download/kali kali-rolling/main amd64 libcc1-0 amd64 14.2.0-19 [42.8 kB]
Get:25 http://kali.download/kali kali-rolling/main amd64 libgomp1 amd64 14.2.0-19 [137 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]bitm1 amd64 14.2.0-19 [26.0 kB]
└─$ 27 http://kali.download/kali kali-rolling/main amd64 libatomic1 amd64 14.2.0-19 [9,308 B]
Get:32 http://kali.download/kali kali-rolling/main amd64 libhwasan0 amd64 14.2.0-19 [1,488 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]64 cpp-14 amd64 14.2.0-19 [1,280 B]
└─$ 49 http://mirror.primelink.net.id/kali kali-rolling/main amd64 golang-1.24-src all 1.24.2-2 [21.2 MB]
Get:42 http://mirror.sg.gs/kali kali-rolling/main amd64 rpcsvc-proto amd64 1.4.3-1 [63.3 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]c6-dev amd64 2.41-6 [1,988 kB]
└─$ 34 http://kali.download/kali kali-rolling/main amd64 libgcc-14-dev amd64 14.2.0-19 [2,672 kB]
Get:10 http://mirror.aktkn.sg/kali kali-rolling/main amd64 binutils-common amd64 2.44-3 [2,509 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]conf amd64 1.8.1-4 [26.2 kB]
└─$ 35 http://kali.download/kali kali-rolling/main amd64 gcc-14-x86-64-linux-gnu amd64 14.2.0-19 [21.4 MB]
Get:38 http://xsrv.moratelindo.io/kali kali-rolling/main amd64 gcc amd64 4:14.2.0-1 [5,136 B]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]libubsan1 amd64 14.2.0-19 [1,074 kB]
└─$ 33 http://mirror.aktkn.sg/kali kali-rolling/main amd64 libquadmath0 amd64 14.2.0-19 [145 kB]
Get:36 http://kali.download/kali kali-rolling/main amd64 gcc-14 amd64 14.2.0-19 [540 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]c-x86-64-linux-gnu amd64 4:14.2.0-1 [1,436 B]
└─$ 39 http://kali.download/kali kali-rolling/main amd64 libc-dev-bin amd64 2.41-6 [57.0 kB]
Get:40 http://kali.download/kali kali-rolling/main amd64 linux-libc-dev all 6.12.25-1kali1 [2,571 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]bcrypt-dev amd64 1:4.4.38-1 [119 kB]
└─$ 44 http://http.kali.org/kali kali-rolling/main amd64 libstdc++-14-dev amd64 14.2.0-19 [2,376 kB]
Get:45 http://http.kali.org/kali kali-rolling/main amd64 g++-14-x86-64-linux-gnu amd64 14.2.0-19 [12.1 MB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]+-14 amd64 14.2.0-19 [22.5 kB]
└─$ 47 http://http.kali.org/kali kali-rolling/main amd64 g++-x86-64-linux-gnu amd64 4:14.2.0-1 [1,200 B]
Get:48 http://http.kali.org/kali kali-rolling/main amd64 g++ amd64 4:14.2.0-1 [1,344 B]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]lang-src all 2:1.24~2 [5,136 B]
└─$ 52 http://http.kali.org/kali kali-rolling/main amd64 golang-go amd64 2:1.24~2 [44.3 kB]
Get:53 http://kali.download/kali kali-rolling/main amd64 libpkgconf3 amd64 1.8.1-4 [36.4 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]npages-dev all 6.9.1-1 [2,122 kB]
└─$ 55 http://kali.download/kali kali-rolling/main amd64 pkgconf-bin amd64 1.8.1-4 [30.2 kB]
Fetched 141 MB in 26s (5,324 kB/s)
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ onfiguring packages ...
(Reading database ... 60314 files and directories currently installed.)
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cking libc-l10n (2.41-6) over (2.40-3) ...
Preparing to unpack .../locales-all_2.41-6_amd64.deb ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../libc6_2.41-6_amd64.deb ...
Checking for services that may need to be restarted...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cking libc6:amd64 (2.41-6) over (2.40-3) ...
Setting up libc6:amd64 (2.41-6) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ king init scripts...

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ on: restarting...done.

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ding database ... 60314 files and directories currently installed.)
Preparing to unpack .../libc-bin_2.41-6_amd64.deb ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up libc-bin (2.41-6) ...
(Reading database ... 60314 files and directories currently installed.)
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cking gcc-14-base:amd64 (14.2.0-19) over (14.2.0-16) ...
Setting up gcc-14-base:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1] installed.)
└─$ aring to unpack .../libgcc-s1_14.2.0-19_amd64.deb ...
Unpacking libgcc-s1:amd64 (14.2.0-19) over (14.2.0-16) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ding database ... 60314 files and directories currently installed.)
Preparing to unpack .../libstdc++6_14.2.0-19_amd64.deb ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up libstdc++6:amd64 (14.2.0-19) ...
Selecting previously unselected package manpages.
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1] installed.)
└─$ aring to unpack .../00-manpages_6.9.1-1_all.deb ...
Unpacking manpages (6.9.1-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../01-libsframe1_2.44-3_amd64.deb ...
Unpacking libsframe1:amd64 (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]64.
└─$ aring to unpack .../02-binutils-common_2.44-3_amd64.deb ...
Unpacking binutils-common:amd64 (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../03-libbinutils_2.44-3_amd64.deb ...
Unpacking libbinutils:amd64 (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../04-libgprofng0_2.44-3_amd64.deb ...
Unpacking libgprofng0:amd64 (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1].
└─$ aring to unpack .../05-libctf-nobfd0_2.44-3_amd64.deb ...
Unpacking libctf-nobfd0:amd64 (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../06-libctf0_2.44-3_amd64.deb ...
Unpacking libctf0:amd64 (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]ux-gnu.
└─$ aring to unpack .../07-binutils-x86-64-linux-gnu_2.44-3_amd64.deb ...
Unpacking binutils-x86-64-linux-gnu (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../08-binutils_2.44-3_amd64.deb ...
Unpacking binutils (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../09-libisl23_0.27-1_amd64.deb ...
Unpacking libisl23:amd64 (0.27-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../10-libmpfr6_4.2.2-1_amd64.deb ...
Unpacking libmpfr6:amd64 (4.2.2-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../11-libmpc3_1.3.1-1+b3_amd64.deb ...
Unpacking libmpc3:amd64 (1.3.1-1+b3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]-gnu.
└─$ aring to unpack .../12-cpp-14-x86-64-linux-gnu_14.2.0-19_amd64.deb ...
Unpacking cpp-14-x86-64-linux-gnu (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../13-cpp-14_14.2.0-19_amd64.deb ...
Unpacking cpp-14 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]u.
└─$ aring to unpack .../14-cpp-x86-64-linux-gnu_4%3a14.2.0-1_amd64.deb ...
Unpacking cpp-x86-64-linux-gnu (4:14.2.0-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../15-cpp_4%3a14.2.0-1_amd64.deb ...
Unpacking cpp (4:14.2.0-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../16-libcc1-0_14.2.0-19_amd64.deb ...
Unpacking libcc1-0:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../17-libgomp1_14.2.0-19_amd64.deb ...
Unpacking libgomp1:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../18-libitm1_14.2.0-19_amd64.deb ...
Unpacking libitm1:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../19-libatomic1_14.2.0-19_amd64.deb ...
Unpacking libatomic1:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../20-libasan8_14.2.0-19_amd64.deb ...
Unpacking libasan8:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../21-liblsan0_14.2.0-19_amd64.deb ...
Unpacking liblsan0:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../22-libtsan2_14.2.0-19_amd64.deb ...
Unpacking libtsan2:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../23-libubsan1_14.2.0-19_amd64.deb ...
Unpacking libubsan1:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../24-libhwasan0_14.2.0-19_amd64.deb ...
Unpacking libhwasan0:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../25-libquadmath0_14.2.0-19_amd64.deb ...
Unpacking libquadmath0:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1].
└─$ aring to unpack .../26-libgcc-14-dev_14.2.0-19_amd64.deb ...
Unpacking libgcc-14-dev:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]-gnu.
└─$ aring to unpack .../27-gcc-14-x86-64-linux-gnu_14.2.0-19_amd64.deb ...
Unpacking gcc-14-x86-64-linux-gnu (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../28-gcc-14_14.2.0-19_amd64.deb ...
Unpacking gcc-14 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]u.
└─$ aring to unpack .../29-gcc-x86-64-linux-gnu_4%3a14.2.0-1_amd64.deb ...
Unpacking gcc-x86-64-linux-gnu (4:14.2.0-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../30-gcc_4%3a14.2.0-1_amd64.deb ...
Unpacking gcc (4:14.2.0-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../31-libc-dev-bin_2.41-6_amd64.deb ...
Unpacking libc-dev-bin (2.41-6) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../32-linux-libc-dev_6.12.25-1kali1_all.deb ...
Unpacking linux-libc-dev (6.12.25-1kali1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../33-libcrypt-dev_1%3a4.4.38-1_amd64.deb ...
Unpacking libcrypt-dev:amd64 (1:4.4.38-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../34-rpcsvc-proto_1.4.3-1_amd64.deb ...
Unpacking rpcsvc-proto (1.4.3-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../35-libc6-dev_2.41-6_amd64.deb ...
Unpacking libc6-dev:amd64 (2.41-6) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]d64.
└─$ aring to unpack .../36-libstdc++-14-dev_14.2.0-19_amd64.deb ...
Unpacking libstdc++-14-dev:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]-gnu.
└─$ aring to unpack .../37-g++-14-x86-64-linux-gnu_14.2.0-19_amd64.deb ...
Unpacking g++-14-x86-64-linux-gnu (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../38-g++-14_14.2.0-19_amd64.deb ...
Unpacking g++-14 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]u.
└─$ aring to unpack .../39-g++-x86-64-linux-gnu_4%3a14.2.0-1_amd64.deb ...
Unpacking g++-x86-64-linux-gnu (4:14.2.0-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../40-g++_4%3a14.2.0-1_amd64.deb ...
Unpacking g++ (4:14.2.0-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../41-golang-1.24-src_1.24.2-2_all.deb ...
Unpacking golang-1.24-src (1.24.2-2) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../42-golang-1.24-go_1.24.2-2_amd64.deb ...
Unpacking golang-1.24-go (1.24.2-2) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../43-golang-src_2%3a1.24~2_all.deb ...
Unpacking golang-src (2:1.24~2) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../44-golang-go_2%3a1.24~2_amd64.deb ...
Unpacking golang-go:amd64 (2:1.24~2) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../45-libpkgconf3_1.8.1-4_amd64.deb ...
Unpacking libpkgconf3:amd64 (1.8.1-4) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../46-manpages-dev_6.9.1-1_all.deb ...
Unpacking manpages-dev (6.9.1-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../47-pkgconf-bin_1.8.1-4_amd64.deb ...
Unpacking pkgconf-bin (1.8.1-4) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../48-pkgconf_1.8.1-4_amd64.deb ...
Unpacking pkgconf:amd64 (1.8.1-4) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up libc-l10n (2.41-6) ...
Setting up manpages (6.9.1-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up linux-libc-dev (6.12.25-1kali1) ...
Setting up libctf-nobfd0:amd64 (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up locales-all (2.41-6) ...
Setting up libsframe1:amd64 (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up rpcsvc-proto (1.4.3-1) ...
Setting up libmpfr6:amd64 (4.2.2-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up libmpc3:amd64 (1.3.1-1+b3) ...
Setting up libatomic1:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up pkgconf-bin (1.8.1-4) ...
Setting up libubsan1:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up libcrypt-dev:amd64 (1:4.4.38-1) ...
Setting up libasan8:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up libbinutils:amd64 (2.44-3) ...
Setting up libisl23:amd64 (0.27-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up golang-src (2:1.24~2) ...
Setting up libcc1-0:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up libitm1:amd64 (14.2.0-19) ...
Setting up libctf0:amd64 (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up golang-go:amd64 (2:1.24~2) ...
Setting up pkgconf:amd64 (1.8.1-4) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up cpp-14-x86-64-linux-gnu (14.2.0-19) ...
Setting up cpp-14 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up libgcc-14-dev:amd64 (14.2.0-19) ...
Setting up libstdc++-14-dev:amd64 (14.2.0-19) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up cpp-x86-64-linux-gnu (4:14.2.0-1) ...
Setting up binutils (2.44-3) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up gcc-14-x86-64-linux-gnu (14.2.0-19) ...
Setting up gcc-x86-64-linux-gnu (4:14.2.0-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up g++-14-x86-64-linux-gnu (14.2.0-19) ...
Setting up g++-x86-64-linux-gnu (4:14.2.0-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up gcc (4:14.2.0-1) ...
Setting up g++ (4:14.2.0-1) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]/c++ (c++) in auto mode
└─$ essing triggers for systemd (257.2-3) ...
Processing triggers for libc-bin (2.41-6) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ wget https://golang.org/dl/go1.18.10.linux-amd64.tar.gz
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]nux-amd64.tar.gz
└─$ lving golang.org (golang.org)... 142.251.221.145, 2404:6800:4007:83b::2011
Connecting to golang.org (golang.org)|142.251.221.145|:443... connected.
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]ly
└─$ tion: https://go.dev/dl/go1.18.10.linux-amd64.tar.gz [following]
--2025-06-24 05:36:30--  https://go.dev/dl/go1.18.10.linux-amd64.tar.gz
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]216.239.34.21, ...
└─$ ecting to go.dev (go.dev)|216.239.38.21|:443... connected.
HTTP request sent, awaiting response... 302 Found
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]r.gz [following]
└─$ 25-06-24 05:36:31--  https://dl.google.com/go/go1.18.10.linux-amd64.tar.gz
Resolving dl.google.com (dl.google.com)... 142.250.77.110, 2404:6800:4007:834::200e
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]:443... connected.
└─$  request sent, awaiting response... 200 OK
Length: 141977100 (135M) [application/x-gzip]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
go1.18.10.linux-amd64.tar.gz              100%[=====================================================================================>] 135.40M  7.99MB/s    in 18s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ -06-24 05:36:51 (7.43 MB/s) - ‘go1.18.10.linux-amd64.tar.gz’ saved [141977100/141977100]

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ export PATH=$PATH:/usr/local/go/bin
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ go version
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ creating new go.mod: module service_1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker-compose build service_1
WARN[0001] /home/legil/docker-Assignment/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]nce.
└─$ do so, set COMPOSE_BAKE=true.
[+] Building 105.5s (9/10)                                                                                                                               docker:default
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]ile                                                                                                     0.8s
└─$ => transferring dockerfile: 410B                                                                                                                               0.2s
 => [service_1 internal] load metadata for docker.io/library/golang:1.18-alpine                                                                                    1.3s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.8s
└─$ => transferring context: 2B                                                                                                                                    0.0s
 => [service_1 1/5] FROM docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                              7.2s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                                        7.1s
└─$ [service_1 internal] load build context                                                                                                                       56.1s
 => => transferring context: 142.01MB                                                                                                                             55.4s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]-1.docker.io                                                                                            0.0s
└─$ CACHED [service_1 2/5] WORKDIR /app                                                                                                                            0.1s
 => [service_1 3/5] COPY . .                                                                                                                                      27.6s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        6.5s
└─$ --
 > [service_1 4/5] RUN go mod tidy:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ 6 /app/go.mod:3: invalid go version '1.24.2': must match format 1.23
------
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]omplete successfully: exit code: 1
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ erfile  go1.18.10.linux-amd64.tar.gz  go.mod

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cat go.mod
module service_1
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ .24.2

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ls
Dockerfile  go1.18.10.linux-amd64.tar.gz  go.mod
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cat D
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ e the official Golang image
FROM golang:1.18-alpine
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ t the current working directory inside the container
WORKDIR /app
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ py the Go application code into the container
COPY . .
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ stall Go dependencies
RUN go mod tidy
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ pose the application port
EXPOSE 8081
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ild the Go application
RUN go build -o service1 .
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ n the application
CMD ["./service1"]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ le service_1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ [0000] /home/legil/docker-Assignment/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
Compose can now delegate builds to bake for better performance.
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ Building 35.4s (8/9)                                                                                                                                 docker:default
 => [service_1 internal] load build definition from Dockerfile                                                                                                     0.7s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.1s
└─$ [service_1 internal] load metadata for docker.io/library/golang:1.18-alpine                                                                                    1.5s
 => [service_1 internal] load .dockerignore                                                                                                                        0.4s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.0s
└─$ [service_1 1/5] FROM docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                              0.9s
 => => resolve docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                                        0.9s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.5s
└─$ => transferring context: 107B                                                                                                                                  0.1s
 => CACHED [service_1 2/5] WORKDIR /app                                                                                                                            0.2s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                       17.9s
└─$ ERROR [service_1 4/5] RUN go mod tidy                                                                                                                         10.2s
------
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ 8 go: errors parsing go.mod:
6.848 /app/go.mod:3: invalid go version '1.24.2': must match format 1.23
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ed to solve: process "/bin/sh -c go mod tidy" did not complete successfully: exit code: 1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker-compose build service_1
WARN[0000] /home/legil/docker-Assignment/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]nce.
└─$ do so, set COMPOSE_BAKE=true.
[+] Building 4.9s (5/9)                                                                                                                                  docker:default
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]ile                                                                                                     0.6s
└─$ => transferring dockerfile: 410B                                                                                                                               0.1s
 => [service_1 internal] load metadata for docker.io/library/golang:1.18-alpine                                                                                    1.3s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.6s
└─$ => transferring context: 2B                                                                                                                                    0.1s
 => [service_1 1/5] FROM docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                              0.8s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                                        0.6s
└─$ [service_1 internal] load build context                                                                                                                        0.7s
 => => transferring context: 107B                                                                                                                                  0.0s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ le service_1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
module: command not found
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ go 1.18
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ 'go help' for usage.

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ FROM golang:1.18-alpine
FROM: command not found
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker system prune -a
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ all stopped containers
  - all networks not used by at least one container
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1] them
└─$ all build cache

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ted Images:
untagged: docker-assignment-nginx:latest
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]2452c19a60e414228ba6f
└─$ ted: sha256:54a74eb8bacd1c575f729583764bd2dad5aae397576152ec2ce77f8195001080
deleted: sha256:4d2e3190efd29ef71dd745681a9d5f8bc102b252cda0c4af4766e19b97339e4a
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]7e2fa3617d6c17371d7f3
└─$ ted: sha256:6eb2d031bf96b5c629560f2e1578efce5be3fbe492fcab8a9b7fd8cf2944e90d
deleted: sha256:29c83a190be3467d2b68d77ae3f2b71af1cd5e9e648be183e8514338abbd5b87
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ gged: hello-world:latest
deleted: sha256:940c619fbd418f9b2b1b63e25d8861f9cc1b46e3fc8b018ccfe8b78f19b8cc4f
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]a6b57c02d1ef85efcc25c
└─$ ted: sha256:74cc54e27dc41bb10dc4b2226072d469509f2f22f1a3ce74f4a59661a1d44602
deleted: sha256:e6590344b1a5dc518829d6ea1524fc12f8bcd14ee9a02aa6ad8360cce3a9a9e9
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
Deleted build cache objects:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ 7cdeerrkx1zhaz4jq3etx
y5j6ts64utlme6rfax3fbdep0
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ snhfuzj22prin2iv5lmy7
fbx5g55gxytd8rd55h5qgijm2
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ v3ew94toc9a0u2ds7plv4
lktwb55yn864cgc4wztj0vj9w
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ uwrgyikskekc9z4ycgy79
wxyw3879z0uxxd9mkx5rlpbxv
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ taflns4aisuc8ls7e99sc
rrkemqi8fjjsq5sccrj5e6yjy
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ oqptstt8g39uws6tk21kg
qh4d3wd632fvr20h7lfxrzbn4
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ f7zsij982y1vth1z22ppr
pef2uoma6zrj6p5e633l600r2
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ fig5l1vm85yhb3ffrqdew
9c2x5h68awtxpb7pyr4ld6dhm
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ xc2llnx7u6o3yvgm4rpzm
e9y2i0f6f1meing2v0jxpmcm0
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ 7eo1xqym4zfnri88l21gi
kr3q34per7hflm3mcmn8fi142
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ab1nr2mqznfjbb5uzu7bp
9ba6osjpn2j8t7gdu8rra2z0i
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ l reclaimed space: 694.4MB

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker-compose run service_1 /bin/sh
WARN[0001] /home/legil/docker-Assignment/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ etwork docker-assignment_backend  Created                                                                                                                      1.0s
Compose can now delegate builds to bake for better performance.
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ Building 256.6s (9/10)                                                                                                                               docker:default
 => [service_1 internal] load build definition from Dockerfile                                                                                                     1.2s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.1s
└─$ [service_1 internal] load metadata for docker.io/library/golang:1.18-alpine                                                                                   11.4s
 => [service_1 auth] library/golang:pull token for registry-1.docker.io                                                                                            0.0s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        1.1s
└─$ => transferring context: 2B                                                                                                                                    0.1s
 => [service_1 1/5] FROM docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                            176.9s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                                        0.7s
└─$ => sha256:8921db27df2831fa6eaa85321205a2470c669b855f3ec95d5a3c2b46de0442c9 3.37MB / 3.37MB                                                                     4.2s
 => => sha256:dbc2308a458705184f3d2a5000ced1c12609903c00c09a9cf01284764b57315b 155B / 155B                                                                         2.4s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]929d22467df250536bb 115.40MB / 115.40MB                                                                57.1s
└─$ => sha256:a2f8637abd914a8a62416e027a351293d0472bc4b4f44383c6f425fd0e03861c 284.81kB / 284.81kB                                                                 3.3s
 => => extracting sha256:8921db27df2831fa6eaa85321205a2470c669b855f3ec95d5a3c2b46de0442c9                                                                          6.2s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]472bc4b4f44383c6f425fd0e03861c                                                                          3.3s
└─$ => extracting sha256:4ba80a8cd2c7b1695ffb2166c58c8d0f0d4562c943fdb929d22467df250536bb                                                                        106.5s
 => => extracting sha256:dbc2308a458705184f3d2a5000ced1c12609903c00c09a9cf01284764b57315b                                                                          6.6s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                       71.0s
└─$ => transferring context: 142.01MB                                                                                                                             63.8s
 => [service_1 2/5] WORKDIR /app                                                                                                                                  17.8s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                       35.1s
└─$ ERROR [service_1 4/5] RUN go mod tidy                                                                                                                          7.4s
------
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ 1 go: errors parsing go.mod:
6.171 /app/go.mod:3: invalid go version '1.24.2': must match format 1.23
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ed to solve: process "/bin/sh -c go mod tidy" did not complete successfully: exit code: 1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ go version
d tidy
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ warning: "all" matched no packages

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ go --version
flag provided but not defined: -version
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
Usage:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     go <command> [arguments]

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
        bug         start a bug report
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     clean       remove object files and cached files
        doc         show documentation for package or symbol
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     fix         update packages to use new APIs
        fmt         gofmt (reformat) package sources
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     get         add dependencies to current module and install them
        install     compile and install packages and dependencies
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     mod         module maintenance
        work        workspace maintenance
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     telemetry   manage telemetry data and settings
        test        test packages
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     version     print Go version
        vet         report likely mistakes in packages
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ "go help <command>" for more information about a command.

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
        buildconstraint build constraints
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     buildmode       build modes
        c               calling between Go and C
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     environment     environment variables
        filetype        file types
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     go.mod          the go.mod file
        gopath          GOPATH environment variable
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     importpath      import path syntax
        modules         modules, module versions, and more
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     packages        package lists and patterns
        private         configuration for downloading non-public code
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     testfunc        testing functions
        vcs             controlling version control with GOVCS
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ "go help <topic>" for more information about that topic.

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ mod --version
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$  apt install monodoc-base

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ tidy --version
Command 'tidy' not found, but can be installed with:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ o] password for legil:
Installing:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
Installing dependencies:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
Summary:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]166
└─$ wnload size: 252 kB
  Space needed: 1,198 kB / 1,022 GB available
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ inue? [Y/n] y
Get:1 http://kali.download/kali kali-rolling/main amd64 libtidy58 amd64 2:5.8.0-2 [222 kB]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]y amd64 2:5.8.0-2 [29.7 kB]
└─$ hed 252 kB in 2s (121 kB/s)
Selecting previously unselected package libtidy58:amd64.
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1] installed.)
└─$ aring to unpack .../libtidy58_2%3a5.8.0-2_amd64.deb ...
Unpacking libtidy58:amd64 (2:5.8.0-2) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ aring to unpack .../tidy_2%3a5.8.0-2_amd64.deb ...
Unpacking tidy (2:5.8.0-2) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ing up tidy (2:5.8.0-2) ...
Processing triggers for libc-bin (2.41-6) ...
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker images
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$  provided but not defined: -version
Go is a tool for managing Go source code.
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ e:

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
The commands are:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     bug         start a bug report
        build       compile packages and dependencies
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     doc         show documentation for package or symbol
        env         print Go environment information
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     fmt         gofmt (reformat) package sources
        generate    generate Go files by processing source
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]install them
└─$     install     compile and install packages and dependencies
        list        list packages or modules
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     work        workspace maintenance
        run         compile and run Go program
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     test        test packages
        tool        run specified go tool
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     vet         report likely mistakes in packages

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]d.
└─$
Additional help topics:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     buildconstraint build constraints
        buildjson       build -json encoding
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     c               calling between Go and C
        cache           build and test caching
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     filetype        file types
        goauth          GOAUTH environment variable
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     gopath          GOPATH environment variable
        goproxy         module proxy protocol
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     modules         modules, module versions, and more
        module-auth     module authentication using go.sum
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     private         configuration for downloading non-public code
        testflag        testing flags
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$     vcs             controlling version control with GOVCS

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1].
└─$

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ golng --version
golng: command not found
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ FROM golang:1.18-alpine
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ le service_1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ [0000] /home/legil/docker-Assignment/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
Compose can now delegate builds to bake for better performance.
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ Building 53.2s (9/10)                                                                                                                                docker:default
 => [service_1 internal] load build definition from Dockerfile                                                                                                     0.6s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.1s
└─$ [service_1 internal] load metadata for docker.io/library/golang:1.18-alpine                                                                                    4.9s
 => [service_1 auth] library/golang:pull token for registry-1.docker.io                                                                                            0.0s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.5s
└─$ => transferring context: 2B                                                                                                                                    0.0s
 => [service_1 1/5] FROM docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                              0.9s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                                        0.8s
└─$ [service_1 internal] load build context                                                                                                                        0.4s
 => => transferring context: 107B                                                                                                                                  0.0s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.1s
└─$ [service_1 3/5] COPY . .                                                                                                                                      26.8s
 => ERROR [service_1 4/5] RUN go mod tidy                                                                                                                         15.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ service_1 4/5] RUN go mod tidy:
13.71 go: errors parsing go.mod:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]h format 1.23
└─$ --
failed to solve: process "/bin/sh -c go mod tidy" did not complete successfully: exit code: 1
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
[+] Building 84.0s (9/9) FINISHED                                                                                                                        docker:default
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]ile                                                                                                     0.4s
└─$ => transferring dockerfile: 410B                                                                                                                               0.0s
 => [service_1 internal] load metadata for docker.io/library/golang:1.18-alpine                                                                                    4.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.3s
└─$ => transferring context: 2B                                                                                                                                    0.0s
 => [service_1 1/5] FROM docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                              0.7s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                                        0.6s
└─$ [service_1 internal] load build context                                                                                                                        0.4s
 => => transferring context: 144B                                                                                                                                  0.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.0s
└─$ [service_1 3/5] COPY . .                                                                                                                                      48.1s
 => [service_1 4/5] RUN go mod tidy                                                                                                                               19.7s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        6.7s
└─$ --
 > [service_1 5/5] RUN go build -o service1 .:
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ --
failed to solve: process "/bin/sh -c go build -o service1 ." did not complete successfully: exit code: 1
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ls *.go
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ erfile  go1.18.10.linux-amd64.tar.gz  go.mod

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ls *.go.mod
ls: cannot access '*.go.mod': No such file or directory
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ touch main.go
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ nano main.go
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ nano Dockerfile
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker-compose build --no-cache service_1
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
└─$ ose can now delegate builds to bake for better performance.
 To do so, set COMPOSE_BAKE=true.
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                              docker:default
└─$ [service_1 internal] load build definition from Dockerfile                                                                                                     0.5s
 => => transferring dockerfile: 516B                                                                                                                               0.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]y/golang:1.18-alpine                                                                                    6.6s
└─$ [service_1 auth] library/golang:pull token for registry-1.docker.io                                                                                            0.0s
 => [service_1 internal] load .dockerignore                                                                                                                        0.4s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.0s
└─$ [service_1 1/5] FROM docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                              0.7s
 => => resolve docker.io/library/golang:1.18-alpine@sha256:77f25981bd57e60a510165f3be89c901aec90453fd0f1c5a45691f6cb1528807                                        0.6s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.4s
└─$ => transferring context: 705B                                                                                                                                  0.0s
 => CACHED [service_1 2/5] WORKDIR /app                                                                                                                            0.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                       25.2s
└─$ [service_1 4/5] RUN go mod tidy                                                                                                                                7.9s
 => [service_1 5/5] RUN go build -o service1 .                                                                                                                     7.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                      100.6s
└─$ => exporting layers                                                                                                                                           69.8s
 => => exporting manifest sha256:c8cfb637675f22a0c91ac9bb5f82af0ec91d1719588025a8a1b2a63debad6188                                                                  3.7s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]e271c56342d3d82bf16c50947292fbc79fd7                                                                    2.4s
└─$ => exporting attestation manifest sha256:eddc95e7b7f3145b4683467f2321871f6d5e2587326fde0a9b0c7cfabfc89c58                                                      1.2s
 => => exporting manifest list sha256:a3c540c857660df6fbc05ca0046898662a5129b0bf5dfe23826b5463d008de40                                                             1.5s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]e_1:latest                                                                                              0.2s
└─$ => unpacking to docker.io/library/docker-assignment-service_1:latest                                                                                          19.7s
 => [service_1] resolving provenance for metadata file                                                                                                             3.6s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ervice_1  Built                                                                                                                                                0.0s

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]nutes ago   776MB
└─$
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ AINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ docker-compose up
WARN[0000] /home/legil/docker-Assignment/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]nce.
└─$ do so, set COMPOSE_BAKE=true.
[+] Building 143.9s (11/16)                                                                                                                              docker:default
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        2.4s
└─$ => transferring dockerfile: 227B                                                                                                                               0.1s
 => [service_2 internal] load build definition from Dockerfile                                                                                                     2.4s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.0s
└─$ [nginx internal] load metadata for docker.io/library/nginx:latest                                                                                             12.7s
 => [service_2 internal] load metadata for docker.io/library/python:3.9-alpine                                                                                    11.4s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]cker.io                                                                                                 0.0s
└─$ [service_2 auth] library/python:pull token for registry-1.docker.io                                                                                            0.0s
 => [service_2 internal] load .dockerignore                                                                                                                        1.9s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        0.0s
└─$ [nginx internal] load .dockerignore                                                                                                                            2.0s
 => => transferring context: 2B                                                                                                                                    0.0s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]e@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                             116.4s
└─$ => resolve docker.io/library/python:3.9-alpine@sha256:535a2eb2c2769d1ffe8188051b341f6b40c1afc6dc11b1b938e3c9031ca72139                                         1.0s
 => => sha256:1155a024ecd595abfc1d6d517f2a4339b62ca11f392ac0fe27faebf5fe66b987 249B / 249B                                                                         1.7s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]3b093193aa7079a41c2 460.22kB / 460.22kB                                                                 2.4s
└─$ => sha256:0c75cc4c879f8b1be0ef5e7ac1a4efe9caf769575cc4875f2d45578723ae189c 14.88MB / 14.88MB                                                                 114.5s
 => => sha256:fe07684b16b82247c3539ed86a65ff37a76138ec25d380bd80c869a1a4c73236 3.80MB / 3.80MB                                                                     4.5s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]6138ec25d380bd80c869a1a4c73236                                                                         13.3s
└─$ => extracting sha256:7de06ff945306f4194448c4c54ada65369102e1cb92db3b093193aa7079a41c2                                                                         14.3s
 => => extracting sha256:0c75cc4c879f8b1be0ef5e7ac1a4efe9caf769575cc4875f2d45578723ae189c                                                                         65.2s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]2ca11f392ac0fe27faebf5fe66b987                                                                          4.0s
└─$ [service_2 internal] load build context                                                                                                                        5.1s
 => => transferring context: 324B                                                                                                                                  0.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                        6.7s
└─$ => transferring context: 677B                                                                                                                                  0.1s
 => [nginx 1/2] FROM docker.io/library/nginx:latest@sha256:6784fb0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d                                      126.1s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]0834aa7dbbe12e3d7471e69c290df3e6ba810dc38b34ae33d3c1c05f7d                                              2.1s
└─$ => sha256:32e44235e1d55a34be577baa12271f067c0bf79ba4e1ca4e6ec484424540132a 1.40kB / 1.40kB                                                                     9.2s
 => => sha256:d2a7ba8dbfee4f222961ad1449648ab46a6cea044e4cfb374450325ecee8f67f 1.21kB / 1.21kB                                                                     9.7s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]0cc6a3840a0952f3d57 405B / 405B                                                                         9.4s
└─$ => sha256:1bc5dc8b475d9de62c94cf69a139df17aaf7fe7bac76b5133d8048a058fb3c3b 956B / 956B                                                                         3.3s
 => => sha256:3b00567da96412a0298328fad6b96aadd3bc9ca97e6683534da152b0b9e8a977 44.15MB / 44.15MB                                                                  55.8s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]fb26d9fbcbac4dbc4f6 628B / 628B                                                                         3.7s
└─$ => sha256:dad67da3f26bce15939543965e09c4059533b025f707aad72ed3d3f3a09c66f8 28.23MB / 28.23MB                                                                  46.3s
 => => extracting sha256:dad67da3f26bce15939543965e09c4059533b025f707aad72ed3d3f3a09c66f8                                                                         44.6s
┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]                                                                                                       10.6s
└─$

DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
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

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ cat docker-compose.yml
version: '3.8'

services:
  service_1:
    build: ./service_1
    ports:
      - "8081:8081"
    networks:
      - app-network

  nginx:
    image: nginx:latest
    ports:
      - "80:80"
    networks:
      - app-network
    depends_on:
      - service_1

networks:
  app-network:
    driver: bridge


┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ cd nginx/

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/nginx]
└─$ ls
Dockerfile  nginx.conf

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/nginx]
└─$ cat Dockerfile
# Use the official Nginx image
FROM nginx:latest

# Copy custom nginx.conf into the container
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 80 for the Nginx reverse proxy
EXPOSE 80

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/nginx]
└─$ cat nginx.conf
server {
    listen 80;

    # Log incoming requests with timestamp and path
    access_log /var/log/nginx/access.log;

    # Routing for service_1
    location /service1 {
        proxy_pass http://service_1:8081;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    # Routing for service_2
    location /service2 {
        proxy_pass http://service_2:8082;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/nginx]
└─$ cd ..

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ cd service_1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ls
Dockerfile  go1.18.10.linux-amd64.tar.gz  go.mod  main.go

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cat Dockerfile
# Use the official Golang image (1.18 version)
FROM golang:1.18-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the Go module files and Go source code to the container
COPY . .

# Install Go dependencies (e.g., using go mod tidy)
RUN go mod tidy

# Expose the application port
EXPOSE 8081

# Build the Go application (compile the Go source code)
RUN go build -o service1 .

# Run the application (start the compiled Go binary)
CMD ["./service1"]


┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cd ..

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ cd service_2

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$ ls
Dockerfile

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$ cat Dockerfile
# Use Python base image
FROM python:3.9-alpine

# Set working directory
WORKDIR /app

# Copy Python application into the container
COPY . .

# Install dependencies
RUN pip install -r requirements.txt

# Expose port
EXPOSE 8082

# Start the Python application
CMD ["python", "app.py"]


┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED          SIZE
docker-assignment-nginx       latest    ed49ca895bd8   24 minutes ago   279MB
docker-assignment-service_1   latest    2daf027152a5   32 minutes ago   776MB
nginx                         latest    6784fb0834aa   2 months ago     279MB

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$ docker ps -
docker: 'docker ps' accepts no arguments

Usage:  docker ps [OPTIONS]

Run 'docker ps --help' for more information

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$ docker ps -a
CONTAINER ID   IMAGE                         COMMAND                  CREATED          STATUS                      PORTS     NAMES
b8d18609d60c   nginx:latest                  "/docker-entrypoint.…"   17 minutes ago   Exited (0) 14 minutes ago             docker-assignment-nginx-1
ced2458eca9f   docker-assignment-service_1   "./service1"             17 minutes ago   Exited (0) 16 minutes ago             docker-assignment-service_1-1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$ cd ..

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ ls
docker-compose.yml  nginx  service_1  service_2

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ cd nginx/

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/nginx]
└─$ ls
Dockerfile  nginx.conf

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/nginx]
└─$ cat Dockerfile
# Use the official Nginx image
FROM nginx:latest

# Copy custom nginx.conf into the container
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 80 for the Nginx reverse proxy
EXPOSE 80

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/nginx]
└─$ cd nginx.conf
-bash: cd: nginx.conf: Not a directory

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/nginx]
└─$ cat nginx.conf
server {
    listen 80;

    # Log incoming requests with timestamp and path
    access_log /var/log/nginx/access.log;

    # Routing for service_1
    location /service1 {
        proxy_pass http://service_1:8081;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    # Routing for service_2
    location /service2 {
        proxy_pass http://service_2:8082;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/nginx]
└─$ cd ..

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ ls
docker-compose.yml  nginx  service_1  service_2

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ cd service_1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ ls
Dockerfile  go1.18.10.linux-amd64.tar.gz  go.mod  main.go

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cat Dockerfile go.mod  main.go
# Use the official Golang image (1.18 version)
FROM golang:1.18-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy the Go module files and Go source code to the container
COPY . .

# Install Go dependencies (e.g., using go mod tidy)
RUN go mod tidy

# Expose the application port
EXPOSE 8081

# Build the Go application (compile the Go source code)
RUN go build -o service1 .

# Run the application (start the compiled Go binary)
CMD ["./service1"]

module service_1

go 1.18

package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}


┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_1]
└─$ cd ..

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ cd service_2

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$ ls
Dockerfile

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$ cat Dockerfile
# Use Python base image
FROM python:3.9-alpine

# Set working directory
WORKDIR /app

# Copy Python application into the container
COPY . .

# Install dependencies
RUN pip install -r requirements.txt

# Expose port
EXPOSE 8082

# Start the Python application
CMD ["python", "app.py"]


┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$ docker ps -a
cd ..
CONTAINER ID   IMAGE                         COMMAND                  CREATED          STATUS                      PORTS     NAMES
b8d18609d60c   nginx:latest                  "/docker-entrypoint.…"   48 minutes ago   Exited (0) 46 minutes ago             docker-assignment-nginx-1
ced2458eca9f   docker-assignment-service_1   "./service1"             49 minutes ago   Exited (0) 48 minutes ago             docker-assignment-service_1-1

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment/service_2]
└─$ cd ..

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ docker inspect --format '{{.State.Health.Status}}' <container_id_or_name>
-bash: syntax error near unexpected token `newline'

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$ docker inspect --format '{{.State.Health.Status}}' ced2458eca9f

template parsing error: template: :1:8: executing "" at <.State.Health.Status>: map has no entry for key "Health"

┌──(legil㉿DESKTOP-42MV8LH)-[~/docker-Assignment]
└─$











































































