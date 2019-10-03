# Struct
记录c中常见的各种struct

### stdint.h
c语言中定义了各种int的type的头文件，在c++中被重写入"cstdint"。在linux中位于`usr/include`路径下，里面的内容大概是：
```c
typedef signed char		int8_t;
typedef short int		int16_t;
typedef int			int32_t;

typedef unsigned char		uint8_t;
typedef unsigned short int	uint16_t;
typedef unsigned int		uint32_t;
```
还有一些fast，least的type，暂时不知道是干什么用的
