
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

Slices hold a reference to the underlying array. As long as the slice is in memory, the array cannot be garbage collected. One way to solve this problem is to use the copy function func copy(dst, src []T) int to make a copy of that slice. This way we can use the new slice and the original array can be garbage collected.

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

#### Accessing individual characters of a string

```go
package main

import (
    "fmt"
)

func printBytes(s string) {
    fmt.Printf("Bytes: ")
    for i := 0; i < len(s); i++ {
        fmt.Printf("%x ", s[i])
    }
}

func printCharsByRunes(s string) {
    fmt.Printf("Runes: ")
    runes := []rune(s)
    for i := 0; i < len(runes); i++ {
        fmt.Printf("%c ", runes[i])
    }
}

func printChars(s string) {
    fmt.Printf("Characters: ")
    for i := 0; i < len(s); i++ {
    fmt.Printf("%c ", s[i])
    }
}

func main() {
    name := "Señor"
    fmt.Printf("String: %s\n", name)
    printBytes(name)
    fmt.Printf("\n")
    printChars(name)
    fmt.Printf("\n")
    printCharsByRunes(name)
}

```

#### Mutate string

```go
package main

import "fmt"

func mutate(s []rune, ch rune, id int) string {
    s[id] = ch
    return string(s)
}

func main() {
    s := "hello"
    s = mutate([]rune(s), 'x', 0)
    fmt.Println(s)
}

```

#### Creating pointers using the new function

```go
package main

import (
    "fmt"
)

func main() {
    p := new(int)
    fmt.Printf("Value of p is %d, type of p is %T, address of p is %v\n", *p, p, p)
    *p = 85
    fmt.Println("New value is", *p)
}

```

#### Modify array using slice

```go
package main

import (
    "fmt"
)

func modify(sls []int) {
    sls[0] = 90
}

func main() {
    a := [3]int{89, 90, 91}
    modify(a[:])
    fmt.Println(a)
}

```

#### Methods of anonymous struct fields

Methods belonging to anonymous fields of a struct can be called as if they belong to the structure where the anonymous field is defined.

```go
package main

import (  
    "fmt"
)

type address struct {  
    city  string
    state string
}

func (a address) fullAddress() {  
    fmt.Printf("Full address: %s, %s", a.city, a.state)
}

type person struct {  
    firstName string
    lastName  string
    address
}

func main() {  
    p := person{
        firstName: "Elon",
        lastName:  "Musk",
        address: address {
            city:  "Los Angeles",
            state: "California",
        },
    }

    p.fullAddress() //accessing fullAddress method of address struct

}
```

#### Methods with non-struct receivers

```go
package main

import "fmt"

type myInt int

func (a myInt) add(b myInt) myInt {  
    return a + b
}

func main() {  
    num1 := myInt(5)
    num2 := myInt(10)
    sum := num1.add(num2)
    fmt.Println("Sum is", sum)
}
```

#### Practical use of an interface

```go
package main

import (  
    "fmt"
)

type SalaryCalculator interface {  
    CalculateSalary() int
}

type Permanent struct {  
    empId    int
    basicpay int
    pf       int
}

type Contract struct {  
    empId    int
    basicpay int
}

//salary of permanent employee is the sum of basic pay and pf
func (p Permanent) CalculateSalary() int {  
    return p.basicpay + p.pf
}

//salary of contract employee is the basic pay alone
func (c Contract) CalculateSalary() int {  
    return c.basicpay
}

/*
total expense is calculated by iterating through the SalaryCalculator slice and summing  
the salaries of the individual employees  
*/
func totalExpense(s []SalaryCalculator) {  
    expense := 0
    for _, v := range s {
        expense = expense + v.CalculateSalary()
    }
    fmt.Printf("Total Expense Per Month $%d", expense)
}

func main() {  
    pemp1 := Permanent{
        empId:    1,
        basicpay: 5000,
        pf:       20,
    }
    pemp2 := Permanent{
        empId:    2,
        basicpay: 6000,
        pf:       30,
    }
    cemp1 := Contract{
        empId:    3,
        basicpay: 3000,
    }
    employees := []SalaryCalculator{pemp1, pemp2, cemp1}
    totalExpense(employees)

}
```

#### Interface internal representation

An interface can be thought of as being represented internally by a tuple (type, value). type is the underlying concrete type of the interface and value holds the value of the concrete type.

```go
package main

import (  
    "fmt"
)

type Worker interface {  
    Work()
}

type Person struct {  
    name string
}

func (p Person) Work() {  
    fmt.Println(p.name, "is working")
}

func describe(w Worker) {  
    fmt.Printf("Interface type %T value %v\n", w, w)
}

func main() {  
    p := Person{
        name: "Naveen",
    }
    var w Worker = p
    describe(w)
    w.Work()
}
```

