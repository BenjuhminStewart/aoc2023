# Advent of Code 2023

This is my solutions for [Advent of Code 2023](https://adventofcode.com/2023)

If you pay attention to the commit dates, you'll notice that I am doing this in 2025.
I did the 2024 AoC back in Jan 2025 -> Feb 2025 and I loved it so I decided to do the previous year in Go.

## Table of Contents
- [Running](#running)
- [Solutions](#solutions)
  - [Day 1](#day-1)
  - [Day 2](#day-2)
  - [Day 3](#day-3)
  - [Day 4](#day-4)
  - [Day 5](#day-5)
  - [Day 6](#day-6)
  - [Day 7](#day-7)
  - [Day 8](#day-8)
  - [Day 9](#day-9)

## Running

```bash
# run a single day
go run src/day01/main.go

# run all days
./run_all.sh
```
# Solutions

## Day 1

[Problem](https://adventofcode.com/2023/day/1)

For this I utilized some built-in string functions like `strconv.Atoi`, `strings.Index`, and `strings.LastIndex`.
Used a map to map string like "two" to the number 2. Used an array of size `len(string)` where the value at each element is the number found at that index.

For example:

```
string = "twoonethree5"
array = [2,0,0,1,0,0,3,0,0,0,0,5]

output => 25
```

then I just took the far left non-zero element of the array as my "left" and the far right non-zero element as my "right".

the problem then needed the answer in the form of left + right as a 2 digit number. i.e. => (left * 10) + right

Time Complexity: O(n)
Space Complexity: O(n)

## Day 2

[Problem](https://adventofcode.com/2023/day/2)

Parsing was the hardest part of this one. My approach was to make a struct `Game` that has fields of an `id` and a `game_states` array that keeps track of used [R, G, B] dice in the game state. Then I can iterate over each game state in the game and make sure they meet the limits given in the problem. This approach amde it really easy to complete part 2.

Time Complexity: O(n*m) where n is the number of games and m is the number of times each game is played

Space Complexity: O(n*m) for each game `n` there is an array of `m` game states in which each game state is an array of size 3 to represent number of R,G,B dice used in each game.

## Day 3

[Problem](https://adventofcode.com/2023/day/3)

Iterated over the input string and made a map of Ranges (Range is a struct that has an `i` index *specifically the row of the number*, a `start` index, an `end` index, and a `final_val` value) that keeps track of the range of numbers that are used in the string. 

Then I iterated over the map of ranges looked for if at any point there was a symbol adjacent to the range. If there was, I added the final value of the range to the sum.

For part 2 this was the same but instead of there was a symbol adjacent to the range AND the symbol was a `*` I added it to a 
`potential_gears` map that mapped a Point to a list of adjacent numbers. Then I iterated over the map of potential gears and for each potential_gear that had EXACTLY 2 adjacent numbers, multiplied those two numbers together and added them to the return sum.

Time Complexity: O(n) where n is the length of the input string
Space Complexity: O(n*m) where n is the length of the input string and m is the amount of numbers in the string

## Day 4

[Problem](https://adventofcode.com/2023/day/4)

Iterated over the input string and made a list of `Scratchcards` that have a `winning_numbers` set and `played_numbers` array field.

Then I iterateed over the list of scratchcards and for each scratchcard, I iterated over the `played_numbers` array and if the number was in the `winning_numbers` set, I added it to the sum in a way that followed the prompts scoring. Did the same for part 2. Fairly simple

Time Complexity: O(n) where n is the length of the input string OR in other terms O(n*m) where n is the number of scratchcards and m is the length of `winning_cards` + `played_cards`. Otherwise known as just length of the input string as the input is a list of rows being `winning_cards` + `played_cards`

Space Complexity: O(n) Same as time complexity but for storing the scratchcards.

## Day 5

[Problem](https://adventofcode.com/2023/day/5)

Created a struct `Range` that has a `dest` a `src` and a `len` field.

Went through the input file and made:
- `seeds []int`: holds all th initial seeds
- `seed_to_soil []Range`
.
.
.
- `humidity_to_location []Range`

'.' Above singifies that I made arrays of type `Range` for the other fields listed in the problem as well

Part 1 was a matter of iterating over the `seeds` array and for each seed, I iterated over each Range array in order to eventually find its corresponding location and return the lowest one.

Time complexity: O(n*m) where n is the number of seeds and m is the number of ranges
Space complexity: O(n+m) where n is the number of seeds and m is the number of ranges

Part 2 utilized going backwards through the ranges and finding the lowest location that has a valid seed in the seed ranges. Iterated in steps of 1000 until I found a valid seed and then went back 1000 and iterated in steps of 1. This took time to run the problem from around 500ms down to 500μs so pretty quick speedup.

Time Complexity: O(n*m) where n is the (lowest location possible / 1000 + 1000) and m is the number of ranges.
For example, the lowest location possible for my problem was 1,493,866 so before it found the solution it had to check

1493866 / 1000 = 1493
1493 + 1000 = 2493

Considering the number of ranges was ~245, the number of operations was 2493 * 245 = 610,785.

Iterative from the front solution with brute force would be ~ 5 min or 300,000ms 
Iterative from the back solution with brute force would be ~ 600ms
Iterative from the back with steps of 1000 was ~ 500μs or 0.5ms.

Still could speed it up but I am happy with 500μs / 0.5ms.

Space Complexity: O(n+m) where n is the number of seeds and m is the number of ranges

## Day 6

[Problem](https://adventofcode.com/2023/day/6)

Created a struct `Race` that has a `time` and a `distance` field.

Went through the input file and made:
- `races []Race`: holds all the races

This was a really simple problem. Just used binary search to find the furthest left and right time that the race could be completed in. then multiply the total ways to win for each race.

Time Complexity: O(n*log(m)) where n is the number of races and m is the `time` for each race
Space Complexity: O(n) where n is the number of races

For Part 2 Since I already had the binary search, I just merged all the times and distances into one string each and then `strconv.Atoi` them to integers, add them to a new `Race` struct `race`, then get the furthest left working and furthest right working and find that total (`furthest_right - furthest_left + 1`.)

Time Complexity: O(log(n)) where n is the total `time` allowed for each race.
Space Complexity: O(1)


## Day 7

[Problem](https://adventofcode.com/2023/day/7)

Created a struct `CamelCard`
```go
type CamelCard struct {
    hand string
    bid int
    score1 int
    score2 int
}
```

Went through the input file and made an array of pointers to `CamelCard` structs as `var camel_cards []*CamelCard`
as i went through the input file, I also got the `score` for each hand and added it to the struct.

`five of a kind` -> 6 points
`four of a kind` -> 5 points
`full house` -> 4 points
`three of a kind` -> 3 points
`two pair` -> 2 points
`one pair` -> 1 point
`high card` -> 0 points

Then I sorted the array of camel cards by the `score` field of the struct. For cases where they had the same score,The prompt called for choosing the highest card that comes first. For example


hand 1: `QQQA2`
hand 2: `QQQ32`

hand 1 would be better because they are both 3 of a kind, and when you compare them in order they are the same until you hit the 4th card, and A is better than 3.

Then I iterated through the sorted array and for each camel card, multiplied the `bid` by their `hand_rank` (e.g., last place would be 1 and first place would be the number of hands played at the table) and added it to the `winnings` and return it.

Time Complexity: O(n*log(n)) where n is the number of camel cards
Space Complexity: O(n) where n is the number of camel cards

For part 2 I just used the scores I got already from part 1, then I looked at the amount of jokers the hand had and for each case added to the total based on the possible hand.

The process for adding to the total was the same as part 1.

Overall, I think I could have solved it with less lines of code but I was able to do it in a way that was easy to understand and I was able to solve it in less time than I would have if I had used a different approach.

Time Complexity: O(n*log(n)) where n is the number of camel cards
Space Complexity: O(n) where n is the number of camel cards

## Day 8

[Problem](https://adventofcode.com/2023/day/8)

Created a "enum" type `Direction` that has the following values:

```go
type Direction int

const (
    LEFT Direction = iota
    RIGHT
)
```

Went through the input file and made:
```go 
var instructions []Directions
var network map[string][2]string
``` 

Adding all instructions to the `instructions` array and then making a map of `network` that has a key of the root `node` and has a value of `[2]string` where the first index is the `left` node and the second index is the `right` node

For part 1 I just iterated over the instructions array from the start "AAA" and updated our current location until we hit the end "ZZZ" and incremented our step counter. Then our step counter at the end is our answer.

Time Complexity: O(n*m) where n is the number of instructions and m is the number of nodes in the network
Space Complexity: O(n+m) where n is the number of instructions and m is the number of nodes in the network


For part 2, I defined a result array `results` and then iterated over the nodes in the network. For each node:

- If the node does NOT end with an "A", I continue to the next node.
- set steps to 0
- while the current node does NOT end with a "Z"
  - get the next `direction` (i.e., `instructions[steps%len(instructions)]`)
  - set the current node equal to the `node` that the next `direction` points to
  - increment `steps`
- append the steps to the `result` array

Then, when we are out of the for loop, we have a list of steps. We use this to iterate through and set the `steps` equal to the Least Common Multiple of the `steps` (which starts at `results[0]`) and `results[i]` where `i` starts at 1 and goes until but not including `len(results)`

Finally, we have the answer stored in `steps`.

## Day 9

[Problem](https://adventofcode.com/2023/day/9)

For readability, I created the following type:

```go
type History []int
```

To store the `histories` in the input file, I made:

```go
var histories []History
```

I went through each line of the input file where each line is a `History` and added it to the `histories` array. (Note: a `History` is just an []int)

For part 1, I iterated over each `History` and for each one, made a `diffs` array that stores the difference between each element `i` and `i+1`. While the `diffs` array is not all zeroes, we will keep getting the next `diffs`. Due to the nature of the problem, `extrapolating` the answer was as simple as adding up all the values in the last index of the `diffs` array.

For example:

```
10 13 16 21 30 45

1) Getting the diffs
10 13 16 21 30 45
  3  3  5  9  15
    0  2  4  6
     2  2  2
      0  0

2) Extrapolating
0 + 2 + 6 + 15 + 45 = 68

3) Return 68
```
It was just a matter of summing up all those extrapolated values to get the answer.

Time Complexity: O(n*m) where n is the number of histories and m is the number of iterations it takes to get the `diffs` array to be all zeroes.
Space Complexity: O(n) where n is the number of histories. Created 1 addition array for each history that is updated at each step in iteration

For part 2, I did the exact same thing but instead of being able to add up the values in the last index of the `diffs` array in place, I had to make a separate array called `diffs` where I stored the `first` value of each diff. Then I passed it to an `extrapolate` function
that did a reverse iteration over the `first_diffs` and subtracted it from the `new_firsts` array that index. It is pretty confusing to read but here is the algorithm in practice:

```
10 13 16 21 30 45

1) Getting the diffs
10 13 16 21 30 45
  3  3  5  9  15
    0  2  4  6
     2  2  2
      0  0

2) Store first diffs in array
[10, 3, 0, 2, 0]

3) Extrapolate

2 - 0 = 2
0 - 2 = -2
3 -(-2) = 5
10 - 5 = 5

4) Return 5

```

Then, like part 1, I just summed up all the extrapolated values to get the answer.

Time Complexity: O(n*m) where n is the number of histories and m is the number of iterations it takes to get the `diffs` array to be all zeroes.
Space Complexity: O(n) where n is the number of histories. Created 1 addition array for each history that is updated at each step in iteration
