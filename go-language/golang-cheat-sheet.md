---
icon: notebook
---

# Golang Cheat sheet

## Cheatsheet Golang (Go)

### Dasar Go

#### Instalasi & Setup

```bash
# Download dari https://golang.org/dl/
# Cek instalasi
go version

# Setup workspace (opsional)
mkdir -p $HOME/go/{bin,src,pkg}

# Tambahkan ke PATH (di .bashrc, .zshrc, dll)
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

#### Struktur Program Go

```go
// Deklarasi package
package main

// Import paket
import (
    "fmt"
    "math"
)

// Import dengan alias
import (
    f "fmt"
    m "math"
)

// Fungsi utama
func main() {
    fmt.Println("Hello, World!")
}
```

#### Kompilasi & Menjalankan

```bash
# Run tanpa kompilasi (untuk development)
go run main.go

# Kompilasi ke executable
go build main.go

# Kompilasi dan install di $GOPATH/bin
go install

# Format kode
go fmt main.go

# Tes kode
go test

# Menampilkan dokumentasi
go doc fmt.Println
```

#### Go Modules

```bash
# Inisialisasi modul baru
go mod init github.com/username/repo

# Download dependencies
go mod download

# Merapikan dependencies
go mod tidy

# Vendor dependencies lokal
go mod vendor
```

### Tipe Data & Variabel

#### Deklarasi Variabel

```go
// Deklarasi dengan tipe
var nama string = "John"
var umur int = 25
var tinggi float64 = 175.5
var aktif bool = true

// Deklarasi tanpa nilai awal
var nama string
var umur int

// Deklarasi singkat (inferensi tipe)
nama := "John"
umur := 25

// Deklarasi banyak variabel
var (
    nama   string = "John"
    umur   int    = 25
    tinggi float64 = 175.5
)

// Konstanta
const Pi = 3.14159
const (
    StatusOK = 200
    StatusNotFound = 404
)

// iota (enumerasi)
const (
    Senin = iota // 0
    Selasa       // 1
    Rabu         // 2
)
```

#### Tipe Data Primitif

```go
// Numerik
var i int       // int bergantung pada arsitektur (32/64 bit)
var i8 int8     // -128 sampai 127
var i16 int16   // -32768 sampai 32767
var i32 int32   // -2^31 sampai 2^31-1
var i64 int64   // -2^63 sampai 2^63-1

var ui uint      // uint bergantung pada arsitektur (32/64 bit)
var ui8 uint8    // 0 sampai 255
var ui16 uint16  // 0 sampai 65535
var ui32 uint32  // 0 sampai 2^32-1
var ui64 uint64  // 0 sampai 2^64-1

var f32 float32  // IEEE-754 32-bit
var f64 float64  // IEEE-754 64-bit

var c64 complex64    // bilangan kompleks dengan float32 real dan imajiner
var c128 complex128  // bilangan kompleks dengan float64 real dan imajiner

// Boolean
var benar bool = true
var salah bool = false

// String
var str string = "Hello, 世界"
var char byte = 'A'    // alias untuk uint8, ASCII
var rune rune = '世'    // alias untuk int32, Unicode

// Konversi tipe
i := 42
f := float64(i)
s := string(i)  // Konversi ke karakter Unicode
```

#### Array, Slice, dan Map

**Array**

```go
// Array dengan ukuran tetap
var arr [5]int = [5]int{1, 2, 3, 4, 5}
// atau
arr := [5]int{1, 2, 3, 4, 5}
// atau dengan inferensi ukuran
arr := [...]int{1, 2, 3, 4, 5}

// Akses elemen
fmt.Println(arr[0])  // 1

// Panjang array
fmt.Println(len(arr))  // 5

// Array multidimensi
matrix := [2][3]int{
    {1, 2, 3},
    {4, 5, 6},
}
```

**Slice**

```go
// Slice (array dinamis)
slice := []int{1, 2, 3, 4, 5}

