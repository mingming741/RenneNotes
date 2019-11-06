# ns3
network similator 3, 用于做网络的simulation

### build (waf)
使用`waf`小程序，首先configure需要build的environment。这里推荐在development的时候使用debug模式，因为logging和assertions只在debug模式存在。在script完成后进行大规模实验，或者调整参数的时候，使用optimize的模式。`--out`指明了build的result的路径
```sh
./waf configure --build-profile=optimized --enable-examples --enable-tests --out=build/optimized
# 或者
./waf configure --build-profile=debug --enable-examples --enable-tests --out=build/debug
```
相当于configure build的选项，`waf`会检查host计算机中的各种配置，因为一些option需要底层的一些library，（可以是c也可以是python）。然后列出build能enable的ns3 feature。如果这里想要的feature没有被enable的话，建议安装dependency。使用下面cmd查看configure：
```sh
./waf --check-profile
```
在configure完成后，使用`waf` build ns3：
```sh
./waf
```
这样就会开始编译各种库，编译过的应该会重新编译，更多信息使用`./waf --help`。
