New Keywords - consteval and constinit
---

### consteval 
A function specified with the keyword **consteval** is an immediate function that is executed at compile-time.   
Each call to this function must produce a compile-time constant expression.   
  
This immediate function must satisfy all requirements applicable to **constexpr** functions.
<table>
<tr>
<th>
C++14 - constexpr function
</th>

<th>
C++20 - consteval function
</th>

</tr>
<tr>
<td  valign="top">
<pre lang="cpp">
// executes at compile-time or run-time
constexpr int squareNumX(int n) 
{
    return n * n;
}
</pre>
</td>

<td  valign="top">
<pre lang="cpp">
// executes at compile-time
consteval int squareNumV(int n) 
{
    return n * n;
}
</pre>
</td>
</tr>

</table>

Immediate functions (specified with consteval) cannot be applied to:
- destructors
- functions which allocate or deallocate

<table>
<tr>
<th>
C++14
</th>
<th>
C++20
</th>
</tr>
<tr>
<td  valign="top">
<pre lang="cpp">
{
    constexpr int a = squareNumX(100);
    int b = squareNumX(100);
}
</pre>
</td>

<td  valign="top">
<pre lang="cpp">
{
    constexpr int a = squareNumV(100);                                  
    int b = sqrC(100);    //ERROR - b is not a constant expression
}
</pre>
</td>

</tr>
</table>

### constinit
Applied to variables with static storage duration or thread storage duration.  
**constinit** variables are initialized at compile-time.

**constinit** does not imply constness such as const or constexpr(2).  
<table>
<tr>
<th>
C++14 - constexpr
</th>
<th>
C++20 - constinit
</th>
</tr>

<tr>
<td  valign="top">
<pre lang="cpp">
// static storage duration
constexpr int a = 100;  
  
int main(){
    // constexpr or const can be created as a local 
        variable but cannot be modifed
    ++a;                      // ERROR 
    constexpr auto b = 100;   
}
</pre>
</td>

<td  valign="top">
<pre lang="cpp">
// static storage duration
constinit int a = 100;  
  
int main(){
    // constinit cannot be created as a local
        variable but can be modified
    ++a; 
    constinit auto b = 100;  // ERROR
  
    // has thread storage duration
    constinit thread_local auto res3 = 100;
}
</pre>
</td>
</tr>

</table>