# set

使用红黑树实现的关联容器，用来存储一组按一定顺序排列的元素，并且元素不重复。它提供了一种自动排序的机制，可以快速的进行插入删除和查找操作。

set中的元素按照升序排列，也可以通过自定义比较函数来改变排序方式。

定义方式：

~~~c++
#include <set>

std::set<int> myset; //定义一个空的set，其中元素类型为int
~~~

## 操作

1. insert(val)

   三种重载形式

   向set中插入一个元素，**如果该元素已经存在，则不进行任何操作**

   ~~~c++
   std::set<int>myset;
   myset.insert(10);
   ~~~

   **可以同时插入多个元素**

   ~~~c++
   std::set<int> myset;
   myset.inset({10, 20, 30, 40});
   ~~~

   **可以使用迭代器插入元素**

   ~~~c++
   std::set<int> myset;
   std::vector<int> myVector = {10, 20, 30, 40};
   myset.insert(myVector.begin(), myVector.end());
   ~~~

2. erase()

   三种重载形式

   直接删除

   ~~~c++
   std::set<int> mySet;
   mySet.insert({10, 20, 30, 40});
   mySet.erase(20);	//删除20
   ~~~

   **使用迭代器删除**

   ~~~c++
   std::set<int> mySet;
   mySet.insert({10, 20, 30, 40});
   auto it = mySet.find(20);
   mySet.erase(it);	//删除20
   ~~~

   **删除一定范围内的元素**

   ~~~c++
   std::set<int> mySet;
   mySet.insert({10, 20, 30, 40});
   auto start = mySet.find(20);
   auto end = mySet.find(30);
   mySet.erase(start, end); //删除[20, 30)范围内的元素
   ~~~

3. count()

   返回set中等于指定元素的元素个数，因为set中的元素是唯一的，因此返回值只能是0或1。

   ~~~c++
   std::set<int> mySet;
   mySet.insert({10, 20, 30, 40});
   if(mySet.count(20)){
   	std::cout << "Element found" << std::endl;
   }else{
   	std::cout << "Element not found" << std::endl;
   }
   ~~~

4. clear()

   清空set中的所有元素。

   ~~~c++
   std::set<int> mySet;
   mySet.insert({10, 20, 30, 40});
   mySet.clear();
   ~~~

5. size()

   返回set中元素的个数

   ~~~c++
   std::set<int> mySet;
   mySet.insert({10, 20, 30, 40});
   std::cout << "Size of set: " << mySet.size() << std::endl;
   ~~~

6. empty()

   判断set是否为空，如果为空则返回true，否则返回false

   ~~~c++
   std::set<int> mySet;
   if(mySet.empty()){
       std::cout << "Set is empty" << std::endl;
   }else{
       std::cout << "Set is not empty" << std::endl;
   }
   ~~~

7. find(val)

   查找set中是否存在某个元素，如果存在，则返回该元素的迭代器，否则返回end()迭代器。

   可以使用以下方式：

   ~~~c++
   std::set<int> mySet;
   mySet.insert({10, 20, 30, 40});
   auto it = mySet.find(20);
   if(it != mySet.end()){
   	std::cout << "Element found: "<< *it << std::endl;
   }else{
       std::cout << "Element not found" << std::endl;
   }
   ~~~

8. begin()与end()

   begin()函数返回set中第一个元素的迭代器，end()函数返回set中**最后一个元素后面的迭代器**

   ~~~C++
   std::set<int> mySet;
   mySet.insert({10, 20, 30, 40});
   for(auto it = mySet.begin(); it != mySet.end(); ++it){
       std::cout << *it << std::endl;
   }
   ~~~

9. rbegin()与rend()

   ***reversed begin with end***

   rbegin()函数返回set中最后一个元素的迭代器，rend()函数返回set中第一个元素前面的迭代器

   ~~~c++
   std::set<int> mySet;
   mySet.insert({10, 20, 30, 40});
   for(auto it = mySet.rbegin(); it != mySet.rend(); ++it){
       std::cout << *it << std::endl;
   }
   ~~~

