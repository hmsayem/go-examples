
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

#### Implementing interfaces using pointer receivers and value receivers

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


#### Example of channels

```go
package main

import (  
    "fmt"
)

func calcSquares(number int, squareop chan int) {  
    sum := 0
    for number != 0 {
        digit := number % 10
        sum += digit * digit
        number /= 10
    }
    squareop <- sum
}

func calcCubes(number int, cubeop chan int) {  
    sum := 0 
    for number != 0 {
        digit := number % 10
        sum += digit * digit * digit
        number /= 10
    }
    cubeop <- sum
} 

func main() {  
    number := 589
    sqrch := make(chan int)
    cubech := make(chan int)
    go calcSquares(number, sqrch)
    go calcCubes(number, cubech)
    squares, cubes := <-sqrch, <-cubech
    fmt.Println("Final output", squares + cubes)
}
```

#### Unidirectional channels

It is possible to convert a bidirectional channel to a send only or receive only channel but not the vice versa.

```go
package main

import "fmt"

func sendData(sendch chan<- int) {  
    sendch <- 10
}

func main() {  
    chnl := make(chan int)
    go sendData(chnl)
    fmt.Println(<-chnl)
}
```

#### Closing channels 

Senders have the ability to close the channel to notify receivers that no more data will be sent on the channel.

```go
package main

import (  
    "fmt"
)

func producer(chnl chan int) {  
    for i := 0; i < 10; i++ {
        chnl <- i
    }
    close(chnl)
}

func main() {  
    ch := make(chan int)
    go producer(ch)
    for {
        v, ok := <-ch
        if ok == false {
            break
        }
        fmt.Println("Received ", v, ok)
    }
}
```

```go
package main

import (  
    "fmt"
)

func digits(number int, dchnl chan int) {  
    for number != 0 {
        digit := number % 10
        dchnl <- digit
        number /= 10
    }
    close(dchnl)
}
func calcSquares(number int, squareop chan int) {  
    sum := 0
    dch := make(chan int)
    go digits(number, dch)
    for digit := range dch {
        sum += digit * digit
    }
    squareop <- sum
}

func calcCubes(number int, cubeop chan int) {  
    sum := 0
    dch := make(chan int)
    go digits(number, dch)
    for digit := range dch {
        sum += digit * digit * digit
    }
    cubeop <- sum
}

func main() {  
    number := 589
    sqrch := make(chan int)
    cubech := make(chan int)
    go calcSquares(number, sqrch)
    go calcCubes(number, cubech)
    squares, cubes := <-sqrch, <-cubech
    fmt.Println("Final output", squares+cubes)
}
```

#### Example of buffered channel

```go
package main

import (  
    "fmt"
    "time"
)

func write(ch chan int) {  
    for i := 0; i < 5; i++ {
        ch <- i
        fmt.Println("successfully wrote", i, "to ch")
    }
    close(ch)
}
func main() {  
    ch := make(chan int, 2)
    go write(ch)
    time.Sleep(2 * time.Second)
    for v := range ch {
        fmt.Println("read value", v,"from ch")
        time.Sleep(2 * time.Second)
    }
}
```

#### Closing buffered channels

It's possible to read data from a already closed buffered channel. The channel will return the data that is already written to the channel and once all the data has been read, it will return the zero value of the channel.

```go
package main

import (  
    "fmt"
)

func main() {  
    ch := make(chan int, 5)
    ch <- 5
    ch <- 6
    close(ch)
    n, open := <-ch 
    fmt.Printf("Received: %d, open: %t\n", n, open)
    n, open = <-ch 
    fmt.Printf("Received: %d, open: %t\n", n, open)
    n, open = <-ch 
    fmt.Printf("Received: %d, open: %t\n", n, open)
}
```

```go
func main() {  
    ch := make(chan int, 5)
    ch <- 5
    ch <- 6
    close(ch)
    for n := range ch {
        fmt.Println("Received:", n)
    }
}
```

#### Length and capacity of the buffered channel

The capacity of a buffered channel is the number of values that the channel can hold. The length of the buffered channel is the number of elements currently queued in it.

