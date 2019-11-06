# ns3
network similator 3, 用于做网络的simulation

### build
使用`waf`小程序，首先configure需要build的environment
```console
./waf configure --build-profile=optimized --enable-examples --enable-tests
# 或者
./waf configure --build-profile=debug --enable-examples --enable-tests
```
