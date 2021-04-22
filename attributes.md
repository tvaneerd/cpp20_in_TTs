some new [[attributes]]
---


**[[no_unique_address]]**

If a data member need not have a distinct address, then the compiler may optimise it to occupy no space.  
  
For an object to occupy no space, these requirements must apply:
1. The object is empty.  
   i. Objects of type std::allocator do not actually store anything (stateless allocators)
   2. Empty struct/class 
2. The second object is a non-static data member from the same class that must be:  
     i. Of a different type or have a different address in memory
  
<table>
<tr>
<th>
C++17
</th>
<th>
C++20
</th>
</tr>
<tr>
<td  valign="top">

<pre lang="cpp">
struct Empty {}; 
 
template < typename T, typename S >
struct Test {
    T t;
    S s;
};

int main() {
    static_assert(sizeof(Test< Empty,char >) >= sizeof(char) + 1); // 2
    static_assert(sizeof(Test< Empty,int >) >= sizeof(int) + 1);   // 8
}
</pre>
</td>
<td  valign="top">

<pre lang="cpp">
struct Empty {}; 
 
template < typename T, typename S >
struct Test {
    [[no_unique_address]] T t;
    [[no_unique_address]] S s;
};

int main() {
    // Different type of objects 
    static_assert(sizeof(Test< Empty,char >) == sizeof(char)); // 1
    static_assert(sizeof(Test< Empty,int >) == sizeof(int));   // 4

    // Same objects
    static_assert(sizeof(Test< Empty,Empty >) == 2);   
}
</pre>
</td>
</tr>

</table>

  
**[[likely]]** and **[[unlikely]]**

The use of these attributes aid the compiler in optimizing for the case in which the paths of execution
are more or less likely than any alternative paths of execution that do not include the usage of these attributes.
<table>
<tr>
<th>
C++17
</th>
<th>
C++20
</th>
</tr>
<tr>
<td  valign="top">

<pre lang="cpp">
{
    const auto start = std::chrono::high_resolution_clock::now();
    for (int i = 1; i <= 9000; i++) {
        if (i % 5 == 0) {
            std::cout << i << " is div by 5" << std::endl;
        }
        else {
            continue;
        }
    }
    const std::chrono::duration<double> diff = std::chrono::high_resolution_clock::now() - start;
    std::cout << "Time: " << diff.count() << " sec " << std::endl;
}
// When using a number < 9000, the attributes did not seem to have much of an impact
</pre>
</td>
<td  valign="top">

<pre lang="cpp">
{
    const auto start = std::chrono::high_resolution_clock::now();
    for (int i = 1; i <= 9000; i++) {
        if (i % 5 == 0) [[unlikely]] {
            std::cout << i << " is div by 5" << std::endl;
        }
        else [[likely]] {
            continue;
        }
    }
    const std::chrono::duration<double> diff = std::chrono::high_resolution_clock::now() - start;
    std::cout << "Time: " << diff.count() << " sec " << std::endl;
}

</pre>
</td>
</tr>

<tr>
<th>
Output
</th>
<th>
Output
</th>
</tr>

<tr>
<td  valign="top">
Time: 0.00655365 sec 
</td>
<td  valign="top">
Time: 0.00608944 sec  
</td>
</tr>

</table>

Both attributes do not need to be used, only using one of **[[likely]]** or **[[unlikely]]** would suffice.  
  
They can also be used in switch statements.  

<table>
<tr>
<th>
C++20
</th>
</tr>
<tr>

<td  valign="top">
<pre lang="cpp">
bool enoughCoffee(int coffeeLeft) {
    switch(coffeeLeft){
        case 0: return false;
        [[likely]] case 1: return false;
    }
    return true;
}
</pre>
</td>
</tr>

</table>