#### Type Assertion

Type assertion is used to extract the underlying value of the interface.

i.(T) is the syntax which is used to get the underlying value of interface i whose concrete type is T.

```go
package main

import (  
    "fmt"
)

func assert(i interface{}) {  
    s := i.(int) //get the underlying int value from i
    fmt.Println(s)
}

func main() {  
    var s interface{} = 56
    assert(s)
}
```

If the type doesn't match with the concrete type of the interface, program will panic.  To solve the above problem, we can use the syntax

```go
v, ok := i.(T) 
```

If the concrete type of i is not T then ok will be false and v will have the zero value of type T and the program will not panic. 

#### Type switch

A type switch is used to compare the concrete type of an interface against multiple types specified in various case statements.

```go
package main

import (  
    "fmt"
)

func findType(i interface{}) {  
    switch i.(type) {
    case string:
        fmt.Printf("I am a string and my value is %s\n", i.(string))
    case int:
        fmt.Printf("I am an int and my value is %d\n", i.(int))
    default:
        fmt.Printf("Unknown type\n")
    }
}

func main() {  
    findType("Naveen")
    findType(77)
    findType(89.98)
}

```

```go
package main

import "fmt"

type Describer interface {  
    Describe()
}
type Person struct {  
    name string
    age  int
}

func (p Person) Describe() {  
    fmt.Printf("%s is %d years old", p.name, p.age)
}

func findType(i interface{}) {  
    switch v := i.(type) {
    case Describer:
        v.Describe()
    default:
        fmt.Printf("unknown type\n")
    }
}

func main() {  
    findType("Naveen")
    p := Person{
        name: "Naveen R",
        age:  25,
    }
    findType(p)
}
```

#### Implementing interfaces using pointer receivers vs value receivers

```go
package main

import "fmt"

type Describer interface {  
    Describe()
}
type Person struct {  
    name string
    age  int
}

func (p Person) Describe() { //implemented using value receiver  
    fmt.Printf("%s is %d years old\n", p.name, p.age)
}

type Address struct {  
    state   string
    country string
}

func (a *Address) Describe() { //implemented using pointer receiver  
    fmt.Printf("State %s Country %s", a.state, a.country)
}

func main() {  
    var d1 Describer
    p1 := Person{"Sam", 25}
    d1 = p1
    d1.Describe()
    p2 := Person{"James", 32}
    d1 = &p2
    d1.Describe()

    var d2 Describer
    a := Address{"Washington", "USA"}

    /* compilation error if the following line is
       uncommented
       cannot use a (type Address) as type Describer
       in assignment: Address does not implement
       Describer (Describe method has pointer
       receiver)
    */
    //d2 = a

    d2 = &a //This works since Describer interface
    //is implemented by Address pointer in line 22
    d2.Describe()

}
```

It is legal to call a pointer-valued method on anything that is already a pointer or whose address can be taken. The concrete value stored in an interface is not addressable and hence it is not possible for the compiler to automatically take the address of a and hence this code fails.

#### Embedding interfaces

```go
package main

import (  
    "fmt"
)

type SalaryCalculator interface {  
    DisplaySalary()
}

type LeaveCalculator interface {  
    CalculateLeavesLeft() int
}

type EmployeeOperations interface {  
    SalaryCalculator
    LeaveCalculator
}

type Employee struct {  
    firstName string
    lastName string
    basicPay int
    pf int
    totalLeaves int
    leavesTaken int
}

func (e Employee) DisplaySalary() {  
    fmt.Printf("%s %s has salary $%d", e.firstName, e.lastName, (e.basicPay + e.pf))
}

func (e Employee) CalculateLeavesLeft() int {  
    return e.totalLeaves - e.leavesTaken
}

func main() {  
    e := Employee {
        firstName: "Naveen",
        lastName: "Ramanathan",
        basicPay: 5000,
        pf: 200,
        totalLeaves: 30,
        leavesTaken: 5,
    }
    var empOp EmployeeOperations = e
    empOp.DisplaySalary()
    fmt.Println("\nLeaves left =", empOp.CalculateLeavesLeft())
}
```

#### Zero value of Interface

The zero value of a interface is nil. Both value and concrete type of a nil interface is nil.

```go
package main

import "fmt"

type Describer interface {  
    Describe()
}

func main() {  
    var d1 Describer
    if d1 == nil {
        fmt.Printf("d1 is nil and has type %T value %v\n", d1, d1)
    }
}
```

### Reference

- [golangbot.com](https://golangbot.com/learn-golang-series/)
