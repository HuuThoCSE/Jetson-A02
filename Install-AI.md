# Thông tin
- GCC/G++ 7.5.0
- CMake 3.10.2
- CUDA 10.2
- JetPack 4.6.1
- RAM 4 GB

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

sudo apt install gcc-8 g++-8
```

Kiểm tra
```
gcc-8 --version
g++-8 --version
```


```
sudo apt update
sudo apt install -y git cmake build-essential

cd ~
wget https://github.com/Kitware/CMake/releases/download/v3.31.12/cmake-3.31.12-linux-aarch64.tar.gz
tar -xzf cmake-3.31.12-linux-aarch64.tar.gz
sudo mv cmake-3.31.12-linux-aarch64 /opt/cmake
echo 'export PATH=/opt/cmake/bin:$PATH' >> ~/.bashrc
git clone https://github.com/ggerganov/llama.cpp
cd ~/llama.cpp
mkdir -p build
cd build

CC=gcc-8 CXX=g++-8 cmake ..
```

Nếu cuối cùng hiện:
```
-- Configuring done
-- Generating done
```

Tiếp tục
```

make -j2
```

Sau khi build xong:
```
ls ~/llama.cpp/build/bin
```
