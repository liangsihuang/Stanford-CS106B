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
### C++ file I/O
* File streams declared in <fstream>
    * streams are objects, dot notation used
    * ifstream for reading, ofstream for writing
    ```cpp
    #include <fstream>
    ifstream in;
    ofstream out;
    ```
* Use open to attach stream to file on disk
    ```cpp
    in.open("names.txt");
    out.open(filename.c_str()); // requires C-string!
    ```
* Check status with fail, clear to reset after error
    ```cpp
    if (in.fail())
        in.clear();
### Stream operations 
* Read/write single characters
    ```cpp
    ch = in.get();
    out.put(ch);
    ```
* Read/write entire lines
    ```cpp
    getline(in, line);
    out << line << endl;
    ```
* Formatted read/write
    ```cpp
    in >> num >> str;
    out << num << str;
    ```
* Use fail to check for error
    ```cpp
    if (in.fail()) ...
    ```
### Class libraries
* Some libraries provide free functions
    * RandomInteger, getline, sqrt etc
* Other libraries provide classes
    * string, stream
* Class = data + operations
    * Tight coupling between value and operations that manipulate it
    * Class interface describes abstraction
        * Models string/time/ballot/database/etc with appropriate features
* Client use of object
    * Learn the abstraction, use public interface
    * Unconcerned with implementation details
### Why is OO so successful?
* Tames complexity
    * Large programs become interacting objects
    * Each class developed/tested indepandently
    * Clean separation between client & implementer
* Objects can model real-world
    * Time, Ballot, ClassList, etc    
    * Build on existing understanding of concepts
* Facilitates re-use
    * Also easily change/extend class in future
### CS106 class library
* Provide common functionality, highly leveraged
    * Scanner
    * Vector, Grid, Stack, Queue, Map, Set
* Why?
    * Living "higher on the food chain"
    * Efficient, debugged
    * Clean abstraction
* We study as client and later as implementer
    * Why client-first?
### CS106 Scanner
* Scanner's job: break apart input string into tokens
    * Mostly divide on white-space
    * Some logic for recognizing numbers, punctuation, etc.
* Operations
    * setInput
    * nextToken/hasMoreTokens
    * Fancy options available with set/get
* Used for?
    * Handling user input, reading text files, parsing expressions, processing commands, etc.
    ![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/Scanner.png)  
### Scanner interface
```cpp
class Scanner{
    public :
        Scanner(); // constructor (invoked when allocated)
        ~Scanner();// destructor (invoked when deallocated)

        void setInput(string str); // set string to be scanned
        string nextToken();
        bool hasMoreTokens();
        enum spaceOptionT { PreserveSpaces, IgnoreSpaces };
        void setSpaceOption(spaceOptionT option);
        spaceOptionT getSpaceOption();
        // other advanced options excerpted for clarity
}
```
### Client use of Scanner
```cpp
void CoutTokens()
{
    Scanner scanner;
    cout << "Please enter a sentence: ";
    scanner.setInput(GetLine());
    int count = 0;
    while (scanner.hasMoreTokens()){
        scanner.nextToken();
        cout++;
    }
    cout << "You entered " << count << "tokens." << endl;
}
```
### Containers
* Most classes in our library are container classes
    * Store data, provide convenient and efficient access
    * High utility for all types of programs
* C++ has a built-in "raw array"
    * Functional, but serious weaknesses (sizing, safty)
* CS106B Vector class as a "better" array
    * Bounds-checking
    * Add, insert, remove
    * Memory management, knows its size
### Template containers
* C++ templates perfect for container classes
    * Template is pattern with one or more placeholders
    * Client using template fills in placeholder to indicate specific version
* Vector class as template
    * Template class has placeholder for type of element being stored
    * Interface/implementation written using placeholder
    * Client instantiates specific vectors (vector of chars, vector of doubles) as needed
### Vector interface
```cpp
template <typename ElemType>
class Vector {
    public:
        Vector();
        ~Vector();

        int size();
        bool isEmpty();

        ElemType getAt(int index);
        void setAt(int index, ElemType value);

