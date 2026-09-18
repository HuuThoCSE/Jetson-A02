Mình khuyên dùng docker compose, cấu trúc như sau:
```
/home/ubuntu/n8n/
├── docker-compose.yml
└── data/
```

```
sudo apt update
sudo apt install -y docker-compose

sudo usermod -aG docker $USER
newgrp docker
docker ps

exit
exit
(ssh lại vào server)
```

Tạo thư mục:
```
mkdir -p ~/n8n/data
cd ~/n8n
```

```
nano ~/n8n/docker-compose.yml
```

```
version: "3.3"

services:
  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    restart: unless-stopped

    ports:
      - "5678:5678"

    environment:
      - TZ=Asia/Ho_Chi_Minh
      - GENERIC_TIMEZONE=Asia/Ho_Chi_Minh
      - N8N_HOST=0.0.0.0
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - N8N_SECURE_COOKIE=false

    volumes:
      - ./data:/home/node/.n8n
```

```
docker-compose config
docker-compose pull
docker-compose up -d
```

### Nếu pull về lỗi thì xóa toàn bộ cài lại
```
cd ~/n8n

docker-compose down 2>/dev/null
docker image rm n8nio/n8n:latest 2>/dev/null

docker image prune -f

sudo systemctl restart docker
```
