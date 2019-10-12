---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "A template metaprogramming interview question"
subtitle: ""
summary: ""
authors: [Qiang Kou]
tags: [C++]
categories: []
date: 2019-10-11T22:33:03-07:00
lastmod: 2019-10-11T22:33:03-07:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

This is an interview question. It is pretty interesting. I never expect I would be asked about template metaprogramming in a job interview.
The problem is as follow:

```cpp
#include <iostream>
#include <type_traits>
/**
 * Write a compile-time solution that checks is a list of parameters
 * contains a given type. (See the test cases bellow)
 *
 * By convention, first type is the target of the lookup, the remaining types are used as pack
 *
 * 1. Add a definition of `val` that specialize the case where the list _has_ a type
 * 2. Implement the list-lookup solution
 * 3. Fix the alias definition with your solution
 * 4. Test
 *
 * Tips:
 *  - Read the code carefully, in the current implementation lies treasures of information.
 *  - Think about you would solve this problem in Lisp or Haskell.
 */

namespace detail {
/////////////////////////////////////////////
// TODO: Look in the type list for a matching type.
/////////////////////////////////////////////

// Convenience call alias
template <typename T, typename... Pack>
using tuple_contains_type = /* TODO: Provide the type declaration here */;

// Conditional values for test
template<bool>
struct val {
    static constexpr char* value = "Does not have type";
};

// TODO: Add a `val` definition for the case where the type _is_ in the list.
} // namespace detail

template<typename LookupType, typename... Pack>
struct test : public detail::val<detail::tuple_contains_type<LookupType, Pack...>::value> {};

struct A{};
struct B{};
struct C{};
struct D{};

using pass = test<A, B, A, D, A, D>;
using fail = test<A, C, D, B, C>;

int main() {
    std::cout << "Test A in B, A, D, A, D: " << pass::value << std::endl;	
    std::cout << "Test A in C, D, B, C: " << fail::value << std::endl;	
    return 0;
}
```

First, let's see how we will solve this problem in Racket.
The instruction mentioned lisp, but Racket is a very close one.
A simple solution is shown below:

```racket
#lang racket
(define (fun x lst)
  (cond
    [(empty? lst) #f]
    [(equal? x (first lst)) #t]
    [else (fun x (rest lst))]))
```
The idea is quite straightforward. If the first element of the list is what we are looking for, we found it; otherwise, we continue with the rest of the list. It can be called with lists of any types.
```
> (fun 1 (list 2 3 4 5))
#f
> (fun 1 (list 5 4 3 2 1))
#t
> (fun 'a (list 'a 'b))
#t
```

Then, our question is how to implement this using C++.
Actually, there is a huge hint in the code `tuple_contains_type`.
We can use `std::tuple` to mimic the list.

So here is my solution to the problem

```cpp
template <typename T, typename U>
struct lookup_type_tuple : std::false_type {};

template <typename T, typename... Args>
struct lookup_type_tuple<T, std::tuple<T, Args...>> : std::true_type {};

template <typename T, typename U, typename... Args>
struct lookup_type_tuple<T, std::tuple<U, Args...>>
    : lookup_type_tuple<T, std::tuple<Args...>> {};

// Convenience call alias
template <typename T, typename... Pack>
using tuple_contains_type = lookup_type_tuple<T, std::tuple<Pack...>>;
```

We are using the `std::tuple` and template to mimic the `first` and `rest` functions in Racket.

