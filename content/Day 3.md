---
title: Day 3 solution
---
# Advent of Code 2024 - Day 3 🧮

## Overview

For **Day 3**, the challenge involved parsing a string with custom syntax and computing sums based on the presence of mathematical expressions. Using **Go**’s `regexp` package, I extracted specific patterns from the input and computed results for two parts. Here's how I tackled the problem.

---

## Part 1: Multiplying Matched Pairs

The task was to find and evaluate all occurrences of the expression `mul(x, y)` in the input string, where `x` and `y` are integers. The product of these numbers is summed to produce the final result.

### Approach

1. **Extracting Patterns**:
   - Used a regular expression `mul\((\d+),(\d+)\)` to match the pattern and extract `x` and `y`.

2. **Converting and Calculating**:
   - Converted the extracted strings to integers using `strconv.Atoi`.
   - Computed the product of the two numbers and added it to a running total.

3. **Result**:
   - The sum of all such products is returned.

### Code Implementation

```go
func Part1(input []byte) int {
	input_string := aoc.ParseInput(input) // Utility function to parse input.
	fmt.Println(input_string)

	re := regexp.MustCompile(`mul\((\d+),(\d+)\)`)
	matches := re.FindAllStringSubmatch(input_string[0], -1)

	sum := 0
	for _, match := range matches {
		int1, err := strconv.Atoi(match[1])
		if err != nil {
			panic(err)
		}
		int2, err := strconv.Atoi(match[2])
		if err != nil {
			panic(err)
		}

		product := int1 * int2
		sum += product
	}

	return sum
}
```

### Example Input
```
mul(911,768)what(805,778)mul(690,737)from()who())select()<~mul(248,530)mul(638,821)mul(218,217)
```

### Explanation
- Matches found: `mul(911,768)`, `mul(690,737)`, etc.
- Products calculated: `911 * 768`, `690 * 737`, etc.
- Result is the sum of these products.

---

## Part 2: Enabling and Disabling Operations

Part 2 introduced additional commands:
- **`do()`**: Enables multiplication processing.
- **`don't()`**: Disables multiplication processing.

### Approach

1. **Regex Matching**:
   - Used `regexp.MustCompile` to match `mul(x, y)`, `do()`, and `don't()` commands.

2. **Toggling Multiplication**:
   - Maintained a boolean flag `mulenabled` to track whether multiplication was allowed.
   - Adjusted the flag based on `do()` and `don't()` commands.

3. **Conditional Processing**:
   - Only processed `mul(x, y)` when `mulenabled` was `true`.

4. **Result**:
   - Sum of valid multiplications after considering toggling is returned.

### Code Implementation

```go
func Part2(input []byte) int {
	input_string := aoc.ParseInput(input)

	re := regexp.MustCompile(`mul\(\d+,\d+\)|do\(\)|don't\(\)`)

	matches := re.FindAllString(input_string[0], -1)

	mulenabled := true
	sum := 0
	for _, match := range matches {
		if match == "do()" {
			mulenabled = true
		} else if match == "don't()" {
			mulenabled = false
		} else if mulenabled {
			var a, b int
			fmt.Sscanf(match, "mul(%d,%d)", &a, &b)
			sum += a * b
		}
	}

	return sum
}
```

### Example Input
```
mul(248,530)do()mul(638,821)don't()mul(218,217)do()mul(684,550)
```

### Explanation
1. **Commands**:
   - `do()`: Enables multiplication processing.
   - `don't()`: Disables multiplication processing.

2. **Matches**:
   - Initially, `mul(248,530)` is processed.
   - After `do()`, `mul(638,821)` is processed.
   - After `don't()`, `mul(218,217)` is ignored.
   - After the second `do()`, `mul(684,550)` is processed.

3. **Result**:
   - Sum of valid products: `248*530 + 638*821 + 684*550`.

---

## Helper Function: `findMulSum`

To simplify the extraction of `mul(x, y)` patterns and calculation of their sums, I implemented the `findMulSum` utility function. This function is reusable and avoids redundant code.

```go
func findMulSum(s string) int {
	re := regexp.MustCompile(`mul\((\d+),(\d+)\)`)
	matches := re.FindAllStringSubmatch(s, -1)

	sum := 0
	for _, match := range matches {
		int1, err := strconv.Atoi(match[1])
		if err != nil {
			panic(err)
		}
		int2, err := strconv.Atoi(match[2])
		if err != nil {
			panic(err)
		}

		product := int1 * int2
		sum += product
	}

	return sum
}
```

---

## Learnings & Insights

1. **Regular Expressions**:
   - Powerful tool for pattern matching.
   - Used `FindAllStringSubmatch` for extracting multiple matches with capture groups.

2. **State Management**:
   - Implemented toggling with a boolean flag to conditionally enable or disable processing.

3. **Error Handling**:
   - `strconv.Atoi` requires robust error handling to ensure valid integer conversion.

---

## Final Thoughts

Day 3 presented an engaging challenge that tested regex and state management skills. The addition of toggling operations in Part 2 made the problem even more interesting. I learned the importance of modular and reusable code while handling complex input patterns.

Check out the [GitHub Repository](https://github.com/hawkaii/advent_of_code_2024_go) for the full implementation and solutions for other days!

Happy coding! 🎄✨

