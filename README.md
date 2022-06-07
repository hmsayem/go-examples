
## Go Examples

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

#### Multiple expressions in switch case

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

#### Memory optimisation with garbage collection

```go
package main

import (  
    "fmt"
)

func countries() []string {  
    countries := []string{"USA", "Singapore", "Germany", "India", "Australia"}
    neededCountries := countries[:3]
    countriesCpy := make([]string, len(neededCountries))
    copy(countriesCpy, neededCountries) //copies neededCountries to countriesCpy
    return countriesCpy
}
func main() {  
    countriesNeeded := countries()
    fmt.Println(countriesNeeded)
}
```

#### Comparing maps using reflection

```go
package main
 
import (
    "fmt"
    "reflect"
)
 
func main() {
 
    map_1 := map[int]string{
 
        200: "Hossain",
        201: "Mahmud",
    }
    map_2 := map[int]string{
 
        200: "Hasan",
        201: "Mahmud",
    }
 
    res := reflect.DeepEqual(map_1, map_2)
    fmt.Println("Is Map 1 is equal to Map 2: ", res)
}
```