10. lower_bound()与upper_bound()

    lower_bound()函数返回一个迭代器，它指向set中第一个不小于指定元素的元素，如果不存在这样的元素，则返回end()迭代器。upper_bound()函数返回一个迭代器，它指向set中第一个大于指定元素的元素，如果不存在这样的元素，则返回end()迭代器。

    ~~~c++
    std::set<int> mySet;
    mySet.insert({10, 20, 30, 40});
    auto it1 = mySet.lower_bound(20);
    auto it2 = mySet.upper_bound(20);
    std::cout << "lower_bound: " << *it1 << std::endl; //20
    std::cout << "upper_bound: " << *it2 << std::endl; //30
    ~~~

11. equal_range(val)

    返回一对迭代器，这两个迭代器分别是lower_bound和upper_bound

    ~~~c++
    std::set<int> mySet;
    mySet.insert({10, 20, 30, 40});
    auto it_pair = mySet.equal_range(20);
    for(auto it = it_pair.first; it != it_pair.second; ++it){
        std::cout << *it << " ";
    }
    std::cout << std::endl;
    ~~~

## 技巧

1. 使用***emplace()***函数代替***insert()***

   emplace()函数直接在set中创建对象，而insert()函数需要先创建一个临时函数，然后再将这个临时对象拷贝或移动到set容器中。

2. 使用自定义比较函数

   这里要用自定义比较函数的话，要用到函数包装器，而在set中，这里默认的参数为二元谓词less<key>。在c++14中，也可以通过没有类型参数的***std::less<>***或***std::greater<>***谓词来启用异类查找。

   ~~~c++
   #include <functional>
   std::set<std::pair<int, int>, std::function<bool(const std::pair<int, int>&, const std::pair<int, int>&)>> mySet([](const std::pair<int, int>& a, const std::pair<int, int>& b){
       return a.second < b.second;
   });
   mySet.insert(std::make_pair(1, 2));
   mySet.insert(std::make_pair(3, 1));
   mySet.insert(std::make_pair(2, 3));
   for(const auto& p: mySet){
       std::cout << "(" << p.first << ", " << p.second << ")" << std::endl;
   }
   ~~~

   

3. 使用***set_intersection()***函数和***set_union()***函数

   如果需要对两个set容器进行交集或者并集操作，可以使用set_intersection()函数和set_union()函数。这两个函数的使用方法和标准库算法库中的函数类似。

   ~~~c++
   #include <algorithm>
   std::set<int> set1{1, 2, 3, 4, 5};
   std::set<int> set2{3, 4, 5, 6, 7};
   std::set<int> intersection;
   std::set_intersection(set1.begin(), set1.end(), set2.begin(), set2.end(), std::inserter(intersection, intersection.begin()));
   std::cout << "Intersection: ";
   for(const auto& x : intersection){
       std::cout << x << " ";
   }
   std::cout << std::endl;
   std::set<int> Union;
   std::set_union(set1.begin(), set1.end(), set2.begin(), set2.end(), std::inserter(Union, Union.begin()));
   std::cout << "Union:";
   for(const auto& x: Union){
       std::cout << x << " ";
   }
   std::cout << std::endl;
   ~~~

4. 使用***distance()***函数

   如果需要计算set容器中两个迭代器之间的元素个数，可以使用distance()函数。

   函数计算元素的规则为左闭右开，时间复杂度为O(n)，不建议在循环中使用

   ~~~c++
   #include <iterator>
   std::set<int> mySet{1, 2, 3, 4, 5};
   auto it1 = mySet.lower_bound(2);
   auto it2 = mySet.upper_bound(4);
   std::cout << "Distance: " << std::distance(it1, it2) << std::endl; //3
   ~~~

   

​		





