# Thông tin
- GCC/G++ 7.5.0
- CMake 3.10.2
- CUDA 10.2
- JetPack 4.6.1
- RAM 4 GB

# Cài đặt

```
cd ~
curl -fsSL https://kreier.github.io/llama.cpp-jetson.nano/install.sh | bash
source ~/.bashrc

```

Kiểm tra:
```
llama-cli --version
```

Chạy mô hình
```
wget -O Qwen2.5-0.5B-Instruct-Q4_K_M.gguf \
https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct-GGUF/resolve/main/qwen2.5-0.5b-instruct-q4_k_m.gguf
```

`-ngl` = number of GPU layers — số layer của model được đưa sang GPU xử lý.

Chạy với CPU
```
llama-cli \
  -m ~/models/Qwen2.5-0.5B-Instruct-Q4_K_M.gguf \
  -ngl 0 \
  -c 512 \
  -t 4 \
  -n 32 \
  -p "Xin chào, hãy trả lời bằng tiếng Việt."
```


Chạy với GPU
```
llama-cli \
  -m ~/models/Qwen2.5-0.5B-Instruct-Q4_K_M.gguf \
  -ngl 99 \
  -c 512 \
  -t 4 \
  -n 64 \
  -p "Xin chào, hãy trả lời bằng tiếng Việt."
```

Chạy với server
```
llama-server \
  -m ~/models/Qwen2.5-0.5B-Instruct-Q4_K_M.gguf \
  --host 0.0.0.0 \
  --port 8080 \
  -c 512 \
  -t 4 \
  -ngl 99
```

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


```
sudo apt-get update
sudo apt-get install -y build-essential libgmp-dev libmpfr-dev libmpc-dev
```

Tải GCC 8.5:
```
cd ~
wget https://ftp.gnu.org/gnu/gcc/gcc-8.5.0/gcc-8.5.0.tar.gz
tar -xzf gcc-8.5.0.tar.gz
cd gcc-8.5.0
./contrib/download_prerequisites
```

```
cd ~/gcc-8.5.0
rm -rf build-lite
mkdir build-lite
cd build-lite
```

Configure:
```
../configure \
  --enable-languages=c,c++ \
  --disable-multilib \
  --disable-bootstrap \
  --disable-libgomp \
  --disable-libsanitizer \
  --disable-libquadmath

sudo make install
/usr/local/bin/gcc --version
/usr/local/bin/g++ --version
```


```
cd ~/llama.cpp
nano CMakeLists.txt
```

Sửa thành
```
project("llama.cpp" C CXX)

if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU" AND CMAKE_CXX_COMPILER_VERSION VERSION_LESS 9)
    link_libraries(stdc++fs)
endif()

include(CheckIncludeFileCXX)
```

Tạo thư mục build:
```
cd ~/llama.cpp
rm -rf build
mkdir build
cd build
```

```
CC=/usr/local/bin/gcc \
CXX=/usr/local/bin/g++ \
cmake .. \
  -DLLAMA_BUILD_TESTS=OFF \
  -DCMAKE_EXE_LINKER_FLAGS="-lstdc++fs" \
  -DCMAKE_SHARED_LINKER_FLAGS="-lstdc++fs"
```

Sau đó build. Trên Jetson Nano tôi khuyên:
```
make -j2
```

```
sudo make install
```
