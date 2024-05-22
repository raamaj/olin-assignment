# 1. Find index from addition of two numbers same with target
``` go
package main

import (
	"fmt"
)

func main() {
	nums := []int{2, 7, 11, 15}
	target := 18

	lastNum := 0
	lastIdx := 0
	var output []int
	for i, v := range nums {
		if lastNum+v == target {
			output = append(output, []int{lastIdx, i}...)
		}

		lastNum = v
		lastIdx = i
	}

	fmt.Print(output)
}

```

# 2. Find Triplets Zero
```go
package main

import (
	"fmt"
)

func main() {
	nums := []int{-1, 0, 1, 2, -1, -4}
	fmt.Print(findTripletsZero(nums))
}

func findTripletsZero(nums []int) (result [][]int) {
	for i := 0; i <= len(nums)-2; i++ {
		if i > 0 && nums[i] == nums[i-1] {
			continue
		}
		for j := i + 1; j < len(nums)-1; j++ {
			if j > i+1 && nums[j] == nums[j-1] {
				continue
			}
			for k := j + 1; k < len(nums); k++ {
				if k > j+1 && nums[k] == nums[k-1] {
					continue
				}
				if nums[i]+nums[j]+nums[k] == 0 {
					result = append(result, []int{nums[i], nums[j], nums[k]})
				}
			}
		}
	}

	return result
}

```
>Time Complexity of the function is O(n)×O(n)×O(n)=O(n3) which is based on 3 iterations on the function

# 3. Substring with concatenation of all words 
```go
package main

import (
	"fmt"
)

func main() {
	s := "wordgoodgoodgoodbestword"
	words := []string{"word", "good", "best", "word"}

	// Map Word Freq
	mapFreq := make(map[string]int)
	currMapFreq := make(map[string]int)
	for _, word := range words {
		mapFreq[word] = 1
	}

	lenS := len(s)
	n := len(words)
	wordSize := len(words[0])
	windowSize := wordSize * n

	var result []int

	for pos := 0; pos < wordSize; pos++ {
		start := pos
		for {
			currMapFreq = mapFreq
			isMatch := true
			for i := 0; i < lenS; i++ {
				currWord := s[start+i*wordSize : wordSize]
				_, ok := currMapFreq[currWord]
				if !ok || currMapFreq[currWord] == 0 {
					isMatch = false
					break
				}
				currMapFreq[currWord]--
			}

			if isMatch {
				result = append(result, start)
			}

			if start+windowSize-1 < lenS {
				break
			}
		}
	}
	fmt.Println(result)
}

```
>Uncompleted
