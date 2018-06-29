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
### Specific plot functions
```cpp
const double Incr = .1;
void PlotSin(double start, double stop)
{
    double centerY = GetWindowHeight()/2.0;
    MovePen(start, centerY + sin(start));
    for (double x = start; x <= stop; x += Incr)
        LineTo(x, centerY + sin(x));
}
void PlotSqrt(double start, double stop)
{
    double centerY = GetWindowHeight()/2.0;
    MovePen(start, centerY + sqrt(start));
    for (double x = start; x <= stop; x += Incr)
        LineTo(x, centerY + sqrt(x));
}
```
* Code is identical, except for function invoked
    * Let's unify!
### Generic plot function
```cpp
void Plot(double start, double stop, double (fn)(double))
{
    double centerY = GetWindowHeight()/2.0;
    MovePen(start, centerY + fn(start));
    for (double x = start; x <= stop; x += Incr)
        LineTo(x, centerY + fn(x));
}
```
* Using function as data!
    * Client passes function by name to `Plot` which graphs it
```cpp
int main()
{
    Plot(0, 2, sin);
    Plot(1, 10, sqrt);
    Plot(2, 5, MyFunction);
    Plot(2, 5, GetLine); // doesn't compile!
}
```
### Back to Set
* Set needs to compare elements to establish order
* Default strategy applies relational ops:
```cpp
{
    if (one == two) return 0;
    else if (one < two) return -1;
    else return 1;
}
```
* What happens if this doesn't make sense for the client's type?
### Client callback function (little different from lec06)
* Functions as data provides solution!
    * Set written to use a function to compares two elements
    * By default it uses OperatorCmp, which applies <, ==
* Client can supply their own function
    * Must match prototype as specified by Set
        * Takes two elements, returns int
* Client's function does comparison of elements
    * Using desired info to get right sense of equal/order
    * Result is negative/zero/positive
* Client passes function to Set constructor
    * Set holds onto fn, and will `callback` client whenever it needs to compare two elements

### Solving problems recursively
* Recursion is an indispensable tool in a programmer's toolkit
    * Simple solutions to complex problems
    * Elegance can lead to better programs: easier to modify, extend, verify
* Get help solving the problem from coworkers (clones) who work and act like you do
    * Delegate similar, smaller problem to clone
    * Combine result from clone(s) to solve total problem
### Recursive decomposition
* Standard decomp devides problem into dissimilar subproblems
    * Read file, store numbers, sort, ...
* Recursive decomp divides problem into smaller versions of same problem
    * Campus survey
    * Phone trees
    * Fractal drawing
* Recursive problems have "self-similar" structure in solution

## Lec08
### Solving problems recursively
* A recursive function calls itself - wacky!
* Idea: solve problem using coworkers (clones) who work and act like you
    * Delegate similar, smaller problem to clone
    * Combine result from clone(s) to solve total problem
    * Work toward trival version that is directly solvable
* For porblems that exhibit "self-similarity"
    * Structure repeats within at different levels of scale
    * Solving larger problem means solving smaller problem(s) within
* Feels mysterioius at first
    * "Leap of faith"required
    * With pratice, master the art of recursive decomposition
    * Eventually grok the underlying patterns
### Functional recursion
* Function that returns answer/result
    * Outer problem result uses result from smaller, same problem(s)
* Base case
    * Simplest version of problem
    * Can be directly solved
* Recursive case
    * Make call(s) to self to get results for smaller, simpler version(s)
    * Recursive calls must advance toward base case
    * Results of recursive calls combined to solve larger version
### Power example
* C++ has no exponentiation op
* Iterative formulation for Raise function
```cpp
int Raise(int base, int exp)
{
    int result = 1;
    for(int i = 0; i < exp; i++)
        result *= base;
    return result;
}
```
### Recursive version
```cpp
int Raise(int base, int exp)
{
    if (exp == 0)
        return 1;
    else
        return base * Raise(base, exp-1);
}
```
### More efficient recursion
```cpp
int Raise(int base, int exp)
{
    if (exp == 0)
        return 1;
    else {
        int half = Raise(base, exp/2);
        if (exp % 2 == 0)
            return half * half;
        else
            return base * half * half;
    }
}
```
### Avoid "arm's length" recursion
* Aim for simple, clean base case
    * No need to anticipate other earlier stopping points
    * Avoid looking ahead before recursive calls, just let simple base case handle
### Recursion and efficiency
* Recursion provides no guarantee of (in)efficiency
    * Recursion can require same resources as alternative approach
        * Or recursion may be much more or much less efficient
    * For problems with simple iterative solution, iteration is likely the best
* Why recursion then?
    * Can express with clear, direct, elegant code
    * Can intuitively model a task that is recursive in nature
    * Solution may require recursion - iteration won't do!
### Palindromes
* A palindrome string reads same when reversed
    * e.g. "was it a car or a cat i saw"
* Recursive insight
    * First and last letter match and interior is palindrome
* Base case?
```cpp
bool IsPalindrome(string s)
{
    if (s.length() <= 1) return true;
    return s[0] == s[s.length()-1] && IsPalindrome(s.substr(1,s.length()-2));
}
```
### Binary search
* Searching for key within vector
    * Linear search starts at beginning and searches to end
    * Binary search uses divide-and-conquer (requires `sorted` vector)
        * Much faster method!
