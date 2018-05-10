std::remove_cvref
===================

Summary
-------

<table>
<tr>
<th>Before C++20</th>
<th>After C++20</th>
</tr>
<tr>
<td valign="top">

<pre lang="cpp">
typename std::remove_cv&lt;typename std::remove_reference&lt;T&gt;::type&gt;::type

std::remove_cv_t&lt;std::remove_reference_t&lt;T&gt;&gt;
</pre>
</td>
<td valign="top">

<pre lang="cpp">
typename std::remove_cvref&lt;T&gt;::type

std::remove_cvref_t&lt;T&gt;
</pre>
</td>
</tr>
</table>

Detail
------

`std::decay<T>` is often (mis)used to obtain the type name for a type that
is potentially a reference, cv-qualified, or both:

<pre lang="cpp">
template &lt;typename T&gt;
auto f(T&& t) {
    // Removes reference and cv-qualifiers...
    using type_name = std::decay_t&lt;T&gt;;

    // To avoid this problem...
    static_assert(!std::is_same_v&lt;int&, int&gt;);
    static_assert(!std::is_same_v&lt;int const, int&gt;);

    // When evaluating the following conditional expression.
    if constexpr (std::is_same_v&lt;type_name, int&gt;) {
        use_int(t);
    }

    // Similarly, a reference type is not an array...
    else if constexpr (std::is_array_v&lt;type_name&gt;) {
        use_array(t);
    }

    // Nor is it a function.
    else if constexpr (std::is_function_v&lt;type_name&gt;) {
        use_function(t);
    }

    else {
        use_other(t);
    }
}

int main() {
    auto const i = 42;
    auto const s = std::string();

    f(i); // T == 'int const&', type_name == 'int'; invokes 'use_int'.
    f(s); // T == 'std::string const&', type_name == 'std::string'; invokes 'use_other'.
}
</pre>

However, `std::decay<T>` introduces decay semantics for array and
function types:

<pre lang="cpp">
int arr[3];

// T == 'int (&)[3]', but type_name == 'int*' due to array-to-pointer decay.
// 'std::is_array_v&lt;int*&gt;' is false; invokes 'use_other'.
f(arr);

void g();

// T == 'void (&)()', but type_name == 'void (*)()' due to function-to-pointer decay.
// 'std::is_function&lt;void (*)()&gt;' is false; invokes 'use_other'.
f(g);
</pre>

In C++20, the standard library provides the `std::remove_cvref<T>`
type trait to fulfill this purpose without introducing unwanted
decay semantics:

<pre lang="cpp">
template <typename T>
auto f(T&& t) {
    using type_name = std::remove_cvref_t&lt;T&gt;;

    ...
}

int main() {
    auto const i = 42;
    int arr[3];
    void g();


    f(0); // Same as before.
    f(i); // Same as before.

    f(arr); // T == 'int (&)[3]', type_name == 'int [3]'; invokes 'use_array'.
    f(g); // T == 'void (&)()', type_name == 'void()'; invokes 'use_function'.
}
</pre>
