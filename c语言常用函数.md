# c语言常用函数

memcpy是memory copy的缩写，意为内存复制，在写C语言程序的时候，我们常常会用到它。它的函原型如下：

```
void *memcpy(void *dest, const void *src, size_t n);
```

它的功能是从src的开始位置拷贝n个字节的数据到dest。如果dest存在数据，将会被覆盖。memcpy函数的返回值是dest的指针。memcpy函数定义在string.h头文件里。



