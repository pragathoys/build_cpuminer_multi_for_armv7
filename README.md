# How to build the cpuminer-multi for a Raspberry Pi machine with ARMv7 architecture
In the following notes we assume that you want to build the cpuminer-multi software for a Raspberry Pi with ARMv7 architecture in other words for a RPi2 machine.

We will use the repository maintained by tpruvot since it is the most active one in comparison to the original repository of lucasjones.

# Check your machine

Just to make sure you have an armv7 RPi you can run the following command in a shell promts:

```shell
$ uname -m
```

You should a response like this:

```shell
root@raspberrypi:~# uname -m
armv7l
root@raspberrypi:~# 
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
                          c11/flax     C11
                          cryptolight  Cryptonight-light
                          cryptonight  Monero
                          decred       Blake-256 14-rounds 180 bytes
                          dmd-gr       Diamond-Groestl
                          drop         Dropcoin
                          fresh        Fresh
                          geek         GeekCash
                          groestl      GroestlCoin
                          heavy        Heavy
                          jha          JHA
                          keccak       Keccak (Old and deprecated)
                          keccakc      Keccak (CreativeCoin)
                          luffa        Luffa
                          lyra2re      Lyra2RE
                          lyra2rev2    Lyra2REv2
                          lyra2v3      Lyra2REv3 (Vertcoin)
                          myr-gr       Myriad-Groestl
                          neoscrypt    NeoScrypt(128, 2, 1)
                          nist5        Nist5
                          pluck        Pluck:128 (Supcoin)
                          pentablake   Pentablake
                          phi          LUX initial algo
                          phi2         LUX newer algo
                          quark        Quark
                          qubit        Qubit
                          rainforest   RainForest (256)
                          scrypt       scrypt(1024, 1, 1) (default)
                          scrypt:N     scrypt(N, 1, 1)
                          scrypt-jane:N (with N factor from 4 to 30)
                          shavite3     Shavite3
                          sha256d      SHA-256d
                          sia          Blake2-B
                          sib          X11 + gost (SibCoin)
                          skein        Skein+Sha (Skeincoin)
                          skein2       Double Skein (Woodcoin)
                          sonoa        A series of 97 hashes from x17
                          s3           S3
                          timetravel   Timetravel (Machinecoin)
                          vanilla      Blake-256 8-rounds
                          x11evo       Permuted x11
                          x11          X11
                          x12          X12
                          x13          X13
                          x14          X14
                          x15          X15
                          x16r         X16R
                          x16rv2       X16Rv2 (Raven / Trivechain)
                          x16s         X16S (Pigeon)
                          x17          X17
                          x20r         X20R
                          xevan        Xevan (BitSend)
                          yescrypt     Yescrypt
                          zr5          ZR5
  -o, --url=URL         URL of mining server
  -O, --userpass=U:P    username:password pair for mining server
  -u, --user=USERNAME   username for mining server
  -p, --pass=PASSWORD   password for mining server
      --cert=FILE       certificate for mining server using SSL
  -x, --proxy=[PROTOCOL://]HOST[:PORT]  connect through a proxy
  -t, --threads=N       number of miner threads (default: number of processors)
  -r, --retries=N       number of times to retry if a network call fails
                          (default: retry indefinitely)
  -R, --retry-pause=N   time to pause between retries, in seconds (default: 30)
      --time-limit=N    maximum time [s] to mine before exiting the program.
  -T, --timeout=N       timeout for long poll and stratum (default: 300 seconds)
  -s, --scantime=N      upper bound on time spent scanning current work when
                          long polling is unavailable, in seconds (default: 5)
      --randomize       Randomize scan range start to reduce duplicates
  -f, --diff-factor     Divide req. difficulty by this factor (std is 1.0)
  -m, --diff-multiplier Multiply difficulty by this factor (std is 1.0)
  -n, --nfactor         neoscrypt N-Factor
      --coinbase-addr=ADDR  payout address for solo mining
      --coinbase-sig=TEXT  data to insert in the coinbase when possible
      --max-log-rate    limit per-core hashrate logs (default: 5s)
      --no-longpoll     disable long polling support
      --no-getwork      disable getwork support
      --no-gbt          disable getblocktemplate support
      --no-stratum      disable X-Stratum support
      --no-extranonce   disable Stratum extranonce support
      --no-redirect     ignore requests to change the URL of the mining server
  -q, --quiet           disable per-thread hashmeter output
      --no-color        disable colored output
  -D, --debug           enable debug output
  -P, --protocol-dump   verbose dump of protocol-level activities
      --hide-diff       Hide submitted block and net difficulty
  -S, --syslog          use system log for output messages
  -B, --background      run the miner in the background
      --benchmark       run in offline benchmark mode
      --cputest         debug hashes from cpu algorithms
      --cpu-affinity    set process affinity to cpu core(s), mask 0x3 for cores 0 and 1
      --cpu-priority    set process priority (default: 0 idle, 2 normal to 5 highest)
  -b, --api-bind        IP/Port for the miner API (default: 127.0.0.1:4048)
      --api-remote      Allow remote control
      --max-temp=N      Only mine if cpu temp is less than specified value (linux)
      --max-rate=N[KMG] Only mine if net hashrate is less than specified value
      --max-diff=N      Only mine if net difficulty is less than specified value
  -c, --config=FILE     load a JSON-format configuration file
  -V, --version         display version information and exit
  -h, --help            display this help text and exit
```

#  Start mining!

You are ready to use your favorite mining pool (eg multipool.us) and start mining

```shell
root@raspberrypi:~/cpuminer-multi#  ./cpuminer -a scrypt -o stratum+tcp://us.multipool.us:3334 -u <worker_name>  -p 12345

root@raspberrypi:~/cpuminer-multi# ./cpuminer -a sha256 -o stratum+tcp://us.multipool.us:3332 -u <worker_name>  -p 12345
```

# Resources

* Most usefull resource is the repository of the cpuminer-multi:  https://github.com/tpruvot/cpuminer-multi
* Original repo: https://github.com/lucasjones/cpuminer-multi

* You can find more details about the parameter 'march' and the AArch64-Options : https://gcc.gnu.org/onlinedocs/gcc-6.1.0/gcc/AArch64-Options.html

* More details about the various architectures for RPi : https://en.wikipedia.org/wiki/Raspberry_Pi
