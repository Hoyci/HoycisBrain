# Merge Strings Alternately

Description: You are given two strings word1 and word2. Merge the strings by adding letters in alternating order, starting with word1. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return the merged string.


My first solution:

``` golang
import "fmt"

func mergeAlternately(word1 string, word2 string) string {
    var output string

    if len(word2) > len(word1) {
        for index, char := range word2 {
            if index < len(word1) {
                output += fmt.Sprintf("%c", word1[index])
            }

            output += fmt.Sprintf("%c", char)
        }
        return output
    }

     for index, char := range word1 {
        output += fmt.Sprintf("%c", char)

        if index < len(word2) {
            output += fmt.Sprintf("%c", word2[index])
        }
    }

    return output
}
```

When I wrote this code, I thought something like "Hmm, I think that's not the best way to implent this code in Golang. Let me show this code for ChatGPT saying it's a golang specialist and have extensive experience with Data Structure." and this prompt returns me the code above:


``` golang
import (
    "fmt"
    "strings"
)

func mergeAlternately(word1 string, word2 string) string {
    var output strings.Builder

    len1, len2 := len(word1), len(word2)
    maxLen := len1

    if len2 > len1 {
        maxLen = len2
    }

    for i := 0; i < maxLen; i++ {
        if i < len1 {
            output.WriteByte(word1[i])
        }
        if i < len2 {
            output.WriteByte(word2[i])
        }
    }

    return output.String()
}
```

I confess that I don't understand a bit about why use strings package but I found a good explanation in Golang documentation.

Strings package is a good alternative to handle strings because it's implements simple functions to manipulate UTF-8 strings.

```strings.Builder``` are used to build **output** variable saying something like "Oh, you are here to receive bytes instead string". This is good because every time you do ```+=``` you are allocating memory to to store the new string.
I image if the string has 1000 letters, the process of iterate and concatenate can consume a lot memory and be unefficient. On the other hand, using this approach a reasonable quantity of memory is allocated previously and the memory is expanded dynamically.

In the end of the code ```output.String()``` are used because this returns a copy of the value where the buffer are interpreted and converted into unicode based on the UTF-8 encoding.