# Thông tin
GCC/G++ 7.5.0
CMake 3.10.2
CUDA 10.2
JetPack 4.6.1
RAM 4 GB

# Cài đặt

## Bước 01

```
sudo apt update

sudo apt install -y \
  git \
  wget \
  curl \
  libcurl4-openssl-dev \
  libgmp-dev \
  libmpfr-dev \
  libmpc-dev \
  python3-pip
```

Sau đó kiểm tra xem Ubuntu 18.04 của bạn có sẵn GCC 8 không:
```
apt-cache policy gcc-8 g++-8
```
