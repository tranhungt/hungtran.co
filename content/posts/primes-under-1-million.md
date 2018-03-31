---
title: "Primes Under 1 Million"
date: 2013-05-21T18:56:15-05:00
draft: false
---

Finding all primes under 1 million. Sounds easy…but maybe a little more demanding than it appears. Depending on the algorithm, all the primes may or may not be generated in a timely manner. Generally, a number n is checked for primality by iterating through all the numbers from 1 to n, and see if n divides into each value evenly. That would take very very long time. A better way is to only check for values from 1 to squrt(n), since mathematically, the range between from squrt(n) to n is just a repeat of the first half in terms of divisibility.

A much much better way would to make a sieve of eratosthenes. It’s a sieve that first starts with an array of numbers from 2 to n (1 is excluded based on the law of primality). Starting at 2, every 2nd position after 2 up to n is divisible by 2, therefore not prime. Those numbers can be changed to false or 0 to indicate non-primes. Next, starting at 3, every 3rd position after 3 up to nis also converted to false or 0. The iteration continues to squrt(n).

The final array now contains prime numbers with a mix of 0’s or false’s in places where composite numbers once existed. Compact or select the array for all numbers > 0 for final primes array.