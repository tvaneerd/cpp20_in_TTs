#### TL;DR

C++14 gave us _generic lambdas_ where `auto` was used instead of a fixed type, turning the lambda into something of a template. ie

`auto inc = [](auto thing) { thing++; } // works on anything that can be incremented!`

But you couldn't do a bunch of other templaty things, like say `typename T` and then use `T`, etc.
Now you can.

#### Bonus

`[]<>(){}` is now valid syntax, and this is a valid line of code:

`[]<>(){}();`

#### Examples

Limit a lambda to only work with vector.
In addition to making _intent_ more clear,
a compiler error from the left (if you don't pass a vector) would be much worse than from the right.

<table>
<tr>
<th>
C++
</th>
<th>
C++20
</th>
</tr>
<tr>
<td  valign="top">

<pre lang="cpp">
template&lt;typename T&gt;
struct is_vector                 : std::false_type{};
template&lt;typename T>
struct is_vector&lt;std::vector&lt;T&gt;&gt; : std::true_type{};

auto f = [](auto vector) {
   static_assert(is_vector&lt;decltype(vector)&gt;::value, "");
   //...
};
</pre>
</td>
<td  valign="top">

<pre lang="cpp">
 
 
 
 
 
auto f = []&lt;typename T&gt;(std::vector&lt;T&gt; vector) {

   //...
};
</pre>
</td>
</tr>
</table>


Using _types_ inside a generic lambda was hard, verbose, annoying:

<table>
<tr>
<th>
C++
</th>
<th>
C++20
</th>
</tr>
<tr>
<td  valign="top">

<pre lang="cpp">
// luckily vector gives you a way to get its inner type,
// most templates don't!
auto f = [](auto vector) {
   using T = typename decltype(vector)::value_type;
   //...
};
</pre>
</td>
<td  valign="top">

<pre lang="cpp">
 
 
auto f = []&lt;typename T&gt;(std::vector&lt;T&gt; vector) {

   //...
};
</pre>
</td>
</tr>
</table>





#### See also

Examples are mostly from the original proposal (https://wg21.link/p0428) however Louis didn't use Tony Tables in the original. Booo, hiss.
The proposal also includes more examples of how ugly it could get in C++14.

