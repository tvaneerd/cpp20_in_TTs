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

C++20 also introduces *comparison categories* to specify regarding
weak/partial/strong ordering and weak/strong equality.

<table>
<tr>
<th>Before C++20</th>
<th>After C++20</th>
</tr>
<tr>
<td valign="top">

<pre lang="cpp">
template &lt;typename T&gt;
class LinkedList&lt;T&gt;::iterator {
    Node&lt;T&gt;* ptr = nullptr;

public:
    friend bool operator==(iterator const&amp; other) {
        return ptr == other.ptr;
    }

    friend bool operator!=(iterator const&amp; other) {
        return ptr != other.ptr;
    }
};
</pre>
</td>
<td valign="top">

<pre lang="cpp">
template &lt;typename T&gt;
class LinkedList&lt;T&gt;::iterator {
    Node&lt;T&gt;* ptr = nullptr;

public:
    std::strong_equality operator&lt;=&gt;(iterator const&amp;) const = default;
};
</pre>
</td>
</tr>
</table>

Comparison categories implicitly convert to corresponding AS-IF
categories, e.g. `std::strong_ordering` implies/is implicitly
convertible to `std::strong_equality` and `std::weak_ordering`.

> **Note**
>
> Descriptions and examples heavily reference the original paper,
> [Consistent Comparison \[P0515\]](http://wg21.link/p0515).