* Recursive insight:
    * Consider middle elem of vector, if key, you're done
    * Otherwise decide which half to recursively search
* Base case?
### Binary search code
```cpp
const int NotFound = -1;
int BSearch(Vector<string> &v, int start, int stop, string key)
{
    if (start > stop) return NotFound;
    int mid = (start + stop)/2;
    if (key == v[mid])
        return mid;
    else if (key < v[mid])
        return BSearch(v, start, mid-1, key);
    else
        return BSearch(v, mid+1, stop, key);
}
```
* Classic "divide and conquer" algorithm
    * Operates very efficiently! Double size of vector, how much longer to search?
### Choosing a subset
* Given N things, how many different ways can you choose K of them?
    * e.g. given a dorm of 60 people, how many different groups of 4 people can go together to Flicks?
* N-choose-K, written as C(n, k)
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/choose.png)  
### Choose code
* Simplest base case
    * when no choices remain at all
```cpp
int C(int n, int k)
{
    if (k == 0 || k == n)
        return 1;
    else
        return C(n-1, k) + C(n-1, k-1);
}
```
## Lec09 procedural recursion
### Thinking recursively
* Recursive decomposition is the hard part
    * Find recursive sub-structure
        * Solve problem using result from smaller subproblem(s)
    * Identify base case
        * Simplest possible case, directly solvable, recursion advances to it
* Common patterns
    * Handle first and/or last, recur on remaining
    * Divide in half, recur on one/both halves
    * Make a choice among options, recur on updated state
* Placement of recursive call(s)
    * Recur-then-process versus process-then-recur
### Procedural vs functional
* Functional recursion
    * Function returns result
    * Computers using result from recursive call(s)
* Procedural recursion
    * No return value (function returns void)
    * Task accomplished during recursive calls
* Example: drawing fractal
    * Self-similar structure
    * Repeats itself within
    * Outer fractal makes recursive call to draw inner fractal(s)
### A familiar fractal
```cpp
void DrawFractal (double x, double y, double w, double h)
{
    DrawTriangle(x, y, w, h);
    if (w < .2 || h < .2) return;
    double halfH = h/2;
    double halfW = w/2;
    DrawFractal(x, y, halfW, halfH); // left
    DrawFractal(x + halfW/2, y + halfH, halfW, halfH); // top
    DrawFractal(x + halfW, y, halfW, halfH); // right
}
```
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/fractal.png)  
### Recursive art
* Piet Mondrian
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/Mondrian.png) 
### Random pseudo-Mondrian
* Choose one of three opetions
    * Divide canvas horizontally
    * Divide canvas vertically
    * Do nothing
* Dividing produces two smaller canvases
    * That can also be recursively painted in Mondrian style
* Base case stops at too-small canvas
### Mondrian code
```cpp
void DrawMondrian(double x, double y, double w, double h)
{
    if (w < 1 || h < 1) return; // base case
    FillRectangle(x, y, w, h, RandomColor()); // fill background
    switch (RandomInteger(0, 2)) {
        case 0: // do nothing
            break;  
        case 1: // bisect vertically
            double midX = RandomReal(0, w);
            DrawBlackLine(x+midX, y, h);
            DrawMondrian(x, y, midX, h);
            DrawMondrian(x+midX, y, w-midX, h);
            break;
        case 2: // bisect horizontally
            double midY = RandomReal(0, h);
            DrawBlackLine(x, y+midY, w);
            DrawMondrian(x, y, w, midY);
            DrawMondrian(x, y+midY, w, h-midY);
            break;         
    }
}
```

### Towers
* Set of graduated disks stacked on a spindle
* Goal is move tower from source to destination
* Rules
    * All disks on a spindle (when not actively being moved)
    * Have one spare spindle
    * Can move only one disk at a time
    * Can only place disk on top of larger disk
### Tower recursion
* Move tower of height N from A to B, using C
    * Starting thought: devide the tower
        * What is smaller instance of similar problem that helps?
        * Divide N height tower into one disk and tower of height N-1?
            * Which one to separate? Top or bottom disk?
            * What do you do with other tower?
### Tower code
```cpp
void MoveTower(int n, char src, char dst, char tmp)
{
    if (n > 0) {
        MoveTower(n-1, src, tmp, dst);
        MoveSingleDisk(src, dst);
        MoveTower(n-1, tmp, dst, src);
    }
}
```
### Permutations
* Want to enumerate all rearrengements:
    * ABCD permutes to DCBA, CABD, etc.
* Solving recursively
    * Choose a letter from input to append to output
    * Recursively permute remaining letters onto output
    * What other options do you need to explore?
    * How to ensure each letter is used exactly once?
    * What is the base case?
### Permute strategy
* Result is empty, starting input is "abcd"
* Choose a letter to be first, say "a"
* Result so far is "a", remaining input is "bcd"
* Recursively permute to get all "bcd" combos
* After finishing permutations with "a" in front, need to go again with "b" in front and then "c" and so on
### Permute code
```cpp
void RecPermute(string soFar, string rest)
{
    if (rest == "") {
        cout << soFar << endl;
    } else {
        for (int i = 0; i < rest.length(); i++) {
            string next = soFar + rest[i];
            string remaining = rest.substr(0, i) + rest.substr(i+1);
            RecPermute(next, remaining);
        }
    }
}

// "wrapper" function
void ListPermutations(string s)
{
    RecPermute("", s);
}
```
### Tree of recursive calls
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/tree.png) 

