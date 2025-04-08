# Example

## Modul Pembelajaran Golang Komprehensif dengan Source Code

### Daftar Isi

1. [Tingkat Dasar](https://claude.ai/chat/45d80534-ae32-4b37-b79c-330d79a2e07d#tingkat-dasar)
2. [Tingkat Menengah](https://claude.ai/chat/45d80534-ae32-4b37-b79c-330d79a2e07d#tingkat-menengah)
3. [Tingkat Lanjut (Advanced)](https://claude.ai/chat/45d80534-ae32-4b37-b79c-330d79a2e07d#tingkat-lanjut-advanced)
4. [Proyek Aplikasi](https://claude.ai/chat/45d80534-ae32-4b37-b79c-330d79a2e07d#proyek-aplikasi)
5. [Sumber Belajar Tambahan](https://claude.ai/chat/45d80534-ae32-4b37-b79c-330d79a2e07d#sumber-belajar-tambahan)

***

## Tingkat Dasar

### Modul 1: Pengenalan Golang

#### 1.1 Sejarah dan Filosofi Go

Go (atau Golang) adalah bahasa pemrograman open-source yang dikembangkan oleh Google pada tahun 2007 oleh Robert Griesemer, Rob Pike, dan Ken Thompson. Go dirancang untuk mengatasi kritik terhadap bahasa lain dan sekaligus mempertahankan kelebihan mereka:

* Kompilasi cepat seperti bahasa dinamis
* Keamanan tipe statis
* Memiliki garbage collection
* Mendukung konkurensi bawaan
* Mudah dibaca dan sederhana

#### 1.2 Instalasi dan Setup Environment

**Instalasi Go**

Untuk menginstal Go, kunjungi [golang.org/dl](https://golang.org/dl/) dan ikuti petunjuk untuk sistem operasi Anda.

**Untuk Windows:**

1. Download installer
2. Jalankan .msi file
3. Ikuti petunjuk instalasi

**Untuk macOS:**

```bash
# Dengan Homebrew
brew install go
```

**Untuk Linux:**

```bash
# Ubuntu/Debian
sudo apt-get update
sudo apt-get install golang-go

# CentOS/RHEL
sudo yum install golang
```

**Mengatur Workspace**

Go memiliki konvensi workspace tertentu:

```bash
mkdir -p $HOME/go/{bin,src,pkg}
```

Tambahkan ke file .bashrc atau .zshrc:

```bash
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

#### 1.3 Hello World dan Struktur Program Go

**Hello World Program:**

```go
// file: hello.go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

**Cara menjalankan:**

```bash
go run hello.go
```

**Cara compile:**

```bash
go build hello.go
./hello  # Menjalankan binary yang dihasilkan
```

### Modul 2: Dasar-Dasar Bahasa Go

#### 2.1 Variabel dan Tipe Data

```go
package main

import "fmt"

func main() {
    // Deklarasi variabel
    var name string = "Gopher"
    var age int = 25
    
    // Shorthand declaration
    height := 175.5  // float64
    isActive := true // boolean
    
    // Multiple variable declaration
    var (
        firstName string = "John"
        lastName  string = "Doe"
        salary    float64 = 50000.0
    )
    
    // Constants
    const Pi = 3.14159
    const (
        StatusOK = 200
        StatusNotFound = 404
    )
    
    // Zero values
    var defaultInt int         // 0
    var defaultFloat float64   // 0.0
    var defaultBool bool       // false
    var defaultString string   // ""
    
    fmt.Println("Name:", name)
    fmt.Println("Age:", age)
    fmt.Println("Height:", height)
    fmt.Println("Is Active:", isActive)
    fmt.Println("Full Name:", firstName, lastName)
    fmt.Println("Salary:", salary)
    fmt.Println("Pi:", Pi)
    fmt.Println("HTTP OK:", StatusOK)
    
    fmt.Println("Default int:", defaultInt)
    fmt.Println("Default float:", defaultFloat)
    fmt.Println("Default bool:", defaultBool)
    fmt.Println("Default string:", defaultString)
}
```

#### 2.2 Operator dan Ekspresi

```go
package main

import "fmt"

func main() {
    // Arithmetic operators
    a, b := 10, 3
    fmt.Println("a + b =", a+b)  // Addition
    fmt.Println("a - b =", a-b)  // Subtraction
    fmt.Println("a * b =", a*b)  // Multiplication
    fmt.Println("a / b =", a/b)  // Division (integer)
    fmt.Println("a % b =", a%b)  // Modulus (remainder)
    
    // Comparison operators
    fmt.Println("a == b:", a == b)  // Equal
    fmt.Println("a != b:", a != b)  // Not equal
    fmt.Println("a > b:", a > b)    // Greater than
    fmt.Println("a < b:", a < b)    // Less than
    fmt.Println("a >= b:", a >= b)  // Greater than or equal
    fmt.Println("a <= b:", a <= b)  // Less than or equal
    
    // Logical operators
    c, d := true, false
    fmt.Println("c && d:", c && d)  // Logical AND
    fmt.Println("c || d:", c || d)  // Logical OR
    fmt.Println("!c:", !c)          // Logical NOT
    
    // Assignment operators
    x := 10
    x += 5  // Same as: x = x + 5
    fmt.Println("x after += 5:", x)
    
    x -= 3  // Same as: x = x - 3
    fmt.Println("x after -= 3:", x)
    
    x *= 2  // Same as: x = x * 2
    fmt.Println("x after *= 2:", x)
    
    x /= 4  // Same as: x = x / 4
    fmt.Println("x after /= 4:", x)
    
    // Bitwise operators
    p, q := 12, 25  // In binary: 1100, 11001
    fmt.Printf("p & q = %d\n", p&q)    // Bitwise AND
    fmt.Printf("p | q = %d\n", p|q)    // Bitwise OR
    fmt.Printf("p ^ q = %d\n", p^q)    // Bitwise XOR
    fmt.Printf("p << 1 = %d\n", p<<1)  // Left shift
    fmt.Printf("p >> 1 = %d\n", p>>1)  // Right shift
}
```

#### 2.3 Kontrol Alur

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    // If-Else Statement
    age := 18
    if age >= 18 {
        fmt.Println("You are an adult")
    } else {
        fmt.Println("You are a minor")
    }
    
    // If with short statement
    if score := 85; score >= 60 {
        fmt.Println("You passed!")
        if score >= 90 {
            fmt.Println("With distinction!")
        }
    } else {
        fmt.Println("You failed!")
    }
    
    // Switch statement
    day := time.Now().Weekday()
    switch day {
    case time.Saturday, time.Sunday:
        fmt.Println("It's the weekend!")
    default:
        fmt.Println("It's a weekday.")
    }
    
    // Switch without expression (like if-else-if)
    hour := time.Now().Hour()
    switch {
    case hour < 12:
        fmt.Println("Good morning!")
    case hour < 17:
        fmt.Println("Good afternoon!")
    default:
        fmt.Println("Good evening!")
    }
    
    // For loop - standard
    for i := 0; i < 5; i++ {
        fmt.Printf("%d ", i)
    }
    fmt.Println()
    
    // For loop - like while loop
    count := 0
    for count < 5 {
        fmt.Printf("%d ", count)
        count++
    }
    fmt.Println()
    
    // For loop - infinite with break
    sum := 0
    for {
        sum++
        if sum > 10 {
            break
        }
    }
    fmt.Println("Sum:", sum)
    
    // For loop with continue
    for i := 0; i < 10; i++ {
        if i%2 == 0 {
            continue  // Skip even numbers
        }
        fmt.Printf("%d ", i)
    }
    fmt.Println()
    
    // For-range loop for slices
    fruits := []string{"apple", "banana", "cherry"}
    for index, fruit := range fruits {
        fmt.Printf("%d: %s\n", index, fruit)
    }
    
    // For-range loop for maps
    scores := map[string]int{"Alice": 98, "Bob": 85, "Charlie": 92}
    for name, score := range scores {
        fmt.Printf("%s scored %d\n", name, score)
    }
}
```

#### 2.4 Fungsi

```go
package main

import (
    "fmt"
    "strings"
)

// Basic function
func greet(name string) string {
    return "Hello, " + name
}

// Multiple return values
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("cannot divide by zero")
    }
    return a / b, nil
}

// Named return values
func calculate(width, height float64) (area, perimeter float64) {
    area = width * height
    perimeter = 2 * (width + height)
    return  // Naked return
}

// Variadic function
func sum(numbers ...int) int {
    total := 0
    for _, num := range numbers {
        total += num
    }
    return total
}

// Function as parameter
func apply(fn func(int, int) int, a, b int) int {
    return fn(a, b)
}

// Anonymous function
var factorial = func(n int) int {
    if n <= 0 {
        return 1
    }
    return n * factorial(n-1)
}

// Closure
func makeCounter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}

// Deferred function
func processFile(filename string) {
    fmt.Println("Opening file:", filename)
    defer fmt.Println("Closing file:", filename)  // Will execute when function returns
    
    // Process file contents
    fmt.Println("Processing file contents...")
    
    // Multiple defers execute in LIFO order
    defer fmt.Println("Cleanup operation 1")
    defer fmt.Println("Cleanup operation 2")
}

func main() {
    // Basic function call
    message := greet("Gopher")
    fmt.Println(message)
    
    // Multiple return values
    result, err := divide(10, 2)
    if err != nil {
        fmt.Println("Error:", err)
    } else {
        fmt.Println("Result:", result)
    }
    
    // Named return values
    area, perimeter := calculate(5, 3)
    fmt.Printf("Area: %.2f, Perimeter: %.2f\n", area, perimeter)
    
    // Variadic function
    fmt.Println("Sum:", sum(1, 2, 3, 4, 5))
    
    // Pass a slice to variadic function
    numbers := []int{1, 2, 3, 4, 5}
    fmt.Println("Sum from slice:", sum(numbers...))
    
    // Function as parameter
    multiply := func(x, y int) int { return x * y }
    fmt.Println("Apply multiply:", apply(multiply, 5, 3))
    
    // Anonymous function
    fmt.Println("5! =", factorial(5))
    
    // Immediately invoked function expression (IIFE)
    result = func(x, y int) int {
        return x + y
    }(10, 20)
    fmt.Println("IIFE result:", result)
    
    // Closure
    counter := makeCounter()
    fmt.Println(counter())  // 1
    fmt.Println(counter())  // 2
    fmt.Println(counter())  // 3
    
    // Deferred function
    processFile("example.txt")
    
    // Practical defer use: measure function execution time
    start := time.Now()
    defer func() {
        fmt.Printf("Execution time: %v\n", time.Since(start))
    }()
    
    // Simulate work
    time.Sleep(100 * time.Millisecond)
}
```

### Modul 3: Struktur Data Dasar

#### 3.1 Array dan Slice

```go
package main

import "fmt"

func main() {
    // Arrays
    // Fixed size, same type
    var scores [5]int                      // Declare an array of 5 integers
    scores[0] = 95                         // Assign value to first element
    fmt.Println("Scores array:", scores)   // [95 0 0 0 0]
    
    // Array initialization
    names := [3]string{"Alice", "Bob", "Charlie"}
    fmt.Println("Names array:", names)
    
    // Array with inferred size
    days := [...]string{"Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"}
    fmt.Println("Days array length:", len(days))
    
    // Multi-dimensional arrays
    matrix := [2][3]int{
        {1, 2, 3},
        {4, 5, 6},
    }
    fmt.Println("Matrix:", matrix)
    
    // Slices
    // Dynamic size, same type
    var fruits []string                // Declare a slice, initialized to nil
    fmt.Println("Fruits slice:", fruits)
    fmt.Println("Is fruits nil?", fruits == nil)
    
    // Create slice with make
    numbers := make([]int, 5)          // length=5, capacity=5
    fmt.Println("Numbers slice:", numbers)
    
    // Create slice with make and specify capacity
    moreNumbers := make([]int, 3, 10)  // length=3, capacity=10
    fmt.Printf("Length: %d, Capacity: %d\n", len(moreNumbers), cap(moreNumbers))
    
    // Slice literal
    cities := []string{"New York", "London", "Tokyo"}
    fmt.Println("Cities slice:", cities)
    
    // Slice from array
    weekdays := days[0:5]              // Mon to Fri
    fmt.Println("Weekdays:", weekdays)
    
    weekend := days[5:7]               // Sat and Sun
    fmt.Println("Weekend:", weekend)
    
    // Slices are references to underlying arrays
    weekdays[0] = "Monday"
    fmt.Println("Updated first day:", days[0])
    
    // Slice operations
    // Append
    fruits = append(fruits, "Apple")
    fruits = append(fruits, "Banana", "Cherry")
    fmt.Println("Fruits after append:", fruits)
    
    // Append one slice to another
    more_fruits := []string{"Orange", "Mango"}
    fruits = append(fruits, more_fruits...)
    fmt.Println("Combined fruits:", fruits)
    
    // Copy
    src := []int{1, 2, 3, 4, 5}
    dst := make([]int, len(src))
    copied := copy(dst, src)
    fmt.Printf("Copied %d elements: %v\n", copied, dst)
    
    // Slicing operations
    letters := []string{"a", "b", "c", "d", "e", "f"}
    fmt.Println("Letters:", letters)
    
    // Slice from beginning
    fmt.Println("First three:", letters[:3])  // [a b c]
    
    // Slice to end
    fmt.Println("Last three:", letters[3:])   // [d e f]
    
    // Slice from middle
    fmt.Println("Middle two:", letters[2:4])  // [c d]
    
    // Remove element (mid)
    i := 2 // Remove 'c'
    letters = append(letters[:i], letters[i+1:]...)
    fmt.Println("After removing 'c':", letters)
    
    // 2D Slices
    grid := make([][]int, 3)
    for i := range grid {
        grid[i] = make([]int, 4)
        for j := range grid[i] {
            grid[i][j] = i*4 + j
        }
    }
    fmt.Println("2D slice:")
    for _, row := range grid {
        fmt.Println(row)
    }
}
```

#### 3.2 Map

```go
package main

import "fmt"

func main() {
    // Declare a nil map
    var emptyMap map[string]int
    fmt.Println("Empty map:", emptyMap)
    fmt.Println("Is map nil?", emptyMap == nil)
    
    // Initialize map with make
    scores := make(map[string]int)
    
    // Add key-value pairs
    scores["Alice"] = 98
    scores["Bob"] = 85
    scores["Charlie"] = 92
    fmt.Println("Scores map:", scores)
    
    // Map literal
    capitals := map[string]string{
        "USA":    "Washington, D.C.",
        "France": "Paris",
        "Japan":  "Tokyo",
        "India":  "New Delhi",
    }
    fmt.Println("Capitals map:", capitals)
    
    // Access map elements
    aliceScore := scores["Alice"]
    fmt.Println("Alice's score:", aliceScore)
    
    // Access non-existent key
    davidScore := scores["David"]  // Returns zero value (0 for int)
    fmt.Println("David's score:", davidScore)
    
    // Check if key exists
    carolScore, exists := scores["Carol"]
    if exists {
        fmt.Println("Carol's score:", carolScore)
    } else {
        fmt.Println("Carol's score not found")
    }
    
    // Delete a key-value pair
    delete(scores, "Bob")
    fmt.Println("After delete:", scores)
    
    // Check if key exists after delete
    _, bobExists := scores["Bob"]
    fmt.Println("Does Bob's score exist?", bobExists)
    
    // Iterate through the map
    fmt.Println("Scores:")
    for name, score := range scores {
        fmt.Printf("%s: %d\n", name, score)
    }
    
    // Map size
    fmt.Println("Number of capitals:", len(capitals))
    
    // Maps are references
    copiedScores := scores
    copiedScores["Alice"] = 100
    fmt.Println("Original scores after modification:", scores)
    
    // Clear a map
    for key := range capitals {
        delete(capitals, key)
    }
    fmt.Println("Cleared capitals map:", capitals)
    fmt.Println("Capitals map size:", len(capitals))
    
    // Maps as sets
    seen := make(map[string]bool)
    words := []string{"apple", "banana", "apple", "cherry", "banana"}
    
    for _, word := range words {
        seen[word] = true
    }
    
    fmt.Print("Unique words: ")
    for word := range seen {
        fmt.Printf("%s ", word)
    }
    fmt.Println()
    
    // Nested maps
    users := map[string]map[string]string{
        "user1": {
            "name":  "John Doe",
            "email": "john@example.com",
        },
        "user2": {
            "name":  "Jane Smith",
            "email": "jane@example.com",
        },
    }
    
    fmt.Println("User 1 name:", users["user1"]["name"])
    
    // Add a new nested map
    users["user3"] = map[string]string{
        "name":  "Bob Johnson",
        "email": "bob@example.com",
    }
    
    // Print all users
    fmt.Println("All users:")
    for id, user := range users {
        fmt.Printf("ID: %s, Name: %s, Email: %s\n", id, user["name"], user["email"])
    }
}
```

#### 3.3 Struct

```go
package main

import (
    "fmt"
    "time"
)

// Basic struct definition
type Person struct {
    FirstName string
    LastName  string
    Age       int
}

// Struct with embedded struct
type Employee struct {
    Person     // Embedded struct (anonymous field)
    EmployeeID string
    Position   string
    HireDate   time.Time
}

// Struct with tags
type Product struct {
    ID          int     `json:"id"`
    Name        string  `json:"name"`
    Description string  `json:"description,omitempty"`
    Price       float64 `json:"price"`
    Quantity    int     `json:"qty"`
}

// Struct with methods
type Rectangle struct {
    Width  float64
    Height float64
}

// Method on struct
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Method with pointer receiver
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

func main() {
    // Create a struct instance
    p1 := Person{
        FirstName: "John",
        LastName:  "Doe",
        Age:       30,
    }
    fmt.Println("Person:", p1)
    
    // Access struct fields
    fmt.Println("Name:", p1.FirstName, p1.LastName)
    
    // Update struct fields
    p1.Age = 31
    fmt.Println("Updated age:", p1.Age)
    
    // Struct literal with partial fields
    p2 := Person{FirstName: "Jane"}
    fmt.Printf("Person 2: %+v\n", p2)  // %+v shows field names
    
    // Struct pointer
    pp := &Person{FirstName: "Bob", LastName: "Smith", Age: 25}
    fmt.Println("Person pointer:", *pp)
    
    // Access fields through pointer (automatic dereferencing)
    pp.Age = 26
    fmt.Println("Updated pointer age:", pp.Age)
    
    // Create employee with embedded struct
    emp := Employee{
        Person:     Person{FirstName: "Alice", LastName: "Johnson", Age: 28},
        EmployeeID: "EMP001",
        Position:   "Software Engineer",
        HireDate:   time.Now(),
    }
    
    // Access embedded struct fields
    fmt.Println("Employee name:", emp.FirstName, emp.LastName)  // Promoted fields
    fmt.Println("Employee position:", emp.Position)
    
    // Anonymous struct
    conf := struct {
        APIKey     string
        APISecret  string
        Production bool
    }{
        APIKey:     "abcd1234",
        APISecret:  "xyz789",
        Production: false,
    }
    fmt.Printf("Config: %+v\n", conf)
    
    // Struct comparison
    p3 := Person{FirstName: "John", LastName: "Doe", Age: 30}
    fmt.Println("p1 == p3:", p1 == p3)
    
    // Struct with methods
    rect := Rectangle{Width: 10, Height: 5}
    fmt.Println("Rectangle area:", rect.Area())
    
    // Call method with pointer receiver
    rect.Scale(2)
    fmt.Printf("Scaled rectangle: %+v, Area: %.2f\n", rect, rect.Area())
    
    // Array of structs
    people := []Person{
        {FirstName: "John", LastName: "Doe", Age: 30},
        {FirstName: "Jane", LastName: "Smith", Age: 28},
        {FirstName: "Bob", LastName: "Johnson", Age: 35},
    }
    
    fmt.Println("People:")
    for _, person := range people {
        fmt.Printf("  %s %s, %d years old\n", person.FirstName, person.LastName, person.Age)
    }
    
    // Map of structs
    employees := map[string]Employee{
        "EMP001": {
            Person:     Person{FirstName: "Alice", LastName: "Johnson", Age: 28},
            EmployeeID: "EMP001",
            Position:   "Software Engineer",
        },
        "EMP002": {
            Person:     Person{FirstName: "Bob", LastName: "Smith", Age: 35},
            EmployeeID: "EMP002",
            Position:   "Product Manager",
        },
    }
    
    fmt.Println("Employee EMP001:", employees["EMP001"].FirstName)
}
```

### Modul 4: Penanganan Error dan Debugging

#### 4.1 Error Handling

```go
package main

import (
    "errors"
    "fmt"
    "os"
    "strconv"
)

// Custom error type
type DivisionError struct {
    Dividend float64
    Divisor  float64
    Message  string
}

// Error interface implementation
func (e *DivisionError) Error() string {
    return fmt.Sprintf("%s: %f / %f", e.Message, e.Dividend, e.Divisor)
}

// Function that returns a standard error
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero not allowed")
    }
    return a / b, nil
}

