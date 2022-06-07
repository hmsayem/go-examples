
# Go Examples

#### Label to break outer loop

```go
package main

import (  
    "fmt"
)

func main() {  
outer:  
    for i := 1; i < 5; i++ {
        for j := 0; j < 5; j++ {
            fmt.Printf("i = %d , j = %d\n", i, j)
            if i == j {
                break outer
            }
        }

    }
}
```

#### Multiple expressions in Switch Case

```go
package main

import (  
    "fmt"
)

func main() {  
    letter := "i"
    fmt.Printf("Letter %s is a ", letter)
    switch letter {
    case "a", "e", "i", "o", "u": //multiple expressions in case
        fmt.Println("vowel")
    default:
        fmt.Println("not a vowel")
    }
}

```

