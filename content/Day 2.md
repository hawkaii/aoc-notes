---
title: Day 2 solution
---
# Advent of Code 2024 - Day 2 🧮

## Problem Understanding

The task involves processing a series of integer sequences, checking if they are sorted in either ascending or descending order, and ensuring that the absolute difference between any two adjacent numbers is within a safe range (i.e., no greater than 3). Furthermore, we need to explore whether we can remove a single element from any sequence and still maintain the safety of the sequence.

## Key Concepts

1. **Strictly Sorted**: A sequence is strictly sorted if its elements are either in ascending or descending order.
2. **Safe Sequence**: A sequence is considered "safe" if:
   - It is strictly sorted.
   - The absolute difference between adjacent numbers is less than or equal to 3.
3. **Tolerating Safety**: A sequence is considered "tolerant" if removing one element still results in a "safe" sequence.

### Step-by-Step Explanation

### 1. Parsing the Input

The first step is parsing the input into a list of integer sequences. Each sequence is separated by a blank line, and the individual numbers within each sequence are separated by spaces.

```go
func makeSlicelist(input []byte) [][]int {
    lines := strings.Split(strings.TrimSpace(string(input)), "\n\n")
    pairs := strings.Split(lines[0], "\n")

    var slice_list [][]int
    for _, pair := range pairs {
        slice := strings.Split(pair, " ")
        var int_slice []int
        for _, s := range slice {
            i, err := strconv.Atoi(s)
            if err != nil {
                panic(err)
            }
            int_slice = append(int_slice, i)
        }
        slice_list = append(slice_list, int_slice)
    }

    return slice_list
}
```

- **Explanation**: We split the input into blocks of sequences (`lines[0]`), then split each block into individual integers. Each sequence is stored in `slice_list`.

### 2. Checking if a Sequence is Strictly Sorted

A sequence is strictly sorted if its elements are in strictly increasing or decreasing order. We can implement this check using two flags (`isAscending` and `isDescending`).

```go
func isStrictlySorted(nums []int) bool {
    if len(nums) <= 1 {
        return true
    }

    isAscending := true
    isDescending := true

    for i := 1; i < len(nums); i++ {
        if nums[i] <= nums[i-1] {
            isAscending = false
        }
        if nums[i] >= nums[i-1] {
            isDescending = false
        }
    }

    return isAscending || isDescending
}
```

- **Explanation**: We check each element in the sequence to see if it's consistently greater or smaller than the previous element. If either condition holds true, the sequence is sorted.

### 3. Checking if a Sequence is Safe

A sequence is considered "safe" if:
- It is strictly sorted.
- The absolute difference between adjacent elements is less than or equal to 3.

```go
func ifSafe(slice []int) bool {
    if !isStrictlySorted(slice) {
        return false
    }

    for i := 0; i < len(slice)-1; i++ {
        if math.Abs(float64(slice[i]-slice[i+1])) > 3 {
            return false
        }
    }

    return true
}
```

- **Explanation**: This function first checks if the sequence is strictly sorted using `isStrictlySorted`. Then, it checks if the absolute difference between any adjacent pair of numbers is less than or equal to 3. If either condition fails, the sequence is considered unsafe.

### 4. Tolerating Safety

The "tolerating safety" requirement involves removing one element from the sequence and checking if the modified sequence is safe.

```go
func removeIndex(slice []int, index int) []int {
    if index < 0 || index >= len(slice) {
        panic("index out of range")
    }
    return append(slice[:index], slice[index+1:]...)
}

func toleratingIfSafe(slice []int) bool {
    if ifSafe(slice) {
        return true
    }

    for i := 0; i < len(slice); i++ {
        tmp := slices.Clone(slice)
        modifiedSlice := removeIndex(tmp, i)

        fmt.Println("Modified Slice:", modifiedSlice)

        if ifSafe(modifiedSlice) {
            return true
        }
    }

    return false
}
```

- **Explanation**: We first check if the sequence is already safe. If not, we iterate through each index, removing one element at a time and checking if the modified sequence is safe. The `removeIndex` function creates a new slice without the element at the specified index.

### 5. Implementing the Solution

Finally, we implement the main solution where we apply the safety checks to all sequences and count how many sequences are safe or tolerant.

```go
func Part1(input []byte) int {
    slice_list := makeSlicelist(input)
    count_safe := 0

    for _, slice := range slice_list {
        if ifSafe(slice) {
            count_safe++
        }
    }

    fmt.Println(ifSafe(slice_list[0]))
    return count_safe
}

func Part2(input []byte) int {
    slice_list := makeSlicelist(input)
    count_safe := 0

    for _, slice := range slice_list {
        if toleratingIfSafe(slice) {
            count_safe++
        }
    }

    fmt.Println(slice_list[0])
    fmt.Println(toleratingIfSafe(slice_list[0]))
    return count_safe
}
```

- **Explanation**: The `Part1` function simply checks if each sequence is safe. The `Part2` function checks for "tolerant safety" (i.e., if any sequence becomes safe after removing one element).

## Conclusion

In this solution, we used Go's built-in packages like `strings` for parsing and `math` for calculating absolute differences. The key challenge was implementing the logic to check if a sequence is "safe" and whether removing an element can still keep the sequence safe.

By breaking down the problem into smaller functions for checking sequence order, validating safety, and tolerating changes, we created a modular and efficient solution. 

If you want to explore more of Advent of Code problems or dive deeper into Go programming, feel free to ask! Happy coding!