// Function that returns a custom error
func safeDivide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, &DivisionError{
            Dividend: a,
            Divisor:  b,
            Message:  "cannot divide by zero",
        }
    }
    return a / b, nil
}

// Function that wraps an error
func parseAndDivide(aStr, bStr string) (float64, error) {
    a, err := strconv.ParseFloat(aStr, 64)
    if err != nil {
        return 0, fmt.Errorf("error parsing first number: %w", err)
    }
    
    b, err := strconv.ParseFloat(bStr, 64)
    if err != nil {
        return 0, fmt.Errorf("error parsing second number: %w", err)
    }
    
    result, err := divide(a, b)
    if err != nil {
        return 0, fmt.Errorf("error during division: %w", err)
    }
    
    return result, nil
}

func main() {
    // Basic error handling
    result, err := divide(10, 2)
    if err != nil {
        fmt.Println("Error:", err)
    } else {
        fmt.Println("Result:", result)
    }
    
    // Division by zero error
    result, err = divide(10, 0)
    if err != nil {
        fmt.Println("Error:", err)
    } else {
        fmt.Println("Result:", result)
    }
    
    // Custom error type
    result, err = safeDivide(10, 0)
    if err != nil {
        fmt.Println("Error:", err)
        
        // Type assertion for custom error
        if divErr, ok := err.(*DivisionError); ok {
            fmt.Printf("Tried to divide %f by %f\n", divErr.Dividend, divErr.Divisor)
        }
    }
    
    // Error wrapping
    result, err = parseAndDivide("10", "0")
    if err != nil {
        fmt.Println("Error:", err)
        
        // Check for wrapped errors
        if errors.Is(err, strconv.ErrSyntax) {
            fmt.Println("Syntax error in number parsing")
        }
    }
    
    // File operations with error handling
    file, err := os.Open("nonexistent.txt")
    if err != nil {
        if os.IsNotExist(err) {
            fmt.Println("File does not exist")
        } else {
            fmt.Println("Other error:", err)
        }
    } else {
        defer file.Close()
        fmt.Println("File opened successfully")
    }
    
    // Creating a new file
    newFile, err := os.Create("example.txt")
    if err != nil {
        fmt.Println("Error creating file:", err)
        return
    }
    defer newFile.Close()
    
    // Writing to file with error handling
    _, err = newFile.WriteString("Hello, Go Error Handling!")
    if err != nil {
        fmt.Println("Error writing to file:", err)
        return
    }
    
    fmt.Println("Successfully wrote to file")
    
    // Multiple error checks
    if _, err := os.Stat("example.txt"); err == nil {
        // File exists
        fmt.Println("File exists, removing it")
        err = os.Remove("example.txt")
        if err != nil {
            fmt.Println("Error removing file:", err)
        } else {
            fmt.Println("File removed successfully")
        }
    } else if os.IsNotExist(err) {
        fmt.Println("File doesn't exist")
    } else {
        fmt.Println("Error checking file:", err)
    }
}
```

#### 4.2 Panic dan Recover

````go
package main

import (
    "fmt"
    "runtime/debug"
)

// Function that causes a panic
func dangerousOperation() {
    fmt.Println("Starting dangerous operation")
    
    // This will cause a panic
    panic("something went horribly wrong")
    
    // This code will not be executed
    fmt.Println("This won't be printed")
}

// Function with panic recovery
func safeOperation() {
    defer func() {
        // Recover from panic
        if r := recover(); r != nil {
            fmt.Println("Recovered from panic:", r)
            fmt.Println("Stack trace:")
            debug.PrintStack()
        }
    }()
    
    fmt.Println("Starting safe operation")
    dangerousOperation()
    fmt.Println("This won't be printed")
}

// Function that handles multiple types of panics
func multiRecovery() {
    defer func() {
        if r := recover(); r != nil {
            switch x := r.(type) {
            case string:
                fmt.Println("Panic with string:", x)
            case error:
                fmt.Println("Panic with error:", x.Error())
            default:
                fmt.Println("Unknown panic type:", r)
            }
        }
    }()
    
    // Will cause panic with different types
    panicType := 2
    
    switch panicType {
    case 1:
        panic("string panic")
    case 2:
        panic(fmt.Errorf("error panic"))
    case 3:
        panic(42)
    }
}

// Function with nested defer and recover
func nestedFunction(depth int) {
    defer func() {
        if r := recover(); r != nil {fmt.Printf("Recovered at depth %d: %v\n", depth, r)
        }
    }()
    
    fmt.Printf("Entering function at depth %d\n", depth)
    
    if depth > 0 {
        nestedFunction(depth - 1)
    } else {
        panic("panic at depth 0")
    }
    
    fmt.Printf("Exiting function at depth %d\n", depth)
}

func main() {
    // Basic panic example (uncomment to see panic)
    // dangerousOperation()
    
    // Panic with recovery
    safeOperation()
    fmt.Println("Program continues after recovery")
    
    // Different types of panic
    multiRecovery()
    
    // Nested recovery
    nestedFunction(3)
    
    // Common use case: array bounds checking
    func() {
        defer func() {
            if r := recover(); r != nil {
                fmt.Println("Recovered from index out of bounds:", r)
            }
        }()
        
        items := []int{1, 2, 3}
        
        // This will cause a panic
        item := items[5]
        fmt.Println("Item:", item)
    }()
    
    fmt.Println("Program completed successfully")
}
```

