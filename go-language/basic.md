---
icon: pepper-hot
---

# Basic

## Percabangan

Dalam bahasa pemrograman Go (Golang), pernyataan **`if`** dan **`else`** digunakan untuk pengambilan keputusan, yaitu mengeksekusi blok kode berdasarkan kondisi tertentu.

### **Struktur Dasar `if`**

```go
if kondisi {
    // blok kode yang akan dijalankan jika kondisi bernilai true
}
```

### **Struktur Dasar `if-else`**

```go
if kondisi {
    // blok kode jika kondisi bernilai true
} else {
    // blok kode jika kondisi bernilai false
}
```

### **Struktur Dasar `if-else if-else`**

```go
if kondisi1 {
    // blok kode jika kondisi1 bernilai true
} else if kondisi2 {
    // blok kode jika kondisi2 bernilai true
} else {
    // blok kode jika semua kondisi sebelumnya bernilai false
}
```

***

#### **Contoh 1: Penggunaan `if` sederhana**

```go
package main

import "fmt"

func main() {
    angka := 10

    if angka > 5 {
        fmt.Println("Angka lebih besar dari 5")
    }
}
```

**Penjelasan:**

* Variabel `angka` memiliki nilai `10`.
* Kondisi `angka > 5` bernilai **true**, sehingga pernyataan `fmt.Println("Angka lebih besar dari 5")` dijalankan.

***

#### **Contoh 2: Penggunaan `if-else`**

```go
package main

import "fmt"

func main() {
    angka := 3

    if angka%2 == 0 {
        fmt.Println("Angka adalah genap")
    } else {
        fmt.Println("Angka adalah ganjil")
    }
}
```

**Penjelasan:**

* Kondisi `angka%2 == 0` mengecek apakah angka genap.
* Jika kondisi bernilai **true**, maka akan mencetak "Angka adalah genap".
* Jika kondisi bernilai **false**, maka akan mencetak "Angka adalah ganjil".

***

#### **Contoh 3: Penggunaan `if-else if-else`**

```go
package main

import "fmt"

func main() {
    nilai := 75

    if nilai >= 85 {
        fmt.Println("Grade A")
    } else if nilai >= 70 {
        fmt.Println("Grade B")
    } else if nilai >= 50 {
        fmt.Println("Grade C")
    } else {
        fmt.Println("Grade D")
    }
}
```

**Penjelasan:**

* Jika `nilai` lebih besar atau sama dengan 85, mencetak **Grade A**.
* Jika tidak, program mengecek apakah `nilai` lebih besar atau sama dengan 70, mencetak **Grade B**.
* Dan seterusnya.

***

#### **Contoh 4: `if` dengan deklarasi variabel langsung**

Go mendukung deklarasi variabel di dalam pernyataan `if`:

```go
goCopy codepackage main

import "fmt"

func main() {
    if angka := 15; angka > 10 {
        fmt.Println("Angka lebih besar dari 10")
    } else {
        fmt.Println("Angka kurang dari atau sama dengan 10")
    }
}
```

**Penjelasan:**

* Variabel `angka` dideklarasikan di dalam pernyataan `if`.
* Variabel ini hanya berlaku di dalam blok `if` dan `else`.

***

#### **Contoh 5: Kombinasi logika di dalam `if`**

Anda dapat menggabungkan beberapa kondisi dengan operator logika seperti **`&&`** (dan), **`||`** (atau):

```go
package main

import "fmt"

func main() {
    angka := 8

    if angka > 5 && angka < 10 {
        fmt.Println("Angka berada di antara 5 dan 10")
    } else {
        fmt.Println("Angka berada di luar rentang 5-10")
    }
}
```

**Penjelasan:**

* Kondisi `angka > 5 && angka < 10` mengecek apakah `angka` lebih besar dari 5 **dan** lebih kecil dari 10.

***

#### **Kesimpulan**

* Gunakan **`if`** untuk pengambilan keputusan sederhana.
* Gunakan **`if-else`** jika ada dua kemungkinan hasil.
* Gunakan **`if-else if-else`** untuk lebih dari dua kemungkinan hasil.
* Anda dapat mendeklarasikan variabel langsung di dalam `if`.
* Gunakan operator logika seperti **`&&`** dan **`||`** untuk kondisi kompleks.

Jika Anda memiliki pertanyaan lebih lanjut atau ingin penjelasan lebih spesifik, silakan tanyakan! 😊

4o

**You said:**

jelaskan secara detail for loop pada go beserta contohnya

**ChatGPT said:**

ChatGPT

Dalam bahasa pemrograman Go, **`for`** adalah satu-satunya jenis loop yang tersedia, namun sangat fleksibel sehingga dapat digunakan untuk berbagai kebutuhan seperti perulangan sederhana, perulangan dengan kondisi, atau perulangan tanpa batas.

***

#### **Struktur Dasar `for`**

```go
for init; condition; post {
    // blok kode yang akan diulang
}
```

* **`init`**: Ekspresi inisialisasi yang dijalankan sekali di awal loop.
* **`condition`**: Kondisi yang diperiksa sebelum setiap iterasi. Jika **`false`**, loop berhenti.
* **`post`**: Ekspresi yang dijalankan setelah setiap iterasi.

***

#### **Contoh 1: Perulangan Sederhana**

```go
package main

import "fmt"

func main() {
    for i := 0; i < 5; i++ {
        fmt.Println("Nilai i:", i)
    }
}
```

**Penjelasan:**

* **`i := 0`**: Inisialisasi variabel `i` dengan nilai 0.
* **`i < 5`**: Loop berjalan selama nilai `i` kurang dari 5.
* **`i++`**: Setelah setiap iterasi, nilai `i` bertambah 1.

Output:

```less
lessCopy codeNilai i: 0
Nilai i: 1
Nilai i: 2
Nilai i: 3
Nilai i: 4
```

***

#### **Contoh 2: Perulangan Tanpa Inisialisasi dan Post**

```go
goCopy codepackage main

import "fmt"

func main() {
    i := 0
    for i < 5 {
        fmt.Println("Nilai i:", i)
        i++
    }
}
```

**Penjelasan:**

* Hanya **`condition`** yang digunakan (`i < 5`).
* Variabel `i` dideklarasikan di luar loop, dan increment dilakukan di dalam blok.

Output sama seperti contoh 1.

***

#### **Contoh 3: Perulangan Tak Terbatas**

```go
goCopy codepackage main

import "fmt"

func main() {
    i := 0
    for {
        if i >= 5 {
            break // Menghentikan loop jika kondisi terpenuhi
        }
        fmt.Println("Nilai i:", i)
        i++
    }
}
```

**Penjelasan:**

* Perulangan ini berjalan tanpa batas karena tidak ada **`condition`**.
* Gunakan **`break`** untuk menghentikan loop secara manual ketika syarat tertentu terpenuhi.

Output:

```less
lessCopy codeNilai i: 0
Nilai i: 1
Nilai i: 2
Nilai i: 3
Nilai i: 4
```

***

#### **Contoh 4: Perulangan dengan Slice atau Array**

Go menyediakan cara sederhana untuk mengiterasi elemen dalam slice, array, atau map menggunakan **`for range`**.

**Iterasi Slice**

```go
goCopy codepackage main

import "fmt"

func main() {
    angka := []int{10, 20, 30, 40, 50}

    for i, v := range angka {
        fmt.Printf("Index: %d, Value: %d\n", i, v)
    }
}
```

**Penjelasan:**

* **`i`**: Indeks elemen.
* **`v`**: Nilai elemen pada indeks tersebut.
* Kata kunci **`range`** digunakan untuk iterasi.

Output:

```yaml
yamlCopy codeIndex: 0, Value: 10
Index: 1, Value: 20
Index: 2, Value: 30
Index: 3, Value: 40
Index: 4, Value: 50
```

**Mengabaikan Indeks atau Nilai**

* Gunakan **`_`** jika salah satu tidak diperlukan.

```go
goCopy codefor _, v := range angka { // Mengabaikan indeks
    fmt.Println("Value:", v)
}
```

