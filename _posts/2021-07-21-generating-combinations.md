---
layout: post
title: Generating Combinations using C++
subtitle: Code and explanation
categories: coding
tags: cpp coding
---

## Introduction

The task for this post is to help you understand how to generate combinations of size `r` using numbers from 1 to `n` and then extend that code to generate combinations for any set of objects.

When I say `n = 5` and `r = 3`, it means we are dealing with the first five natural numbers to generate combinations of size 3.

## Understanding Combinations

We shall be generating combinations in lexicographic order. Also, since combination does not concern itself with the relative order the chosen elements are arranged in, we shall always be having our chosen elements in lexicographical order too.

The first combination we choose has to be the first `r` elements when the `n` objects are sorted. This is because it gives us the lexicographically smallest word with the individual elements themselves in lexicographical order too. (basically, the if all combinations were arranged in a dictionary, then we want to start with the first one) So, in case of `n = 5` and `r = 3`, the first combination is `123`.

Now, think what should be the last combination. I propose that it will be the last `r` elements when the `n` objects are sorted. Why should it be? When we are generating combinations in lexicographical order, each combination should have a greater position in a dictionary. So the last combination should have the greatest elements from given objects. The chosen elements themselves should also be in lexicographical order.

The knowledge of last combination also gives us an interesting observation. The maximum value at a given index `idx` can be `n - r + idx + 1`, where index starts from zero. This is necessary to maintain the dictionary order of individual elements in a combination.

Take for example the case of `n = 5` and `r = 3`. If we position the number 5 at index 1, then the combination will look like `X5_`, where `X` may be any number lower than 5. The blank at third position should have a number greater than 5 if we are to maintain dictionary order. But placing 6 there is not possible since we only have 5 elements. According to the formula `n - r + idx + 1`, the maximum value at third position (or second index) can be 4.

Another thing to note is that every time we increment a position, we need to set the elements to its right as one more than its left neighbor. This will be clear when I discuss the code.

## Formal Algorithm

```
fun next_combination :
    parameters: integer n - max objects available
                integer r - size of chosen set
                array curr_comb - denoting
                  current combination of r elements
                integer idx - index to increment

    return:     true if next combination exists
                  curr_comb is updated in place
                  with the next combination
                false if last combination is provided
    ____

    if idx < 0 :
        return false

    if curr_comb[idx] < n - r + idx + 1 :
        ++curr_comb[idx]
        for i in (idx + 1, n - 1):
            curr_comb[i] = curr_comb[i - 1]
        return true

    else :
        return next_combinaition(n, r, curr_comb, idx - 1)
```

For this function to work, it should be called with `idx` as `r - 1`, since we want to start incrementing from last index. This code first check if the index to increment is less than 0. If yes, it means that the next combination does not exist, or in other words, we are already at last combination.

Then it checks if the value at `curr_comb[idx]` is less than its maximum permissible value. If yes, then it increments the value at `idx` and sets its following elements as one plus their left neighbor.

If the value it `idx` is already the maximum allowed, then the function recursively calls itself with same parameters, but `idx` decremented by one. This continues till either the next combination is found, or the function is called with `idx` less than zero, in that case next combination does not exist.

The function is recursive, so it must pass the sanity test. Does it stop recursing at some point? Each recursive call decrements `idx` by 1. Eventually, `idx` will be less than zero, in which case the function will no longer recurse. So, it is guaranteed that the function does not recurse indefinitely.

## C++ Code

```cpp
void increment_neighbor(std::vector<int> &arr, int idx)
{

    for (int i = idx; i < arr.size(); ++i)
        arr.at(i) = arr.at(i - 1) + 1;
}

bool _next_combination(int n, int r, std::vector<int> &curr_comb,
                       int idx)
{

    if (idx < 0)
        return false;

    if (curr_comb.at(idx) < n - r + idx + 1) {
        ++curr_comb.at(idx);
        increment_neighbor(curr_comb, idx + 1);
        return true;
    }
    else
        return _next_combination(n, r, curr_comb, idx - 1);
}

bool next_combination(int n, int r, std::vector<int> &curr_comb)
{

    return _next_combination(n, r, curr_comb, r - 1);
}
```

Following are the required includes:

```cpp
#include <algorithm>
#include <utility>
#include <vector>
```

This code is a direct implementation of the algorithm I shared above. The part that increments the neighbors by 1 has been extracted to its own function called `increment_neighbor`.

You might ask, why are there two functions named `next_combination`, one with a preceding underscore. The goal of a good interface is to be as simple as possible. In the recursive function, we initially call it with `idx = r - 1`. For a programmer reusing our function, it may be tedious to pass the fourth parameter, when it can be simply deduced from the third second one.

So, we create a helper function that takes the three required parameters and calls the original recursive function with fourth parameter set.

## Using Our Function

```cpp
int main()
{

    int n = 0, r = 0;

    // take input for n and r

    std::cout << "Enter r individual elements (integers) of"
                 " current combination separated by spaces\n";

    std::vector<int> numbers(r);

    for (int i = 0; i < r; ++i)
        std::cin >> numbers.at(i);

    if (next_combination(n, r, numbers)) {
        std::cout << "Next combination is\n";
        for (auto x : numbers)
            std::cout << x << " ";
        std::cout << "\n";
    }
    else {
        std::cout << "The input is already the final combination\n";
    }

    return 0;
}
```

This code will simply check if `next_combination` return true or false. If true, it means that the next combination was found and it is printed. Otherwise, inform user that we are already at last combination.

To generate all combination, use the following snippet.

```cpp
std::vector<int> numbers(r);

for (int i = 1; i <= n; ++i)
    numbers.at(i) = i;

do {
    for (auto x : numbers)
        std::cout << x << " ";
    std::cout << "\n";
} while (next_combination(n, r, numbers));
```

## Running on Custom Elements

Lets say the user does not want to generate combination of numbers, but instead wants to generate combinations of a custom set of objects. The object can be anything - character, string, struct etc.

The basic idea is to store the `n` individual elements in an array. Generate the combination on numbers from 1 to `n` as we did earlier. But while printing the combination, instead of number, print the element at that position.

As an example, here is the snippet for generating combinations of words.

```cpp
std::vector<int> numbers(r);
std::vector<int> words(r);

for (int i = 1; i <= n; ++i)
    numbers.at(i) = i;

std::cout << "Enter n individual elements (words)"
                " separated by spaces\n";

for (int i = 0; i < n; ++i)
    std::cin >> words.at(i);

do {
    for (auto x : numbers)
        std::cout << word.at(x) << " ";
    std::cout << "\n";
} while (next_combination(n, r, numbers));
```

## Memoization

Recursive algorithms can be made much faster when using memoization. Do we need memoization?

Memoization helps when the function is called with the same parameters multiple times. By storing intermediate results, it does not need to do computations if the parameters provided are not new.

In our function, `n` and `r` are constant throughout. `idx` changes its value, but it does so in a very small range. So there seems to be lot af scope for repetition of parameters. However, the parameter `curr_comb` does not repeat. Each combination is different. When recursing with the same combination, then the value of `idx` is guaranteed to change.

So we need not use memoization, as the intermediate values are never reused.

## Conclusion

That was the end of this post. Hope you learned something new. You can always shoot me feedback on my email, which is visible on my GitHub profile once you log in.

Follow me on GitHub : [flyingcakes85](https://github.com/flyingcakes85)

You can receive latest updates from this blog as an RSS feed. See [Follow this blog via RSS](/blog/website/2021/05/23/follow-this-blog.html).
