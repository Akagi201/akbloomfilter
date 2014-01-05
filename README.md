bloomfilter x64实现
==================

仿照Cassandra中的BloomFilter实现，Hash选用[MurmurHash2]，通过双重散列公式生成散列函数
~~~~~ c
  Hash(key, i) = (H1(key) + i * H2(key)) % m
~~~~~


bloomfilter使用例子
---------------------------------
~~~~~ c
  #include "akblookfilter.h"

  static BaseBloomFilter stBloomFilter = {0};

  // 初始化BloomFilter：
  InitBloomFilter(&stBloomFilter, 0, 100000, 0.00001);
  // 重置BloomFilter：
  ResetBloomFilter(&stBloomFilter);
  // 释放BloomFilter:
  FreeBloomFilter(&stBloomFilter);

  // 向BloomFilter中新增一个数值（0-正常，1-加入数值过多）：
  uint32_t dwValue;
  iRet = BloomFilter_Add(&stBloomFilter, &dwValue, sizeof(uint32_t));

  // 检查数值是否在BloomFilter内（0-存在，1-不存在）：
  iRet = BloomFilter_Check(&stBloomFilter, &dwValue, sizeof(uint32_t));
~~~~~

版本历史
---------------------------------
 * 2012/12    完成初始版本
 * 2013/4/10  增加k/m的动态计算功能，参考：http://hur.st/bloomfilter
 * 2013/01/05 重构代码结构将实现和声明分离

[MurmurHash2]: https://sites.google.com/site/murmurhash/