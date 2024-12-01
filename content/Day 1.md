---
title: Day 1 solution
---
# Advent of Code 2024 - Day 1 🧮

This is my solution for **Day 1** of Advent of Code 2024, implemented in **Go**. Below, I outline my approach for both parts of the problem, including the code snippets and explanations.

---

## Input Handling 📂

First, we read the input file containing the puzzle data.

```go
func main() {
	input, err := os.ReadFile("./inputs/day1.txt")
	if err != nil {
		panic(err)
	}
	ans := day1.Part1(input)
	println(ans)
}
```

### Explanation
- **`os.ReadFile`** reads the input file as a byte slice.
- The `day1.Part1` function is called to process the data for Part 1 of the challenge.

---

## Processing Data into Lists 📋

The input data is parsed into two separate integer lists. Here’s the function that handles this:

```go
func makeIntSlice(input []byte) ([]int, []int) {
	lines := strings.Split(strings.TrimSpace(string(input)), "\n\n")
	pairs := strings.Split(lines[0], "\n")

	var list1, list2 []int

	for i, v := range pairs {
		if v == "" {
			continue
		}

		slice := strings.Split(strings.TrimSpace(v), " ")
		num1, err := strconv.Atoi(slice[0])
		if err != nil {
			fmt.Println(i, err)
			return nil, nil
		}
		num2, err := strconv.Atoi(slice[3])
		if err != nil {
			fmt.Println(err)
			return nil, nil
		}

		list1 = append(list1, num1)
		list2 = append(list2, num2)
	}

	sort.Ints(list1)
	sort.Ints(list2)
	return list1, list2
}
```

### Explanation
- **Input parsing**: The input is split into sections and lines, and each pair is extracted as integers.
- **Sorting**: Both lists are sorted for further operations.

---

## Part 1 - Calculating Differences ✂️

The task for Part 1 involves calculating the absolute difference between corresponding elements of the two lists and summing them up.

```go
func Part1(input []byte) int {
	list1, list2 := makeIntSlice(input)

	differences := make([]int, len(list1))
	for i := range list1 {
		tmp := list2[i] - list1[i]
		if tmp < 0 {
			tmp = -tmp
		}
		differences[i] = tmp
	}

	return sum(differences)
}

func sum(list []int) int {
	var sum int
	for _, v := range list {
		sum += v
	}
	return sum
}
```

### Explanation
1. **Difference Calculation**: The absolute difference between elements in `list1` and `list2` is stored in a new list.
2. **Summing**: The `sum` function computes the total of these differences.

---

## Part 2 - Counting and Weighting 🧮

For Part 2, we calculate a weighted sum based on the frequency of elements.

```go
func Part2(input []byte) int {
	list1, list2 := makeIntSlice(input)

	count_list := make([]int, len(list1))
	counts := make(map[int]int)

	for _, v := range list2 {
		counts[v]++
	}
	for i, v := range list1 {
		count_list[i] = v * counts[v]
	}

	return sum(count_list)
}
```

### Explanation
1. **Frequency Map**: A map `counts` keeps track of how often each number in `list2` appears.
2. **Weight Calculation**: Each number in `list1` is multiplied by its frequency in `list2` to create the weighted list.
3. **Summing**: The total is computed as the sum of the weighted list.

---

## Key Learnings ✍️

- Using **`strconv.Atoi`** to convert strings to integers and handle potential errors effectively.
- Leveraging **maps** for frequency counting simplifies tasks that involve counting occurrences.
- Sorting the lists helps ensure data consistency for pairwise operations.

---

## References 🔗

- [Advent of Code 2024](https://adventofcode.com/2024)
- [Official Go Documentation](https://go.dev/doc/)

---

Feel free to check out my full repository for Advent of Code 2024: [GitHub Repository](https://github.com/hawkaii/advent_of_code_2024_go). Happy coding! 🎄✨