        void add(ElemType value);
        void insertAt(int pos, ElemType value);
        void removeAt(int pos);
}
```
### Templates are type-safe!
```cpp
#include "vector.h"
void TestVector()
{
    Vector<int> nums;
    nums.add(7);
    
    Vector<string> words;
    words.add("apple");

    nums.add("banana"); //compile error!
    char c = words.getAt(0); //compile error!
    Vector<double> s = nums; //compile error!
}
```
### Rules for template clients
* Client includes interface file as usual
    * #include "vector.h"
* Client must specialize to fill in the placeholder
    * Cannot use Vector without qualification, must be Vector<char>
    * Applies to declarations (variables, parameters, return types) and calling constructor
* Vector is specialized for its element type
    * Attempt to add locationT into Vector<char> will not compile!
### Client use of Vector
```cpp
#include "vector.h"
Vector<int> MakeRandomVector(int sz)
{
    Vector<int> numbers;
    for (int i = 0; i < sz; i++)
        numbers.add(RandomInteger(1,100));
    return numbers;
}
void PrintVector(Vector<int> &v)
{
    for (int i = 0; i < v.size(); i++)
        cout << v[i] << " ";
}
int main()
{
    Vector<int> nums = MakeRandomVector(10);
    PrintVector(nums);
    ...
}
```
## Lec05
### Vector class
* Indexed, linear homogenous collection
    * Knows its size
    * Access is bounds-checked
    * Storage automatically handled (grow & shrink)
    * Convenient insert/remove
    * Deep-copy on assignment, pass/return-by-value
* Usage
    * Constructor creats empty vector
    * Add/insert adds new element
    * Access elements using setAt, getAt or operator []
* Useful for:
    * every kind of list you can imagine!
### Vector interface
```cpp
template <typename ElemType>
class Vector {
    public:
        Vector();
        ~Vector();

        int size();
        boole isEmpty();

        ElemType getAt(int index);
        void setAt(int index, ElemType value);

        void add(ElemType value);
        void insertAt(int pos, ElemType value);
        void removeAt(int pos);
}
```
### Template specialization
```cpp
class Vector <double> {
    public:
        Vector<double>();
        ~Vector<double>();

        int size();
        boole isEmpty();

        double getAt(int index); //ElemType 全部换成 double!!!
        void setAt(int index, double value);

        void add(double value);
        void insertAt(int pos, double value);
        void removeAt(int pos);
}
```
### Client use of Vector
见lec04末尾
### Templates are type-safe!
见lec04末尾前一点
### Grid class
* 2-D homogenous collection indexed by row & col
    * Access to elements is bounds-checked
    * Deep-copy on assignment, pass/return by value
* Usage
    * Set dimensions in constructor (can later resize)
    * Elements have default value for type before explecitly assigned
    * Access elements using getAt/setAt or operator()
* Useful for:
    * Game board
    * Images
    * Matrices
    * Tables
### Grid interface
```cpp
template <typename ElemType>
class Grid {
    public:
        Grid();
        Grid(int numRows, int numCols); // overloaded constructor
        ~Grid();

        int numRows();
        int numCols();

        ElemType getAt(int row, int col);
        void setAt(int row, int col, ElemType value);

        void resize(int numRows, int numCols);
}
```
### Client use of Grid
```cpp
#include "grid.h"
// Returns a new 3x3 grid of chars, where each
// elem is initialized to space character
Grid<char> CreateEmptyBoard()
{
    Grid<char> board(3,3); // create 3x3 board of chars
    for (int row = 0; row < board.numRows(); row++)
        for (int col = 0; col < board.numCols(); col++)
            board(row, col) = ' '; //board.setAt(row, col, ' ')
    return board; // btw, it's ok to return object
}
```
### Stack class
* Linear collection, last-in-first-out
    * Limited-access vector
    * Can only add/remove from top of stack
    * Deep-copy on assignment, pass/return by value
* Usage
    * Constructor creates empty stack
    * push to add objects, pop to remove
* Useful for:
    * Reversing a sequence
    * Managing a series of undoable actions
    * Tracking history when web browsing
### Stack interface
```cpp
template <typename ElemType>
class Stack {
    public:
        Stack();
        ~Stack();

        int size();
        bool isEmpty();