***

#### **Contoh 5: Iterasi Map**

```go
goCopy codepackage main

import "fmt"

func main() {
    mahasiswa := map[string]int{
        "Alice": 85,
        "Bob":   90,
        "Cindy": 88,
    }

    for nama, nilai := range mahasiswa {
        fmt.Printf("Nama: %s, Nilai: %d\n", nama, nilai)
    }
}
```

**Penjelasan:**

* **`nama`**: Kunci dari map.
* **`nilai`**: Nilai yang terkait dengan kunci tersebut.

Output:

```yaml
yamlCopy codeNama: Alice, Nilai: 85
Nama: Bob, Nilai: 90
Nama: Cindy, Nilai: 88
```

***

#### **Contoh 6: Nested Loop**

Perulangan di dalam perulangan (nested loop) digunakan untuk kasus seperti matriks.

```go
goCopy codepackage main

import "fmt"

func main() {
    for i := 1; i <= 3; i++ {
        for j := 1; j <= 3; j++ {
            fmt.Printf("i: %d, j: %d\n", i, j)
        }
    }
}
```

**Penjelasan:**

* Loop pertama mengontrol baris (**`i`**).
* Loop kedua mengontrol kolom (**`j`**).

Output:

```yaml
yamlCopy codei: 1, j: 1
i: 1, j: 2
i: 1, j: 3
i: 2, j: 1
...
```

***

#### **Contoh 7: Melanjutkan Iterasi dengan `continue`**

Gunakan **`continue`** untuk melewati iterasi tertentu tanpa keluar dari loop.

```go
goCopy codepackage main

import "fmt"

func main() {
    for i := 1; i <= 5; i++ {
        if i == 3 {
            continue // Melewati iterasi ketika i == 3
        }
        fmt.Println("Nilai i:", i)
    }
}
```

Output:

```less
lessCopy codeNilai i: 1
Nilai i: 2
Nilai i: 4
Nilai i: 5
```

***

#### **Kesimpulan**

* **`for`** di Go dapat digunakan dalam berbagai bentuk:
  * Perulangan dengan inisialisasi, kondisi, dan post.
  * Perulangan dengan kondisi saja.
  * Perulangan tanpa batas.
  * Iterasi melalui koleksi data seperti slice atau map menggunakan **`range`**.
* Kata kunci tambahan seperti **`break`** dan **`continue`** membantu mengontrol alur loop.

Jika Anda memiliki kasus spesifik yang ingin dibahas, jangan ragu untuk bertanya! 😊

4o

**You said:**

jelaskan switch secara detail dalam bahasa go beserta contohnya

**ChatGPT said:**

ChatGPT

Dalam bahasa pemrograman Go, **`switch`** digunakan untuk membuat pengambilan keputusan berdasarkan ekspresi atau nilai. Go memiliki beberapa fitur unik pada **`switch`** yang membuatnya lebih fleksibel dibandingkan dengan bahasa lain, seperti tidak membutuhkan kata kunci **`break`** secara eksplisit dan mendukung evaluasi ekspresi yang kompleks.

***

#### **Struktur Dasar `switch`**

```go
goCopy codeswitch ekspresi {
case nilai1:
    // blok kode untuk nilai1
case nilai2:
    // blok kode untuk nilai2
default:
    // blok kode jika tidak ada case yang cocok
}
```

**Catatan:**

1. **`default`** bersifat opsional.
2. Evaluasi berhenti setelah case yang cocok ditemukan (tidak memerlukan **`break`**).

***

#### **Contoh 1: `switch` Sederhana**

```go
goCopy codepackage main

import "fmt"

func main() {
    angka := 3

    switch angka {
    case 1:
        fmt.Println("Angka adalah 1")
    case 2:
        fmt.Println("Angka adalah 2")
    case 3:
        fmt.Println("Angka adalah 3")
    default:
        fmt.Println("Angka tidak diketahui")
    }
}
```

**Penjelasan:**

* Variabel `angka` memiliki nilai `3`.
* Case yang cocok adalah **`case 3`**, sehingga mencetak "Angka adalah 3".

Output:

```
Copy codeAngka adalah 3
```

***

#### **Contoh 2: Menggunakan `default`**

```go
goCopy codepackage main

import "fmt"

func main() {
    angka := 5

    switch angka {
    case 1:
        fmt.Println("Angka adalah 1")
    case 2:
        fmt.Println("Angka adalah 2")
    default:
        fmt.Println("Angka tidak cocok dengan case yang tersedia")
    }
}
```

**Penjelasan:**

* Karena `angka` tidak cocok dengan **`case 1`** atau **`case 2`**, blok **`default`** dijalankan.

Output:

```arduino
arduinoCopy codeAngka tidak cocok dengan case yang tersedia
```

***

#### **Contoh 3: Banyak Nilai pada Satu `case`**

```go
goCopy codepackage main

import "fmt"

func main() {
    hari := "Minggu"

    switch hari {
    case "Senin", "Selasa", "Rabu", "Kamis", "Jumat":
        fmt.Println("Hari kerja")
    case "Sabtu", "Minggu":
        fmt.Println("Akhir pekan")
    default:
        fmt.Println("Hari tidak valid")
    }
}
```

**Penjelasan:**

* Anda dapat menggabungkan beberapa nilai pada satu case menggunakan koma (,).
* Jika `hari` adalah **"Minggu"**, maka mencetak "Akhir pekan".

Output:

```
Copy codeAkhir pekan
```

***

#### **Contoh 4: Evaluasi Ekspresi pada `case`**

```go
goCopy codepackage main

import "fmt"

func main() {
    angka := 10

    switch {
    case angka%2 == 0:
        fmt.Println("Angka genap")
    case angka%2 != 0:
        fmt.Println("Angka ganjil")
    }
}
```

**Penjelasan:**

* Anda dapat menggunakan **`switch`** tanpa ekspresi, dan setiap **`case`** memiliki ekspresi logis.
* **`angka%2 == 0`** bernilai **true**, sehingga mencetak "Angka genap".

Output:

```
Copy codeAngka genap
```

***

#### **Contoh 5: Fallthrough**

Dalam Go, perintah **`fallthrough`** digunakan untuk meneruskan eksekusi ke case berikutnya tanpa memeriksa kondisinya.

```go
goCopy codepackage main

import "fmt"

func main() {
    angka := 2

    switch angka {
    case 1:
        fmt.Println("Case 1")
    case 2:
        fmt.Println("Case 2")
        fallthrough
    case 3:
        fmt.Println("Case 3")
    default:
        fmt.Println("Default case")
    }
}
```

**Penjelasan:**

* Setelah mencetak "Case 2", **`fallthrough`** menyebabkan eksekusi dilanjutkan ke **`case 3`** tanpa mengevaluasi kondisinya.

Output:

```sql
sqlCopy codeCase 2
Case 3
```

***

#### **Contoh 6: Tipe Data pada `switch`**

Anda juga bisa menggunakan **`switch`** untuk mengevaluasi tipe data dengan kata kunci **`type`** (Type Switch).

```go
goCopy codepackage main

import "fmt"

func main() {
    var data interface{} = "Halo"

    switch v := data.(type) {
    case int:
        fmt.Println("Tipe data adalah int, nilai:", v)
    case string:
        fmt.Println("Tipe data adalah string, nilai:", v)
    case bool:
        fmt.Println("Tipe data adalah bool, nilai:", v)
    default:
        fmt.Println("Tipe data tidak diketahui")
    }
}
```

**Penjelasan:**

* **`data.(type)`** digunakan untuk mengecek tipe data dari variabel.
* Jika **`data`** bertipe **`string`**, maka mencetak "Tipe data adalah string".

Output:

```c
cCopy codeTipe data adalah string, nilai: Halo
```

***

#### **Kesimpulan**

* **`switch`** di Go fleksibel dan mendukung:
  * Evaluasi nilai atau ekspresi.
  * Beberapa nilai dalam satu case.
  * Fallthrough untuk melanjutkan eksekusi ke case berikutnya.
  * Evaluasi tipe data dengan type switch.
