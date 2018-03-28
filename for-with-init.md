Range-based for, with init
---

C++17 gave us if statements with initializers, like:

<table>
<tr>
<th>
C++17
</th>
</tr>
<tr>
<td  valign="top">

<pre lang="cpp">
{
   // note the semicolon
   if (QVariant var = getAnswer();  var.isValid())
      use(var);
      
   more_code();
}
</pre>
</td>
</tr>
</table>

C++20 adds a similar init, for range-based `for` statements

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
{
   T thing = f();
   for (auto& x : thing.items()) {
      // Note: “for (auto& x : f().items())” is WRONG
      mutate(&x);
      log(x);
   }
}
</pre>
</td>
<td  valign="top">

<pre lang="cpp">




for (T thing = f();  auto& x : thing.items()) {
   mutate(&x);
   log(x);
}

</pre>
</td>
</tr>
</table>

Also helps with "combined" index-y and range-y loops:

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
{
   std::size_t i = 0;
   for (const auto& x : foo()) {
      bar(x, i);
      ++i;
   }
}
</pre>
</td>
<td  valign="top">

<pre lang="cpp">




for (std::size_t i = 0;  const auto& x : foo()) {
   bar(x, i);
   ++i;
}
</pre>
</td>
</tr>
</table>

_These examples, already in table form, were taken directly from the paper [wg21.link/p0614](wg21.link/p0614).  Thanks Thomas!_
