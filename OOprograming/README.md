# OO programing
### Why object-oriented programming? -From lec17
* Most programs organized around data
    * Making data the focus is good fit
* Objects leverage analogy to real world
    * Time, Stack, Event, Message, etc.
* Abstraction clears away details
    * Can focus on other tasks instead
* Encapsulation provides robustness
    * Object internals can be kept private and secure
* Modularity in development
    * Design, develop, test classes independently
* Potential for reuse
    * Class is tidy package that can be re-used in other programs
### Class division
* java 没有分，都在一个文件，有好有坏, 好处：一处改就好，坏处：细节暴露给用户
* Client
    * Code file `client.cpp`
    * Contains code using objects
    * `#include class.h` interface for each class used
* Interface
    * Header file `class.h`
    * Contains declaration of class interface (data members and member functions)
* Implementation
    * Code file `class.cpp`
    * Contains code for class member functions
    * `#include class.h` interface
### Class interface in.h file
* Class interface lists data and operations
    * Data members (fields)
    * Member functions (methods)
    * Use public/private sections to control visibility to clients
```cpp
/* File: time.h */
class Time {
    public: 
        void setHour(int newValue);
        int getHour();
        void shiftBy(int dh, int dm);
        string toString();
        /* And so on ... */
    private:
        int hour, minute;
}
```
### Storage for objects
* A Time object has two data members
* Object about same size as comparable struct
* Declare Time object on stack
* Each Time object has its own copy of data member
* When accessing members, it is always a particular object's copy
### Accessing members
* Members accessed like struct fields
    * Usually declare on stack, but can use new for heap
    * Use . or -> depending on whether pointer or not
* Client can access `public` features
```cpp
Time t;
t.setHour(3);
cout << t.getHour();
t.hour = 3; // only ok if field public
```
* Object being messaged is called `receiver`
* Error for client to access private member
### Class implementation
* Implementation goes in .cpp file
    * Must `#include class.h` file
    * Contains code for member functions
    * Function name must include class scope (else assumed global function)
```cpp
/* File: time.cpp */
# include "time.h"
void Time::setHour(int newValue)
{
    hour = newValue;
}
string Time::toString()
{
    return IntegerToString(hour) + ":" + IntegerToString(minute);
}
```
### implementing member function
* Members of receiver accessible in member function
```cpp
void Time::shiftBy (int dh, int dm) {
    hour += dh;
    minute += dm;
}
```
* Can send other messages to receiver
```cpp
void Time::shiftBy(int dh, int dm) {
    setHour(hour + dh);
    setMinute(minute + dm);
}
```
* Special variable: this (pointer to receiver)
```cpp
void Time::shiftBy(int dh, int dm) {
    this -> hour += dh;
    this -> setMinute(minute + dm);
}
```
### Maintaining object consistency
* Setters can constrain to correct range
```cpp
void Time::setHour(int newValue) {
    if (newValue < 1) hour = 1;
    else if (newValue > 12) hour = 12;
    else hour = newValue;
}
void Time::setMinute(int newValue) {
    minute = newValue % 60;
}
```
* What if data members were public?
* What is advantage of making all access, even within implementation, go through setters?
### Constructors
* Special function to init newly created object
    * Data members for new object are uninitialized otherwise (not automaticaly set to zero as in Java)
* Called automatically when declared/allocated
    * Allocation and initialization go hand-in-hand
* Special prototype
    * Must have exact same name as class
    * No return type
    * Can have whatever parameters you need
### Add Time constructor
* Declare constructor in `time.h` interface
```cpp
class Time {
    public:
        Time(int hr, int min);
        void setHour(int newValue);
}
```
* Implement constructor in `time.cpp`
```cpp
Time::Time(int hr, int min) {
    hour = hr;
    mimute = min;
}
```
* Give args to constructor when declaring
`Time t(2, 15)`
### Destructors
* Special function to clean up object
    * Data members may be orphaned otherwise
    * Called automatically on delete or exiting scope of object
* Special prototype
    * Same name as class prefixed with ~
    * No parameters
    * No return type
* Not always needed
    * Only if dynamically allocated members to delete, open files to close, etc.
### Basic thoughts on object design
* Never let object get into malformed state
    * No public data members
    * Correctly initialize all members in constructor
    * Only provide setters if needed, be sure properly constrained
* Object is responsible for own behavior
    * Interface includes complete set of operations
    * Need to print a Time? Add print method to class, don't pull out the hour/minute fields and do it yourself
    * Same for converting time to string, comparing two times, shifting a time forward, etc/
### Internal vs external representation
* Client might expect Time work in terms of hours and minutes
    * But this is difficult to manipulate internally
    * Considering mixing in AM/PM, too
    * What is required to shift time or compare?
* Consider comparing two Times:
```cpp
bool Time::isLessThan(Time other) {
    return ((hour < other.hour) || (hour == other.hour && minute < other.minute));
}
```
* Is there a better way?
### Better representation
* If Time stored in military 24-hour time?
    * Somewhat easier to shift, avoids problems with AM/PM
* If internally tracked as munutes since midnight...?
    * Trivial to implementation "lessThan" operation
    * Trivial to shift, easy to handle wrap around
* Can provide accessor for hour/minute if needed
    * Simple to compute from internal representation
* Does changing internal data affect client use?
    * What impact does making things public have on implementation flexibility?
### ADTs (abstract data types)
* Client uses class as abstraction
    * Invokes public operations only
    * Internal implementation not relevant!
* Client can't and shouldn't muck with internals
    * Class data should private
* Imagine a "wall" between client and implementor
    * Wall prevents either from getting involved in other's business
    * Interface is the "chink" in the wall
        * Conduit allows controlled access between the two
* Consider Lexicon
    * Abstraction is a word list operations to verify word/prefix
    * How does it store list? using array? vector? set? does it matter to client?

## lec18 Let's implement Vector
### Why ADTs?
* Abstraction
    * Client insulated from details, works at higher-level
* Encapsulation
    * Internals private to ADT, not accessible by client
* Independence
    * Separate tasks for each side (once agreed on interface)
* Flexibility
    * ADT implementation can be changed without affecting client
## lec19 Implement Stack and Queue
### Rules for template implementers
* Template header everywhere!
    * `template <typename ElemType>` used to introduce class interface (in.h) and every member function (in.cpp)
    * Scope of class is `Class<ElemType>::`
* All code written in terms of placeholder
    * Use `ElemType` to declare private data members, local variables, parameters, return types, etc.
* Quirky C++ template compilation
    * `class.cpp` is not added to the project (not compiled normally!)
    * `class.h` has `#include "class.cpp"` at end to pull in code
    * Including a .cpp file is wacky, only used for class template situation

## Lec20