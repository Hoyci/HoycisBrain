# Contains Duplicate

Description: Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

My solution:

```golang
import "slices"

func containsDuplicate(nums []int) bool {
    var slice []int

    for i := 0; i < len(nums); i++ {
        if slices.Contains(slice, nums[i]) {
            return true
        }
        array = append(slice, nums[i])
    }

    return false
}
```

This code was giving me a weird feeling because I wrote ```var slice []int```
to store the values already viewed but that was the only solution that I could imagine of.
So, after I implemented this solution I went to ChatGPT using the following prompt

```
You are an expert in Golang and have extensive experience in data structures and algorithms.
How can I improve the code below and make it more idiomatic for Golang?
**My solution here**
```
With this prompt, ChatGPT opened my mind with some advices:

* 1. Use map is better than slice because:
    * 1.1. Map has exclusive keys. This means that I cannot create a new key if it's already exists another with the same key.
    * 1.2. Map has complexity O(1). This means that the verification is direct and constant. Slice, on the other hand, has complexity O(n^2) in the worst case. This means that should be necessary iterate by all elements to check if it has already been viewed.
* 2. Use ```Range``` form of the for loop
    * 2.1. This approach allows us to iterate directly on the elements of ```nums```. This is good because it improves the readability.

With these advices, that is the final code

```golang
func containsDuplicate(nums []int) bool {
	seen := make(map[int]bool)

	for _, num := range nums {
		if seen[num] {
			return true
		}
		seen[num] = true
	}

	return false
}
```

