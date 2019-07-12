<table><tr><td>
```
// variable declarations always with initialization
const a:=4
var b:="bee"
```
</td><td>
```cpp
const auto a=4;
auto b="bee";
```
</td></tr><tr><td>
```
// implicitly const argument
function compute(i:int); end
```
</td><td>
```cpp
void compute(const int i){}
```
</td></tr><tr><td>
```
// mutable argument
function compute(var i:int); end
```
</td><td>
```cpp
void compute(int i){}
```
</td></tr><tr><td>
```
// default value
function compute(i:=1338)
```
</td><td>
```cpp
void compute(decltype(1338) i=1338){}
```
</td></tr><tr><td>
```
// omitting argument type creates templated function
function compute(var a); end
```
</td><td>
```cpp
template <typename T0>
void compute(T0 a){}
```
</td></tr><tr><td>
```
// simple return
function compute -> int
	return 0
end
```
</td><td>
```cpp
int compute(){
	return 0;
}
```
</td></tr><tr><td>
```
// argument-dependent return type
function compute(a) -> typeof(a)
	return a*2
end
```
</td><td>
```cpp
template <typename T0>
auto compute(T0 a) -> decltype(a){
	return a*2;
}
```
</td></tr><tr><td>
```
// tuple return
function compute -> (string, var int) // omitting var is implicitly const
	return "take", 5
end
```
</td><td>
```cpp
auto compute() -> std::tuple<std::string, var int>{
	return std::tuple<std::string, var int>{"take", 5};
}
```
</td></tr><tr><td>
```
// struct return
// does not work with typeof
function compute -> (a:int, b:string, var c:double)
	return 1, "two", 3.0
end
```
</td><td>
```cpp
struct compute__return{
	const int a;
	const std::string b;
	double c;
};
compute__return compute(){
	return {1, "two", 3.0};
}
```
</td></tr><tr><td>
```
// work with auto, return simple type if just one return, tuple otherwise
// very convenient but we're back to the medieval age of having to define functions above their use
function compute(var a:int, b:=0)
	return a*2, "awa"
end

function inc(x)
	return x+1
end
```
</td><td>
```cpp
auto compute(int a, decltype(0) b=0){
	return std::make_tuple(a*2, "awa");
}
template <typename T0>
auto inc(T0 x){
	return x+1;
}
```
</td></tr><tr><td>
```
// create kwarg-wrapper for using external library functions with kwargs
kwargs func(a:int, b:=.5, c:="c") -> int
// implicitly done for all functions defined in cinnamon

// calls:
func(1, 2)
func(3, c:="cc")
```
</td><td>
```cpp
namespace func__params {
    std::size_t constexpr a = 0;
    std::size_t constexpr b = 1;
    std::size_t constexpr c = 2;
};

template<typename... Ts>
int func__kwargs(Ts&&... ts) {
    return func(
        kw::arg_get<0>(std::forward<Ts>(ts)...),
        kw::arg_get<1>(std::forward<Ts>(ts)..., kw::arg<1>(.5)),
        kw::arg_get<2>(std::forward<Ts>(ts)..., kw::arg<2>("c"))
    );
}

// calls:
func(1, 2)
func__kwargs(3, kw::arg<func__params::c>("cc")
```
</td></tr><tr><td>
```
//immediately invoked lambda:
const x:=scribble
	if a>b
		return a
	else
		return b
	end
end + 3
```
</td><td>
```cpp
const auto x=[&](){
	if(a>b){
		return a;
	}
	else{
		return b;
	}
```
</td></tr></table>
