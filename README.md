### Cài CUPS và công cụ build
```
sudo apt update
sudo apt install -y cups build-essential git autoconf libtool libcups2-dev libcupsimage2-dev
```

Thêm user ubuntu vào nhóm máy in:
```
sudo usermod -aG lpadmin ubuntu
```

### Build driver LBP2900 cho ARM64
```
cd ~
git clone https://github.com/agalakhov/captdriver.git
cd captdriver
autoreconf -i
./configure
make
```

```
sudo cp src/rastertocapt /usr/lib/cups/filter/
sudo mkdir -p /usr/share/ppd/custom
sudo cp Canon-LBP2900.ppd /usr/share/ppd/custom/
sudo chmod 755 /usr/lib/cups/filter/rastertocapt
```

```
sudo lpadmin \
  -p LBP2900 \
  -E \
  -v 'usb://Canon/LBP2900?serial=0000C1E9I1iC' \
  -P /usr/share/ppd/custom/Canon-LBP2900.ppd
```

Sau đó đặt làm mặc định:
```
sudo lpadmin -d LBP2900
```
Kiểm tra:
```
lpstat -p -d
```

Rồi test in:
```
echo "Jetson Nano Canon LBP2900 test" | lp -d LBP2900
``

Nếu không in, xem queue:
```
lpstat -t
```