## Lec10

### Subsets
* Enumerate all subsets of input
    * "abc" has subsets "a", "b", "ab", "ac", ...
    * Order doesn't matter, "ab" is same as "ba"
* Solving recursively
    * Separate one element from input
    * Can either include in current subset or not
    * Recursively form subsets including it
    * Recursively form subsets not including it
    * What is the base case?
* Remind you of any other problem you've seen?
    * Same patterns often resurface!
### Subset strategy
* Result is empty, starting input is "abcd"
* Consider first element: "a"
* Add to subset, remaining input is "bcd"
* Recursively find all subsets from here
* Repeat recursion without including "a"
### Subsets code
```cpp
void RecSubsets(string soFar, string rest)
{
    if (rest == "")
        cout << soFar << endl;
    else {
         // add to subset, remove from rest, recur
        RecSubsets(soFar + rest[0], rest.substr(1));
        // don't add to subset, remove from rest, recur
        RecSubsets(soFar, rest.substr(1));
    }
}
void ListSubsets(string str)
{
    RecSubsets("", str);
}
```
### Tree of recursive calls
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/tree2.png)

### Exhaustive recursion
* Permutations/subsets are about choice
    * Both have deep/wide tree of recursive calls
    * Depth represents total number of decisions made
    * Width of branching represents number of available options per decision