// Membuat slice dari array
arr := [5]int{1, 2, 3, 4, 5}
slice := arr[1:4]  // [2, 3, 4]

// Slice kosong
var slice []int

// Membuat slice dengan make
slice := make([]int, 5)      // len=5, cap=5
slice := make([]int, 0, 5)   // len=0, cap=5

// Append elemen
slice = append(slice, 6)
slice = append(slice, 7, 8, 9)

// Menggabungkan slice
slice1 := []int{1, 2}
slice2 := []int{3, 4}
combined := append(slice1, slice2...)

// Panjang dan kapasitas
fmt.Println(len(slice))  // panjang
fmt.Println(cap(slice))  // kapasitas

// Copy slice
dest := make([]int, len(slice))
copy(dest, slice)
```

**Map**

```go
// Deklarasi map
var m map[string]int
m = make(map[string]int)

// Map literal
m := map[string]int{
    "satu": 1,
    "dua": 2,
}

// Akses nilai
v := m["satu"]

// Memeriksa keberadaan key
v, exists := m["satu"]  // v=1, exists=true
v, exists := m["tiga"]  // v=0, exists=false

// Menambah/mengubah elemen
m["tiga"] = 3

// Hapus elemen
delete(m, "dua")

// Panjang map
fmt.Println(len(m))
```

### Kontrol Alur

#### Kondisional

```go
// If-else
if x > 0 {
    fmt.Println("Positif")
} else if x < 0 {
    fmt.Println("Negatif")
} else {
    fmt.Println("Nol")
}

// If dengan statement awal
if v, exists := m["key"]; exists {
    fmt.Println("Ditemukan:", v)
}

// Switch
switch day {
case "Senin":
    fmt.Println("Awal minggu")
case "Jumat":
    fmt.Println("Akhir minggu kerja")
case "Sabtu", "Minggu":
    fmt.Println("Akhir pekan")
default:
    fmt.Println("Hari biasa")
}

// Switch tanpa ekspresi (seperti if-else)
switch {
case n < 0:
    fmt.Println("Negatif")
case n > 0:
    fmt.Println("Positif")
default:
    fmt.Println("Nol")
}

// Type switch
switch v := interfaceValue.(type) {
case string:
    fmt.Println("String:", v)
case int:
    fmt.Println("Integer:", v)
default:
    fmt.Println("Tipe lain")
}
```

#### Loop

```go
// Loop for standar
for i := 0; i < 5; i++ {
    fmt.Println(i)
}

// For sebagai while
i := 0
for i < 5 {
    fmt.Println(i)
    i++
}

// For tanpa kondisi (infinite loop)
for {
    // lakukan sesuatu
    break  // keluar dari loop
}

// Continue
for i := 0; i < 10; i++ {
    if i%2 == 0 {
        continue  // skip ke iterasi berikutnya
    }
    fmt.Println(i)  // mencetak bilangan ganjil
}

// Range pada slice
slice := []string{"a", "b", "c"}
for i, v := range slice {
    fmt.Println(i, v)  // index, nilai
}

// Range pada map
m := map[string]int{"a": 1, "b": 2}
for k, v := range m {
    fmt.Println(k, v)  // key, value
}

// Range pada string (iterasi per rune/karakter)
for i, ch := range "Hello" {
    fmt.Printf("%d: %c\n", i, ch)
}
```

#### Fungsi

```go
// Deklarasi fungsi dasar
func tambah(a, b int) int {
    return a + b
}

// Multiple return values
func swap(a, b string) (string, string) {
    return b, a
}

// Named return values
func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return  // naked return
}

// Variadic function
func sum(nums ...int) int {
    total := 0
    for _, num := range nums {
        total += num
    }
    return total
}
// Penggunaan: sum(1, 2, 3) atau sum(nums...)

// Function adalah first-class citizen
func compute(fn func(int, int) int) int {
    return fn(3, 4)
}

