# Book: A Tour to C++

## Introduction

### Start
- Core Langauage Features: Char and Int
- Standard Library Components: Vector and Map
- C++ Statically Typed Language: Menas type of every entity(object, value, name and expression) must be known to the compiler at its point of use. the type of an object determine the set of operations applicable to it
- Type of Function: Return Type and sequence of argument type 
    - `Ex- double get(const vector<double>&vec, int index) // type: double(const vector<double&>, int)`
    - Function Can be member of class , then name of calss also part of the function type, `type: char&Tring::(int)`
- Function overloading
    ```c++
    void print(int,double);
    void print(double, int);
    print(0,0); // compiler erro : ambiguous
    ```
### Types, Variables and Arithmetic
- A ***declaration*** is a statement that introduces an entity into the program. It specifies the type for the entity
    - A ***type*** defines a set of possile values and a set of operation (for an object)
    - An ***object*** is some memory that holds a value of some type
    - A ***value*** is a set of bits interpreted according to a type
    - A ***variable*** is a name object
- We can use \` this in long literals ex 3.14159\`263535
- Char variable is of the natural size to hold a character on given machine (typically 8-bit)
- The conversion used in expression are called ***the usuall arithmetic conversion*** aim to ensure that expression are computed at the highest precision of its operands.
- **Order of evaluation** The order of evaluation is left-to right for `x.y, x->y, x(y), x[y], x<<y, x>>y, x&&y, and x||y`. For assignments `(e.g., x+=y)`, the order is right-to-left. For historical reasons realated to optimization, the order of evaluation of other expressions `(e.g., f(x)+g(y))` and of function arguments `(e.g., h(f(x),g(y)))` is unfortunately unspecified.

> **Note:** Evaluation and Associtaivty both are different

### Initialization
```
double d1 = 2.3;
double d2 {2.3};
double d3 = {2.3}; // the = is optional with {...}
complex<double>z2 {d1,d2}
complex<double>z3 = {d1, d2} // the = is optional with {...} 
```
- {} this saves you from conversion that losse information
    - ex, int i1 = 7.8 // i becomes 7 (surprise?)
    - ex, int i2{7.8} // error: floating-point to iteger conversion 
    - unfortunately, conveersion that loose information, ***narrowing conversions***, such as double to int and int to char are allowed and implitcly applied.
- Don"t need to state its type explicitly when it can be deduced from the initializer
    - auto b = true; // a bool
    - auto ch = 'x'; // a char
    - use in generic programming where the long type names and the exact type of an object can be hard for programmer to know

### Scope and Initialization
- Scope
    - Local Scope - Declared in a function or lambda is called a local name.
    - Class Scope- Class Member Name, outside of any function, lambda or enum class 
    - Namespace Scope - outside of any function, class, or enum class.
- For a namespace object the point of destruction is the end of the program, For a member the point of destruction is determine by the point of destruction oth object of which it is a member


### Constants
- Two notions of immutability
    - ***const***: "I promise not to change this value". The compiler enforces the promise made by const. The value of a *const* can be calculated at run time.
        ```c++
        const int runtimeValue = std::rand();  // OK, value not known at compile time
        runtimeValue = 5;  // Error: cannot assign to const variable
        ```
        > **Note:** If a runtime-initialized const variable is placed in read-only memory, it triggers a memory protection fault (segmentation fault) when written to at runtime.

    - ***constexpr***: to be evaluated at compile time, to allow placement of data in read only memory, the value of a contexpr must be calculated by the compiler
        ```c++
        constexpr int compileTimeValue = 42;         //OK
        constexpr int bad = std::rand();             //Error: rand() is not constexpr
        ```
        - Funtion to be usable in a *constant exppression*, must define contexpr
            ```
            constexpr double square(double x){return x*x;}
            constexpr double max1 = 1.4*square(17);// ok, evaluated at compiletime
            constexpr double max1 = 1.4*square(17);// error: var is not a constant expression
            const double max3 =1.4*square(var);// ok, may be evaluated at runtime
            ```
        - contexpr function can be used for non constant argument, so we can use the same funcntion for constexpr and for variables, without defining two.
        > **Note:** c++20, consteval introduce, if we want only be evaluated at compile time then define with **consteval**
        - contexpr or consteval functions cannot modify non-local variables, but it can have loop[s and use its own local variable

### Pointers, Array and Refernces

- Pointers and Array
    - In declaration
        - [] means: **array of**
        - * menas:  **pointer to**
        - & means   **refernce to**
    - In expression
        - * means: **contents of**
        - & means: **address of**
- C++ rang-for statement, for loops that traverse a sequence in the simplest way
    ```c++
    void print()
    {
        int v[]={0,1,2,3,4,5};
        for(auto x:v) // for each x in v
         cout << x << '\n';

        for(auto x:{10,21,32,35})
            cout << x << '\n';

        for(auto& x:v) // add 1 to each x in v
        {
            ++x;
            cout << x << '\n';
        }   
    }
    ```
- Declarator operators
    ```c++
    T a[n] // T[n]: a is an array of n Ts
    T* p   // T*: p is a pointer to T
    T& r   // T&: r is a reference to T
    Tf(A)  // T(A), is a funtion argument of type A and return type T
    ```
- Null Pointer
    ```c++
    Link<Record>*Ist = nullptr; // pointer to a link to a Record 
    int x = nullptr; //error:nullptr id a ponter not an integer
    ```

### Tests

- Like a for-statement, an if-statement can introduce a variable and test it.
    ```C++
    if(auto n = v.size(); n!=0) // the integer n defined for use within the if-statement and intialized with v.size
    {
        // ...we get here if n!=0
    }

    // simplified version
     if(auto n = v.size())
    {
        // ...we get here if n!=0
    }
    ```

### Mapping to hardware

- Assignments
    - An assignment of built-in type is a simple machine copy operation, 'int x =2; int y=3', both are independent object
    - Assignement to a refernce, does not change what the reference refers to but assigns to the referenced object
        ```
        int x = 2;
        int y = 3;
        int& r =x;
        int& r2 = y;
        r = r2; // read through r2, write through r: x becomes 3
        ```
    - To access the value pointed to by a pointer, you use *; that is automatically (implicitly) done for a refernce.

- Initilization
    - Initialization differs from assignment. In general, for an assignment to work correctly, the assigned-to object must have a value. On the other hand, the task of initialization is to make an uninitialized piece of memory into a valid object
    - You can use = to initialize a reference but please don’t let that confuse you. `int& r = x;      // bind r to x (r refers to x)`This is still initialization and binds r to x, rather than any form of value copy.
 > **Note:** The basic semantics of argument passing and function value return are that of initialization. 

