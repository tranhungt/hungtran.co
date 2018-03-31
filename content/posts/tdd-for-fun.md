---
title: "Tdd for Fun"
date: 2013-11-22T18:57:27-05:00
draft: false
---

Writing tests is important. Though, there's a difference between writing tests first and writing tests afterwards to cover your bases.
Over the weekend, I decided to approach a simple tic-tac-toe game written in ruby via TDD. What I've found was that TDD requires a new mentality where I must first think in little units. Every idea must be broken down to methods, and every method into even smaller testable blocks of code. I continually ask myself, what's the next smallest code I need - what should happen before, what should happen after, and what are the data being returned?
Writing in TDD helps ensure my code is more robust and that I can rely on them to make sure my expectations are consistent. It also helps me become a better programmer, as I begin to think of code in testable and reusable units. I helped me break my code down into the most basic blocks and DRY out my code.

Although TDD and testing in general ensures a certain consistency in my code, the tests are only as good as the person writing the test - me. I find that I continually break my game once I actually play with it. Edge cases that were not originally thought of during testing comes through. When that happens though, I go back into my specs and write more thorough tests to cover all my bases.