// Anonymous function / closure
func() {
    fmt.Println("Anonymous function")
}()

add := func(a, b int) int {
    return a + b
}
```

### Struktur Data & OOP

#### Struct

```go
// Definisi struct
type Person struct {
    Name string
    Age  int
}

// Instansiasi struct
var p Person
p = Person{Name: "John", Age: 25}
// atau
p := Person{Name: "John", Age: 25}
// atau tanpa nama field (urutan harus sesuai)
p := Person{"John", 25}

// Akses field
fmt.Println(p.Name)
p.Age = 26

// Struct pointer
pp := &Person{Name: "John", Age: 25}
fmt.Println(pp.Name)  // shorthand untuk (*pp).Name

// Struct embedding (komposisi)
type Employee struct {
    Person
    Title string
}

e := Employee{
    Person: Person{Name: "John", Age: 25},
    Title:  "Developer",
}
fmt.Println(e.Name)  // Akses langsung field dari struct yang di-embed
```

#### Method

```go
// Method pada struct
type Rectangle struct {
    Width, Height float64
}

// Method dengan receiver
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Method dengan pointer receiver (dapat memodifikasi struct)
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

// Penggunaan
r := Rectangle{Width: 10, Height: 5}
fmt.Println(r.Area())  // 50
r.Scale(2)
fmt.Println(r.Area())  // 200
```

#### Interface

```go
// Definisi interface
type Geometry interface {
    Area() float64
    Perimeter() float64
}

// Implementasi interface (implisit)
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2*r.Width + 2*r.Height
}

// Interface kosong (any value)
var i interface{}
i = 42
i = "hello"

// Type assertion
str, ok := i.(string)  // cek apakah i berisi string
```

### Concurrency

#### Goroutines

```go
// Menjalankan fungsi sebagai goroutine
go func() {
    fmt.Println("Hello from goroutine")
}()

// Menjalankan fungsi yang sudah ada
func say(s string) {
    fmt.Println(s)
}
go say("Hello")
```

#### Channels

```go
// Membuat channel
ch := make(chan int)      // Unbuffered channel
ch := make(chan int, 100) // Buffered channel dengan kapasitas 100

// Mengirim data ke channel
ch <- 42

// Menerima data dari channel
v := <-ch

// Menutup channel
close(ch)

// Range pada channel
for v := range ch {
    fmt.Println(v)  // Menerima hingga channel ditutup
}

// Select
select {
case v := <-ch1:
    fmt.Println("Received from ch1:", v)
case v := <-ch2:
    fmt.Println("Received from ch2:", v)
case ch3 <- 42:
    fmt.Println("Sent to ch3")
default:
    fmt.Println("No communication")
}

// Timeout dengan select
select {
case v := <-ch:
    fmt.Println("Received:", v)
case <-time.After(1 * time.Second):
    fmt.Println("Timeout")
}
```

#### Sinkronisasi

```go
// WaitGroup untuk menunggu goroutine selesai
var wg sync.WaitGroup
wg.Add(1)  // Tambah counter
go func() {
    defer wg.Done()  // Kurangi counter saat selesai
    // Kode
}()
wg.Wait()  // Tunggu semua goroutine selesai

// Mutex untuk mutual exclusion
var mu sync.Mutex
mu.Lock()
// Critical section
mu.Unlock()

// RWMutex (Read-Write Mutex)
var rwmu sync.RWMutex
rwmu.RLock()  // Lock untuk baca
// Membaca
rwmu.RUnlock()

rwmu.Lock()   // Lock untuk tulis
// Menulis
rwmu.Unlock()

// Once untuk menjalankan kode sekali saja
var once sync.Once
once.Do(func() {
    // Kode ini dijamin hanya dijalankan sekali
})
```

### Error Handling

#### Error

```go
// Mengembalikan error
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("pembagian dengan nol")
    }
    return a / b, nil
}

