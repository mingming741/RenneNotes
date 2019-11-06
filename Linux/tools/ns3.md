# ns3
network similator 3, 用于做网络的simulation

### build
使用`waf`小程序，首先configure需要build的environment
```sh
./waf configure --build-profile=optimized --enable-examples --enable-tests
# 或者
./waf configure --build-profile=debug --enable-examples --enable-tests
```
相当于configure build的选项，`waf`会检查host计算机中的各种配置，因为一些option需要底层的一些library，（可以是c也可以是python）。然后列出build能enable的ns3 feature。如果这里想要的feature没有被enable的话，建议安装dependency。使用下面cmd查看configure：
```

```
