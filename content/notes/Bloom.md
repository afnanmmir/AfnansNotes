---
title: "Bloom Filters" 
tags:
- software
- academic
enableToc: true
---

# Motivation
Imagine you have a service that requires users to create a unique account and password for the service, and let's say there are already millions of users registered in your service. You want a way to make sure that the username a user uses to create their account is unique. There are many conventional ways to do this. You could do a linear search through all registered accounts to check if the username is unique. With this many accounts, this is inefficient time wise and space wise, as it will be linear for both.

We can create a more time-efficient implementation easily. We could either store the accounts in alphabetical order and perform a binary search, which will be logarithmic time efficiency. We could even take this down all the way to constant time complexity by creating a hashtable, where we simply use the hash function(s) to check if the element exists.

The problem with these, however, is space complexity. Each of these solutions still requires us to store information about every single account in the database in order to work properly. What if we could reduce the space complexity so that we did not have to store information about every username to determine if the username is already present. That is where bloom filters come into play.

# What are Bloom Filters
Bloom filters are a probabalistic data structure whose main purpose is to determine whether an element is in a set or not. Because it is a probabilistic data structure, it cannot guarantee that an element is in the set. It can only tell you with a certain degree of confidence that the the element exists in the set. Though it can fall victim to false positives (says element is in the set but it actually isn't), it will never result in a false negative (says element is not in the set even though it actually is).

It is implemented as follows: there is an array of length $n$ that will store bits (0s and 1s), and there are $k$ hash functions. If you want to insert an element $x$ into the set, you will put $x$ through all $k$ hash functions and take the result modulo $n$:
$$ hash_1(x) \% n, hash_2(x) \% n, \dots, hash_k(x) \% n   $$
You will then take all of these results, and for every result, the corresponding index in the array will be set to 1. For example if $hash_2(x) \% n = i$, then $arr[i] = 1$.

If we want to check if an element $y$ is present in the set, we perform all $k$ hash functions on $y$ . For each result, we check the index corresponding to that result. If they are all 1, then we can say that it is probable that the element exists in the set. If any of the indinces is still 0, then it is impossible for the element to exist in the set.

You can see why false positives are possible and why false negatives are not possible. If an element was inserted, then it is impossible for any of the indices resulted by the hash functions to be a 0. However, it is possible for an element to generate the same exact hash results as another element, leading to a potential false positive. Intuitively, you can come to the conclusion that the percentage of false positives is some function of the number of elements in the array and the number of hash functions. Furthermore, there is an optimal amount of hash functions and length of the array we can choose.