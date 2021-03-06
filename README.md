# riseab/boringnginx

[![](https://images.microbadger.com/badges/image/alexhaydock/nginx-boringssl-latest.svg)](https://microbadger.com/images/alexhaydock/nginx-boringssl-latest "Get your own image badge on microbadger.com")

This container builds the [latest mainline Nginx](https://nginx.org/en/CHANGES) with the latest BoringSSL code. It was created to aid with the easy deployment of TLS 1.3 services at a time when most Linux distributions were not packaging a version of OpenSSL that could handle it.

### Run This Container (Testing)
Run this container as a quick test (it will listen on http://127.0.0.1 and you will see logs directly in the terminal when connections are made):
```
docker run --rm -it -p 80:80 alexhaydock/nginx-openssl-latest
```

### Run This Container (Production)
Run this container as a daemon with your own config file:
```
docker run -d -p 80:80 -p 443:443 -v /path/to/nginx.conf:/etc/nginx.conf:ro --name nginx alexhaydock/nginx-boringssl-latest
```

### Build This Container (x86_64)
If you have an `x64_64` machine, you can build this container like so:
```
docker build -t nginx-boringssl-latest github.com/alexhaydock/nginx-boringssl-latest
```

### Build This Container (Raspberry Pi)
If you are running a Raspberry Pi on an `armhf` OS like [Raspbian](https://www.raspberrypi.org/downloads/raspbian/), you can use:
```
docker build -t nginx-boringssl-latest -f Dockerfile.arm32v6 github.com/alexhaydock/nginx-boringssl-latest
```

If you are running a Raspberry Pi with an `aarch64` OS like [Fedora](https://fedoraproject.org/wiki/Architectures/ARM/Raspberry_Pi), you can use:
```
docker build -t nginx-boringssl-latest -f Dockerfile.arm64v8 github.com/alexhaydock/nginx-boringssl-latest
```
