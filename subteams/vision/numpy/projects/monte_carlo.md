---
permalink: /vision/numpy/monte_carlo/
---

# Monte Carlo Craps Simulation

[Back to NumPy](/docs/vision/numpy/)

Need a refresher? [Check out the Intro to Numpy tutorial.](/docs/vision/numpy/intro/)

## Monte Carlo Problem

Implement a way to find the odds of winning a game of craps. Use a brute force method to run 1 million iterations of the game. Using numpy, we can quickly find the win rate.

Roll 2 dice and sum values.

Part 1: First roll Win if roll 7 or 11 Loose if roll 2, 3 or 12 Go onto Part 2 if did not win or loose

Part 2: Roll until win/loose Win if re-roll number from part 1 Loose if roll 7 Roll again if did not win or loose

Brute force method to find probability of winning craps. 1M iterations of the game. This can be done with all games taking place in one array in order to maximize time in numpy compiler and minimize code complexity.

Answer: 49.2% win rate

[My Solution](https://github.com/fallscameron01/Monte_Carlo_Simulation/blob/master/craps.py)