* **`default`** digunakan untuk menangani nilai yang tidak cocok dengan case lain.

Jika Anda ingin penjelasan lebih lanjut atau contoh tambahan, silakan tanyakan! 😊

4o

**You said:**

jelaskan secara detail Break and Continue beserta contohnya dalam bahasa go

**ChatGPT said:**

ChatGPT

Dalam bahasa pemrograman Go, **`break`** dan **`continue`** adalah dua perintah kontrol yang digunakan untuk mengatur alur eksekusi dalam loop atau pernyataan kontrol lainnya.

***

#### **1. `break`**

**Fungsi:**

* **`break`** digunakan untuk menghentikan perulangan atau keluar dari blok pernyataan seperti `switch` sebelum loop atau blok selesai berjalan.

***

**Contoh 1: `break` pada Loop**

```go
goCopy codepackage main

import "fmt"

func main() {
    for i := 1; i <= 10; i++ {
        if i == 5 {
            fmt.Println("Loop dihentikan pada i =", i)
            break // Keluar dari loop saat i == 5
        }
        fmt.Println("i =", i)
    }
}
```

**Penjelasan:**

* Ketika **`i == 5`**, perintah **`break`** dijalankan, dan loop langsung dihentikan.

**Output:**

```css
cssCopy codei = 1
i = 2
i = 3
i = 4
Loop dihentikan pada i = 5
```

***

**Contoh 2: `break` pada `switch`**

```go
goCopy codepackage main

import "fmt"

func main() {
    angka := 3

    switch angka {
    case 1:
        fmt.Println("Angka adalah 1")
    case 2:
        fmt.Println("Angka adalah 2")
    case 3:
        fmt.Println("Angka adalah 3")
        break // Keluar dari blok switch (opsional di Go)
    default:
        fmt.Println("Angka tidak diketahui")
    }
}
```

**Penjelasan:**

* Dalam **`switch`**, Go secara otomatis keluar dari blok setelah menemukan case yang cocok. Namun, Anda tetap dapat menggunakan **`break`** untuk kejelasan.

**Output:**

```
Copy codeAngka adalah 3
```

***

#### **2. `continue`**

**Fungsi:**

* **`continue`** digunakan untuk melewati iterasi saat ini dalam loop dan langsung melanjutkan ke iterasi berikutnya.

***

**Contoh 1: Melewati Iterasi dengan `continue`**

```go
goCopy codepackage main

import "fmt"

func main() {
    for i := 1; i <= 5; i++ {
        if i == 3 {
            fmt.Println("Melewati iterasi pada i =", i)
            continue // Melewati iterasi ketika i == 3
        }
        fmt.Println("i =", i)
    }
}
```

**Penjelasan:**

* Ketika **`i == 3`**, perintah **`continue`** dijalankan, sehingga kode setelahnya dilewati, dan loop dilanjutkan dengan iterasi berikutnya.

**Output:**

```css
cssCopy codei = 1
i = 2
Melewati iterasi pada i = 3
i = 4
i = 5
```

***

**Contoh 2: Menggunakan `continue` pada Loop Tertanam (Nested Loop)**

```go
goCopy codepackage main

import "fmt"

func main() {
    for i := 1; i <= 3; i++ {
        for j := 1; j <= 3; j++ {
            if j == 2 {
                continue // Melewati iterasi saat j == 2
            }
            fmt.Printf("i = %d, j = %d\n", i, j)
        }
    }
}
```

**Penjelasan:**

* Ketika **`j == 2`**, iterasi untuk **`j`** dilewati, tetapi **`i`** tetap melanjutkan loop.

**Output:**

```css
cssCopy codei = 1, j = 1
i = 1, j = 3
i = 2, j = 1
i = 2, j = 3
i = 3, j = 1
i = 3, j = 3
```

***

#### **Menggabungkan `break` dan `continue`**

Keduanya dapat digunakan dalam loop untuk memberikan kontrol lebih baik.

**Contoh: `break` dan `continue` Bersama**

```go
goCopy codepackage main

import "fmt"

func main() {
    for i := 1; i <= 10; i++ {
        if i == 3 {
            fmt.Println("Melewati iterasi pada i =", i)
            continue // Melewati iterasi ketika i == 3
        }
        if i == 7 {
            fmt.Println("Menghentikan loop pada i =", i)
            break // Menghentikan loop ketika i == 7
        }
        fmt.Println("i =", i)
    }
}
```

**Penjelasan:**

1. Ketika **`i == 3`**, perintah **`continue`** dijalankan, dan iterasi saat itu dilewati.
2. Ketika **`i == 7`**, perintah **`break`** dijalankan, dan loop dihentikan.

**Output:**

```css
cssCopy codei = 1
i = 2
Melewati iterasi pada i = 3
i = 4
i = 5
i = 6
Menghentikan loop pada i = 7
```

***

#### **Kesimpulan**

* **`break`**:
  * Menghentikan eksekusi loop atau blok.
  * Sering digunakan untuk keluar dari loop atau `switch`.
* **`continue`**:
  * Melewati iterasi saat ini dan melanjutkan ke iterasi berikutnya.
  * Berguna untuk melewati kondisi tertentu tanpa menghentikan loop.

Keduanya memberikan kontrol yang kuat untuk mengelola alur logika dalam loop. Jika ada pertanyaan lebih lanjut, silakan ajukan! 😊



## Fungsi

### **Deklarasi Fungsi**

Dalam bahasa Go, **fungsi** adalah blok kode yang dirancang untuk melakukan tugas tertentu dan dapat dipanggil di berbagai bagian program. Fungsi adalah salah satu elemen utama yang memungkinkan penggunaan kembali kode (reusability).

***

#### **Sintaks Dasar Fungsi**

```go
goCopy codefunc namaFungsi(parameter1 tipe, parameter2 tipe, ...) (returnType1, returnType2, ...) {
    // blok kode
    return nilai
}
```

**Keterangan:**

1. **`func`**: Kata kunci untuk mendefinisikan fungsi.
2. **`namaFungsi`**: Nama fungsi yang digunakan untuk memanggilnya.
3. **`parameter`**: Input fungsi yang bersifat opsional, terdiri dari nama dan tipe data.
4. **`returnType`**: Tipe data nilai yang dikembalikan oleh fungsi. Jika fungsi tidak mengembalikan nilai, bagian ini bisa diabaikan.
5. **`return`**: Digunakan untuk mengembalikan nilai dari fungsi.

***

#### **Contoh Fungsi Sederhana**

**Fungsi Tanpa Parameter dan Tanpa Return**

```go
goCopy codepackage main

import "fmt"

func sayHello() {
    fmt.Println("Halo, Selamat datang!")
}

func main() {
    sayHello() // Memanggil fungsi
}
```

**Output:**

```
Copy codeHalo, Selamat datang!
```

***

### **Fungsi dengan Parameter**

**Fungsi dengan Satu Parameter**

```go
goCopy codepackage main

import "fmt"

func greet(name string) {
    fmt.Printf("Halo, %s!\n", name)
}

func main() {
    greet("Budi") // Memanggil fungsi dengan argumen "Budi"
}
```

**Output:**

```
Copy codeHalo, Budi!
```

**Fungsi dengan Beberapa Parameter**

```go
goCopy codepackage main

import "fmt"

func add(a int, b int) {
    fmt.Printf("Hasil penjumlahan %d + %d = %d\n", a, b, a+b)
}

func main() {
    add(5, 3) // Memanggil fungsi dengan argumen 5 dan 3
}
```

**Output:**

```
Copy codeHasil penjumlahan 5 + 3 = 8
```

***

#### **Fungsi dengan Return**

**Fungsi dengan Satu Nilai Return**

```go
goCopy codepackage main

import "fmt"

func square(number int) int {
    return number * number
}

func main() {
    hasil := square(4)
    fmt.Println("Hasil kuadrat:", hasil)
}
```

**Output:**

```yaml
yamlCopy codeHasil kuadrat: 16
```