* Exhaustive recursion is, well, exhaustive
    * Explores every possible option at every decision point
    * Typical very expensive
        * N! permutations, 2^N subsets
        * (Recursion isn't the problem, there just is a huge space to explore)
* Consider partial exploration of exhaustive space
    * Similar exhaustive structure, but stop at first "satisfactory" outcome
### Recursive backtracking
* Cast problem in terms of decision points
    * Identify what decisions need to be made
    * Identify what options are available for each decision
    * A recursive call makes one decision, and recurs on remaining decisions
* Backtracking approach
    * Design recursion function to return success/failure
    * At each call, choose one option and go with it
    * Recursively proceed and see what happens
    * If it works out, great, otherwise unmake choice and try again
    * If no option worked, return fail result which triggers backtracking (i.e. un-making earlier decisions)
* Heuristics may help efficiency
    * Eliminate dead ends early by pruning
    * Pursue most likely choice(s) first
### Backtracking pseudocode
```cpp
bool Solve(configuration conf)
{
    if (no more choices) // base case
        return (conf is goal state);
    for (all available choices) {
        try one choice c;
        // solve from here, is works out, you're done
        if (Solve(conf with choice c made)) return true;
        unmake choice c;
    }
    return false; // tried all choices, no soln found
}
```
### Permute -> anagram finder
```cpp
void RecPermute(string soFar, string rest)
{
    if (rest == "") {
        cout << soFar << endl;
    } else {
        for (int i = 0; i < rest.length(); i++) {
            RecPermute(soFar+rest[i], rest.substr(0,i)+rest.substr(i+1));
        }
    }
}
bool IsAnagram(string soFar, string rest, Lexicon &lex)
{
    if (rest == "") {
        return lex.containsWord(soFar);
    } else {
        for (int i = 0; i < rest.length(); i++) {
            if (IsAnagram(soFar+rest[i], rest.substr(0, i)+rest.substr(i+1),lex))
        }
    }
    return false;
}
```

### 8 Queens
* Goal: place N queens on board so none threatened
    * Queen can attack in any straight line (horizontally, vertically, diagonally)
* Cast as in terms of decision
    * Each call will make one decision and recur on rest
    * How many decisions do you have to make?
    * What options do you have for each?
### N queens code
```cpp
bool Solve(Grid<bool> &board, int col)
{
    if (col >= board.numCols()) return true; // base case
    for (int rowToTry=0; rowToTry < board.numRows(); rowToTry++) {
        if (IsSafe(board, rowToTry, col)) {
            PlaceQueue(board, rowToTry, col);
            if (Solve(board, col+1)) return true;
            RemoveQueue(board, rowToTry, col); 
        }
    }
    return false;
}
```

## Lec11
### Sudoku solver
* Arrange 1 to 9 with no repeats in row, col, or block
    * Solve by recursive backtracking
    * Not much logic, just brute-force
* Cast as decision problem
    * Each call will make one decision and recur on rest
    * How many decisions do you have to make?
    * What options do you have for each?
### Sudoku code
```cpp
bool SolveSudoku(Grid<int> &grid)
{
    int row, col;
    if (!FindUnassignedLocation(grid, row, col))
        return true; // all locations successfully assigned!
    for (int num = 1; num <= 9; num++) {  // options are 1-9
        if (NoConflicts(grid, row, col, num)) { // if # looks ok
            grid(row, col) = num; //try assign #
            if (SolveSudoku(grid)) return true; //recur if succeed stop
            grid(row, col) = UNASSIGNED; // undo & try again
        }
    }
    return false; // this triggers backtracking from early decisions
}
```

### Cryptarithmetic
* Encryted arithmetic puzzle
    * Assign letter to digit (S=4, E=7, ...) so math is correct, each digit/letter used once
    * Recognize the recursive core?
        * Assign D E M N O R S Y to digits 0-9 is like building permutations of DEMNORSY
* Dumb, exhaustive strategy
    * Find unassigned letter, assign digit
    * Recur from there and see if solution works out
    * If not, unmake assignment and try again
### Dumb solver
```cpp
bool DumbSolve(puzzleT puzzle, string lettersToAssign)
{
    if (lettersToAssign == "")
        return PuzzleSolved(puzzle);
    for (int digit = 0; digit <= 9; digit++) {
        if (AssignLetter(lettersToAssign[0], digit)) {
            if (DumbSolve(puzzle, lettersToAssign.substr(1))) return ture;
            UnassignLetter(lettersToAssign[0], digit);
        }
    }
    return false; // nothing worked, need to backtrack
}    
```
### Smarter solver
* Not all permutations plausible!
    * Don't waste time on ridiculous choices
* Use grade-school addition knowledge
    * Start with lastmost column (least significant digit)
        * Assign 'D', then assign 'E', now consider 'Y'
        * Assign 'Y' value so math works out (if impossible, fail here)
        * Recur on next column
* Heuristics
    * Avoids niggling around in dead ends
    * Choose more likely options to explore first
    * Eliminate obvious bad choices
### Looking for patterns
* Knapsack filling
    * Sack holds 50 lbs, which items to select for highest value?
* Traveling salesman
    * Visit 10 cities, how to cover shortest total distance?
* Dividing into fair teams
    * Equal total team IQ?
* Findin hidden words
    * Richard Milhaus Nixon -> "criminal"

### Pointers
* A pointer is an address
    * All data is stored in memory
    * Each location in memory is indexed by address
    * Can refer to data by using its address in memory
* Why use pointers?
    * Provide shared access to common data
    * Build flexible, dynamic data structures
    * Precisely control allocation/deallocation
* Why are pointers considered scary?
    * Operations can be error-prone
    * Pointer mistakes have wide variation in symptoms
    * Memory bugs can be hard to understand and fix
### Simple pointer operations
```cpp
int main()
{
    int num;
    int *p, *q;

    p = new int;
    *q = 10;
    
    q = new int;
    *q = *p;
    q = p;
    delete p;
    delete q; // bad idea, q already deleted!
    q = NULL;
}
```
## Lec12
### Pointer basics
* Pointers are distinguished by type of pointee
    * Type `double*` not same as int `int*`
* Pointers are uninitialized until assigned
    * Dereferencing a uninitialized pointer is bad news
* Dynamic allocation via new
    * Operator `new` allocates memory from heap, returns address
* Manual deallocation via delete
    * Forgetting to delete means memory is orphaned
    * Accessing deleted memory has unpredictable consuquences
### Pointers and dynamic arrays
```cpp
int main()
{
    int *arr = new int[10];
    for (int i = 0; i < 10; i++)
        arr[i] = i;
    delete[] arr; // delete[] if allocated with new[]
}
```
* Raw arrays can be trouble
    * Manually allocated and deallocated
    * Don't know their length
    * No bounds-checking
    * Cannot easily change size once allocated
        * Allocate new space, copy over, update pointer
* Vector uses array behind scenes, but hides issue
### Use of pointers
* Axess database
```cpp
struct studentT {
    string first, last;
    string address, phone;
}
struct courseT {
    string dept, name;
    Vector<studentT *> students;
}
```
* A course has pointers to enrolled students
    * Allocate studentT record in heap for new student
    * Each course student enrolls in stores pointer to record
    * Saves space by not repeating student information
    * If student gets new phone number, change in one place only!
### Recursive data
* Recursion applied to data
    * Self-referential, self-similar
    * Within itself, data has smaller version repeated
* Examples
    * Matroshka dolls
    * Nesting boxes
    * Onions
    * Structure containing pointer to same structure
### A recursive struct
```cpp
struct Entry {
    string name, address, phone;
    Entry *next;
}
```
* Each entry points to another Entry!
* Wired together, you get a linked list!
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/linkedlist.png)
### Creating a node
```cpp
Entry *GetNewEntry()
{
    cout << "Enter name (ENTER to quit):";
    string name = GetLine();
    if (name == "") return NULL;

    Entry *newOne = new Entry;
    newOne->name = name;
    cout << "Enter address: ";
    newOne->address = GetLine();
    cout << "Enter phone: ";
    newOne->phone = GetLine();
    newOne->next = NULL; // no one follows
    return newOne;
}
```
### Building a linked list of nodes
```cpp
Entry *BuildAddressBook()
{
    Entry *listHead = NULL;
    while (true) {
        Entry *newOne = GetNewEntry();
        if (newOne == NULL) break;
        newOne ->next = listHead;
        listHead = newOne;
    }
    return listHead;
}
```
* What order does this build the list in?
### Printing list
```cpp
void PrintEntry(Entry *entry)
{
    cout << entry->name << " " << entry->phone << endl;
}
void PrintList(Entry *list)
{
    for (Entry *cur = list; cur!= NULL; cur = cur->next)
        PrintEntry(cur);
}
```
* Idiomatic loop to iterate over list, compare to
` for (int i = 0; i < n; i++)`