// Menggunakan error
result, err := divide(10, 0)
if err != nil {
    fmt.Println("Error:", err)
    return
}
fmt.Println("Result:", result)

// Custom error
type MyError struct {
    When time.Time
    What string
}

func (e *MyError) Error() string {
    return fmt.Sprintf("%v: %v", e.When, e.What)
}

func run() error {
    return &MyError{
        time.Now(),
        "it didn't work",
    }
}
```

#### Defer, Panic, dan Recover

```go
// Defer - dijalankan saat fungsi selesai
func CopyFile(src, dst string) error {
    srcFile, err := os.Open(src)
    if err != nil {
        return err
    }
    defer srcFile.Close()  // Dipanggil saat fungsi selesai
    
    // Kode lainnya
    return nil
}

// Beberapa defer dieksekusi dengan urutan LIFO (Last In First Out)
func countdown() {
    for i := 0; i < 5; i++ {
        defer fmt.Println(i)  // Akan mencetak: 4 3 2 1 0
    }
}

// Panic - menghentikan eksekusi normal
func divide(a, b int) int {
    if b == 0 {
        panic("pembagian dengan nol")
    }
    return a / b
}

// Recover - menangkap panic
func safeExecution() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered:", r)
        }
    }()
    
    // Kode yang mungkin panic
    panic("something bad happened")
}
```

### Paket Standar

#### fmt - Formatting & I/O

```go
// Print ke stdout
fmt.Print("Hello")         // tanpa newline
fmt.Println("Hello")       // dengan newline
fmt.Printf("Hi %s\n", name) // dengan format

// Scanning input
var nama string
fmt.Scanln(&nama)          // input satu baris
fmt.Scanf("%s %d", &nama, &umur) // dengan format

// String formatting
s := fmt.Sprintf("Hi %s, umur %d", nama, umur)

// Format specifiers
fmt.Printf("%v", val)      // default format
fmt.Printf("%+v", val)     // struct dengan nama field
fmt.Printf("%#v", val)     // Go syntax representation
fmt.Printf("%T", val)      // tipe
fmt.Printf("%d", 123)      // desimal
fmt.Printf("%x", 123)      // heksadesimal
fmt.Printf("%f", 123.45)   // float
fmt.Printf("%.2f", 123.45) // float dengan presisi
fmt.Printf("%s", "string") // string
fmt.Printf("%q", "string") // string dengan quotes
fmt.Printf("%p", &val)     // pointer
```

#### strings - Manipulasi String

```go
// Split string
parts := strings.Split("a,b,c", ",")  // ["a", "b", "c"]

// Join string
s := strings.Join([]string{"a", "b", "c"}, "-")  // "a-b-c"

// Contains, HasPrefix, HasSuffix
ok := strings.Contains("seafood", "foo")  // true
ok := strings.HasPrefix("seafood", "sea") // true
ok := strings.HasSuffix("seafood", "food") // true

// Replace
s := strings.Replace("oink oink oink", "k", "ky", 2)  // "oinky oinky oink"
s := strings.ReplaceAll("oink oink oink", "oink", "moo")  // "moo moo moo"

// Uppercase & Lowercase
s := strings.ToUpper("Hello")  // "HELLO"
s := strings.ToLower("Hello")  // "hello"

// Trim
s := strings.TrimSpace(" Hello ")  // "Hello"
s := strings.Trim("!!!Hello!!!", "!")  // "Hello"

// Index
idx := strings.Index("chicken", "ken")  // 4
idx := strings.LastIndex("chicken", "c")  // 0

// Count
count := strings.Count("cheese", "e")  // 3
```

#### strconv - Konversi String

```go
// String ke int
i, err := strconv.Atoi("42")  // i=42, err=nil
i, err := strconv.ParseInt("42", 10, 64)  // base 10, 64 bit

// Int ke string
s := strconv.Itoa(42)  // "42"
s := strconv.FormatInt(42, 16)  // "2a" (hex)

