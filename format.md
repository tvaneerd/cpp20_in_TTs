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
whoops!
</th>
<th>
Apples! 5 baskets left
</th>
</tr>
<tr>
<td  valign="top">

<pre lang="cpp">

std::printf("%s! %d baskets left", n, desc);
 
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




#### Formatting




Detailed formatting is similar to printf, but actually based on [Python!](https://docs.python.org/3/library/string.html#formatspec)

The general form is

_fill-and-align sign `#` `0` width precision `L` type_ 		

(with each part optional).





<table>
<tr>
<th>
</th>
<th>
format
</th>
<th>
result
</th>
</tr>
<tr>
<td/>
<td  valign="top">

<pre lang="cpp">

std::string s;

// fill-and-align

s = std::format("{:6}",   123); //default
s = std::format("{:<6}",  123); //left
s = std::format("{:^6}",  123); //center
s = std::format("{:>6}",  123); //right

s = std::format("{:6}",  "abc"); //default
s = std::format("{:<6}", "abc"); //left
s = std::format("{:^6}", "abc"); //center
s = std::format("{:>6}", "abc"); //right

s = std::format("{:$^6}", "abc"); //fill with

// sign

s = std::format("{}",    123); //default
s = std::format("{}",   -123); //default

s = std::format("{:-}",  123); //same as def
s = std::format("{:-}", -123); //same as def

s = std::format("{:+}",  123); //always sign
s = std::format("{:+}", -123); //always sign

s = std::format("{: }",  123); //space if pos
s = std::format("{: }", -123); //space if pos

</pre>
</td>
<td  valign="top">

<pre lang="cpp">

// results...

// fill-and-align

assert(s == "   123"); // numbers >
assert(s == "123   ");
assert(s == " 123  "); // slightly <
assert(s == "   123");

assert(s == "abc   "); // strings <
assert(s == "abc   ");
assert(s == " abc  "); // slightly <
assert(s == "   abc");

assert(s == "$abc$$"); // fill with $

// sign

/* {} */ assert(s == "123");
/* {} */ assert(s == "-123");

/*{:-}*/ assert(s == "123");
/*{:-}*/ assert(s == "-123");

/*{:+}*/ assert(s == "+123");
/*{:+}*/ assert(s == "-123");

/*{: }*/ assert(s == " 123");
/*{: }*/ assert(s == "-123");


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