### 4.3 Debugging

```go
package main

import (
    "fmt"
    "log"
    "os"
    "runtime"
    "time"
)

// Basic debugging with print statements
func debugPrint() {
    x := 10
    y := 20
    
    fmt.Println("x =", x)
    fmt.Println("y =", y)
    fmt.Println("x + y =", x+y)
    
    // Print with different formatting
    fmt.Printf("x = %d, y = %d, x + y = %d\n", x, y, x+y)
}

// Using log package for debugging
func setupLogging() {
    // Log with date and time
    log.SetFlags(log.Ldate | log.Ltime)
    
    // Log to file
    logFile, err := os.Create("debug.log")
    if err != nil {
        log.Fatal("Could not create log file:", err)
    }
    log.SetOutput(logFile)
}

// Debug function to print runtime info
func debugInfo() {
    fmt.Println("--- Debug Info ---")
    fmt.Printf("NumCPU: %d\n", runtime.NumCPU())
    fmt.Printf("GOMAXPROCS: %d\n", runtime.GOMAXPROCS(0))
    fmt.Printf("NumGoroutine: %d\n", runtime.NumGoroutine())
    
    var memStats runtime.MemStats
    runtime.ReadMemStats(&memStats)
    fmt.Printf("Alloc: %d KB\n", memStats.Alloc/1024)
    fmt.Printf("TotalAlloc: %d KB\n", memStats.TotalAlloc/1024)
    fmt.Printf("Sys: %d KB\n", memStats.Sys/1024)
    fmt.Printf("NumGC: %d\n", memStats.NumGC)
    fmt.Println("-----------------")
}

// Custom logger
type DebugLogger struct {
    enabled bool
    prefix  string
}

func NewDebugLogger(enabled bool, prefix string) *DebugLogger {
    return &DebugLogger{
        enabled: enabled,
        prefix:  prefix,
    }
}

func (l *DebugLogger) Log(format string, args ...interface{}) {
    if l.enabled {
        message := fmt.Sprintf(format, args...)
        timestamp := time.Now().Format("15:04:05.000")
        fmt.Printf("[%s] %s: %s\n", timestamp, l.prefix, message)
    }
}

// Function to demonstrate debug messages in a loop
func processItems(logger *DebugLogger) {
    items := []string{"apple", "banana", "cherry"}
    
    for i, item := range items {
        logger.Log("Processing item %d: %s", i, item)
        
        // Simulate processing
        time.Sleep(100 * time.Millisecond)
        
        logger.Log("Finished processing item %d", i)
    }
}

func main() {
    // Basic debug printing
    debugPrint()
    
    // Setup logging
    // setupLogging() // Uncomment to log to file
    
    // Log debug messages
    log.Println("Starting program")
    log.Println("Initializing...")
    
    // Debug info about runtime
    debugInfo()
    
    // Custom debug logger
    debugLogger := NewDebugLogger(true, "DEBUG")
    errorLogger := NewDebugLogger(true, "ERROR")
    
    debugLogger.Log("Program started")
    
    // Process items with debug logging
    processItems(debugLogger)
    
    // Example of error logging
    _, err := os.Open("nonexistent.txt")
    if err != nil {
        errorLogger.Log("Failed to open file: %v", err)
    }
    
    // Final debug info
    debugInfo()
    debugLogger.Log("Program completed")
    
    log.Println("Program finished")
    
    // Debugging tip: Use environment variables to control debug output
    if os.Getenv("DEBUG") == "1" {
        fmt.Println("Additional debug info enabled via environment variable")
    }
}
```
````

