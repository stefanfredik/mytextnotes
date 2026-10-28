# Modul 2

## Modul Pembelajaran Golang: Dari Dasar Hingga Advanced

### Daftar Isi

1. [Tingkat Dasar](https://claude.ai/chat/45d80534-ae32-4b37-b79c-330d79a2e07d#tingkat-dasar)
2. [Tingkat Menengah](https://claude.ai/chat/45d80534-ae32-4b37-b79c-330d79a2e07d#tingkat-menengah)
3. [Tingkat Lanjut (Advanced)](https://claude.ai/chat/45d80534-ae32-4b37-b79c-330d79a2e07d#tingkat-lanjut-advanced)
4. [Proyek Aplikasi](https://claude.ai/chat/45d80534-ae32-4b37-b79c-330d79a2e07d#proyek-aplikasi)
5. [Sumber Belajar Tambahan](https://claude.ai/chat/45d80534-ae32-4b37-b79c-330d79a2e07d#sumber-belajar-tambahan)

***

### Tingkat Dasar

#### Modul 1: Pengenalan Golang

* **1.1 Sejarah dan Filosofi Go**
  * Asal usul Go: Diciptakan oleh Google (Robert Griesemer, Rob Pike, Ken Thompson)
  * Tujuan desain: Sederhana, efisien, handal, dan aman
  * Karakteristik: Kompilasi cepat, garbage collection, konkurensi bawaan
* **1.2 Instalasi dan Setup Environment**
  * Instalasi Go di berbagai sistem operasi (Windows, macOS, Linux)
  * Mengatur GOPATH dan GOROOT
  * Pengenalan Go modules
  * Tools dasar: `go build`, `go run`, `go get`
* **1.3 Hello World dan Struktur Program Go**
  * Membuat program pertama
  * Struktur dasar program Go
  * Package dan import
  * Fungsi main

#### Modul 2: Dasar-Dasar Bahasa Go

* **2.1 Variabel dan Tipe Data**
  * Deklarasi variabel (var, :=)
  * Tipe data dasar: int, float, string, boolean
  * Konstanta
  * Zero value
* **2.2 Operator dan Ekspresi**
  * Operator aritmatika
  * Operator perbandingan
  * Operator logika
  * Operator assignment
  * Operator bitwise
* **2.3 Kontrol Alur**
  * Pernyataan if-else
  * Switch case
  * Loops: for, for-range
  * break dan continue
  * Goto (dan mengapa jarang digunakan)
* **2.4 Fungsi**
  * Deklarasi fungsi
  * Parameter dan return value
  * Multiple return values
  * Named return values
  * Variadic functions
  * Deferred function calls

#### Modul 3: Struktur Data Dasar

* **3.1 Array dan Slice**
  * Perbedaan array dan slice
  * Membuat dan memanipulasi array
  * Membuat dan memanipulasi slice
  * Fungsi-fungsi slice: append, copy, len, cap
* **3.2 Map**
  * Membuat dan menggunakan map
  * Operasi CRUD pada map
  * Iterasi map
  * Penggunaan delete
* **3.3 Struct**
  * Mendefinisikan struct
  * Embedded struct
  * Anonymous struct
  * Tags pada struct

#### Modul 4: Penanganan Error dan Debugging

* **4.1 Error Handling**
  * Konsep error di Go
  * Membuat dan mengembalikan error
  * Menggunakan errors.New dan fmt.Errorf
  * Pengecekan error (if err != nil)
* **4.2 Panic dan Recover**
  * Memahami panic
  * Menggunakan recover
  * Kapan menggunakan panic vs error
* **4.3 Debugging**
  * Debugging dengan print statement
  * Menggunakan package log
  * Pengenalan debugging tools: Delve

#### Modul 5: Package dan Modules

* **5.1 Membuat dan Menggunakan Package**
  * Struktur package
  * Visibilitas: exported vs unexported
  * Import dan alias
* **5.2 Go Modules**
  * Inisialisasi modul dengan go mod init
  * Versioning dan Semantic Versioning
  * go.mod dan go.sum
  * Upgrade dan downgrade dependencies
* **5.3 Penggunaan Package Standard Library**
  * fmt: formatting dan printing
  * strings dan strconv
  * time
  * os dan io

***

### Tingkat Menengah

#### Modul 6: Pointer dan Memory Management

* **6.1 Dasar-Dasar Pointer**
  * Konsep pointer
  * Deklarasi dan penggunaan pointer
  * Pointer arithmetic (atau ketiadaannya di Go)
  * Pointer ke struct
* **6.2 Pass by Value vs Pass by Reference**
  * Memahami konsep pass by value
  * Menggunakan pointer untuk pass by reference
  * Kasus penggunaan pointer vs value
* **6.3 Memory Management di Go**
  * Garbage collection
  * Escape analysis
  * Memory leaks dan cara menghindarinya
  * Menggunakan pprof untuk profiling

#### Modul 7: Interface dan Type System

* **7.1 Interface**
  * Konsep interface
  * Implementasi implisit
  * Type assertion dan type switch
  * Empty interface (interface{})
* **7.2 Type Conversions**
  * Konversi tipe dasar
  * Konversi antara custom types
  * Konversi ke dan dari interface{}
* **7.3 Generics (sejak Go 1.18)**
  * Dasar-dasar generics
  * Type parameters
  * Constraint interface
  * Implementasi fungsi dan tipe generik

#### Modul 8: Konkurensi Dasar

* **8.1 Goroutines**
  * Konsep goroutine
  * Membuat dan menjalankan goroutine
  * Scheduler Go
* **8.2 Channels**
  * Konsep channel
  * Buffered vs unbuffered channels
  * Mengirim dan menerima data
  * Range dan close pada channel
  * Select statement
* **8.3 Sinkronisasi**
  * WaitGroup
  * Mutex dan RWMutex
  * atomic package
  * Kondisi race dan race detector

#### Modul 9: Testing

* **9.1 Unit Testing**
  * Membuat dan menjalankan unit test
  * Table-driven tests
  * Subtest
  * Test coverage
* **9.2 Benchmarking**
  * Menulis benchmark
  * Menjalankan benchmark
  * Menganalisis hasil benchmark
* **9.3 Mocking dan Testify**
  * Pengenalan testify
  * Assertions
  * Mock objects
  * Stubbing

#### Modul 10: I/O dan File Handling

* **10.1 File I/O**
  * Membaca dan menulis file
  * Buffered I/O
  * Scanner dan Writer
* **10.2 I/O Interface**
  * io.Reader dan io.Writer
  * io.ReadWriter
  * Buffered readers dan writers
* **10.3 Serialisasi dan Deserialisasi**
  * JSON encoding/decoding
  * XML encoding/decoding
  * gob package
  * Protocol buffers

***

### Tingkat Lanjut (Advanced)

#### Modul 11: Konkurensi Lanjutan

* **11.1 Pola Konkurensi**
  * Worker pools
  * Pipeline
  * Fan-out, fan-in
  * Rate limiting
  * Context untuk pembatalan operasi
* **11.2 Synchronization Primitives Lanjutan**
  * sync.Once
  * sync.Cond
  * sync.Pool
  * atomic.Value
* **11.3 Konkurensi vs Paralelisme**
  * Perbedaan konkurensi dan paralelisme
  * GOMAXPROCS
  * CPU-bound vs I/O-bound tasks

#### Modul 12: Networking

* **12.1 HTTP Client dan Server**
  * net/http package
  * Membuat HTTP server
  * Routing
  * Middleware
* **12.2 REST API**
  * Desain REST API
  * Implementasi handler
  * Authentication
  * Documentation (Swagger/OpenAPI)
* **12.3 WebSocket**
  * Protokol WebSocket
  * Implementasi WebSocket server
  * Implementasi WebSocket client
* **12.4 gRPC**
  * Pengenalan Protocol Buffers
  * Implementasi gRPC client dan server
  * Streaming RPC

#### Modul 13: Database

* **13.1 SQL dengan database/sql**
  * Koneksi database
  * Query dan Exec
  * Prepared statements
  * Transactions
* **13.2 ORM dengan GORM**
  * Setup dan konfigurasi
  * Model dan migrations
  * CRUD operations
  * Relationships
  * Querying
* **13.3 NoSQL dengan MongoDB**
  * Setup dan konfigurasi
  * CRUD operations
  * Aggregation
  * Indexing

#### Modul 14: Arsitektur Aplikasi

* **14.1 Clean Architecture**
  * Prinsip-prinsip Clean Architecture
  * Dependency Injection
  * Repository pattern
  * Service layer
* **14.2 Microservices**
  * Prinsip microservices
  * Service discovery
  * API Gateway
  * Event-driven architecture
* **14.3 Domain-Driven Design**
  * Bounded Context
  * Entities, Value Objects, Aggregates
  * Domain Events
  * Event Sourcing

#### Modul 15: Performance Optimization

* **15.1 Profiling**
  * CPU profiling
  * Memory profiling
  * Block profiling
  * Mutex profiling
* **15.2 Optimasi Kode**
  * Escape analysis
  * Menghindari allocations
  * Struct packing
  * Efficient string handling
* **15.3 Caching**
  * In-memory caching
  * Redis integration
  * Cache invalidation strategies

#### Modul 16: DevOps dan Deployment

* **16.1 Containerization dengan Docker**
  * Dockerfile untuk Go
  * Multi-stage builds
  * Docker Compose
* **16.2 CI/CD**
  * GitHub Actions
  * GitLab CI
  * Jenkins
* **16.3 Kubernetes**
  * Kubernetes basics
  * Deploying Go apps on Kubernetes
  * ConfigMaps dan Secrets
  * Health checks dan autoscaling

#### Modul 17: Security

* **17.1 Authentication dan Authorization**
  * JWT
  * OAuth 2.0
  * RBAC
* **17.2 Secure Coding Practices**
  * Input validation
  * SQL injection prevention
  * XSS prevention
  * CSRF protection
* **17.3 Cryptography**
  * Hashing
  * Encryption/Decryption
  * TLS/SSL
  * golang.org/x/crypto

#### Modul 18: Monitoring dan Observability

* **18.1 Logging**
  * Structured logging
  * Log levels
  * Zap dan logrus
* **18.2 Metrics**
  * Prometheus
  * Grafana
  * Custom metrics
* **18.3 Tracing**
  * OpenTelemetry
  * Jaeger
  * Distributed tracing

***

### Proyek Aplikasi

#### Proyek 1: CLI Tool

* Membuat aplikasi CLI dengan Cobra
* Flags dan arguments
* Interactive prompts
* Progress bars

#### Proyek 2: REST API

* Web API dengan Gin/Echo/Chi
* Database integration
* Authentication dan authorization
* Swagger documentation

#### Proyek 3: Microservices

* Multiple services
* Service communication
* Message queues (RabbitMQ, Kafka)
* API Gateway

#### Proyek 4: Real-time Web Application

* WebSocket server
* Real-time frontend (optional)
* Message broadcasting
* User presence

#### Proyek 5: Sistem Terdistribusi

* Consensus algorithms
* Distributed caching
* Service discovery
* Circuit breaking

***

### Sumber Belajar Tambahan

#### Buku

* "The Go Programming Language" oleh Alan A. A. Donovan dan Brian W. Kernighan
* "Go in Action" oleh William Kennedy
* "Concurrency in Go" oleh Katherine Cox-Buday
* "Go Web Programming" oleh Sau Sheong Chang
* "Let's Go" dan "Let's Go Further" oleh Alex Edwards

#### Situs Web

* [Go by Example](https://gobyexample.com/)
* [Go Tour](https://tour.golang.org/)
* [Go Documentation](https://golang.org/doc/)
* [Effective Go](https://golang.org/doc/effective_go)
* [Go Patterns](https://github.com/tmrts/go-patterns)

#### Kursus Online

* Go Programming (Golang): The Complete Developer's Guide (Udemy)
* Learn How To Code: Google's Go (golang) Programming Language (Udemy)
* Master Go (Golang) Programming: The Complete Go Bootcamp (Udemy)
* Go: The Complete Developer's Guide (Udemy)

#### Komunitas

* [Go Forum](https://forum.golangbridge.org/)
* [r/golang](https://www.reddit.com/r/golang/)
* [Gophers Slack](https://invite.slack.golangbridge.org/)
* [Go GitHub Issues](https://github.com/golang/go/issues)
