<table><tr><td><pre>
// variable declarations
// always with initialization
const a:=4
var b:="bee"
</pre>
</td><td>
<pre>
const auto a=4;
auto b="bee";
</pre>
</td></tr><tr><td><pre>
// implicitly const argument
function compute(i:int); end
</pre>
</td><td>
<pre>
void compute(const int i){}
</pre>
</td></tr><tr><td><pre>
// mutable argument
function compute(var i:int); end
</pre>
</td><td>
<pre>
void compute(int i){}
</pre>
</td></tr><tr><td><pre>
// default value
function compute(i:=1338)
</pre>
</td><td>
<pre>
void compute(decltype(1338) i=1338){}
</pre>
</td></tr><tr><td><pre>
// omitting argument type
// creates templated function
function compute(var a); end
</pre>
</td><td>
<pre>
template <typename T0>
void compute(T0 a){}
</pre>
</td></tr><tr><td><pre>
// simple return
function compute -> int
    return 0
end
</pre>
</td><td>
<pre>
int compute(){
    return 0;
}
</pre>
</td></tr><tr><td><pre>
// argument-dependent return type
function compute(a) -> typeof(a)
    return a*2
end
</pre>
</td><td>
<pre>
template &lt;typename T0>
auto compute(T0 a) -> decltype(a){
    return a*2;
}
</pre>
</td></tr><tr><td><pre>
// tuple return
// omitting var is implicitly const
function compute -> (string, var int)
    return "take", 5
end
</pre>
</td><td>
<pre>
auto compute() ->
        std::tuple&lt;std::string, var int>{
    return std::tuple&lt;std::string, var int>
            {"take", 5};
}
</pre>
</td></tr><tr><td><pre>
// struct return
// does not work with typeof
function compute ->
        (a:int, b:string, var c:double)
    return 1, "two", 3.0
end
</pre>
</td><td>
<pre>
struct compute__return{
    const int a;
    const std::string b;
    double c;
};
compute__return compute(){
    return {1, "two", 3.0};
}
</pre>
</td></tr><tr><td><pre>
// work with auto, return simple type
// if just one return, tuple otherwise
// very convenient but we're back to the
// medieval age of having to define
// functions above their use
function compute(var a:int, b:=0)
    return a*2, "awa"
end

function inc(x)
    return x+1
end
</pre>
</td><td>
<pre>
auto compute(int a, decltype(0) b=0){
    return std::make_tuple(a*2, "awa");
}
template &lt;typename T0>
auto inc(T0 x){
    return x+1;
}
</pre>
</td></tr><tr><td><pre>
// create kwarg-wrapper for using external
// library functions with kwargs
kwargs func(a:int, b:=.5, c:="c") -> int

// implicitly done for all functions
// defined in cinnamon

// calls:
func(1, 2)
func(3, c:="cc")
</pre>
</td><td>
<pre>
namespace func__params {
    std::size_t constexpr a = 0;
    std::size_t constexpr b = 1;
    std::size_t constexpr c = 2;
};

template&lt;typename... Ts>
int func__kwargs(Ts&&... ts) {
    return func(
        kw::arg_get&lt;0>(std::forward&lt;Ts>
                (ts)...),
        kw::arg_get&lt;1>(std::forward&lt;Ts>
                (ts)..., kw::arg&lt;1>(.5)),
        kw::arg_get&lt;2>(std::forward&lt;Ts>
                (ts)..., kw::arg&lt;2>("c"))
    );
}

// calls:
func(1, 2)
func__kwargs(3, kw::arg&lt;func__params::c>("cc")
</pre>
</td></tr><tr><td><pre>
//immediately invoked lambda:
const x:=scribble
    if a>b
        return a
    else
        return b
    end
end + 3
</pre>
</td><td>
<pre>
const auto x=[&](){
    if(a>b){
        return a;
    }
    else{
        return b;
    }() + 3;
</pre>
</td></tr></table>
