# Stanford-CS106B

## Lec14
Algorithm analysis  
time spent and memory used  
Evaluating performance  
Empirically-time with stopwatch  
Mathematically-analyze algorithm  
## Lec16
```cpp
int Partition(Vector<int> &arr, int start, int step)
{
    int 
}
void Quicksort(Vector<int> &arr, int start, int step)
{
    if (step>start){
        int pivot = Partition(arr, start, step);
        Quicksort(arr, start,);
    }
}
```
Quicksort code  
Partition is linear O(N)
T(N) = N + 2T(N/2) => O(NlogN)h
### Assuming bad 90/10 split  
still O(NlogN)
### Assuming worst N-1/1 split
Ack! O(N^2)  
already sorted!
* What to do?
  * Choose pivot differently
  * Compute actual median
  * "Median of three"
  * Random
为什么没有泡沫排序？更难写，易出错，又更慢  

### template 一劳永逸地写出排序
* A proliferation of Swap  
作为引子。  
* Function template 
用法比class template简单，不用<>来表示什么类型，complier会根据传递的参数来infer  
* Instantation errors
### Client use of sort template
并不是什么都可以比较，有些比较需要额外的比较函数
## Lec17
* sort template with callback fn
    * Now can truly work for all types!
    * Supplying callback fn
* Final version of Sort template
默认使用大于小于
* Use of Sort template
* Why object-oriented programming?
    * Most programs organized around data
        * Making data the focus is good fit
    * Astraction
    * Capsulation
    * Modularity in development
    * Package reuse
* Class division
java 没有分，都在一个文件，有好有坏, 好处：一处改就好，坏处：细节暴露给用户
* Class interface in .h file
    * Class interface lists data and operations
* Accessing members
    * Members 
* Class implementation
    * Implementation goes in .cpp file
        * Must #include class.h file
    scoping :: 否则也不知道hour是什么
* Implementing member functions
    * Members of receiver accessible 
* Maintaining object consistency
* Constructors
* Destructors  
Java没有
### Basic thoughts on object design

## Lec18

* ADTs (abstract data types)
    * Client uses class as abstraction
    * Client can't and shouldn't muck with internals
    * 
    * Lexicon 词典？
* Why ADTs?
    * Abstraction
    * Encapsulation
    * Independence
    * Flexibility
### Let's implement Vectors!
```cpp
#ifdef _ //??
# //??
class MyVector
{
public:
    MyVector();
    -MyVector();
    
    int size();
    void add(string s);
    string getAt(int index);
    
private:
    string *arr;
    int numUsed, numAllocated;
}
```
## Lec19
* Quirky C++ template compilation ??? "古怪离奇"
### Implement Stack and Queue
    