// String ke float
f, err := strconv.ParseFloat("3.14", 64)  // f=3.14, err=nil

// Float ke string
s := strconv.FormatFloat(3.14, 'f', 2, 64)  // "3.14"

// String ke bool
b, err := strconv.ParseBool("true")  // b=true, err=nil

// Bool ke string
s := strconv.FormatBool(true)  // "true"
```

#### time - Waktu & Tanggal

```go
// Waktu sekarang
now := time.Now()

// Membuat waktu spesifik
t := time.Date(2023, time.January, 10, 15, 30, 0, 0, time.UTC)

// Format waktu
s := t.Format("2006-01-02 15:04:05")  // YYYY-MM-DD HH:MM:SS
s := t.Format(time.RFC3339)  // Format standar

// Parse string ke waktu
t, err := time.Parse("2006-01-02", "2023-01-10")
t, err := time.Parse(time.RFC3339, "2023-01-10T15:30:00Z")

// Mengakses komponen waktu
year, month, day := t.Date()
hour, min, sec := t.Clock()

// Operasi waktu
tomorrow := now.AddDate(0, 0, 1)  // tambah 1 hari
later := now.Add(2 * time.Hour)   // tambah 2 jam
diff := later.Sub(now)            // durasi

// Timer & Ticker
timer := time.NewTimer(2 * time.Second)
<-timer.C  // tunggu timer selesai

ticker := time.NewTicker(1 * time.Second)
for t := range ticker.C {
    fmt.Println("Tick at", t)
}
ticker.Stop()  // hentikan ticker

// Sleep
time.Sleep(2 * time.Second)
```

#### io - Input/Output

```go
// Reader & Writer interface
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

// Copy dari reader ke writer
n, err := io.Copy(dst, src)

// Read semua bytes
data, err := io.ReadAll(reader)

// Multi reader/writer
mr := io.MultiReader(r1, r2)
mw := io.MultiWriter(w1, w2)

// Pipe
r, w := io.Pipe()
```

#### os - Operasi Sistem

```go
// Argumen baris perintah
args := os.Args  // [0] adalah nama program

// Environment variables
value := os.Getenv("PATH")
os.Setenv("API_KEY", "secret")

// File I/O
file, err := os.Open("file.txt")
file, err := os.Create("file.txt")
defer file.Close()

data, err := os.ReadFile("file.txt")
err := os.WriteFile("file.txt", data, 0644)

// Direktori
err := os.Mkdir("dir", 0755)
err := os.MkdirAll("path/to/dir", 0755)
dirs, err := os.ReadDir(".")

// Exit
os.Exit(1)  // keluar dengan kode error
```

#### net/http - HTTP Client & Server

```go
// HTTP Client
resp, err := http.Get("https://example.com")
resp, err := http.Post("https://example.com", "application/json", requestBody)
req, err := http.NewRequest("GET", "https://example.com", nil)
client := &http.Client{}
resp, err := client.Do(req)

// Membaca response
defer resp.Body.Close()
body, err := io.ReadAll(resp.Body)

// HTTP Server
http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, %s!", r.URL.Path[1:])
})
http.ListenAndServe(":8080", nil)

// HTTP Handler
type Handler interface {
    ServeHTTP(ResponseWriter, *Request)
}

// Server dengan custom handler
type MyHandler struct{}
func (h *MyHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello World!")
}
http.ListenAndServe(":8080", &MyHandler{})
```

#### encoding/json - JSON

```go
// Struct tag untuk JSON
type Person struct {
    Name  string `json:"name"`
    Age   int    `json:"age,omitempty"`
    Email string `json:"email,omitempty"`
}

// Marshal (struct ke JSON)
p := Person{Name: "John", Age: 25}
data, err := json.Marshal(p)

// MarshalIndent (JSON terformat)
data, err := json.MarshalIndent(p, "", "  ")

// Unmarshal (JSON ke struct)
var p Person
err := json.Unmarshal(data, &p)