### Modul 5: Package dan Modules

Struktur proyek untuk contoh:

```
my-go-project/
├── go.mod
├── main.go
├── utils/
│   ├── math.go
│   └── strings.go
└── models/
    └── user.go
```

#### 5.1 math.go (dalam folder utils)

```go
// File: utils/math.go
package utils

// Add returns the sum of two integers
// This function is exported (starts with capital letter)
func Add(a, b int) int {
    return a + b
}

// Subtract returns the difference between two integers
// This function is exported
func Subtract(a, b int) int {
    return a - b
}

// multiply is an unexported function (starts with lowercase)
// It can only be used within the utils package
func multiply(a, b int) int {
    return a * b
}

// Multiply is an exported wrapper for the unexported multiply function
func Multiply(a, b int) int {
    return multiply(a, b)
}

// These variables are exported and can be accessed from other packages
var (
    Pi = 3.14159
    E  = 2.71828
)

// this constant is unexported
const maxValue = 1000

// MaxValue is an exported function to access the unexported constant
func MaxValue() int {
    return maxValue
}
```

#### 5.2 strings.go (dalam folder utils)

```go
// File: utils/strings.go
package utils

import (
    "strings"
    "unicode"
)

// Reverse returns the reverse of a string
func Reverse(s string) string {
    runes := []rune(s)
    for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
        runes[i], runes[j] = runes[j], runes[i]
    }
    return string(runes)
}

// Capitalize capitalizes the first letter of each word in a string
func Capitalize(s string) string {
    words := strings.Fields(s)
    for i, word := range words {
        if len(word) > 0 {
            r := []rune(word)
            r[0] = unicode.ToUpper(r[0])
            words[i] = string(r)
        }
    }
    return strings.Join(words, " ")
}

// init function runs when the package is imported
func init() {
    // Initialization code goes here
    // This runs before main()
    // Each file can have its own init function
    // They execute in the order they are imported
}
```

