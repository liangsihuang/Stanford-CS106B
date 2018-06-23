# Stanford-CS106B
## Lec02
### C++ vs Java : what's the same?
* General syntax
    * comment sequence
    * use of braces, parentheses, commas, semi-colons
    * variable/parameter declarations, function call
* Primitive variable types
    * char, int, double, but note Java boolean is C++ bool
* Operators
    * arithmetic, relational, logical
* Control structures
    * for, while, if/else, switch, return
### Dissecting a C++ program
```cpp
/*
 * average.cpp
 * -------------
 * This program adds scores and prints their average.
 */

# include "genlib.h"
# include "simpio.h"
# include <iostream>

const int NumScores = 4;

double GetScoresAndAverage(int numScores);

int main()
{
    cout << "This program averages" << NumScores << "scores." << endl;
    double average = GetScoresAndAverage(NumScores);
    cout << "The average is " << average << "." << endl;
    return 0;
}
```
### average.cpp (cont'd)
```cpp
/* Function: GetScoresAndAverage
 * Usage: avg = GetScoresAndAverage(10);
 * ------------------------------------
 * This function prompts the user for a set of values and returns
 * the average.
 */
double GetScoresAndAverage(int numScores)
{   
    int sum = 0;
    for (int i = 0; i < numScores; i++) {
        cout << "Next score? ";
        int nextScore = GetInteger();
        sum += nextScore;
    }
    return double(sum)/numScores;
}       
```
### C++ user-defined types
* Enumerations
    * Define new type with set of constrained options 
    ```cpp
    enum directionT {North, South, East, West};
    directionT dir =East;
    if (dir == West) ...
    ```
* Records
    * Define new type which aggregates a set of fields
    ```cpp
    struct pointT {
        double x;
        double y;
    };
    pointT p,q;
    p.x = 0;
    p = q;
    ```
### C++ parameter passing
* Default is pass-by-value
   * Parameter copies value, changes affect local copy
   ```cpp
   void Binky(int x, int y)
   {
      x* = 2;
      y = 0;
    }
    int main()
    {
      int a = 4, b = 20;
      Binky(a, b);
      ...
    }
   ```
* Add & to declaration for pass-by-reference
   * Parameter is now reference to original variable, which can change
   ```cpp
   void Binky(int &x, int y)
   {
      x* = 2;
      y = 0;
   }
   int main()
   {
      int a = 4, b = 20;
      Binky(a,b);
      ...
   }
   ```
   * Ref param also used for efficiency to avoid coping large data
### C++ libraries
* Groups related operations
   * Header file provides function prototypes and usage comments
   * Compiled library contains implementation
* C++ standard libraries
   * e.g. string, iostream, fstream
   * #include <iostream>
   * Terse, lowercase names: cout getline substr
* CS106 libraries
   * e.g. simpio, random, graphics
   * #include "random.h"
   * Capitalized verbose names: GetInteger RandomChange DrawLine

## Lec03
### CS106 random.h
* Library of functions to provide randomness
   * Support for shuffing, dice-rolling, coin-flipping, etc.
   * Free functions
* void Randomize()
   * Call once at start to initialize new random sequence
* int RandomInteger(int low, int high)
   * Returns int chosen at random from range low-high inclusive
* double RandomReal(double low, double high)
   * Same, but for real values
* bool RandomChance(double probability)
   * Returns true with odds of probability, false otherwise
* Coherent, convenient, complete
### C++ string
* Models a sequence of characters
* string defines a class, strings are objects
   * many operations are member functions that operate on receiver string
* Simple operations
   * member function `.length` returns number of chars
   * square brackets to access individual chars
   * C++ strings are mutable! (unlike Java)
   ```cpp
   int main()
   {
       string s;
       s = "cs106";
       for (int i = 0; i<s.length(); i++)
            s[i] = toupper(s[i]);
   }
   ```
### Operators on strings
* Assign using =, makes new copy
* Compare with relational ops (<,==,>=,...)
    * lexicographic ordering
* `+` is overloaded to do concatenation
    * operands must be chars or strings only
    ```cpp
    int main()
    {
        string s, t = "hello";
        s = t;
        t[0] = 'j';
        s = s + ' ';
        if (s != t) t += t;
    }
    ```
### string member functions
* Invoke member functions using dot notation
    str.function(args)
* Sample member functions:
    * int length()
    * int find(char ch, int pos = 0)
    * int find(string pattern, int pos = 0)
        returns index of first occurrence or string::npos if not found
    * string substr(int pos, int len)
        returns new string, copies len characters starting from pos
    * void insert(int pos, string txt)
        changes receiver, inserts txt at pos
    * void replace(int pos, int len, string txt)
        changes receiver, removes len chars start at pos, replace with txt
    * void erase(int pos, int len)
        changes receiver, removes len chars starting at pos
### CS106 strutils.h
* Few convenience free functions for string
* Converting between case
    * string ConvertToLowerCase(string s)
    * string ConvertToUpperCase(string s)
* Converting numbers to string and back
    * int StringToInteger(string s)
    * string IntegerToString(int num)
    * double StringToReal(string s)
    * string RealToString(double num)
### C++ string vs C-string
* C++ inherits legacy of old-style C-string
    * pointer to character array, null-terminated
    * String literals are actually C-strings
* Converting C-string to C++ string
    * Happens automatically in most contexts
    * Can force using string("abc")
* Converting C++ string to C-string
    * Using member function `a.c_str()`
* Why do you care?
    * Some older functionality requires use of C-string
    * C-string not quite compatible with concatenation
### Concatenation pitfall
* If one operand is true C++ string, all good
    ```cpp
    string str = "abc";
    str + "def";
    str + 'd';
    str + ch;
    ```
* If both operands are C-string/char, bad times
    ```cpp
    "abc" + "def"; // won't compile
    "abc" + 'd'; // compiles, but doesn't work
    "abc" + ch; // same
* Can force conversion if needed
    string("abc") + ch;
### C++ consle I/O
* Stream objects cout/cin
    * cout is the console output stream, cin for console input
    * << is stream insertion, >> is stream extraction
    ```cpp
    #include <iostream>
    int main()
    {
        int x, y;
        cout << "Enter two numbers: ";
        cin >> x >> y;
        cout << "You said: "<< x << " and " << y << endl;
    }
    ```
* Safer, easier read from console using out simpio.h
    ```cpp
    #include "simpio.h"
    ```
    int main()
    {
        int x = GetInteger();
        string answer = GetLine();
    }

## Lec04

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
    

