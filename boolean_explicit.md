
_explicit(boolean-expression)_



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


struct MyInt
{
    int val = 0;

    // explicit if the int type coming in
    // is bigger than what we can hold.
    // eg explicit if you pass a long,
    //    implicit if you pass a short
    // TODO: handle unsigned...
    template &lt;typename Init,
        std::enabled_if_t&lt;sizeof(Init) &gt;  sizeof(int)&gt;
    explicit X(Init init)
        : val(static_cast&lt;int&gt;(init))
    {
    }
    template &lt;typename Init,
        std::enabled_if_t&lt;sizeof(Init) &lt;= sizeof(int)&gt;
    X(Init init)
        : val(static_cast&lt;int&gt;(init))
    {
    }
};


</pre>
</td>
<td  valign="top">

<pre lang="cpp">


struct MyInt
{
    int val = 0;

    // explicit if the int type coming in
    // is bigger than what we can hold.
    // eg explicit if you pass a long,
    //    implicit if you pass a short
    // (TODO: check for unsigned...) 
    template &lt;typename Integer&gt;
    explicit(sizeof(Integer) &gt; sizeof(val))
    X(Integer init)
        : val(static_cast&lt;int&gt;(init))
    {
    }
};

</pre>
</td>
</tr>
</table>

That can be useful on its own.  But an interesting corollary is "explicit(false)".
What's the point of that over the "equivalent" of just not saying explicit at all?
It is being explicit about an implicit constructor. For example:


<table>
<tr>
<th>
Questionable C++
</th>
<th>
Better C++
</th>
<th>
C++20
</th>
</tr>
<tr>
<td  valign="top">

<pre lang="cpp">

struct Foo
{
    Foo(Bar b);
};

</pre>
</td>
<td  valign="top">

<pre lang="cpp">

struct Foo
{
    /* yes, implicit*/ Foo(Bar b);
};

</pre>
</td>
<td  valign="top">

<pre lang="cpp">

struct Foo
{
    explicit(false) Foo(Bar b);
};

</pre>
</td>
</tr>
</table>


In code review of the first example, I would probably ask if the constructor should be explicit - the general rule is that most constructors should be explicit (it is another default that C++ got wrong).
(because implicit conversions often lead to mistaken conversions, which leads to bugs)
So some people leave comments, to make it clear.  But comments that just explain syntax (and not _why_) are better done as more clear code.

Using `explicit(false)` shows that the implicitness wasn't an accidental omission, but a real design decision. (You might still want to leave a comment as to _why_ that is the right design decision.)

P.S. Please don't `#define implicit explicit(false)`. _Just Don't_. Do Not. Not Do. Don't even Try.

See also [wg21.link/p0892](https://wg21.link/p0892)