**Fungsi dengan Banyak Nilai Return**

```go
goCopy codepackage main

import "fmt"

func divide(a, b int) (int, int) {
    return a / b, a % b // Mengembalikan hasil bagi dan sisa
}

func main() {
    hasil, sisa := divide(10, 3)
    fmt.Println("Hasil bagi:", hasil)
    fmt.Println("Sisa:", sisa)
}
```

**Output:**

```yaml
yamlCopy codeHasil bagi: 3
Sisa: 1
```

***

#### **Fungsi dengan Parameter Default**

Go tidak mendukung parameter default secara langsung. Anda dapat mengatasinya dengan cara berikut:

```go
goCopy codepackage main

import "fmt"

func greet(name string) {
    if name == "" {
        name = "Tamu" // Mengatur nilai default
    }
    fmt.Printf("Halo, %s!\n", name)
}

func main() {
    greet("")      // Menggunakan nilai default
    greet("Andi") // Menggunakan nilai argumen
}
```

**Output:**

```
Copy codeHalo, Tamu!
Halo, Andi!
```

***

#### **Fungsi dengan Parameter Variadic**

Fungsi variadic memungkinkan Anda untuk menerima sejumlah parameter yang tidak terbatas.

```go
goCopy codepackage main

import "fmt"

func sum(numbers ...int) int {
    total := 0
    for _, num := range numbers {
        total += num
    }
    return total
}

func main() {
    fmt.Println("Total:", sum(1, 2, 3, 4, 5)) // Menerima banyak argumen
}
```

**Output:**

```makefile
makefileCopy codeTotal: 15
```

***

#### **Fungsi Anonim**

Fungsi tanpa nama (anonim) dapat digunakan langsung atau disimpan dalam variabel.

```go
goCopy codepackage main

import "fmt"

func main() {
    // Fungsi anonim langsung dijalankan
    func() {
        fmt.Println("Ini fungsi anonim")
    }()

    // Fungsi anonim disimpan dalam variabel
    greet := func(name string) {
        fmt.Printf("Halo, %s!\n", name)
    }

    greet("Dewi")
}
```

**Output:**

```
Copy codeIni fungsi anonim
Halo, Dewi!
```

***

#### **Fungsi Sebagai Parameter**

Fungsi dapat digunakan sebagai parameter untuk fungsi lain.

```go
goCopy codepackage main

import "fmt"

func calculate(a, b int, operation func(int, int) int) int {
    return operation(a, b)
}

func main() {
    add := func(x, y int) int {
        return x + y
    }
    hasil := calculate(5, 3, add)
    fmt.Println("Hasil:", hasil)
}
```

**Output:**

```makefile
makefileCopy codeHasil: 8
```

***

#### **Fungsi dengan Nama Return (Named Return)**

Anda dapat memberikan nama pada nilai return untuk meningkatkan keterbacaan.

```go
goCopy codepackage main

import "fmt"

func rectangleArea(length, width int) (area int) {
    area = length * width // area sudah dideklarasikan
    return
}

func main() {
    fmt.Println("Luas persegi panjang:", rectangleArea(5, 3))
}
```

**Output:**

```yaml
yamlCopy codeLuas persegi panjang: 15
```

***

#### **Kesimpulan**

* Fungsi di Go mendukung parameter, nilai return, dan bahkan banyak nilai return.
* Anda bisa membuat fungsi anonim, parameter variadic, atau bahkan fungsi yang menerima fungsi sebagai parameter.
* Dengan memahami cara kerja fungsi, Anda dapat menulis kode yang lebih modular, efisien, dan mudah dipahami.



## **Defer, Panic, dan Recover**

Go memiliki tiga mekanisme penting untuk mengelola alur program terkait **cleanup** dan **error handling**: **`defer`**, **`panic`**, dan **`recover`**. Ketiganya sering digunakan bersama untuk menangani situasi tidak terduga atau membersihkan resource.

***

### **`Defer`**

**Fungsi:**

* **`defer`** digunakan untuk menunda eksekusi suatu fungsi atau pernyataan hingga blok fungsi saat ini selesai. Pernyataan yang menggunakan `defer` akan dieksekusi setelah fungsi berakhir, bahkan jika fungsi berakhir dengan `panic`.

**Ciri-ciri:**

* **Dijalankan di akhir fungsi:** Semua pernyataan `defer` dalam fungsi dieksekusi dalam urutan **LIFO** (Last In, First Out).
* Berguna untuk membersihkan resource seperti menutup file, koneksi, atau menangani cleanup lainnya.

***

**Contoh: Menggunakan `defer`**

```go
goCopy codepackage main

import "fmt"

func contohDefer() {
    fmt.Println("Langkah 1")
    defer fmt.Println("Langkah 3 (Ditunda dengan defer)")
    fmt.Println("Langkah 2")
}

func main() {
    contohDefer()
}
```

**Output:**

```go
goCopy codeLangkah 1
Langkah 2
Langkah 3 (Ditunda dengan defer)
```

**Penjelasan:**

* **`defer`** menunda eksekusi hingga akhir fungsi, sehingga "Langkah 3" dicetak terakhir.

***

**Contoh: Cleanup Resource**

```go
goCopy codepackage main

import (
    "fmt"
    "os"
)

func writeFile(filename string) {
    file, err := os.Create(filename)
    if err != nil {
        fmt.Println("Gagal membuat file:", err)
        return
    }
    defer file.Close() // Pastikan file selalu ditutup

    file.WriteString("Hello, Go!") // Menulis ke file
    fmt.Println("File berhasil ditulis")
}

func main() {
    writeFile("contoh.txt")
}
```

**Penjelasan:**

* **`defer file.Close()`** memastikan file selalu ditutup, bahkan jika terjadi error.

***

### **`Panic`**

**Fungsi:**

* **`panic`** digunakan untuk menghentikan eksekusi program secara tiba-tiba. Biasanya digunakan untuk menangani kesalahan kritis.
* Ketika **`panic`** dipanggil:
  1. Fungsi saat ini langsung dihentikan.
  2. Semua pernyataan `defer` yang ada dieksekusi.
  3. Program akan berhenti (kecuali jika ada `recover` untuk menangani).

**Contoh: `panic` Sederhana**

```go
goCopy codepackage main

import "fmt"

func divide(a, b int) int {
    if b == 0 {
        panic("Tidak bisa membagi dengan nol!") // Menghentikan program
    }
    return a / b
}

func main() {
    fmt.Println("Hasil:", divide(10, 2))
    fmt.Println("Hasil:", divide(10, 0)) // Memicu panic
}
```

**Output:**

```makefile
makefileCopy codeHasil: 5
panic: Tidak bisa membagi dengan nol!
```

***

**Contoh: `panic` dengan `defer`**

```go
package main

import "fmt"

func main() {
    defer fmt.Println("Ini akan tetap dicetak meskipun panic terjadi.")
    panic("Terjadi kesalahan serius!") // Memicu panic
}
```

**Output:**

```go
Ini akan tetap dicetak meskipun panic terjadi.
panic: Terjadi kesalahan serius!
```

**Penjelasan:**

* Meskipun program dihentikan oleh **`panic`**, pernyataan `defer` tetap dijalankan.

***

### **`Recover`**

**Fungsi:**

* **`recover`** digunakan untuk menangkap dan menangani kondisi `panic`. Ini mencegah program benar-benar berhenti dan memungkinkan program melanjutkan eksekusi.
* Hanya bisa digunakan di dalam fungsi yang dipanggil oleh **`defer`**.

***

**Contoh: Menangkap `panic` dengan `recover`**

```go
package main

import "fmt"

func safeDivide(a, b int) {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recover dari panic:", r)
        }
    }()

    if b == 0 {
        panic("Tidak bisa membagi dengan nol!")
    }
    fmt.Println("Hasil:", a/b)
}

func main() {
    safeDivide(10, 2) // Berjalan normal
    safeDivide(10, 0) // Memicu panic, tapi ditangkap dengan recover
    fmt.Println("Program tetap berjalan.")
}
```

