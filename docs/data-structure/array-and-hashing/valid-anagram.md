# Valid Anagram

Description: Given two strings s and t, return true if t is an anagram of s, and false otherwise.
An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

My solution:

```golang
import "reflect"

func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }

    firstLetters := make(map[string]int)
    secondLetters := make(map[string]int)

    for _, letter := range s {
        firstLetters[string(letter)]++
    }

    for _, letter := range t {
        secondLetters[string(letter)]++
    }

    return reflect.DeepEqual(firstLetters, secondLetters)
}
```
For this exercise, ChatGPT told me that `reflect.DeepEqual()` should be avoid whenever possible because: 

1. This method doesn't contain static typing.

    Therefore there isn't type verification in compilation time. This can result in errors that are difficult to detect and can only be observed at run time.
    An example of this error:
    ```golang
    package main

    import (
        "fmt"
        "reflect"
    )

    func main() {
        var a int = 42
        var b float64 = 42

        result := reflect.DeepEqual(a, b)
        fmt.Println("The result of comparison is", result)
    }
    ```
    In this example, we are comparing a *int* with a *float64*. In compilation time, the code will be compiled without errors because this is allowed. But, in run time, this code will burst the following error: `panic: reflect: DeepEqual of incomparable types int and float64`.
2. Perfomance
    `reflect.DeepEqual()` ia a generalist method and this have a price. Is better if We use methods more specifics for our problems.

Another thing that ChatGPT told me was that using array to count letters should be better since the letters are ASCII characters. I confess that I didn't understand this at first. So I asked for ChatGPT to explain it better and that is what I understood.

This approach means create an array of integers with a fixed size of 256 elements of ASCII table. Therefore, we can associate each ASCII character for each position in the array and count the number of occurence of this character.

```golang
func isAnagram(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }

    var letterCount [256]int

    for _, letter := range s {
        letterCount[letter]++
    }

    for _, letter := range t {
        letterCount[letter]--
        if letterCount[letter] < 0 {
            return false
        }
    }

    return true
}
```