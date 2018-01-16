# memory leak checking 内存泄漏
思路
===
内存泄漏问题的排查一般是重写标准的内存分配入口函数，在自定义的入口函数中加入一些内存申请与释放的相关信息统计，其中重要的一点信息就是内存申请的调用栈，即已经检测出发生泄漏时，能知道是何处申请的。

手段
===
使用ld提供的–wrap选项，比如重写malloc函数，我们先定义__wrap_malloc函数，如下
void * __wrap_malloc( size_t size) {
  void* result;
  result= __real_malloc( size);
  printf("Malloc: allocate %d byte mem, start at %p", size, result);

  return result;
}
程序链接的时候使用gcc -Wl,--wrap=malloc参数
