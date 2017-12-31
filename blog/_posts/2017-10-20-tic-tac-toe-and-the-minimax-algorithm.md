---
title: "Tic-Tac-Toe and the Minimax algorithm"
type: code
---

I had to build a tic-tac-toe game as part of the coding challenges from [freeCodeCamp](http://www.freecodecamp.com). The most interesting part of the project was probably learning about the Minimax algorithm, a simple artificial intelligence algorithm that keeps track of all the different combinations of plays that each player could make and then finds out which one would be the most beneficial to that player based on a score that it calculates.

I'll try to explain how the minimax algorithm works by showing step by step how it would analyze a game of tic-tac-toe.

In order to keep this explanation short, let's assume that some plays have already been made:

starting_position.png

The first thing that the algorithm does is creating a tree of the possible moves, and then it will go down each branch and do the same while keeping scores.