- Ex
    - Initialization
        ```c++
        // Creating a variable and giving it a value at the same time.
        int x = 10;           // Initialization (built-in type)
        std::string s = "hi"; // Initialization (calls constructor)
        ```
    - Assignment
        ```c++
        // Changing the value of an already initialized variable.
        x = 20;               // Assignment (built-in type: overwrite)
        s = "hello";          // Assignment (calls assignment operator)
        ```

## User Defined Types

### Introduction
- The C++ abstraction mechanism are primarily designed to let programmers design and imp[lement their own type.
- ***User-defined types (classes and enumeration)*** are less error prone and easy to use than built-in types
- structure 
    ```c++
    struct Vector
    {
        int sz;
        double *elem;
    };


    void vector(Vector& v, int s)
    {
        v.elem = new double[s];
        v.sz =  s;
    }

    void f(Vector v, kVector& rv, Vector* pv)
    {
        int i1 = vsz;    // access through name
        int i2 = rv.sz;  // access through reference
        int i3 = pv->sz; //access through the pointer
    }
    ```
### Classes
- A tighter connection between the representation and the operation is needed for a user-defined type to have all the properties expected of a **real-type**. To do that we have to distinguish between the interface to a type (to be used by all) and its implementation (which has access to the otherwise inaccssible data). The language mechnaism of for that is called a class.
- The interface defined by public member of the class and private member only accessible only through that interface.
```c++
class Vector {
public:
    Vector(int s):elem{new double[s]},sz{s}{} //contruct a vector
    double& operator[](int i){return elem[i];} //element access: subscripting
    int size(){return sz;}
private:
    doouble* elem; // pointer the elemennts 
    int sz // the number of elements
}

double read_and_sum(int s)
{
    Vector v(6);
    for(int i=0; i<v.size(); ++i)
        cin >>v[i];
    
    double sum = 0;
    for(int i=0; i<v.size(); ++i)
        sum +=v[i];

    return sum;
}
```

### Unions
- Occupies only as much space as its largest element

```c++
struct Entry{
    str name;
    Type t;
    Node* p; // use p if t==ptr
    int i;   // use i if t==num
}

union Value{
    Node* p;
    int i;
};

struct Entry{
    str name;
    Type t;
    Value v;
}
```
> **Note:** shoul not use naked union, alway use with type or call it tagged union

- **varaint**, standard library type can eliminate most direct use of unions, ` variant<Node*, int>, holds_alternative<int>(pe->v),get<int>`

```c++
struct Entry{
    string name;
    variant<Node*, int> v;
}

void f(Entry *pe)
{
    if(holds_alternative<int>(pe->v))
        cout << get<int>(pe->v);
}
```

### Enumerations
```c++
enum class Color {red, blue, green};
enum class traffic_light{green, yellow, red};
Color x = red; //error: which red?
Color y = Traffic_light::red; //error:that red is not a Color
Color z = Color::red; //ok

int i = Color:: red //error: Color::red is not an int
Color c = 2 //initialization error:2 is not a color
```
> ***Note:*** enum - allow implicit conversion, enum class not allow implicit conversion - only explixit conversion allowed, means class after enum specifies that an enumeration is *strongly typed*. Also using enum class also specifies thier scope, menas being separate types

```c++
Color x = Color{5}; // ok, but verbose
Color y{6}; //also ok
```
- By default **enum class** has only assignment, intialization and comparison defined. However an enumeartion is user defined type, so we can define operator for it:

```c++
Traaffic_light& operator++(Traffic_light& t)
{
    switch(t){
        case Traffic_loght::green: return t=Traffic_light::yellow;
        case Traffic_light::yellow return t=Traffic_light::red;
        case Traffic_light::red return t=Traffic_light::green;
    }
}

Traffic_light next = ++light; // next becomes Traffic_light::green
```

- dont want to explicitly qualify enmuerator names and want enumerator values to be ints (without need for an expliti conversion) remove the **class** from enum.
```c++
enum Color {red, green, blue};
int col =  green; // col get 1
```

### Advice
- Organize related data into structures
- Prefer enum class over plain enum
- Avoid naked unions
- define contructors to gurantee and simplyfy the intitialization of class
- Define operation on enumerations for safe and simple use

## Modularity

### Introduction