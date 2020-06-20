`std::format` is a new way to format text which

- separates formatting from outputting
- is more type-safe
- allows reordering


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

std::printf("%d baskets of %s", n, desc);

//OR

std::cout << n << " baskets of " << desc;
 
</pre>
</td>
<td  valign="top">

<pre lang="cpp">

std::cout << std::format("{} baskets of {}", n, desc);
 
</pre>
</td>
</tr>
</table>


#### Ordering


<table>
<tr>
<th>
:-(
</th>
<th>
Apples! 5 baskets left
</th>
</tr>
<tr>
<td  valign="top">

<pre lang="cpp">

std::printf("%s! %d baskets left", n, desc);

//OR

std::cout << desc << "! " << n << " baskets left";
 
</pre>
</td>
<td  valign="top">

<pre lang="cpp">

std::cout <<
   std::format("{1}! {0} baskets left", n, desc);
 
</pre>
</td>
</tr>
</table>

####  Chrono


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

???

 
</pre>
</td>
<td  valign="top">

<pre lang="cpp">

std::cout << std::format("The time is {}", std::system_clock::now());
 
</pre>
</td>
</tr>
</table>

