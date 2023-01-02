## L2 Containers

#### ADT (abstract data type)

Combines data with valid operation and their behaviors on stored data.

#### Data Structure

Provides a concrete implementation of an ADT.

### Stack

**Interface**

```c++
push(object);
pop();
object &top();
size();
empty();
```

**Implementation**

```c++
std::deque<>;
std::list<>;
std::vector<>;
```

### Queue

**Interface**

```c++
push(object);
pop();
object &front();
size();
empty();
```

**Implementation**

```c++
std::deque<>;
std::list<>;
```

### Deque

Double-Ended Queue

**Interface**

```c++
push_front(object);
pop_front();
object &front();
push_back(object);
push_back();
object &back();
size();
empty();
```

**Implementation**

```c++
std::deque<>;
```



## L13 Strings and Sequences

```c++
bool is_palindrome(const string &s) {
    return equal(begin(s), begin(s) + s.size()/2, rbegin(s));
}
```

Implement < and == for a class supports all the other comparison operators
