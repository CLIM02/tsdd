docker run -d --name tsddweb_86 -e API_URL=https://xiaobobo.acone.icu:8090 -p 86:80  tsddweb:latest

docker run -d --name tsddweb_86 -e API_URL=https://bao.123gov.icu:8090 -p 86:80  tsddweb:latest


docker run -d --name tsddweb_87 -e API_URL=https://hao.123gov.icu:8090 -p 87:80  tsddweb:latest

docker run -d --name tsddweb_88 -e API_URL=https://yun.123gov.icu:8090 -p 88:80  tsddweb:latest

docker run -d --name tsddweb_89 -e API_URL=https://yuan.123gov.icu:8090 -p 89:80  tsddweb:latest

docker run -d --name tsddweb_91 -e API_URL=https://bao.123gov.icu:8090 -p 91:80  tsddweb:latest


docker run -d --name tsddweb_86 -e API_URL=https://kun.123hao.icu/api -p 86:80  tsddweb:latest

先锋模版

services:
  tsddweb_86:
    image: tsddweb:latest
    container_name: ${CONTAINER_NAME}
    restart: unless-stopped
    ports:
      - "${HOST_PORT}:${CONTAINER_PORT}"
    environment:
      - API_URL=${API_BASE_URL}


CONTAINER_NAME=tsddweb_87
API_BASE_URL=https://hi.123back.icu/api
HOST_PORT=87
CONTAINER_PORT=80

反向代理API
![alt text](image.png)

反向代理APP
![alt text](image-1.png)


docker run -d --name tsddweb_86 -e API_URL=https://abcedu.icu/api -p 86:80  tsddweb:latest