**Output:**

```yaml
yamlCopy codeHasil: 5
Recover dari panic: Tidak bisa membagi dengan nol!
Program tetap berjalan.
```

**Penjelasan:**

* **`recover`** menangkap pesan dari **`panic`**, sehingga program tidak berhenti.

***

#### **Menggabungkan `defer`, `panic`, dan `recover`**

Ketiganya sering digunakan bersama untuk menangani error kritis sekaligus memastikan resource tetap bersih.

**Contoh Lengkap**

```go
package main

import "fmt"

func process() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recover dari error:", r)
        }
    }()
    fmt.Println("Memulai proses")
    panic("Terjadi kesalahan fatal!") // Memicu panic
    fmt.Println("Proses selesai")     // Tidak akan dijalankan
}

func main() {
    process()
    fmt.Println("Program tetap berjalan.")
}
```

**Output:**

```bash
Memulai proses
Recover dari error: Terjadi kesalahan fatal!
Program tetap berjalan.
```

**Penjelasan:**

* **`panic`** dihentikan oleh **`recover`**, sehingga program dapat terus berjalan tanpa error fatal.

***

#### **Kesimpulan**

| **Keyword**   | **Fungsi**                                                               |
| ------------- | ------------------------------------------------------------------------ |
| **`defer`**   | Menunda eksekusi hingga fungsi selesai. Berguna untuk cleanup resource.  |
| **`panic`**   | Menghentikan eksekusi program dengan error kritis.                       |
| **`recover`** | Menangkap error dari `panic` untuk mencegah program berhenti sepenuhnya. |

Ketiga fitur ini sangat membantu dalam mengelola resource dan error handling yang robust di Go. Jika ada pertanyaan lebih lanjut, silakan ajukan!&#x20;



## **Array**

#### **Array dalam Bahasa Go**

Array di Go adalah struktur data yang digunakan untuk menyimpan sejumlah elemen dengan tipe data yang sama. Ukuran array bersifat tetap dan harus ditentukan saat deklarasi. Elemen array diakses menggunakan indeks yang dimulai dari 0.

***

#### **1. Deklarasi Array**

**Sintaks Dasar**

```go
var namaArray [ukuran]tipeData
```

**Contoh: Deklarasi Array**

```go
package main

import "fmt"

func main() {
    var angka [5]int // Array dengan 5 elemen bertipe int
    fmt.Println(angka) // Output: [0 0 0 0 0] (nilai default adalah 0 untuk tipe int)
}
```

***

**Inisialisasi Array**

* Array dapat diinisialisasi langsung dengan nilai-nilai tertentu.

**Contoh: Inisialisasi Langsung**

```go
package main

import "fmt"

func main() {
    angka := [5]int{1, 2, 3, 4, 5} // Deklarasi dan inisialisasi
    fmt.Println(angka) // Output: [1 2 3 4 5]
}
```

**Contoh: Inisialisasi dengan Nilai Sebagian**

```go
package main

import "fmt"

func main() {
    angka := [5]int{1, 2} // Nilai elemen lainnya default (0)
    fmt.Println(angka) // Output: [1 2 0 0 0]
}
```

**Contoh: Ukuran Otomatis**

Jika ukuran array tidak ditentukan, Go menghitungnya secara otomatis berdasarkan jumlah elemen.

```go
package main

import "fmt"

func main() {
    angka := [...]int{1, 2, 3, 4, 5} // Ukuran otomatis dihitung menjadi 5
    fmt.Println(angka) // Output: [1 2 3 4 5]
}
```

***

#### **2. Akses Elemen Array**

**Mengakses Elemen**

Elemen array dapat diakses menggunakan indeks. Indeks dimulai dari **0** hingga **n-1** (di mana `n` adalah ukuran array).

**Contoh: Akses Elemen**

```go
package main

import "fmt"

func main() {
    angka := [5]int{10, 20, 30, 40, 50}
    fmt.Println("Elemen pertama:", angka[0]) // Output: Elemen pertama: 10
    fmt.Println("Elemen kedua:", angka[1])  // Output: Elemen kedua: 20
}
```

**Mengubah Nilai Elemen**

Nilai elemen array dapat diubah dengan mengakses indeksnya.

```go
package main

import "fmt"

func main() {
    angka := [5]int{1, 2, 3, 4, 5}
    angka[2] = 99 // Mengubah elemen ketiga
    fmt.Println(angka) // Output: [1 2 99 4 5]
}
```

***

#### **3. Iterasi Array**

Iterasi array dilakukan menggunakan perulangan seperti `for` atau `for-range`.

**Menggunakan Perulangan `for`**

```go
package main

import "fmt"

func main() {
    angka := [5]int{10, 20, 30, 40, 50}
    for i := 0; i < len(angka); i++ { // len() digunakan untuk mendapatkan panjang array
        fmt.Printf("angka[%d] = %d\n", i, angka[i])
    }
}
```

**Output:**

```
angka[0] = 10
angka[1] = 20
angka[2] = 30
angka[3] = 40
angka[4] = 50
```

**Menggunakan `for-range`**

* Cara yang lebih sederhana untuk mengiterasi elemen array.

```go
package main

import "fmt"

func main() {
    angka := [5]int{10, 20, 30, 40, 50}
    for i, value := range angka {
        fmt.Printf("Indeks %d: %d\n", i, value)
    }
}
```

**Output:**

```
Indeks 0: 10
Indeks 1: 20
Indeks 2: 30
Indeks 3: 40
Indeks 4: 50
```

***

#### **4. Array Multidimensi**

Go juga mendukung array multidimensi, seperti array 2D.

**Contoh: Deklarasi dan Inisialisasi Array 2D**

```go
package main

import "fmt"

func main() {
    matrix := [2][3]int{
        {1, 2, 3}, // Baris pertama
        {4, 5, 6}, // Baris kedua
    }
    fmt.Println(matrix)
}
```

**Output:**

```
[[1 2 3] [4 5 6]]
```

**Mengakses Elemen pada Array 2D**

```go
package main

import "fmt"

func main() {
    matrix := [2][3]int{
        {1, 2, 3},
        {4, 5, 6},
    }
    fmt.Println("Elemen di baris 1 kolom 2:", matrix[0][1]) // Output: 2
}
```

**Iterasi pada Array 2D**

```go
package main

import "fmt"

func main() {
    matrix := [2][3]int{
        {1, 2, 3},
        {4, 5, 6},
    }
    for i, row := range matrix {
        for j, value := range row {
            fmt.Printf("matrix[%d][%d] = %d\n", i, j, value)
        }
    }
}
```

**Output:**

```
matrix[0][0] = 1
matrix[0][1] = 2
matrix[0][2] = 3
matrix[1][0] = 4
matrix[1][1] = 5
matrix[1][2] = 6
```

***

#### **Kesimpulan**

| **Operasi**            | **Keterangan**                                                               |
| ---------------------- | ---------------------------------------------------------------------------- |
| **Deklarasi Array**    | `var arr [size]type` untuk mendeklarasikan array dengan ukuran tetap.        |
| **Inisialisasi**       | Gunakan `{}` untuk memberikan nilai awal, misalnya `arr := [5]int{1, 2, 3}`. |
| **Akses Elemen**       | Elemen diakses menggunakan indeks, misalnya `arr[0]`.                        |
| **Iterasi**            | Gunakan `for` atau `for-range` untuk mengiterasi elemen.                     |
| **Array Multidimensi** | Array dengan lebih dari satu dimensi, misalnya `var matrix [2][3]int`.       |

Jika ada pertanyaan tambahan, silakan ajukan! 😊

## **Slice**

Slice adalah tipe data di Go yang digunakan untuk menangani koleksi elemen, mirip dengan array tetapi lebih fleksibel. Slice tidak memiliki ukuran tetap dan dapat bertambah atau berkurang dinamis sesuai kebutuhan.

***

#### **1. Pengenalan Slice**

Slice adalah referensi ke bagian dari array. Slice memiliki:

