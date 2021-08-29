# optional
Simple monadic interface for std::optional
## Installation

Just copy and include optional.h header in your project

## Usage

All operations are in bsc::optional namespace so it is advised to add
```c++
using namespace bsc::optional;
```
At begining of a scope. 

Supported operations:

### map
Transforms the value in optional to something else (if present).

```c++
using namespace bsc::optional;
std::optional<int> optInt = 5;
auto timesTwo  = [](auto i) { return i * 2; };

auto result = optInt | map(timesTwo); //result is optional<int> containing 10
```

### flatMap
Transforms the value in optional to something else (if present). 
Same as map, but for functions that return optional

```c++
using namespace bsc::optional;
std::optional<std::string> optBadString = "not a number";
std::optional<std::string> optGoodString = "10";

auto toInt = [](auto i){
    try {
        return std::make_optional<int>(std::stoi(i));
    } catch (const std::invalid_argument&) {
        return std::optional<int>{};
    }
};

auto resultBad = optBadString | flatMap(toInt); //empty optional
auto resultGood = optGoodString | flatMap(toInt); //optional<int> with value = 10
```

### filter

Returns the same value if it matches the predicate or empty optional if it doesn't.

```c++
    using namespace bsc::optional;
    auto isEven               = [](auto i) { return i % 2 == 0; };
    std::optional<int> optFour = 4;
    auto resultEven = optInt | filter(isEven); //result is optional with value =4
    
    std::optional<int> optFive = 5;
    auto resultOdd = optInt | filter(isEven); //result is empty optional
```

### orElse, orElseGet, orElseThrow

If optional is not empty, then return its value or else return supplied object (orElse), result of a callable(orElseGet) or throw exception (orElseThrow) 

```c++
    using namespace bsc::optional;
    auto isEven               = [](auto i) { return i % 2 == 0; };
    std::optional<int> optFour = 4;
    auto resultEven = optInt | filter(isEven) | orElse(10); //result is int = 4
    
    std::optional<int> optFive = 5;
    auto resultOdd = optInt | filter(isEven) | orElse(10); //result is int = 10
```

### ifPresent

If optional hold value, call function passes as argument. 
```c++
using namespace bsc::optional;
std::optional<int> optInt = 5;
auto print  = [](auto i) { std::cout << i << std::endl };

auto result = optInt | ifPresent(print); // prints "5" to standard output
```

## License

optional.h is released under MIT license. 

#Todo

* create cmake configuration for easier installation
* create package for package managers like conan

