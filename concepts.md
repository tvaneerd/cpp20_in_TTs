Concepts. Whew, what a concept.


<table>
<tr>
<th>
C++17
</th>
<th align="left">
C++20
</th>
</tr>

<tr>
<td  valign="top">

<pre lang="cpp">
#include &lt;vector>
#include &lt;list>
#include &lt;algorithm>


template &lt;typename Container>
void sort(Container & c)
{
    std::sort(c.begin(), c.end());
}

void test()
{
    std::list<int> ls = {3, 2, 1, 5, 4};
    sortContainer(ls);
}

</pre>
</td>
<td  valign="top">

<pre lang="cpp">
#include &lt;vector>
#include &lt;list>
#include &lt;algorithm>
#include &lt;ranges>


void sort(std::ranges::random_access_range auto & c)
{
    std::sort(c.begin(), c.end());
}

void test()
{
    std::list<int>  ls = { 1, 2, 3, 4, 5, 6 };
    sortContainer(ls);
}

</pre>
</td>
</tr>

<tr>

<td  valign="top">
<pre lang="cpp">

**4375 lines of compiler error!!!**
S E R I O U S L Y

    

</pre>

</td>
<td  valign="top">
<pre lang="cpp">

source: In function 'void test()':

source:27:21: error: no matching function for call to 'sortContainer(std::__cxx11::list<int>&)'

   27 |     sortContainer(ls);

      |                     ^

source:17:6: note: candidate: 'template<class Container, class auto:16>  requires  random_access_range<auto:16> void sortContainer(auto:16&)'

   17 | void sortContainer(std::ranges::random_access_range auto & c)

      |      ^~~~~~~~~~~~~

source:17:6: note:   template argument deduction/substitution failed:

source:27:21: note:   couldn't deduce template parameter 'Container'

   27 |     sortContainer(ls);

      |                     ^

ASM generation compiler returned: 1

source: In function 'void test()':

source:27:21: error: no matching function for call to 'sortContainer(std::__cxx11::list<int>&)'

   27 |     sortContainer(ls);

      |                     ^

source:17:6: note: candidate: 'template<class Container, class auto:16>  requires  random_access_range<auto:16> void sortContainer(auto:16&)'

   17 | void sortContainer(std::ranges::random_access_range auto & c)

      |      ^~~~~~~~~~~~~

source:17:6: note:   template argument deduction/substitution failed:

source:27:21: note:   couldn't deduce template parameter 'Container'

   27 |     sortContainer(ls);

      |                     ^

Execution build compiler returned: 1
</pre>
</td>

</tr>

</table>


