## cplusplus 常用数据结构

下图展示了数据结构的总图示意图：

![](/home/yzh/Documents/myblog/shenmayufei.github.io/assets/img/数据结构.png)

### Array

定义：是线性表的一种实现，用于存储同类型变量。**线性表**:线性结构的特点是：在数据元素的有限集中，除第一个元素无直接前驱，最后一个元素无直接后续以外，每个数据元素有且仅有一个直接前驱元素和一个直接后续元素。从概念上来看，线性表是一种抽象数据类型；数组是一种具体的数据结构。线性表与数组的逻辑结构是不一样的，线性表是元素之间具有1对1的线性关系的数据元素的集合，而数组是一组数据元素到数组下标的一一映射。并且从物理性质来看，数组中相邻的元素是连续地存储在内存中的；线性表只是一个抽象的数学结构，并不具有具体的物理形式，线性表需要通过其它有具体物理形式的数据结构来实现。在线性表的具体实现中，表中相邻的元素不一定存储在连续的内存空间中，除非表是用数组来实现的。对于数组，可以利用其下标在一个操作内随机存取任意位置上的元素；对于线性表，只能根据当前元素找到其前驱或后继，因此要存取序号为i的元素，一般不能在一个操作内实现，除非表是用数组实现的。线性表是一种非常灵活的数据结构，线性表可以完成对表中数据元素的访问、添加、删除等操作，表的长度也可以随着数据元素的添加和删除而变化。

![C++ Arrays](https://cdn.programiz.com/sites/tutorial2program/files/Arrays-C%2B%2B.jpg)

数据类型的常用数据操作有：访问、搜索、插入、删除。它们对应的操作性能（大O）:

![img](https://alleniverson.gitbooks.io/data-structure-and-algorithms/content/%E7%AE%97%E6%B3%95/images/%E7%AE%97%E6%B3%95%E5%A4%8D%E6%9D%82%E5%BA%A63.png)

- how to work:

  ```c++
  //declare
  //dataType arrayName[arraySize];
  int age[100];  //type and size,declare 
  
  
  //element of an Array and How to access them
  age[1]
  
  
  //how to initialize an array 
  int mark[5] = {19, 10, 8, 17, 9};
  or :
  int mark[] = {19, 10, 8, 17, 9};
  
  
  //how to insert and print array elements
  int mark[5] = {19, 10, 8, 17, 9};
  
  //change the 4th element to 9
  mark[3] = 9
  
  //take input from the user and insert in third element
  cin >> mark[2]
  
  //print first element of the arrray
  cout << mark[0]
  
  
  //example
  
  #include <iostream>
  using namespace std;
  int main(){
      int numbers[5],sum = 0;
      cout << "Enter 5 number";
      for (int i = 0; i < 5; ++i)
      {
          cin >> numbers[i];
          sum += numbers[i];
      }
      
      cout << "Sum = " << sum << endl;
      return 0;
  }
  ```

  ![Initialize an array in C programming](https://cdn.programiz.com/sites/tutorial2program/files/c-array-initialization.jpg)

- [二维数组、多维](http://17de.com/library/CPP/ls17.htm#17.5)

  初始化时若是需要编译器推断高维（也就是第一维度）的数据大小，可以省略，因为数据存储时，先是低维后高维存储。

- 数组作为函数传参时

  传递的时第一个元素对应的指针，然后形参初始化为实参对应的数组第一元素地址；

  声明一个函数的数组参数时，可以不指定大小（多维时，可以省略最高维，和定义是一样的。）

  ```c++
  //定义一个使用二维数组作为参数
  
  void Func(int arr[][5])  //第二维的大小可以不指定
  
  {
  
    ...
  
  }
  //定义一个使用三维数组作为参数
  
  void Func(int arr[][2][5])  //第三维的大小可以不指定
  
  {
  
    ...
  
  }
  ```

- 函数返回值不能是数组

  ```c++
  int[5] Func();  //ERROR!
  int * Fun(); //correct
  //由于数组作为参数时，使用的是传址方式
  ```

- sizeof 计算数组

  ```c++
  int arr[5];
  cout << sizeof(arr);
  
  void fun(int arr[])
  {
      cout << sizeof(arr) << endl;  //error,有函数数组传参数是地址
  }
  ```

  

- how to implement

  顺序表通常用数组存储，在C++中，数组有静态数组和动态数组(动态内存分配)两种，对于顺序表，我们将采用静态数组方式存储。因此，顺序表中插入和删除一个数据元素的时间复杂度为O(n)。定位一个数据元素的时间复杂度为O(n)。其余操作的时间复杂度均为O(1)。

  **但是**：若是不需保持数据元素原来的顺序，则时间复杂度则有变化。









[参考文档]([https://alleniverson.gitbooks.io/data-structure-and-algorithms/content/2.%E9%93%BE%E8%A1%A8/%E9%A1%BA%E5%BA%8F%E8%A1%A8%E4%B8%8E%E9%93%BE%E8%A1%A8%E7%9A%84%E4%B8%8D%E5%90%8C.html](https://alleniverson.gitbooks.io/data-structure-and-algorithms/content/2.链表/顺序表与链表的不同.html))

[cplusplus tutorial](https://www.programiz.com/cpp-programming)