#### 5.3 user.go (dalam folder models)

```go
// File: models/user.go
package models

import (
    "fmt"
    "time"
)

// User represents a user in the system
type User struct {
    ID        int
    Username  string
    Email     string
    CreatedAt time.Time
}

// NewUser creates a new user with the given username and email
func NewUser(username, email string) *User {
    return &User{
        Username:  username,
        Email:     email,
        CreatedAt: time.Now(),
    }
}

// String implements the Stringer interface for User
func (u User) String() string {
    return fmt.Sprintf("User{ID: %d, Username: %s, Email: %s, CreatedAt: %s}",
        u.ID, u.Username, u.Email, u.CreatedAt.Format(time.RFC3339))
}

// ValidateEmail checks if the user's email is valid
// This is a simple example; in a real app, you'd use regex
func (u User) ValidateEmail() bool {
    return len(u.Email) > 3 && strings.Contains(u.Email, "@")
}
```

#### 5.4 main.go (file utama)

```go
// File: main.go
package main

import (
    "fmt"
    "log"
    
    // Import local packages
    "my-go-project/models"
    "my-go-project/utils"
    
    // Import third-party package
    "github.com/google/uuid"
)

func main() {
    // Using functions from utils package
    sum := utils.Add(10, 5)
    diff := utils.Subtract(10, 5)
    product := utils.Multiply(10, 5)
    
    fmt.Println("Sum:", sum)
    fmt.Println("Difference:", diff)
    fmt.Println("Product:", product)
    
    // Using exported variables
    fmt.Println("Pi:", utils.Pi)
    fmt.Println("Max value:", utils.MaxValue())
    
    // Using string utilities
    original := "hello, world"
    reversed := utils.Reverse(original)
    capitalized := utils.Capitalize(original)
    
    fmt.Println("Original:", original)
    fmt.Println("Reversed:", reversed)
    fmt.Println("Capitalized:", capitalized)
    
    // Using models
    user := models.NewUser("johndoe", "john@example.com")
    fmt.Println("User:", user)
    
    if user.ValidateEmail() {
        fmt.Println("Email is valid")
    } else {
        fmt.Println("Email is invalid")
    }
    
    // Using third-party package
    id := uuid.New()
    fmt.Println("Generated UUID:", id.String())
    
    log.Println("Program finished successfully")
}
```