        void push(ElemType element);
        ElemType pop();
        ElemType peek();
}
```
### Client use of Stack
```cpp
void ReverseResponse()
{
    cout << "What say you? ";
    string response = GetLine();

    Stack<char> stack;
    for (int i = 0; i < reponse.length(); i++)
        stack.push(response[i]);
    
    cout << "That backwards is :";
    while (!stack.isEmpty())
        cout << stack.pop();
}
```
### Queue class
* Linear collection, first-in-first-out
    * Limited-access vector
    * Can only add to back, remove from front
    * Deep-copy on assignment, pass/return by value
* Usage 
    * Constructor creates empty queue
    * enqueue to add objects, dequeue to remove
* Useful for:
    * Modeling a waiting line
    * Storing user ketstrokes
    * Ordering jobs for a printer
    * Implementing breadth-first search
### Queue interface
```cpp
template <typename ElemType>
class Queue {
    public:
        Queue();
        ~Queue();

        int size();
        bool isEmpty();

        void enqueue(ElemType element);
        ElemType dequeue();
        ElemType peek();
}
```
### Client use of Queue
```cpp
void ManageQueue()
{
    Queue<string> queue;
    while (true) {
        cout << "?";
        string response = GetLine();
        if (response == "") break;
        if (response == "next") {
            if (queue.isEmpty())
                cout << "No one waiting!" << endl;
            else
                cout << "Handle" <<queue.dequeue() << endl;
        } else {
            queue.enqueue(response);
            cout << "Add" << response << endl;
        }
    }
}
```
### Nested templates
* Queue can hold stacks or vector of vector, etc
    * Vector<Queue<string> > checkoutLines;
    * Grid<Stack<string> > game;
* Need space between >> closers
    * otherwise compiler see stream extraction
* Can use typedef to make shorthand name
    * typedef Vector<Vector<int> > calendarT;
## Lec06
### More containers!
* Sequential containers
    * Vector, Grid, Stack, Queue
    * Store/retrieve elements based on sequence of insert/update
    * Doesn't examine elements, just store/retrieve them
* Associative containers
    * Map, Set
    * Not based on sequence, instead on relationships
    * Efficient, smart access
    * Do examine/compare elements to store/retrieve efficiently
### Map class
* Collection of key-value pairs
    * Associate a key with a value
    * Retrieve value previously associated with key
* Usage
    * Constructor creates empty map
    * Use add to insert new pair (or overwrite previous value)
    * Access elements using getValue
    * Shorthand operator[]
    * Browse pairs using iterator or mapping function
* Useful for:
    * dictionary, database, lookup table, document index, ...
### Example maps
* Dictionary: word mapped to definitions
    * cup -> small open container used for drinking
* Thesaurus: word mapped to synonyms
    * happy -> pleased, joyful, content, delighted
* DMV: license mapped to registration info
    * 2A0B130 -> 1985, Datsun B210, gray
### Map interface
```cpp
// any type of Value, but always string key
template <typename ValueType>
class Map() {
    public:
        Map();
        ~Map();

        int size();
        bool isEmpty();

        void add(string key, ValueType value);
        void remove(string key);

