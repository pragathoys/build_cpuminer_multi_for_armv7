# How to build the cpuminer-multi for a Raspberry Pi machine with ARMv7 architecture
In the following notes we assume that you want to build the cpuminer-multi software for a Raspberry Pi with ARMv7 architecture.

Basically, ARMv7 and below are 32-bit while ARMv8 introduces the 64-bit instruction set. The latest version of Raspberry Pi 4 comes with an ARMv8 64-bit instruction set.

We will use the repository maintained by tpruvot since it is the most active one in comparison to the original repository of lucasjones.

# Check your machine

Just to make sure you have an armv7 RPi you can run the following command in a shell prompt:

```shell
$ uname -m
```

You should get a response like this:

```shell
root@raspberrypi:~# uname -m
armv7l
root@raspberrypi:~# 
```

Another way to check the architecture of your CPU is the lscpu command:
```shell
root@raspberrypi:~# lscpu 
Architecture:        armv7l
Byte Order:          Little Endian
CPU(s):              4
On-line CPU(s) list: 0-3
Thread(s) per core:  1
Core(s) per socket:  4
Socket(s):           1
Vendor ID:           ARM
Model:               4
Model name:          Cortex-A53
Stepping:            r0p4
CPU max MHz:         1200.0000
CPU min MHz:         600.0000
BogoMIPS:            38.40
Flags:               half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32
```

# Install the required libraries in the system

Before starting the build please make sure you have installed the following libraries (make sure you are loggedin as root otherwise you will have to use sudo infront of each of the commands):

```shell
root@raspberrypi:~#  apt update
root@raspberrypi:~#  apt-get install automake autoconf pkg-config libcurl4-openssl-dev libjansson-dev libssl-dev libgmp-dev zlib1g-dev make g++
```

# Clone the GitHub repository of the software
We will use the repository maintained by tpruvot:

```shell
root@raspberrypi:~# git clone https://github.com/tpruvot/cpuminer-multi -b linux
```

# Prepare and build
```shell
root@raspberrypi:~# cd cpuminer-multi


root@raspberrypi:~/cpuminer-multi# ./autogen.sh
root@raspberrypi:~/cpuminer-multi# ./configure --with-crypto --with-curl --disable-assembly CC=gcc CXX=g++ CFLAGS="-fPIC -Ofast -fuse-linker-plugin -ftree-loop-if-convert-stores -march=armv7" LDFLAGS="-march=native"
```
and if no errors appear you are ready to go:

```shell
root@raspberrypi:~/cpuminer-multi# make
```

# Check miner

If everything gone well when you run the command :

```shell
root@raspberrypi:~/cpuminer-multi# ./cpuminer --help 
```

You should get a similar screen like this:

```shell
** cpuminer-multi 1.3.7 by tpruvot@github **
BTC donation address: 1FhDPLPpw18X4srecguG3MxJYe4a1JsZnd (tpruvot)

Usage: cpuminer-multi [OPTIONS]
Options:
  -a, --algo=ALGO       specify the algorithm to use
                          allium       Garlicoin double lyra2
                          axiom        Shabal-256 MemoHash
                          bitcore      Timetravel with 10 algos
                          blake        Blake-256 14-rounds (SFR)
                          blakecoin    Blake-256 single sha256 merkle
                          blake2b      Blake2-B (512)
                          blake2s      Blake2-S (256)
                          bmw          BMW 256
                          .....
  -V, --version         display version information and exit
  -h, --help            display this help text and exit
```

#  Start mining!

You are ready to use your favorite mining pool (eg multipool.us) and start mining

```shell
root@raspberrypi:~/cpuminer-multi#  ./cpuminer -a scrypt -o stratum+tcp://us.multipool.us:3334 -u <worker_name>  -p <pass>

root@raspberrypi:~/cpuminer-multi# ./cpuminer -a sha256 -o stratum+tcp://us.multipool.us:3332 -u <worker_name>  -p <pass>
```

# Resources

* Most usefull resource is the repository of the cpuminer-multi:  https://github.com/tpruvot/cpuminer-multi
* Original repo: https://github.com/lucasjones/cpuminer-multi

* You can find more details about the parameter 'march' and the AArch64-Options : https://gcc.gnu.org/onlinedocs/gcc-6.1.0/gcc/AArch64-Options.html

* More details about the various architectures for RPi : https://en.wikipedia.org/wiki/Raspberry_Pi
