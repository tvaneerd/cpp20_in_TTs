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
T addTwo(T first, T second)
{
     return first + second; 
}
</pre>
</td>

<td  valign="top">
<pre lang="cpp">
// this is a template, without the word 'template'!
auto addTwo(auto first, auto second)
{
    return first + second;  
}
</pre>
</td>

</tr>
</table>

(To be clear - it is the `auto` used as a param - even just one of the params - that makes it a template.)

Looking at it from the point of view of Concepts, `auto` acts like the most unconstrained Concept.