## Lec13
### A recursive twist on printing
* Iteration replaced with recursion
```cpp
void PrintList(Entry *list)
{
    if (list != NULL) {
        PrintEntry(list);
        PrintList(list->next);
    }
}
```
* What happens if we switch the order of these two lines?
### Recursive data -> recursive ops
* Natural to operate on linked list recursively
    * List divides into first node and rest of list
    * Base case: empty list
    * Recursive case: handle first node, recur on rest
```cpp
int Length(Entry *list)
{
    if (list == NULL)
        return 0;
    else
        return 1 + Length(list->next);
}
void Deallocate(Entry *list)
{
    if (list != NULL) {
        Deallocate(list->next);
        delete list;
    }
}
```
### Watch the pointer!
* (Decompose function to add node to front of list)
```cpp
void Prepend(Entry *ent, Entry *first)
{
    ent->next = first;
    first = ent;  // Buggy!
}
Entry *BuildAddressBook()
{
    Entry *listHead = NULL;
    while (true) {
        Entry *newOne = GetNewEntry();
        if (newOne == NULL) break;
        Prepend(newOne, listHead);
    }
    return listHead;
}
```
### Passing pointer by reference
* Tiny modification saves the day!
```cpp
void Prepend(Entry *ent, Entry *& first)
```
### Array vs linked list
* Array/vector stores elements in contiguous memory
    * + Fast, direct access by index
    * - Insert/remove requires shuffing
    * - Cannot easily grow/shrink (must copy over contents)
* Linked list wires elements together using pointers
    * + Insert/remove only requires re-wiring pointers
    * + Each element individually allocated, easy to grow/shrink
    * - Must traverse links to access elements
### Insert in sorted order
* Traverse list to find the position to insert
    * What is true after the loop exists?
    ```cpp
    void InsertSorted(Entry * &list, Entry *newOne)
    {
        Entry *cur;
        for (cur = list; cur != NUll; cur = cur->next){
            if (newOne->name < cur->name) break;
        }
    }
    ```
* Drag previous pointer (one behind cur)
    * prev/cur move down list in parallel, one node apart
    ```cpp
    void InsertSorted(Entry * &list, Entry * newOne)
    {
        Entry *cur, *prev = NULL;
        for (cur=list; cur!=NULL; cur=cur->next){
            if (newOne->name < cur->name) break;
            prev = cur;
        }
        // what are possible values for prev?
    }
    ```
