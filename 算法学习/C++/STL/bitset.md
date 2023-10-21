# bitset

c++标准库的一种位集合容器，表示固定长度的二进制序列，并提供一些位运算操作，底层实现通常是一个无符号整数数组，每个元素都是一个二进制位。对一个长度为n的bitset，其下表为0到n-1，对应二进制序列中的从右往左的位置。

## 定义

~~~c++
#include <bitset>

std::bitset<32> bits;	//定义一个长度为32的bitset
~~~

## 操作

1. operator[]

   通过下标访问bitset的每一位，下标从0开始

   ~~~c++
   std::bitset<4> bits("1010");
   bits[1] = 0; // 将第2位设置为0
   std::cout << bits[2] << std::endl; // 输出第3位的值
   ~~~

2. set, reset, flip

   set将指定为设置为1，reset将指定位置设置为0，flip将指定位置取反

   ~~~c++
   std::bitset<4> bits("1010");
   bits.set(1); // 将第2位设置为1
   bits.reset(2); // 将第3位设置为0
   bits.flip(3); // 将第4位取反
   ~~~

3. count, any, none

   count计算bitset中1的个数，any判断是否存在1，none判断是否不存在1

   ~~~c++
   std::bitset<4> bits("1010");
   std::cout << bits.count() << std::endl; // 输出1的个数
   if (bits.any()) {
       std::cout << "bits contains 1" << std::endl;
   }
   if (bits.none()) {
       std::cout << "bits does not contain 1" << std::endl;
   }
   ~~~

4. operator<<, >>, &, |, ^

   只有位数相同的bitset才可以进行与或非运算

   <<左移，>>右移，&与，|或，^异或

   ~~~c++
   std::bitset<4> bits1("1010");
   std::bitset<4> bits2("0110");
   std::cout << (bits1 << 1) << std::endl; // 输出"0100"
   std::cout << (bits1 & bits2) << std::endl; // 输出"0010"
   std::cout << (bits1 | bits2) << std::endl; // 输出"1110"
   std::cout << (bits1 ^ bits2) << std::endl; // 输出"1100"
   ~~~

   