```go
package main

import (  
    "fmt"
)

func main() {  
    ch := make(chan string, 3)
    ch <- "naveen"
    ch <- "paul"
    fmt.Println("capacity is", cap(ch))
    fmt.Println("length is", len(ch))
    fmt.Println("read value", <-ch)
    fmt.Println("new length is", len(ch))
}
```

#### Worker Pool Implementation

```go
package main

import (
	"fmt"
	"math/rand"
	"sync"
	"time"
)

type job struct {
	id           int
	randomNumber int
}

type result struct {
	job
	sumOfDigits int
}

var jobs = make(chan job, 10)
var results = make(chan result, 10)

func SumOfDigits(num int) int {
	sum := 0
	for num != 0 {
		sum += num % 10
		num /= 10
	}
	return sum
}

func worker(wg *sync.WaitGroup) {
	for job := range jobs {
		output := result{job, SumOfDigits(job.randomNumber)}
		results <- output
	}
	wg.Done()
}

func createWorkerPool(numOfWorkers int) {
	wg := sync.WaitGroup{}
	for i := 1; i <= numOfWorkers; i++ {
		wg.Add(1)
		go worker(&wg)
	}

	wg.Wait()
	close(results)
}

func generateJobs(numOfJobs int) {
	for i := 1; i <= numOfJobs; i++ {
		newJob := job{i, rand.Intn(1000)}
		jobs <- newJob
	}
	close(jobs)
}

func getResult(done chan bool) {
	for result := range results {
		fmt.Println("Job: ", result.job, "Output: ", result.sumOfDigits)
	}
	done <- true
}

func main() {
	startTime := time.Now()
	go generateJobs(100)
	go createWorkerPool(5)
	done := make(chan bool)
	go getResult(done)
	<-done
	fmt.Println("Total time taken: ", time.Now().Sub(startTime).Seconds(), "seconds")
}
```

#### Select

The select statement is used to choose from multiple send/receive channel operations. The select statement blocks until one of the send/receive operations is ready. If multiple operations are ready, one of them is chosen at random. This way we can send the same request to multiple servers and return the quickest response to the user.

```go
package main

import (  
    "fmt"
    "time"
)

func server1(ch chan string) {  
    time.Sleep(6 * time.Second)
    ch <- "from server1"
}
func server2(ch chan string) {  
    time.Sleep(3 * time.Second)
    ch <- "from server2"

}
func main() {  
    output1 := make(chan string)
    output2 := make(chan string)
    go server1(output1)
    go server2(output2)
    select {
    case s1 := <-output1:
        fmt.Println(s1)
    case s2 := <-output2:
        fmt.Println(s2)
    }
}
```

The default case in a select statement is executed when none of the other cases is ready. This is generally used to prevent the select statement from blocking.

```go
package main

import (  
    "fmt"
    "time"
)

func process(ch chan string) {  
    time.Sleep(10500 * time.Millisecond)
    ch <- "process successful"
}

func main() {  
    ch := make(chan string)
    go process(ch)
    for {
        time.Sleep(1000 * time.Millisecond)
        select {
        case v := <-ch:
            fmt.Println("received value: ", v)
            return
        default:
            fmt.Println("no value received")
        }
    }

}
```
#### Solving the race condition using a mutex

```go
package main

import (
	"fmt"
	"sync"
)

var x = 0

func increment(wg *sync.WaitGroup, m *sync.Mutex) {
	m.Lock()
	x = x + 1
	m.Unlock()
	wg.Done()
}
func main() {
	var w sync.WaitGroup
	var m sync.Mutex
	for i := 0; i < 1000; i++ {
		w.Add(1)
		go increment(&w, &m)
	}
	w.Wait()
	fmt.Println("Final value of x", x)
}
```

#### Solving the race condition using channel

```go
package main

import (
	"fmt"
	"sync"
)

var x = 0

func increment(wg *sync.WaitGroup, ch chan bool) {
	ch <- true
	x = x + 1
	<-ch
	wg.Done()
}
func main() {
	var w sync.WaitGroup
	ch := make(chan bool, 0)
	for i := 0; i < 1000; i++ {
		w.Add(1)
		go increment(&w, ch)
	}
	w.Wait()
	fmt.Println("final value of x", x)
}
```

### Reference

- [golangbot.com](https://golangbot.com/learn-golang-series/)