#### 5.5 go.mod dan go.sum

```
// go.mod
module my-go-project

go 1.18

require github.com/google/uuid v1.3.0
```

```
// go.sum
github.com/google/uuid v1.3.0 h1:t6JiXgmwXMjEs8VusXIJk2BXHsn+wx8BZdTaoZ5fu7I=
github.com/google/uuid v1.3.0/go.mod h1:TIyPZe4MgqvfeYDBFedMoGGpEw/LqOeaOT+nhxU+yHo=
```

## Tingkat Menengah

### Modul 6: Pointer dan Memory Management

```go
package main

import (
    "fmt"
    "runtime"
    "unsafe"
)

// Basic value type
type Point struct {
    X, Y int
}

// Method with value receiver
func (p Point) MoveBy(dx, dy int) Point {
    return Point{p.X + dx, p.Y + dy}
}

// Method with pointer receiver
func (p *Point) MoveByPointer(dx, dy int) {
    p.X += dx
    p.Y += dy
}

// Function that takes a value
func doubleValue(x int) int {
    x *= 2
    return x
}

// Function that takes a pointer
func doublePointer(x *int) {
    *x *= 2
}

// Function that modifies a slice
func appendToSlice(slice []int, val int) []int {
    return append(slice, val)
}

// Function that modifies a map
func addToMap(m map[string]int, key string, val int) {
    m[key] = val
}

func main() {
    // Basic pointer declaration and usage
    x := 42
    p := &x         // p is a pointer to x
    fmt.Println("x =", x)
    fmt.Println("p =", p)   // Prints memory address
    fmt.Println("*p =", *p) // Dereference: prints value at address
    
    // Modifying via pointer
    *p = 21
    fmt.Println("After *p = 21, x =", x)
    
    // Pointer to pointer
    var pp **int = &p
    fmt.Println("**pp =", **pp) // Double dereference
    
    // nil pointer
    var nilPtr *int
    fmt.Println("nilPtr =", nilPtr)
    // Uncommenting the next line would cause a panic:
    // fmt.Println("*nilPtr =", *nilPtr) // panic: nil pointer dereference
    
    // Pointer to struct
    point := Point{10, 20}
    pointPtr := &point
    
    fmt.Println("point =", point)
    fmt.Println("pointPtr =", pointPtr)
    fmt.Println("*pointPtr =", *pointPtr)
    
    // Accessing struct fields through pointer (automatic dereferencing)
    pointPtr.X = 15 // Same as (*pointPtr).X = 15
    fmt.Println("After pointPtr.X = 15, point =", point)
    
    // Methods with value and pointer receivers
    p1 := Point{1, 2}
    
    // Value receiver returns a new Point
    p2 := p1.MoveBy(2, 3)
    fmt.Println("p1 after MoveBy =", p1) // Unchanged
    fmt.Println("p2 =", p2)              // New point
    
    // Pointer receiver modifies the existing Point
    p1.MoveByPointer(2, 3)
    fmt.Println("p1 after MoveByPointer =", p1) // Changed
    
    // Pass by value vs Pass by reference
    num := 10
    
    // Pass by value: original unchanged
    result := doubleValue(num)
    fmt.Println("After doubleValue, num =", num)       // Still 10
    fmt.Println("doubleValue result =", result)        // 20
    
    // Pass by reference: original changed
    doublePointer(&num)
    fmt.Println("After doublePointer, num =", num)     // Now 20
    
    // Slices are reference types (contain pointer to underlying array)
    slice := []int{1, 2, 3}
    slice2 := appendToSlice(slice, 4)
    fmt.Println("Original slice =", slice)   // [1 2 3]
    fmt.Println("Modified slice =", slice2)  // [1 2 3 4]
    
    // Maps are reference types
    m := map[string]int{"one": 1, "two": 2}
    addToMap(m, "three", 3)
    fmt.Println("Map after addToMap =", m)  // map[one:1 three:3 two:2]
    
    // Memory management examples
    
    // Size of types
    fmt.Printf("Size of int: %d bytes\n", unsafe.Sizeof(int(0)))
    fmt.Printf("Size of Point: %d bytes\n", unsafe.Sizeof(Point{}))
    fmt.Printf("Size of pointer: %d bytes\n", unsafe.Sizeof(&x))
    
    // Memory allocation
    allocPoint := new(Point) // Allocates memory, returns pointer
    *allocPoint = Point{5, 10}
    fmt.Println("allocPoint =", *allocPoint)
    
    // Manual memory management is not needed in Go
    // The garbage collector automatically reclaims memory
    
    // Force a garbage collection
    printMemStats("Before GC")
    runtime.GC()
    printMemStats("After GC")
    
    // Memory leak example (not recommended!)
    // leakyFunction()
}

// Function to print memory statistics
func printMemStats(msg string) {
    var m runtime.MemStats
    runtime.ReadMemStats(&m)
    
    fmt.Printf("%s:\n", msg)
    fmt.Printf("  Alloc = %v KB\n", m.Alloc / 1024)
    fmt.Printf("  TotalAlloc = %v KB\n", m.TotalAlloc / 1024)
    fmt.Printf("  Sys = %v KB\n", m.Sys / 1024)
    fmt.Printf("  NumGC = %v\n", m.NumGC)
}

// Example of a function that could cause a memory leak
// (keeping a reference to objects that are no longer needed)
func leakyFunction() {
    // This would create a slice that keeps growing
    // but we never release references to the old data
    leakySlice := make([]Point, 0)
    
    for i := 0; i < 1000000; i++ {
        leakySlice = append(leakySlice, Point{i, i})
        
        // In a real leak, we might store this slice somewhere
        // preventing the GC from reclaiming its memory
    }
    
    // In reality, leakySlice will be garbage collected when this function returns
    // but if it were stored in a global variable or closure, it could cause a leak
}
```