* **Pointer:** Mengacu pada elemen array.
* **Length:** Jumlah elemen dalam slice.
* **Capacity:** Jumlah elemen maksimum yang bisa ditampung (dihitung dari awal slice hingga akhir array).

**Deklarasi Slice**

* Slice dapat dideklarasikan dari array atau langsung menggunakan `make()`.

**Contoh: Membuat Slice dari Array**

```go
package main

import "fmt"

func main() {
    array := [5]int{1, 2, 3, 4, 5}
    slice := array[1:4] // Slice dari elemen indeks 1 hingga 3 (4 tidak termasuk)
    fmt.Println(slice) // Output: [2 3 4]
}
```

**Contoh: Slice Tanpa Array**

```go
package main

import "fmt"

func main() {
    slice := []int{10, 20, 30} // Slice langsung tanpa array
    fmt.Println(slice)         // Output: [10 20 30]
}
```

**Contoh: Slice dengan `make()`**

```go
package main

import "fmt"

func main() {
    slice := make([]int, 3, 5) // Panjang 3, kapasitas 5
    fmt.Println(slice)         // Output: [0 0 0] (nilai default)
    fmt.Println("Length:", len(slice), "Capacity:", cap(slice)) // Output: Length: 3 Capacity: 5
}
```

***

#### **2. Manipulasi Slice**

Slice dapat dimanipulasi dengan menambah, mengubah, atau memotong elemen.

**Mengakses dan Mengubah Elemen**

```go
package main

import "fmt"

func main() {
    slice := []int{10, 20, 30}
    fmt.Println("Sebelum:", slice) // Output: [10 20 30]

    slice[1] = 99 // Mengubah elemen kedua
    fmt.Println("Setelah:", slice) // Output: [10 99 30]
}
```

**Memotong Slice**

```go
package main

import "fmt"

func main() {
    slice := []int{10, 20, 30, 40, 50}
    subSlice := slice[1:4] // Slice baru dari elemen 1 hingga 3
    fmt.Println(subSlice)  // Output: [20 30 40]
}
```

**Menghitung Panjang dan Kapasitas**

* **`len(slice)`**: Mengembalikan panjang slice.
* **`cap(slice)`**: Mengembalikan kapasitas slice.

```go
package main

import "fmt"

func main() {
    slice := make([]int, 3, 5)
    fmt.Println("Length:", len(slice), "Capacity:", cap(slice)) // Output: Length: 3 Capacity: 5
}
```

***

#### **3. Fungsi `append`**

**Fungsi:**

* Fungsi bawaan untuk menambah elemen ke dalam slice.
* Jika kapasitas slice sudah penuh, Go akan membuat array baru dengan kapasitas lebih besar secara otomatis.

**Contoh: Menambahkan Elemen**

```go
package main

import "fmt"

func main() {
    slice := []int{10, 20, 30}
    slice = append(slice, 40) // Menambahkan elemen
    fmt.Println(slice)        // Output: [10 20 30 40]
}
```

**Contoh: Menambahkan Banyak Elemen**

```go
package main

import "fmt"

func main() {
    slice := []int{10, 20, 30}
    slice = append(slice, 40, 50, 60) // Menambahkan beberapa elemen
    fmt.Println(slice)                // Output: [10 20 30 40 50 60]
}
```

**Contoh: Menggabungkan Dua Slice**

```go
package main

import "fmt"

func main() {
    slice1 := []int{1, 2, 3}
    slice2 := []int{4, 5, 6}
    slice1 = append(slice1, slice2...) // Menggunakan spread operator (`...`)
    fmt.Println(slice1)                // Output: [1 2 3 4 5 6]
}
```

***

#### **4. Fungsi `copy`**

**Fungsi:**

* Menyalin elemen dari satu slice ke slice lainnya.
* **`copy(dest, src)`**: Menyalin elemen dari `src` ke `dest`. Panjang elemen yang disalin adalah panjang slice terkecil antara `dest` dan `src`.

**Contoh: Menyalin Slice**

```go
package main

import "fmt"

func main() {
    src := []int{1, 2, 3}
    dest := make([]int, 2) // Slice tujuan dengan panjang 2
    copy(dest, src)        // Menyalin elemen
    fmt.Println(dest)      // Output: [1 2]
}
```

**Contoh: Menyalin Semua Elemen**

```go
package main

import "fmt"

func main() {
    src := []int{10, 20, 30}
    dest := make([]int, len(src)) // Panjang slice tujuan sama dengan sumber
    copy(dest, src)               // Menyalin semua elemen
    fmt.Println(dest)             // Output: [10 20 30]
}
```

***

#### **5. Slice Bersifat Referensi**

Slice adalah referensi ke array. Jika elemen slice diubah, elemen array asalnya juga akan berubah.

**Contoh: Efek Referensi**

```go
package main

import "fmt"

func main() {
    array := [5]int{1, 2, 3, 4, 5}
    slice := array[1:4]

    slice[0] = 99 // Mengubah elemen pertama slice
    fmt.Println("Array:", array) // Output: [1 99 3 4 5]
    fmt.Println("Slice:", slice) // Output: [99 3 4]
}
```

***

#### **Kesimpulan**

| **Operasi**    | **Deskripsi**                                                                             |
| -------------- | ----------------------------------------------------------------------------------------- |
| **Deklarasi**  | Slice dapat dibuat dari array atau langsung dideklarasikan dengan `[]type`.               |
| **Manipulasi** | Elemen dapat diubah, panjang dicek dengan `len()`, kapasitas dicek dengan `cap()`.        |
| **`append`**   | Menambah elemen ke slice secara dinamis.                                                  |
| **`copy`**     | Menyalin elemen dari satu slice ke slice lainnya.                                         |
| **Referensi**  | Slice adalah referensi ke array, sehingga perubahan pada slice memengaruhi array aslinya. |

Slice adalah alat penting untuk menangani koleksi data dinamis di Go. Jika ada pertanyaan, silakan tanyakan! 😊

## **Map**

#### **Map dalam Go**

Map di Go adalah struktur data yang digunakan untuk menyimpan pasangan **key-value**. Map memungkinkan pengambilan nilai berdasarkan key, di mana key harus unik dan bertipe data yang valid.

***

#### **1. Pengenalan Map**

* **Key:** Elemen yang digunakan untuk mengakses nilai.
* **Value:** Nilai yang terkait dengan key.
* Map dapat digunakan untuk menyimpan data dengan hubungan logis, seperti tabel hash.

**Deklarasi Map**

```go
var namaMap map[keyType]valueType
```

**Contoh: Deklarasi Map**

```go
package main

import "fmt"

func main() {
    var m map[string]int // Map dengan key bertipe string dan value bertipe int
    fmt.Println(m)       // Output: map[]
}
```

**Inisialisasi Map**

* Map harus diinisialisasi sebelum digunakan, biasanya dengan fungsi `make`.

```go
package main

import "fmt"

func main() {
    m := make(map[string]int) // Membuat map kosong
    fmt.Println(m)            // Output: map[]
}
```

***

#### **2. Operasi Dasar pada Map**

**a. Menambahkan Elemen**

* Gunakan sintaks `map[key] = value` untuk menambahkan elemen ke map.

```go
package main

import "fmt"

func main() {
    m := make(map[string]int)
    m["satu"] = 1 // Menambahkan pasangan key-value
    m["dua"] = 2
    fmt.Println(m) // Output: map[dua:2 satu:1]
}
```

***

**b. Mengakses Nilai**

* Gunakan key untuk mengakses nilai. Jika key tidak ada, nilai default dari tipe data value akan dikembalikan.

```go
package main

import "fmt"

func main() {
    m := map[string]int{"satu": 1, "dua": 2}
    fmt.Println("Nilai key 'satu':", m["satu"]) // Output: Nilai key 'satu': 1
    fmt.Println("Nilai key 'tiga':", m["tiga"]) // Output: Nilai key 'tiga': 0 (default untuk int)
}
```

***

**c. Menghapus Elemen**

* Gunakan fungsi `delete(map, key)` untuk menghapus elemen berdasarkan key.