### Insert in sorted order
```cpp
void InsertSorted(Entry * &list, Entry * newOne)
{
    Entry *cur, *prev = NULL;
    for (cur=list; cur!=NULL; cur=cur->next){
        if (newOne->name < cur->name) break;
        prev = cur;
    }
    newOne->next = cur; // splice outgoing ptr
    if (prev != NULL)
        prev->next = newOne; // splice incoming ptr
    else
        list = newOne; // note special case!
}
### Recursive insert
```cpp
void InsertSorted(Entry * &list, Entry *newOne)
{
    if (list == NULL || newOne->name < list->name){
        newOne->next = list;
        list = newOne;
    } else {
        InsertSorted(list->next, newOne);
    }
}
```
* WOw!
    * Elegant, direct expression of algorithm
    * Dense use of pointers and recursion

## Lec14
### Algorithm analysis
* Problems can often be solved multiple ways
    * Work forward or backward, iteration or recursion, vector ot set, precise answer vs. estimation
    * e.g. sorting, searching, counting, ...
* How to evaluate/compare alternatives?
    * Often interested in execution performance
        * Time spent and memory used
    * Should also consider ease of developing, verifying, maintaining code
    * Be sure complexity is worthwhile!
* Brainstorm: algorithm to count people in this room
### Evaluating performance 
* Empirically - time with stopwatch
    * + Practical, real life results
    * - Have to write & debug code to test it
    * - Subject to variation (hardware, OS, other activity. etc.)
* Mathematically - analyze algorithm
    * + Can analyze without implementing code
    * + Abstract setting, not tied to specific encironment
    * - Empirical results may not exactly match (esp for small size inputs)
### Statement counts
```cpp
double CtoF(double cTemp)
{
    return cTemp*9.0/5.0 + 32;
}
```
* Each statement costs 1
    * Multiply, divide, add, return
    * Total = 4
    * Does value of input matter?
### More statement counts
```cpp
double Average(Vector<int> &v)
{
    int sum = 0;
    for (int i = 0l i < v.size(); i++)
        sum += v[i];
    return double(sum)/v.size();
}
```
* Count statements
    * Outside loop: 4 statements (init sum, init i, divide, return)
    * Loop body: 3 statements (test, add, incr i) per vector element
    * Total = 3N+4
* Does input matter?
    * Double size of vector - how change time required?
### More
```cpp
void GetExtremes(Vector<int> &v, int &min, int &max)
{
    min = max = v[0];
    for (int i = 1; i < v.size(); i++) {
        if (v[i] > max) max = v[i];
    }
    for (int i = 1; i < v.size(); i++) {
        if (v[i] < min) min = v[i];
    }
}
```
* Loop: test i, compare, update, incr  4 per iteration * N iterations
* 2 loops
* Outside loop: init i, init min/max
* 4 + 8N
### Comparing algorithms
* Count statements
    * CtoF(4)   Average(3N+4)   GetExtremes(8N+4)
    * Will GetExtremes always take more time than Average or CToF?
* Consider growth patterns
    * If Average takes 2 ms for 1000 elems, estimate for 2000 or 10000?
    * What about GetExtremes?
### Big-O notation
* Summarize statement counts
    * Use only largest term, ignore others, drop all coefficients
    * Time = 3n + 5 -> O(n)
    * Time = 10n - 2 -> O(n)
    * Time = 1/2n^2 - n -> O(n^2)
    * Time = 2^n + n^3 -> O(2^2)
* Describe growth curve of algorithm in the limit
    * Intuition: avoid details when they don't matter, and they don't matter when input size(N) is big enough
* More formally:
    * O(f(n)) is an upper-bound on the time required
        * Time(n) <= Cf(n) for some constant C and sufficiently large value n
### Using big-O to predict times
* For an O(n) algorithm:
    * 5,000 elements takes 3.2 seconds
    * 10,000 elements takes 6.4 seconds
    * 20,000 elements takes ...?
* For an O(n^2) algorithm:
    * 5,000 elements takes 2.4 seconds
    * 10,000 elements takes 9.6 seconds
    * 20,000 elements takes ...?
### Best-worst-average case
```cpp
bool Search(Vector<string> &names, string key)
{
    for (int i = 0; i < names.size(); i++)
        if (names[i] == key) return true;
    return false;
}
```
* What if key is first? middle? last? What if not found?
* Best case
    * Super-fast in some situations, often not that valuable
* Worst case
    * Upper bound on how bad it can get
* Average case
    * Averaged over all possible inputs, can be harder to compute precisely
### Analyzing recursive algorithms
```cpp
int Factorial(int n)
{
    if (n == 0) return 1;
    else return n * Factorial(n-1);
}
```
* T(n) is time used for input n  
    * T(n) = 1 if n is 0
    * T(n) = 1 + T(n-1) otherwise 
* This is a recurrence relation
### Solving recurrence
* Repeated substitution expands recurrence
    * T(n) = 1 + T(n-1)
    *      = 1 + (1 + T(n-2))
    *      = 1 + (1 + (1 + T(n-3)))  
* Generalize pattern
    *      = i*1 + T(n-i)
* Solve for i = n
    *      = n + T(0)
    *      = n + 1     -> O(n)
### Another example 
```cpp
void MoveTower(int n, char src, char dst, char tmp)
{
    if (n > 0) {
        MoveTower(n-1, src, tmp, dst);
        MoveOneDisk(src, dst);
        MoveTower(n-1, tmp, dst, src);
    }
}
```
* Set up recurrence
    * T(n) = 1      if n = 0
    *      = 1 + 2T(n-1) otherwise
### Solving recurrences
* Repeated substitution
    * T(n) = 1 + 2T(n-1)
    *      = 1 + (2 + 4T(n-2))
    *      = 1 + (2 + (4 + 8T(n-3)))
* Generalize pattern
    *      = 1 + 2 + 4 + 8 + 16 +...+ 2^(i-1) + 2^i*T(n-i)
    *      = 2^i -1 + 2^i * T(n-i)
* Solve for n-i = 0 (i = n)
    *      = 2^n - 1 + 2^n * T(0)
    *      = 2^(n+1) - 1
    *      = 2^(n+1)     -> O(2^n)

### Sorting!
* Very common to need data in order
    * Viewing, printing
    * Faster to search, find min/max, compute median/mode, etc
* Lots of different sorting algorithms
    * From the simple to very complex
    * Some optimized for certain situations (lots of duplicates, almost sorted, etc.)
    * Typically sort array/vector, but algorithms usually can be adapted for other data structures (e.g. linked list)
### Selection sort
* Select smallest and move to front
    * Search to find minimum
    * Place in first slot 
    * Could move elements over to make space, but faster just swap with current first
    * Repeat for second smallest, third, so on
### Selection sort code
```cpp
void SelectionSort(Vector<int> &arr)
{
    for (int i = 0; i < arr.size()-1; i++) {
        int minIndex = i;
        for (int j = i+1; j < arr.size(); j++) {
            if (arr[j] < arr[minIndex])
                minIndex = j;
        }
        Swap(arr[i], arr[minIndex]);
    }
}
```
### 10^6 instr/sec runtimes
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/runtime.png)
### Growth patterns
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/growthpatterns.png)

## lec15
### Selection sort analysis
* Count work inside loops
    * First iteration does N-1 compares, second does N-2, and so on
    * one swap per iteration
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/selectionsort.png)

### Insertion sort algorithm
* How you might sort hand of just-dealt cards...
    * Each subsequent element inserted into proper place
        * Start with first element (already sorted)
        * Insert next element relative to first
        * Repeat for third, fourth, etc.
        * Slide elements over to make space during insert
* Insertion sort code
```cpp
void InsertionSort(Vector<int> &v)
{
    for (int i = 1; i < v.size(); i++) {
        int cur = v[i]; // slide cur down into position to left
        for (int j=i-1; j >= 0 && v[j] > cur; j--)
            v[j+1] = v[j];
        v[j+1] = cur;
    }
}
```
### Insertion sort analysis
* Count work inside loops
    * First time inner loop does 1 compare/move
    * Second iteration does <= 2 compare/move, third <= 3, and so on
    * Last iteration potentially N-1 comparisons
* Cases
    * What is best case? Worst case?
    * Average (expected) case?
### Insertion vs Selection
* Big O?
* Mix of operations?
    * Number of comparisons vs moves
* Best/worst inputs?
* Ease of coding?
* Why do we need multiple algorithms?
### Quadratic growth
* In clock time 
    * 10,000    3 sec
    * 20,000    13 sec
    * 50,000    77 sec
    * 100,000   5 min
* Double input -> 4X time
    * Feasible for small inputs, quickly unmanageble
* Halve input -> 1/4 time
    * Hmm... can recursion save the day>
    * If have two sorted halves, how to produce sorted full result?

### Mergesort idea
* "Divide and conquer" algorithm
    * Divide input in half
    * Recursively sort each half
    * Merge two halves together
* "Easy-split hard-join"
    * No complex decision about which goes where, just divide in middle
    * Merge step preserves ordering from each half
### Merge sort code
```cpp
void MergeSort(Vector<int> &v)
{
    if (v.size() > 1) {
        int n1 = v.size()/2;
        int n2 = v.szie() - n1;
        Vector<int> left = Copy(v, 0, n1);
        Vector<int> right = Copy(v, n1, n2);
        MergeSort(left);
        MergeSort(right);
        Merge(v, left, right);
    }
}
```
* T(N) = N + 2T(N/2)
### Mergesort analysis
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/mergesort.png)
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/mergesort2.png)
### Quadratic vs linearithmic
* Compare SelectionSort to MergeSort
    * 10,000    3 sec   .05 sec
    * 20,000    13 sec  .15 sec
    * 50,000    78 sec  .38 sec
    * 100,000   5 min   .81 sec
    * 200,000   20 min  1.7 sec
    * 1,000,000 8 hrs(est)  9 sec
* O(NlogN) is pretty good, can we do better?
    * Theoretical result (beyond scope of 106B) no general sort algorithm better than NlogN
    * But a better NlogN in practice?
### Quicksort idea
* "Divide and conquer" algorithm
    * Divide input into low half and high half
    * Recursively sort each half
    * Join two halves together
* "Hard-split easy-join"
    * Each element examined and placed in correct half
    * Join step is trivial

## lec16
### Partitioning for quicksort
* Partition step uses "pivot" value
    * All elems less than pivot in one half, all elems greater in other
* How to choose pivot to get even split?
    * How to know range for values in the input at all?
* Simple choice: use first elem as pivot
    * Known to be in range at list
    * We'll examine this choice later
### Quicksort code
```cpp
void Quicksort(Vector<int> &v, int start, int stop)
{
    if (stop > start) {
        int pivot = Partition(v, start, stop);
        Quicksort(v, start, pivot-1);
        Quicksort(v, pivot+1, stop);
    }
}
```
* Assume ideal 50/50 split
    * T(N) = N + 2T(N/2)    => O(NlogN)
* Assume bad 90/10 split
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/quicksort.png)
* Assuming worst N-1/1 split
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/quicksort2.png)
### What input has worst split?
* If pivot is smallest in input
    * Input already in sorted order!
* If pivot is largest in input
    * Input in reverse sorted order
* Others not so likely
    * Smallest, then largest, etc
* "Degenerate" case
    * May tolerate poor worst-case outcome if input is unlikely
    * Does that apply here?
### What to do?
* Choose pivot differently
* Compute actual median
    * O(N) algorithm exists for this
    * Guarantee 50/50 split
* "Median of three"
    * Approximate median
    * Choose middle from first, last, mid
* Random
    * Choose random element
    * Worst-case still possible, but unlikely
### In clock time
* Compare MergeSort to Quicksort
    * 10,000      .05 sec   .008 sec
    * 20,000      .15 sec   .01 sec
    * 50,000      .38 sec   .11 sec
    * 100,000     .81 sec   .21 sec
    * 200,000     1.7 sec   .45 sec
    * 1,000,000    9  sec   2.6 sec
* Both O(NlogN) but Quicksort's advantage
    * No secondary storage
    * Moves elements more quickly to correct position
### A proliferation of Swap
```cpp
void SwapChars(char & ch1, char & ch2)
{
    char tmp = ch1;
    ch1 = ch2;
    ch2 = tmp;
}
void SwapInts(int & num1, int & num2)
{
    int tmp = num1;
    num1 = num2;
    num2 = tmp;
}
void SwapStrings(string & str1, string & str2)
{
    string tmp = str1;
    str1 = str2;
    str2 = tmp;
}
// and so on ...
```
* 为什么没有泡沫排序？更难写，易出错，又更慢 
### Function template: 一劳永逸地写出排序
* Same general idea as class template
    * Generic function uses same algorithm for any type
    * Write/test/debug once, use in many situations
* Template written using one or more placeholders
    * e.g. swap exchanges two values of any type
* Using function instantiates specific version
    * Call to swap passing two doubles uses a different version than a call passing two strings
* Compiler infers placeholder type if possible
    * 用法比class template简单，不用<>来表示什么类型，complier会根据传递的参数来infer  
    * So may not need explicit Swap<double>
    * (Unlike classes where <> always required)
### Swap function template
```cpp
template <typename Type>
void Swap(Type & one, Type & two)
{
    Type tmp = one;
    one = two;
    two = tmp;
}
```
* Template from which to create many Swap functions
    * Can create Swap for ints, chars, strings, etc. from same pattern
### Using function template
```cpp
int main()
{
    int a = 12, b = 45;
    string str1 = "apple", str2 = "orange";
    Swap(a, b); // infers Swap<int>
    Swap(str1, str2); // infers Swap<string>
}
```
* Compiler infers placeholder type if possible
    * Can explicitly call Swap<int>(a, b) but usually isn't necessary
### Template instantiation
```cpp
template <typename Type>
void Swap(Type & one, Type & two)
{
    Type tmp = one;
    one = two;
    two = tmp;
}
```
* What happens on call to Swap?
```cpp
int a = 4, b = 19;
Swap(a, b);
```
* Template instantiated with Type => int
* Compiler internally names this version Swap<int>
* Code is then compiled
### Instantation errors
```cpp
template <typename T>
void PrintVector(Vector<T> &v)
{
    for (int i = 0; i < v.size(); i++)
        cout << v[i] << " "; // this line is trying to use << on a coordT
        cout << endl;
}
```
* Try to instantiate PrintVector for non-primitive type
```cpp
struct coordT {
    double x, y;
}
Vector<coordT> c;
PrintVector(c);
```
* Compiler's response
![](https://github.com/liangsihuang/Stanford-CS106B/raw/master/pics/error.png)
* Template error reporting
    * Template itself is largely ignored by compiler
    * When called, version is created with placeholder filled in, and only then is compiled
    * Errors within template now reported, triggered by client's instantiation
* Template may have hidden requirements on type
    * e.g. Uses << to output or compares using ==
    * Code instantiated won't compile if type doesn't support needed ops
    * Most common operators to watch for: output, assignment, comparison/relational
### Sort template
```cpp
template <typename Type>
void sort(Vector<Type> &v)
{
    for (int i = 0; i < v.size()-1; i++) {
        int minIndex = i ;
        for (int j = i+1; j < v.size(); j++) {
            if (v[j] < v[minIndex])
                minIndex = j;
        }
        Swap(v[i], v[minIndex]);
    }
}
```
* Template functions awesome for algorithms
    * Searching (linear/binary), sorting(all varieties), median, mode, permute, summarize, remove, duplicates, etc.
* Client use of Sort template
```cpp
int main()
{
    Vector<int> nums = ...;
    Sort(nums);

    Vector<string> strs = ...;
    Sort(strs);
}
```
* What must be true about the element type?
    * Will every type work?
    * Consider:
    ```cpp
    struct coordT {
        double x, y;
    }
    Vector<coordT> pts;
    Sort(pts);
    ```
### Fully generic sort
* Sort function template uses < to compare elements
    * This works for some types, but not all
* Division between client/implementor
    * Client knows how data is to be compared
    * Implementor is the one doing the actual comparing
* Need client/implementor cooperation
    * Client tells implementor how to appropriately compare elements
* Add function parameter
    * Client knows how to compare elements, it supplies this knowledge in the form of a function pointer
    * Callback function - implementation "calls back" to client
### Sort template with callback fn
```cpp
template <typename Type>
void Sort(Vector<Type> &v, int (cmp)(Type, Type))
{
    for (int i = 0; i < v.size() -1; i++) {
        int minIndex = i;
        for (int j = i+1; j < v.size(); j++) {
            if (cmp(v[j], v[minIndex]<0) < 0)
                minIndex = j;
        }
        Swap(v[i], v[minIndex]);
    }
}
```
* Now can truly work for all types!
    * Client supplies function pointer to handle compare

## lec17
### Supplying callback fn
```cpp
int CoordCmp(coordT c1, coordT c2)
{
    if (c1.x < c2.x) return (-1);
    else if (c1.x > c2.x) retunr (1);
    else if (c1.y < c2.y) return (-1);
    else if (c1.y > c2.y) return (1);
    else return (0);
}
int main()
{
    Vector<coordT> pts = ... ;
    Sort(pts, CoordCmp);
}
```
### One last convenience
* Currently, client must provide callback
    * Not as convenient when could use built-in < when it would work
* Add behavior to use < by default
* Client can supply function only when needed
    * Default atgument for comparator is generic compare callback
    * OperatorCmp invokes built-in < on arguments
### Final version of Sort template
```cpp
// (from CS106b cmpfn.h)
template <typename Type>
int OperatorCmp(Type one, Type two)
{
    if (one == two) return 0;
    return (one < two ? -1 : 1);
}  // ???
template <typename Type>
void Sort(Vector<Type> &v, int (cmp)(Type one, Type two) = OperatorCmp)
{
    for (int i = 0; i < v.size() -1; i++) {
        // rest of code as before
    }
}
```
### Use of Sort template
```cpp
int ReverseCmp(int a, int b)
{
    if (a < b) return 1;
    else if (a > b) return -1;
    return 0;
}
int main()
{
    Vector<int> num = ...;
    Sort(num);
    Sort(num, ReverseCmp);
}
```