### Modul 7: Interface dan Type System

```go
package main

import (
    "fmt"
    "io"
    "math"
    "sort"
    "strings"
)

// Basic interface definition
type Shape interface {
    Area() float64
    Perimeter() float64
}

// Implementing the Shape interface with Circle
type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * math.Pi * c.Radius
}

// Implementing the Shape interface with Rectangle
type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

// Interface for objects that can have a name
type Named interface {
    Name() string
}

// Person implements Named
type Person struct {
    FirstName, LastName string
}

func (p Person) Name() string {
    return p.FirstName + " " + p.LastName
}

// Employee embeds Person and also implements Named
type Employee struct {
    Person
    EmployeeID string
}

// Interface composition
type Object interface {
    Shape
    Named
}

// Ball implements both Shape and Named
type Ball struct {
    Radius float64
    Color  string
}

func (b Ball) Area() float64 {
    return 4 * math.Pi * b.Radius * b.Radius
}

func (b Ball) Perimeter() float64 {
    // For a sphere, perimeter isn't well-defined, but we implement it
    return 2 * math.Pi * b.Radius
}

func (b Ball) Name() string {
    return fmt.Sprintf("%s ball", b.Color)
}

// Interface with a single method
type Stringer interface {
    String() string
}

// Logger interface
type Logger interface {
    Log(message string)
}

// ConsoleLogger implements Logger
type ConsoleLogger struct {
    Prefix string
}

func (l ConsoleLogger) Log(message string) {
    fmt.Printf("[%s] %s\n", l.Prefix, message)
}

// FileLogger implements Logger
type FileLogger struct {
    FileName string
}

func (l FileLogger) Log(message string) {
    fmt.Printf("Writing '%s' to file: %s\n", message, l.FileName)
    // In a real implementation, we would write to the file
}

// Function that takes an interface
func PrintShapeInfo(s Shape) {
    fmt.Printf("Area: %.2f\n", s.Area())
    fmt.Printf("Perimeter: %.2f\n", s.Perimeter())
}

// Type assertions example function
func describe(i interface{}) {
    fmt.Printf("Type: %T, Value: %v\n", i, i)
    
    // Type assertions
    if val, ok := i.(string); ok {
        fmt.Printf("It's a string of length %d\n", len(val))
    } else if val, ok := i.(int); ok {
        fmt.Printf("It's an integer with value %d\n", val)
    } else if val, ok := i.(Shape); ok {
        fmt.Printf("It's a shape with area %.2f\n", val.Area())
    } else {
        fmt.Println("Type not recognized")
    }
}

// Type switch example function
func describeWithSwitch(i interface{}) {
    switch v := i.(type) {
    case string:
        fmt.Printf("It's a string of length %d\n", len(v))
    case int, int32, int64:
        fmt.Printf("It's an integer: %d\n", v)
    case float64:
        fmt.Printf("It's a float: %.2f\n", v)
    case Shape:
        fmt.Printf("It's a shape with area %.2f\n", v.Area())
    case nil:
        fmt.Println("It's nil")
    default:
        fmt.Printf("Type not recognized: %T\n", v)
    }
}

// Generic function example (Go 1.18+)
func Min[T constraints.Ordered](a, b T) T {
    if a < b {
        return a
    }
    return b
}

// Generic type example (Go 1.18+)
type Stack[T any] struct {
    items []T
}

func (s *Stack[T]) Push(item T) {
    s.items = append(s.items, item)
}

func (s *Stack[T]) Pop() (T, bool) {
    var zero T
    if len(s.items) == 0 {
        return zero, false
    }
    
    lastIdx := len(s.items) - 1
    item := s.items[lastIdx]
    s.items = s.items[:lastIdx]
    return item, true
}

func (s *Stack[T]) IsEmpty() bool {
    return len(s.items) == 0
}

func main() {
    // Using interfaces
    var s Shape
    s = Circle{Radius: 5}
    fmt.Println("Circle area:", s.Area())
    
    s = Rectangle{Width: 4, Height: 3}
    fmt.Println("Rectangle area:", s.Area())
    
    // Passing interface to function
    PrintShapeInfo(Circle{Radius: 10})
    PrintShapeInfo(Rectangle{Width: 4, Height: 3})
    
    // Named interface
    var n Named
    n = Person{FirstName: "John", LastName: "Doe"}
    fmt.Println("Person name:", n.Name())
    
    n = Employee{Person: Person{FirstName: "Jane", LastName: "Smith"}, EmployeeID: "EMP123"}
    fmt.Println("Employee name:", n.Name())
    
    // Interface composition
    var o Object
    o = Ball{Radius: 15, Color: "Red"}
    fmt.Printf("%s has area %.2f\n", o.Name(), o.Area())
    
    // Empty interface (interface{})
    var anything interface{}
    anything = 42
    fmt.Println("anything:", anything)
    
    anything = "hello"
    fmt.Println("anything:", anything)
    
    anything = true
    fmt.Println("anything:", anything)
    
    // Type assertions
    value, ok := anything.(bool)
    if ok {
        fmt.Println("Value is a boolean:", value)
    } else {
        fmt.Println("Value is not a boolean")
    }
    
    // Type assertion without check (panic if wrong type)
    // boolValue := anything.(string) // This would panic
    
    // Using describe function
    describe("Hello")
    describe(42)
    describe(Circle{Radius: 3})
    describe(3.14)
    
    // Using type switch
    describeWithSwitch("World")
    describeWithSwitch(123)
    describeWithSwitch(Rectangle{Width: 10, Height: 20})
    describeWithSwitch(nil)
    
    // Standard library interface example: io.Reader
    reader := strings.NewReader("Hello, Go interfaces!")
    
    // Read into buffer
    buffer := make([]byte, 8)
    for {
        n, err := reader.Read(buffer)
        if err == io.EOF {
            break
        }
        if err != nil {
            fmt.Println("Error reading:", err)
            break
        }
        fmt.Printf("Read %d bytes: %s\n", n, buffer[:n])
    }
    
    // Stringer interface example
    fmt.Println(Person{FirstName: "John", LastName: "Doe"})
    
    // Sorting with interfaces
    names := []string{"David", "Alice", "Bob", "Charlie"}
    sort.Strings(names)
    fmt.Println("Sorted names:", names)
    
    // Custom sorter using sort.Interface
    people := []Person{
        {FirstName: "John", LastName: "Doe"},
        {FirstName: "Jane", LastName: "Smith"},
        {FirstName: "Bob", LastName: "Johnson"},
    }
    
    sort.Slice(people, func(i, j int) bool {
        return people[i].LastName < people[j].LastName
    })
    
    fmt.Println("Sorted by last name:")
    for _, person := range people {
        fmt.Println(person.Name())
    }
    
    // Interface for polymorphism
    var logger Logger = ConsoleLogger{Prefix: "INFO"}
    logger.Log("This is a log message")
    
    logger = FileLogger{FileName: "app.log"}
    logger.Log("This is another log message")
    
    // Generic examples (Go 1.18+)
    fmt.Println("Min(5, 10):", Min(5, 10))
    fmt.Println("Min(3.14, 2.71):", Min(3.14, 2.71))
    fmt.Println("Min(\"abc\", \"def\"):", Min("abc", "def"))
    
    // Generic Stack
    intStack := Stack[int]{}
    intStack.Push(1)
    intStack.Push(2)
    intStack.Push(3)
    
    for !intStack.IsEmpty() {
        value, _ := intStack.Pop()
        fmt.Printf("Popped: %d\n", value)
    }
    
    stringStack := Stack[string]{}
    stringStack.Push("Hello")
    stringStack.Push("World")
    
    for !stringStack.IsEmpty() {
        value, _ := stringStack.Pop()
        fmt.Printf("Popped: %s\n", value)
    }
}
```

