## Program Best Practice

### JNI Practice

build the demo android jni demo for debug 

### cplusplus
- [C++ 参数初始化化](https://www.zfl9.com/cpp-class.html)

- [析构函数妙用](https://www.jianshu.com/p/74bfdf3c1823)

- [Explicitly Defaulted and Deleted Functions in C++ 11](https://www.geeksforgeeks.org/explicitly-defaulted-deleted-functions-c-11/)

  **What is a Defaulted Function?**
  Explicitly defaulted function declaration is a new form of function declaration that is introduced into the C++11 standard which allows you to append the **‘=default;’** specifier to the end of a function declaration to declare that function as an explicitly defaulted function. This makes the compiler generate the default implementations for explicitly defaulted functions, which are more efficient than manually programmed function implementations.

  ```c++
  // C++ code to demonstrate the 
  // use of defaulted functions 
  #include <iostream> 
  using namespace std; 
    
  class A { 
  public: 
    
      // A user-defined  
      // parameterized constructor 
      A(int x)  
      { 
          cout << "This is a parameterized constructor"; 
      } 
        
      // Using the default specifier to instruct 
      // the compiler to create the default  
      // implementation of the constructor. 
      A() = default;  
  }; 
    
  int main() 
  { 
      // executes using defaulted constructor 
      A a;  
        
      // uses parametrized constructor 
      A x(1);  
      return 0; 
  } 
  ```

  

  **What are constrains with making functions defaulted?**
  A defaulted function needs to be a special member function (default constructor, copy constructor, destructor etc), or has no default arguments. For example, the following code explains that non-special member functions **can’t** be defaulted:

  ```c++
  // C++ code to demonstrate that 
  // non-special member functions 
  // can't be defaulted 
  class B { 
  public: 
  
  	// Error, func is not a special member function. 
  	int func() = default; 
  	
  	// Error, constructor B(int, int) is not 
  	// a special member function. 
  	B(int, int) = default; 
  
  	// Error, constructor B(int=0) 
  	// has a default argument. 
  	B(int = 0) = default; 
  }; 
  
  // driver program 
  int main() 
  { 
  	return 0; 
  } 
  
  ```

  

  

  **What are the advantages of ‘=default’ when we could simply leave an empty body of the function using ‘{}’?**
  Even though the two may behave the same, there are still benefits of using default over leaving an empty body of the constructor. The following points explain how:

  1. Giving a user-defined constructor, even though it does nothing, makes the type not an aggregate and also not trivial. If you want your class to be an aggregate or a [trivial ](https://www.geeksforgeeks.org/trivial-classes-c/)type (or by transitivity, a POD type), then you need to use ‘= default’.
  2. Using ‘= default’ can also be used with copy constructor and destructors. An empty copy constructor, for example, will not do the same as a defaulted copy constructor (which will perform member-wise copy of its members). Using the ‘= default’ syntax uniformly for each of these special member functions makes code easier to read.

- **Deleted Function**

  Prior to C++ 11, the operator **delete** had only one purpose, to deallocate a memory that has been allocated dynamically.
  The C++ 11 standard introduced another use of this operator, which is: **To disable the usage of a member function.** This is done by appending the **=delete;** specifier to the end of that function declaration.

  Any member function whose usage has been disabled by using the ‘=delete’ specifier is known as an **expicitly deleted function.**

  Although not limited to them, but this is usually done to implicit functions. The following examples exhibit some of the tasks where this feature comes handy:

  ```c++
  // C++ program to disable the usage of 
  // copy-constructor using delete operator 
  #include <iostream> 
  using namespace std; 
  
  class A { 
  public: 
  	A(int x): m(x) 
  	{ 
  	} 
  	
  	// Delete the copy constructor 
  	A(const A&) = delete; 
  	
  	// Delete the copy assignment operator 
  	A& operator=(const A&) = delete; 
  	int m; 
  }; 
  
  int main() 
  { 
  	A a1(1), a2(2), a3(3); 
  	
  	// Error, the usage of the copy 
  	// assignment operator is disabled 
  	a1 = a2; 
  	
  	// Error, the usage of the 
  	// copy constructor is disabled 
  	a3 = A(a2); 
  	return 0; 
  } 
  
  ```

  **Disabling undesirable argument conversion**

  ```c++
  // C++ program to disable undesirable argument 
  // type conversion using delete operator 
  #include <iostream> 
  using namespace std; 
  
  class A { 
  public: 
  	A(int) {} 
  
  
  	// Declare the conversion constructor as a 
  	// deleted function. Without this step, 
  	// even though A(double) isn't defined, 
  	// the A(int) would accept any double value 
  	// for it's argumentand convert it to an int 
  	A(double) = delete; 
  }; 
  
  int main() 
  { 
  	A A1(1); 
  	
  	// Error, conversion from 
  	// double to class A is disabled. 
  	A A2(100.1); 
  	return 0; 
  } 
  
  ```

  It is very important to note that A deleted function is implicitly inline. A deleted definition of a function must be the first declaration of the function. In other words, the following way is the correct way of declaring a function as deleted:

  ```c++
  class C 
  {
  public:
           C(C& a) = delete;
  };
  
  // Sample C++ code to demonstrate the  
  // incorrect syntax of declaring a member  
  // function as deleted 
  class C  
  { 
  public: 
      C(); 
  }; 
    
  // Error, the deleted definition   
  // of function C must be the first  
  // declaration of the function. 
  C::C() = delete;  
  ```

  **What are the advantages of explicitly deleting functions?**

  1. Deleting of special member functions provides a cleaner way of preventing the compiler from generating special member functions that we don’t want. (As demonstrated in ‘Disabling copy constructors’ example).
  2. Deleting of normal member function or non-member functions prevents problematic type promotions from causing an unintended function to be called (As demonstrated in ‘Disabling undesirable argument conversion’ example).







