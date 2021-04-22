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
    int x = 100;
    const int y = 100;  
    int a = squareNumX(x);
    constexpr int b = squareNumX(x); // ERROR
    int c = squareNumX(y);
    constexpr int d = squareNumX(y);
}
  
// Error when x is not a constant expression but b is
</pre>
</td>

<td  valign="top">
<pre lang="cpp">
{
    int x = 100;
    const int y = 100;
    int a = squareNumV(x);              // ERROR
    constexpr int b = squareNumV(x);    // ERROR
    int c = squareNumV(y);
    constexpr int d = squareNumV(y);  
}
// Error when the function argument (x) is not a constant expression 

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
int main() 
{
      ++a;                      // ERROR 
      constexpr auto b = 100;   
}
// Error since constexpr or const cannot be modifed 
// constexpr or const can be created locally
</pre>
</td>

<td  valign="top">
<pre lang="cpp">
// static storage duration
constinit int a = 100;  
int main()
{  
      ++a; 
      constinit auto b = 100;  // ERROR
      // b has thread storage duration
      constinit thread_local auto b = 100;
}
// Error since constinit cannot be created locally
// constinit can be modified
</pre>
</td>
</tr>

</table>