// Decoder (untuk stream JSON)
dec := json.NewDecoder(r)
err := dec.Decode(&p)

// Encoder (untuk stream JSON)
enc := json.NewEncoder(w)
err := enc.Encode(p)
```

#### database/sql - Database

```go
// Koneksi database
db, err := sql.Open("postgres", "postgres://user:pass@localhost/dbname")
defer db.Close()

// Ping database
err = db.Ping()

// Query
rows, err := db.Query("SELECT id, name FROM users WHERE age > $1", 18)
defer rows.Close()

for rows.Next() {
    var id int
    var name string
    err = rows.Scan(&id, &name)
    // Proses data
}
err = rows.Err()

// QueryRow (satu baris)
var name string
err = db.QueryRow("SELECT name FROM users WHERE id = $1", 1).Scan(&name)

// Exec (tanpa hasil)
result, err := db.Exec("UPDATE users SET name = $1 WHERE id = $2", "John", 1)
rowsAffected, err := result.RowsAffected()

// Prepared statement
stmt, err := db.Prepare("INSERT INTO users(name, age) VALUES($1, $2)")
defer stmt.Close()
_, err = stmt.Exec("John", 25)

// Transactions
tx, err := db.Begin()
_, err = tx.Exec("INSERT INTO users(name) VALUES($1)", "John")
if err != nil {
    tx.Rollback()
    return err
}
err = tx.Commit()
```

#### testing - Unit Test

```go
// File: add_test.go
package main

import "testing"

func TestAdd(t *testing.T) {
    sum := Add(2, 3)
    if sum != 5 {
        t.Errorf("Add(2, 3) = %d; want 5", sum)
    }
}

// Table-driven test
func TestAdd(t *testing.T) {
    tests := []struct {
        a, b, want int
    }{
        {2, 3, 5},
        {-1, 1, 0},
        {0, 0, 0},
    }
    
    for _, test := range tests {
        if got := Add(test.a, test.b); got != test.want {
            t.Errorf("Add(%d, %d) = %d; want %d", test.a, test.b, got, test.want)
        }
    }
}

// Benchmark
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}

// Menjalankan test
// go test
// go test -v
// go test -cover
// go test -bench=.
```

### Tips & Best Practices

#### Penamaan

* Gunakan `camelCase` untuk variabel dan fungsi lokal
* Gunakan `PascalCase` untuk ekspor (publik)
* Nama paket harus singkat, lowercase
* Hindari redundansi: `user.UserID` → `user.ID`

#### Format Kode

* Gunakan `go fmt` sebelum commit
* Gunakan `goimports` untuk mengelola impor
* Gunakan `golint` untuk memeriksa style
* Gunakan `go vet` untuk mendeteksi kesalahan umum

#### Pengelolaan Error

* Periksa error setiap kali dikembalikan
* Jangan gunakan `_` untuk mengabaikan error kecuali benar-benar disengaja
* Kombinasikan `defer` dan `recover` untuk penanganan panic yang elegan

#### Concurrency

* Gunakan channels untuk komunikasi, bukan untuk sinkronisasi
* Jangan lupa menutup channels saat tidak diperlukan
* Hindari goroutines leak dengan `context` atau `select` + `timeout`
* Gunakan `sync.WaitGroup` untuk menunggu goroutines selesai

#### Performance

* Gunakan profiler untuk mengidentifikasi bottleneck
* Gunakan `strings.Builder` untuk concatenation string
* Alokasikan slice dengan kapasitas awal yang cukup jika ukuran diketahui
* Gunakan pointer receiver untuk struct besar atau yang butuh mutasi

#### Debugging

* `fmt.Printf("%+v\n", v)` untuk mencetak struktur secara detail
* `log.Printf("%#v\n", v)` untuk debug dengan timestamp
* `runtime.Stack()` untuk mendapatkan stack trace
* `go run -race` untuk mendeteksi race condition