```go
package main

import "fmt"

func main() {
    m := map[string]int{"satu": 1, "dua": 2}
    delete(m, "satu") // Menghapus key "satu"
    fmt.Println(m)    // Output: map[dua:2]
}
```

***

**d. Memeriksa Keberadaan Key**

* Saat mengakses nilai, Anda bisa mendapatkan informasi apakah key ada atau tidak dengan dua variabel.

```go
package main

import "fmt"

func main() {
    m := map[string]int{"satu": 1, "dua": 2}
    value, exists := m["satu"]
    if exists {
        fmt.Println("Key 'satu' ditemukan dengan nilai:", value)
    } else {
        fmt.Println("Key 'satu' tidak ditemukan")
    }
}
```

**Output:**

```
Key 'satu' ditemukan dengan nilai: 1
```

***

**e. Iterasi pada Map**

* Gunakan `for-range` untuk mengiterasi seluruh pasangan key-value dalam map.

```go
package main

import "fmt"

func main() {
    m := map[string]int{"satu": 1, "dua": 2, "tiga": 3}
    for key, value := range m {
        fmt.Printf("Key: %s, Value: %d\n", key, value)
    }
}
```

**Output (urutan bisa acak):**

```
Key: satu, Value: 1
Key: dua, Value: 2
Key: tiga, Value: 3
```

***

#### **3. Properti Map**

**a. Map Bersifat Referensi**

Map adalah tipe data referensi, sehingga perubahan pada map akan memengaruhi semua variabel yang merujuk ke map tersebut.

```go
package main

import "fmt"

func main() {
    m := map[string]int{"satu": 1}
    n := m // n merujuk ke map yang sama dengan m
    n["dua"] = 2
    fmt.Println("m:", m) // Output: m: map[satu:1 dua:2]
    fmt.Println("n:", n) // Output: n: map[satu:1 dua:2]
}
```

**b. Kapasitas Map**

* Map tidak memiliki kapasitas tetap; ukurannya dapat berubah sesuai kebutuhan.
* Panjang map bisa diperiksa dengan fungsi `len()`.

```go
package main

import "fmt"

func main() {
    m := map[string]int{"satu": 1, "dua": 2}
    fmt.Println("Jumlah elemen:", len(m)) // Output: Jumlah elemen: 2
}
```

***

#### **4. Contoh Penggunaan Map**

**a. Menghitung Frekuensi Elemen**

```go
package main

import "fmt"

func main() {
    kata := []string{"go", "is", "awesome", "go", "is", "fun"}
    frekuensi := make(map[string]int)

    for _, k := range kata {
        frekuensi[k]++ // Menambahkan 1 untuk setiap kemunculan kata
    }

    fmt.Println(frekuensi) // Output: map[awesome:1 fun:1 go:2 is:2]
}
```

**b. Mengelompokkan Data**

```go
package main

import "fmt"

func main() {
    data := []struct {
        name  string
        group string
    }{
        {"Alice", "A"}, {"Bob", "B"}, {"Charlie", "A"}, {"Dave", "B"},
    }

    kelompok := make(map[string][]string)
    for _, d := range data {
        kelompok[d.group] = append(kelompok[d.group], d.name)
    }

    fmt.Println(kelompok) // Output: map[A:[Alice Charlie] B:[Bob Dave]]
}
```

***

#### **Kesimpulan**

| **Operasi**       | **Penjelasan**                                                                |
| ----------------- | ----------------------------------------------------------------------------- |
| **Deklarasi**     | `var m map[keyType]valueType` atau `m := make(map[keyType]valueType)`.        |
| **Tambah Elemen** | `map[key] = value`.                                                           |
| **Akses Elemen**  | `value := map[key]`.                                                          |
| **Hapus Elemen**  | `delete(map, key)`.                                                           |
| **Periksa Key**   | `value, exists := map[key]`.                                                  |
| **Iterasi**       | Gunakan `for key, value := range map`.                                        |
| **Referensi**     | Map adalah referensi; perubahan pada satu variabel memengaruhi variabel lain. |
| **Panjang**       | Gunakan `len(map)` untuk menghitung jumlah elemen.                            |

Map adalah struktur data yang sangat kuat untuk mengelola pasangan key-value. Jika ada pertanyaan atau butuh contoh tambahan, silakan tanyakan! 😊

## **Struct**

**Struct** adalah tipe data komposit di Go yang memungkinkan Anda untuk mengelompokkan berbagai data menjadi satu unit. Struct sering digunakan untuk merepresentasikan data dengan beberapa atribut atau properti.

#### **1. Pengenalan Struct**

**Definisi**

* Struct adalah kumpulan dari field (properti) yang memiliki tipe data tertentu.
* Struct memungkinkan Anda untuk menyimpan data heterogen dalam satu unit logis.

**Deklarasi Struct**

Untuk mendeklarasikan struct, gunakan kata kunci `type` dan definisikan nama struct.

```go
type NamaStruct struct {
    NamaField1 TipeData
    NamaField2 TipeData
    ...
}
```

**Contoh: Struct Dasar**

```go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}

func main() {
    // Membuat instance struct
    person := Person{Name: "John", Age: 30}
    fmt.Println(person) // Output: {John 30}
}
```

***

#### **2. Membuat dan Mengakses Struct**

**a. Membuat Struct**

1.  **Inisialisasi dengan nilai lengkap:**

    ```go
    p := Person{Name: "Alice", Age: 25}
    ```
2.  **Inisialisasi tanpa nilai (default):**

    ```go
    var p Person
    fmt.Println(p) // Output: { 0} (nilai default string adalah "", int adalah 0)
    ```
3.  **Inisialisasi dengan urutan field:**

    ```go
    p := Person{"Bob", 28} // Tidak direkomendasikan karena kurang eksplisit
    ```

**b. Mengakses Field**

Gunakan operator titik (`.`) untuk mengakses atau memodifikasi field dalam struct.

```go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}

func main() {
    p := Person{Name: "Alice", Age: 25}
    fmt.Println("Name:", p.Name) // Output: Name: Alice
    fmt.Println("Age:", p.Age)   // Output: Age: 25

    // Mengubah field
    p.Age = 26
    fmt.Println("Updated Age:", p.Age) // Output: Updated Age: 26
}
```

***

#### **3. Struct dengan Pointer**

Struct dapat digunakan dengan pointer untuk menghemat memori dan menghindari duplikasi data.

```go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}

func main() {
    p := &Person{Name: "Charlie", Age: 35} // Pointer ke struct
    fmt.Println(p.Name)                   // Output: Charlie

    // Modifikasi melalui pointer
    p.Age = 36
    fmt.Println(p.Age) // Output: 36
}
```

***

#### **4. Method pada Struct**

Struct dapat memiliki method (fungsi yang terkait dengan struct). Method diakses menggunakan receiver.

**a. Definisi Method**

```go
func (receiver TipeStruct) NamaMethod() TipeKembalian {
    // Implementasi
}
```

**b. Contoh: Method pada Struct**

```go
package main

import "fmt"

type Rectangle struct {
    Width  float64
    Height float64
}

// Method untuk menghitung area
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Method untuk mengubah dimensi (pointer receiver)
func (r *Rectangle) Resize(width, height float64) {
    r.Width = width
    r.Height = height
}

func main() {
    rect := Rectangle{Width: 10, Height: 5}
    fmt.Println("Area:", rect.Area()) // Output: Area: 50

    rect.Resize(15, 10)
    fmt.Println("Updated Area:", rect.Area()) // Output: Updated Area: 150
}
```

***

#### **5. Embedded Struct (Struct di dalam Struct)**

Struct dapat digunakan sebagai field di dalam struct lainnya. Ini sering digunakan untuk menciptakan hierarki data.

**Contoh: Embedded Struct**

