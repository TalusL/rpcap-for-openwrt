# rpcap-for-openwrt
wireshark remote server for openwrt ramips
**binaryfile <span style="color:red">rpcapd</span> is provided** 

from:https://blog.csdn.net/rc_ll/article/details/80662658

```shell
export PATH=$PATH:/root/lede/staging_dir/toolchain-mipsel_24kc_gcc-5.4.0_musl-1.1.16/bin
export STAGING_DIR=/root/lede/staging_dir/toolchain-mipsel_24kc_gcc-5.4.0_musl-1.1.16
export CC=mipsel-openwrt-linux-musl-gcc
export CXX=mipsel-openwrt-linux-musl-g++
export AR=mipsel-openwrt-linux-musl-ar
export RANLIB=mipsel-openwrt-linux-musl-ranlib
export ac_cv_linux_vers=4.4.61
```

download source from https://www.winpcap.org/devel.htm 

```shell
unzip source.zip
cd winpcap/wpcap/libpcap
chmod +x configure
./configure --build=x86_64-unknown-linux-gnu --host=mipsel-openwrt-linux --with-pcap=linux
make
```

add code to winpcap/wpcap/libpcap/pcap-int.h , or it will take strlcpy error

```c++
 #include <string.h>
```

```shell
cd rpcapd
```

edit rpcapd/Makefile

modify CC to

```shell
CC=mipsel-openwrt-linux-gcc
```

```powershell
#make rpcapd
make
#check elf file format
file rpcapd
# rpcapd: ELF 32-bit LSB executable, MIPS, MIPS32 rel2 version 1, dynamically linked, interpreter /lib/ld-musl-mipsel-sf.so.1, not stripped
```

copy rpcapd to your OpenWrt device,run cmd

```shell
./rpcapd -n
```

```shell
#use warkshark connect to openwrt 
wireshark -B 1 -k -i rpcap://192.168.1.133:2232/eth0
```
