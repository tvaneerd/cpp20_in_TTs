Abbreviated Function Template
---

Generic lambdas were introduced in C\++14, where auto was used as a function parameter. 
<table>
<tr>
<th>
C++14
</th>

</tr>
<tr>

<td  valign="top">
<pre lang="cpp">
// lambda definition
auto addTwo = [](auto first, auto second)
{
    return first + second; 
};
</pre>
</td>
</tr>

</table>

In C\++20, similarly to how auto was used in lambdas, auto can now be used as a function parameter to get a function template. \
C++20 has added abbreviated function templates, used to shorten the template version of the function.

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
template< typename T >
void addTwo(T first, T second)
{
     return first + second; 
}
</pre>
</td>

<td  valign="top">
<pre lang="cpp">
// function definition - removed template
auto addTwo(auto first, auto second)
{
    return first + second;  
}
</pre>
</td>

</tr>
</table>

Template arguments no longer have to be supplied if they can be deduced from context.