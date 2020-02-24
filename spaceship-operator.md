Better Equality
===============

<table>
<tr>
<th>Before C++20</th>
<th>After C++20</th>
</tr>
<tr>
<td valign="top">

<pre lang="cpp">
class Chair
{
    int numLegs = 4;
    Color color = RGB(64, 255, 128);
    Style style;

public:
    friend bool operator==(Chair const & a, Chair const & b)
    {
        return a.numLegs == b.numLegs
            && a.color == b.color
            && a.style == b.style;
    }

    friend bool operator!=(Chair const & a, Chair const & b)
    {
        return !(a == b);
    }
};
</pre>
</td>
<td valign="top">

<pre lang="cpp">
class Chair
{
    int numLegs = 4;
    Color color = RGB(128, 128, 128);
    Style style;

public:
    auto operator==(Chair const &) const = default;
};
</pre>
</td>
</tr>
</table>


Spaceship Operator
==================

<table>
<tr>
<th>Before C++20</th>
<th>After C++20</th>
</tr>
<tr>
<td valign="top">

<pre lang="cpp">
class MyInt {
    int value = 0;

public:
    friend bool operator==(MyInt a, MyInt b) {
        return a.value == b.value;
    }

    friend bool operator!=(MyInt a, MyInt b) {
        return !(a == b);
    }

    friend bool operator&lt;(MyInt a, MyInt b) {
        return a.value &lt; b.value;
    }

    friend bool operator&lt;=(MyInt a, MyInt b) {
        return !(b &lt; a);
    }

    friend bool operator&gt;(MyInt a, MyInt b) {
        return b &lt; a;
    }

    friend bool operator&gt;=(MyInt a, MyInt b) {
        return !(a &lt; b);
    }
};
</pre>
</td>
<td valign="top">

<pre lang="cpp">class MyInt {
    int value = 0;

public:
    auto operator&lt;=&gt;(MyInt) const = default;
};
</pre>
</td>
</tr>
</table>

Details
=======

Usage
-----

C++20 introduces the *three-way comparison operator*, also known as the
"spaceship operator":

    class Person {
        std::string lastname;
        std::string firstname;

    public:
        auto operator<=>(Person const&) const = default;
    };

The spaceship operator behaves similarly to `strcmp` and `memcmp`:

<table>
<tr>
<th><code>strcmp</code>/<code>memcmp</code></th>
<th><code>operator&lt;=&gt;</code></th>
<th>Meaning</th>
</tr>
<tr>
<td valign="top">

<pre lang="cpp">
if (strcmp(a, b) &lt; 0);
</pre>
</td>
<td valign="top">

<pre lang="cpp">
if (auto cmp = a &lt;=&gt; b; cmp &lt; 0);
</pre>
</td>
<td valign="top">
<p>If <code>a</code> is less than <code>b</code>.</p>
</td>
</tr>
<tr>
<td valign="top">

<pre lang="cpp">
if (strcmp(a, b) &gt; 0);
</pre>
</td>
<td valign="top">

<pre lang="cpp">
if (auto cmp = a &lt;=&gt; b; cmp &gt; 0);
</pre>
</td>
<td valign="top">
<p>If <code>a</code> is greater than <code>b</code>.</p>
</td>
</tr>
<tr>
<td valign="top">

<pre lang="cpp">
if (strcmp(a, b) == 0);
</pre>
</td>
<td valign="top">

<pre lang="cpp">
if (auto cmp = a &lt;=&gt; b; cmp == 0);
</pre>
</td>
<td valign="top">
<p>If <code>a</code> is equal to <code>b</code>.</p>
</td>
</tr>
</table>

Thanks to [Symmetry for Spaceship \[P0905\]](http://wg21.link/p0905),
if `a <=> b` is well-formed, then `b <=> a` is also well-formed. This
means only one definition of `operator<=>` is required to provide
heterogeneous type comparison (it does not need to be a (`friend`-ed)
non-member function).

Existing equality and comparison operator overloads take precedence over
the spaceship operator. This means **this is not a breaking change**,
and users can still specialize particular operators as desired.

> **Note**
>
> The spaceship operator resembles a TIE **bomber** `<=>`, not a TIE
> **fighter** `|=|`.

Comparison Categories
---------------------

C++20 also introduces *comparison categories* to specify weak/partial/strong ordering.
ie ints have strong_ordering, but floats have partial_ordering (due to NaNs).  Weak_ordering is for orderings where `!(a < b) && !(b < b)` does NOT imply `a == b`.  ie an ordering that builds _equivalence classes_.  For example, coloured bingo balls that are sorted by number (ignoring color), but where equality _does_ check color.

Recommendations:

- every copyable class should have equality. _Copy_ means _an equal copy_.
- most classes don't actually need `<=>`, as they don't have _natural_ order.  ie `myChair < yourChair` doesn't make much sense. Ordering Chairs in various ways does make sense, but there is no obvious natural order.  If you want to order Chairs to place into a map or set, consider instead unordered_map/set.  If you really want map/set, then pass in a specific ordering, don't rely on `std::less`.
- If your class has a natural ordering, I really hope it is strong_ordering. Users will thank you.  In particular, if you have a weak_ordering, consider instead a named ordering function, not overriding the built in operators.

> **Note**
>
> Original paper: [Consistent Comparison \[P0515\]](http://wg21.link/p0515).  
> Subsequent papers:  
> [`<=> != ==`](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p1185r0.html)  
> [When do you actually use `<=>`?](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p1186r0.html)  
> [I did not order this! Why is it on my bill?](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p1190r0.html)  
> [weak_equality considered harmful](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p1307r0.pdf)  