### Modul 8: Konkurensi Dasar

```go
package main

import (
    "fmt"
    "runtime"
    "sync"
    "sync/atomic"
    "time"
)

// Basic goroutine example
func sayHello(name string) {
    fmt.Printf("Hello, %s!\n", name)
}

// Function that sleeps to demonstrate concurrent execution
func processItem(id int) {
    fmt.Printf("Started processing item %d\n", id)
    time.Sleep(time.Second) // Simulate work
    fmt.Printf("Finished processing item %d\n", id)
}

// Function for passing data between goroutines using channels
func generateNumbers(n int) <-chan int {
    out := make(chan int)
    
    go func() {
        for i := 0; i < n; i++ {
            out <- i
        }
        close(out)
    }()
    
    return out
}

// Function that squares numbers
func squareNumbers(in <-chan int) <-chan int {
    out := make(chan int)
    
    go func() {
        for n := range in {
            out <- n * n
        }
        close(out)
    }()
    
    return out
}

// Worker pool pattern
func worker(id int, jobs <-chan int, results chan<- int, wg *sync.WaitGroup) {
    defer wg.Done()
    
    for job := range jobs {
        fmt.Printf("Worker %d started job %d\n", id, job)
        time.Sleep(time.Millisecond * time.Duration(job))
        fmt.Printf("Worker %d finished job %d\n", id, job)
        results <- job * 2
    }
}

func main() {
    // Basic goroutine
    go sayHello("Gopher")
    
    // Need to wait a bit to see the output before main exits
    time.Sleep(100 * time.Millisecond)
    
    // Multiple goroutines
    for i := 1; i <= 3; i++ {
        go processItem(i)
    }
    
    // Wait for goroutines to finish
    time.Sleep(2 * time.Second)
    
    // Number of CPUs and goroutines
    fmt.Printf("NumCPU: %d\n", runtime.NumCPU())
    fmt.Printf("GOMAXPROCS: %d\n", runtime.GOMAXPROCS(0))
    fmt.Printf("NumGoroutine: %d\n", runtime.NumGoroutine())
    
    // Basic channel
    ch := make(chan string)
    
    go func() {
        ch <- "Hello from goroutine!"
    }()
    
    msg := <-ch
    fmt.Println(msg)
    
    // Buffered channel
    bufCh := make(chan int, 3)
    
    // We can send 3 values without blocking
    bufCh <- 1
    bufCh <- 2
    bufCh <- 3
    
    // Receive values
    fmt.Println(<-bufCh)
    fmt.Println(<-bufCh)
    fmt.Println(<-bufCh)
    
    // Channel as function parameter
    nums := generateNumbers(5)
    squares := squareNumbers(nums)
    
    for square := range squares {
        fmt.Printf("Square: %d\n", square)
    }
    
    // Channel directions
    sendOnlyCh := make(chan<- int)    // Send-only
    receiveOnlyCh := make(<-chan int) // Receive-only
    
    // These are mainly used in function signatures
    _ = sendOnlyCh
    _ = receiveOnlyCh
    
    // Select statement
    ch1 := make(chan string)
    ch2 := make(chan string)
    
    go func() {
        time.Sleep(100 * time.Millisecond)
        ch1 <- "Message from channel 1"
    }()
    
    go func() {
        time.Sleep(50 * time.Millisecond)
        ch2 <- "Message from channel 2"
    }()
    
    // Select wa
```