        bool containsKey(string key);
        ValueType getValue(string keu);
}
```
* What happens if getValue of non-existent key?
### Client use of Map
```cpp
void MapTest(ifstream &in)
{
    Map<int> map;
    string word;
    while (true) {
        in >> word;
        if (in.fail()) break;
        if (map.containsKey(word)) {
            int count = map.getValue(word);
            map.add(word, count+1);
        } else{
            map.add(word, 1);
        }
        cout << map.size() << " unique words." << endl;
    }
}
```
### More on Map
* Shorthand operator[]
    * Can be used to get/set/update value for key
    * Applied to non-existent key will add pair with "default" value
* Why must keys always be string type?
    * Map internally uses known structure of strings to store efficiently
    * Can convert type to string to use as key (e.g. IntegerToString)
* What if more than one value per key?
    * Add new value will overwrite previous
    * Use Vector as value type for one-to-many relationship
* How can you summarize/browse entries?
    * e.g. printing all entries, summing all frequencies, finding the word with largest number of synonyms, and so on
    * Map provides access to all elements in turn via an `iterator`
### Iterating over Map
* Iterator is nested type, declared within Map class
    * Full name is `Map<type>::Iterator`
* Usage
    * Ask map to create iterator
    * Walk through keys using hasNext/next on iterator
    * Iterator will visit all keys, no guarantee on which order
```cpp
void PrintFrequencies(Map<int> &map)
{
    Map<int>::Iterator itr = map.iterator();
    while (itr.hasNext()) {
        string key = itr.next();
        cout << key << " = " << map[key] << endl;
    }
}
```
### Set class
* Unordered collection of unique elements
    * {3, 5} is same as {5, 3}, no duplicate elements
* Usage
    * Constructor creates empty set
    * Add/remove/contains to operate on members
    * High-level ops: unionWith, intersect, subtract, isSubsetOf, equals
    * Iterator to browse members
* Useful features:
    * Fast membership operations
    * Coalesce duplicates
    * High-level ops
        * Unioning our friends to create party invite list
        * Checking is set of courses meets requirements to graduate
        * Intersectiong my desired pizza toppings with yours, subtracting things we both hate
        * Compound boolean queries, AND/OR/NOT
### Set interface
```cpp
template <typename ElemType>
class Set {
    public:
        Set(int (cmpFn)(ElemType, ElemType) = OperatorCmp);
        ~Set();

        int size();
        bool isEmpty();

        void add(ElemType element);
        void remove(ElemType element);
        bool contains(ElemType element);

        bool equals(Set & otherSet);
        bool isSubsetOf(Set & otherSet);
        void unionWith(Set & otherSet);
        void intersect(Set & otherSet);
        void subtract(Set & otherSet);

        Iterator iterator();
}
```
### Client use of Set
```cpp
void RandomTest()
{
    Set<int> seenSoFar;
    while (true) {
        int num = RandomInteger(1, 100);
        if (seenSoFar.contains(num)) break;
        seenSoFar.add(num);
    }
    cout << seenSoFar.size() << " unique before repeat.";
}
void PrintSet(Set<string> &set)
{
    Set<double>::Iterator itr = set.iterator();
    while (itr.hasNext())
        cout << itr.next() << " ";
    //set iterator visits elements in order (unlike Map's)
}
```
### Set higher-level operations
```cpp
struct personT {
    string name;
    Set<string> friends, enemies;
}
Set<string> MakeGuestList(personT one, personT two)
{
    Set<string> result = one.friends;
    result.unionWith(two.friends);
    result.subtract(one.enemies);
    result.subtract(two.enemies);
    return result;
}
```
### Why Set is different
* Other containerrs store/retrieve elements, but Set truly examines them - why?
    * Non-duplication for add
    * Find element for contains, remove
    * High-level ops compare elements for match
* But Set is written as a template!
    * ElemType is just a placeholder
    * How to compare two things of unknown type?
### Default element comparison
* Some types can be compared using < and ==
* Set uses a default function to compare two elements that looks like this:
```cpp
{
    if (one == two ) return 0;
    else if (one < two) return -1;
    else return 1;
}
```
* What happens if this default comparison doesn't make sense for the client's type?
    * e.g. == and < don't work on this type
### Template compilation error
```cpp
struct studentT {
    string first, last;
    int idNum;
    string emailAddress;
}
int main()
{
    set<studentT> students;
}
```
* The above code will generate a compile error that is reported something like this:
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/Scanner.png)  
* < and == don't work for structs!
### Client callback function
* Client writes function that compares two elements
    * Must match prototype as specified by set
* Body of function does comparison
    * As appropriate for type
* Pass this function to Set
    * Set will hold onto it, and "call back" to client whenever it needs to compare two elements

### Supplying callback function
```cpp
struct studentT {
    string first, last;
    int idNum;
}
int CmpById(studentT a, studentT b)
{
    if (a.idNum < b.idNum) return -1;
    else if (a.idNum == b. idNum) return 0;
    else return 1;
}
int main()
{
    Set<studentT> set(CmpById); // ok!
}
```
### Building things: ADTs rock!
* Map of Set
    * Google's web index (word to matching pages)
* Vector of Queues
    * Grocery store chechout lines
* Set of sets
    * Different speciality pizzas
* Stack of Maps
    * Compiler use to enter/exit nested scopes

## Lec07













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
    