```go
package main

import "fmt"

type Address struct {
    City    string
    ZipCode int
}

type Person struct {
    Name    string
    Age     int
    Address // Embedded struct
}

func main() {
    p := Person{
        Name: "David",
        Age:  40,
        Address: Address{
            City:    "New York",
            ZipCode: 10001,
        },
    }

    // Akses langsung ke field Address
    fmt.Println("City:", p.City)     // Output: City: New York
    fmt.Println("Zip Code:", p.ZipCode) // Output: Zip Code: 10001
}
```

***

#### **6. Anonymous Struct**

Struct anonim adalah struct yang tidak diberi nama dan sering digunakan untuk data sementara.

**Contoh: Anonymous Struct**

```go
package main

import "fmt"

func main() {
    // Anonymous struct
    p := struct {
        Name string
        Age  int
    }{
        Name: "Eve",
        Age:  22,
    }

    fmt.Println(p) // Output: {Eve 22}
}
```

***

#### **7. Perbandingan Struct**

Struct di Go dapat dibandingkan jika semua field di dalamnya dapat dibandingkan.

```go
package main

import "fmt"

type Point struct {
    X, Y int
}

func main() {
    p1 := Point{X: 1, Y: 2}
    p2 := Point{X: 1, Y: 2}
    p3 := Point{X: 3, Y: 4}

    fmt.Println(p1 == p2) // Output: true
    fmt.Println(p1 == p3) // Output: false
}
```

***

#### **8. Operasi Lanjutan**

**a. Menggunakan Slice dari Struct**

```go
package main

import "fmt"

type Product struct {
    Name  string
    Price float64
}

func main() {
    products := []Product{
        {"Laptop", 1000.0},
        {"Smartphone", 500.0},
    }

    for _, p := range products {
        fmt.Printf("Product: %s, Price: %.2f\n", p.Name, p.Price)
    }
}
```

**b. Membuat Map dengan Struct**

```go
package main

import "fmt"

type Student struct {
    Name  string
    Grade int
}

func main() {
    students := map[int]Student{
        1: {"Alice", 90},
        2: {"Bob", 85},
    }

    for id, student := range students {
        fmt.Printf("ID: %d, Name: %s, Grade: %d\n", id, student.Name, student.Grade)
    }
}
```

***

#### **Kesimpulan**

| **Fitur**            | **Deskripsi**                                                        |
| -------------------- | -------------------------------------------------------------------- |
| **Deklarasi Struct** | Struct dideklarasikan dengan `type` diikuti nama dan field-fieldnya. |
| **Embedded Struct**  | Struct dapat berisi struct lain untuk komposisi data.                |
| **Method**           | Fungsi yang terkait dengan struct untuk operasi tertentu.            |
| **Pointer Struct**   | Digunakan untuk efisiensi memori atau memodifikasi field struct.     |
| **Anonymous Struct** | Struct tanpa nama untuk data sementara.                              |
| **Perbandingan**     | Struct dapat dibandingkan jika field-fieldnya dapat dibandingkan.    |

Struct di Go adalah alat yang sangat kuat untuk menciptakan tipe data kustom. Jika Anda butuh bantuan lebih lanjut atau contoh tambahan, silakan tanyakan! 😊

**Konsep Object-Oriented Programming (OOP) di Golang** berbeda dari bahasa seperti Java atau Python karena Go **tidak memiliki class**. Namun, Go mendukung paradigma OOP melalui **struct, method, interface**, dan komposisi. Berikut adalah penjelasan lengkapnya:

***

## **Konsep Dasar OOP di Golang**

1. **Struct (Mirip dengan Class)**
   * Dalam Go, **struct** digunakan untuk mendefinisikan tipe data kompleks yang bisa menyimpan properti (field).
   * Struct adalah cara Go untuk mewakili **object**.
2. **Method**
   * Method adalah fungsi yang terkait dengan suatu struct.
   * Ini digunakan untuk mendefinisikan **perilaku** dari suatu struct (mirip dengan metode dalam class).
3. **Encapsulation (Penyembunyian Data)**
   * Encapsulation dilakukan dengan cara membuat field atau method bersifat **private** (dimulai dengan huruf kecil) atau **public** (dimulai dengan huruf besar).
   * Private field/method hanya bisa diakses dari dalam package yang sama.
4. **Inheritance (Pewarisan)**
   * Go **tidak mendukung inheritance langsung**, tetapi mendukung **komposisi** (menyematkan struct dalam struct lain) untuk mencapai perilaku serupa.
5. **Polymorphism (Polimorfisme)**
   * Polymorphism dalam Go dicapai menggunakan **interface**.
   * Interface adalah kontrak yang mendefinisikan kumpulan method yang harus diimplementasikan oleh struct tertentu.

***

#### **Penjelasan dengan Contoh**

**1. Membuat Struct (Representasi Object)**

```go
package main

import "fmt"

// Struct untuk representasi object
type Person struct {
    Name string
    Age  int
}

func main() {
    // Membuat instance dari struct
    person := Person{Name: "John", Age: 30}
    fmt.Println("Nama:", person.Name)
    fmt.Println("Umur:", person.Age)
}
```

**2. Menambahkan Method pada Struct**

```go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}

// Method terkait dengan struct Person
func (p Person) Greet() {
    fmt.Println("Halo, nama saya", p.Name)
}

func main() {
    person := Person{Name: "Alice", Age: 25}
    person.Greet() // Output: Halo, nama saya Alice
}
```

**3. Encapsulation (Membuat Field Private)**

```go
package main

import "fmt"

type Person struct {
    name string // private field
    age  int    // private field
}

// Method untuk mengatur nama (setter)
func (p *Person) SetName(name string) {
    p.name = name
}

// Method untuk mendapatkan nama (getter)
func (p Person) GetName() string {
    return p.name
}

func main() {
    person := &Person{}
    person.SetName("Bob")
    fmt.Println("Nama:", person.GetName()) // Output: Nama: Bob
}
```

**4. Komposisi (Pengganti Inheritance)**

```go
package main

import "fmt"

// Struct untuk representasi kendaraan
type Vehicle struct {
    Brand string
    Model string
}

// Struct untuk representasi mobil yang menyematkan Vehicle
type Car struct {
    Vehicle
    NumberOfDoors int
}

func main() {
    car := Car{
        Vehicle:      Vehicle{Brand: "Toyota", Model: "Corolla"},
        NumberOfDoors: 4,
    }
    fmt.Println("Merek Mobil:", car.Brand)  // Mengakses field dari Vehicle
    fmt.Println("Model Mobil:", car.Model) // Mengakses field dari Vehicle
    fmt.Println("Jumlah Pintu:", car.NumberOfDoors)
}
```

**5. Polymorphism dengan Interface**

```go
package main

import "fmt"

// Interface untuk mendefinisikan kontrak
type Shape interface {
    Area() float64
    Perimeter() float64
}

// Struct untuk lingkaran
type Circle struct {
    Radius float64
}

// Implementasi method Area dan Perimeter untuk Circle
func (c Circle) Area() float64 {
    return 3.14 * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * 3.14 * c.Radius
}

// Struct untuk persegi panjang
type Rectangle struct {
    Width  float64
    Height float64
}

// Implementasi method Area dan Perimeter untuk Rectangle
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

func main() {
    // Polymorphism: Memiliki array Shape
    shapes := []Shape{
        Circle{Radius: 5},
        Rectangle{Width: 3, Height: 4},
    }

    for _, shape := range shapes {
        fmt.Println("Area:", shape.Area())
        fmt.Println("Perimeter:", shape.Perimeter())
    }
}
```

***

#### **Kesimpulan**

1. **Struct** berfungsi sebagai pengganti class dalam Go.
2. **Method** memungkinkan kita menambahkan perilaku (behavior) ke struct.
3. **Encapsulation** dicapai dengan aturan akses public/private berdasarkan huruf besar/kecil.
4. **Komposisi** digunakan sebagai alternatif inheritance.
5. **Polymorphism** diterapkan menggunakan interface, memungkinkan berbagai tipe struct memiliki perilaku seragam.

Dengan pendekatan sederhana ini, Go mampu mengimplementasikan prinsip OOP secara efisien tanpa kompleksitas seperti yang ditemukan dalam bahasa lain.



