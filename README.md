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
