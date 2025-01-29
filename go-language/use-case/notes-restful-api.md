# Notes Restful API

Untuk membuat modul RESTful API menggunakan Go (Golang) yang dapat membuat catatan, kita akan membangun API yang memiliki beberapa endpoint untuk melakukan operasi CRUD (Create, Read, Update, Delete) pada catatan (notes). Berikut adalah langkah-langkah lengkap untuk membuatnya:

#### 1. Persiapan

Pastikan Go sudah terinstal di sistem Anda. Anda bisa mengunduhnya dari [situs resmi Go](https://golang.org/dl/).

#### 2. Membuat Folder Proyek

Buat folder untuk proyek dan masuk ke dalamnya:

```bash
mkdir golang-rest-api-notes
cd golang-rest-api-notes
```

#### 3. Inisialisasi Modul Go

Inisialisasi proyek Go:

```bash
go mod init golang-rest-api-notes
```

#### 4. Instalasi Dependensi

Untuk membangun API RESTful dengan Go, kita membutuhkan beberapa dependensi seperti `gorilla/mux` untuk routing dan `github.com/jinzhu/gorm` untuk ORM database (optional jika ingin menggunakan database). Kita juga akan menggunakan `net/http` untuk membangun server HTTP.

```bash
go get github.com/gorilla/mux
go get github.com/jinzhu/gorm
go get github.com/jinzhu/gorm/dialects/sqlite
```

#### 5. Struktur Proyek

Struktur folder proyek kita akan terlihat seperti ini:

```
golang-rest-api-notes/
|-- main.go
|-- models/
|   |-- note.go
|-- handlers/
|   |-- note_handler.go
|-- routes/
|   |-- routes.go
|-- go.mod
|-- go.sum
```

#### 6. Membuat Model `Note`

Pada folder `models/`, buat file `note.go` untuk mendefinisikan struktur `Note`:

```go
// models/note.go
package models

import (
	"github.com/jinzhu/gorm"
)

// Note struct defines the note entity
type Note struct {
	ID      uint   `json:"id"`
	Title   string `json:"title"`
	Content string `json:"content"`
}

// Initialize DB connection (optional)
var DB *gorm.DB

func InitializeDB() {
	var err error
	DB, err = gorm.Open("sqlite3", "./notes.db")
	if err != nil {
		panic("failed to connect database")
	}
	DB.AutoMigrate(&Note{})
}
```

#### 7. Membuat Handler untuk Catatan

Pada folder `handlers/`, buat file `note_handler.go` untuk mendefinisikan handler fungsi yang menangani request ke API.

```go
// handlers/note_handler.go
package handlers

import (
	"encoding/json"
	"net/http"
	"golang-rest-api-notes/models"
	"github.com/gorilla/mux"
)

// Get all notes
func GetNotes(w http.ResponseWriter, r *http.Request) {
	var notes []models.Note
	models.DB.Find(&notes)
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(notes)
}

// Create a new note
func CreateNote(w http.ResponseWriter, r *http.Request) {
	var note models.Note
	_ = json.NewDecoder(r.Body).Decode(&note)
	models.DB.Create(&note)
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(note)
}

// Get a single note by ID
func GetNoteByID(w http.ResponseWriter, r *http.Request) {
	params := mux.Vars(r)
	var note models.Note
	if err := models.DB.First(&note, params["id"]).Error; err != nil {
		http.Error(w, "Note not found", http.StatusNotFound)
		return
	}
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(note)
}

// Update an existing note
func UpdateNote(w http.ResponseWriter, r *http.Request) {
	params := mux.Vars(r)
	var note models.Note
	if err := models.DB.First(&note, params["id"]).Error; err != nil {
		http.Error(w, "Note not found", http.StatusNotFound)
		return
	}
	_ = json.NewDecoder(r.Body).Decode(&note)
	models.DB.Save(&note)
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(note)
}

// Delete a note
func DeleteNote(w http.ResponseWriter, r *http.Request) {
	params := mux.Vars(r)
	var note models.Note
	if err := models.DB.First(&note, params["id"]).Error; err != nil {
		http.Error(w, "Note not found", http.StatusNotFound)
		return
	}
	models.DB.Delete(&note)
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(note)
}
```

#### 8. Membuat Routing

Pada folder `routes/`, buat file `routes.go` untuk mendefinisikan routing API.

```go
// routes/routes.go
package routes

import (
	"github.com/gorilla/mux"
	"golang-rest-api-notes/handlers"
)

// SetupRouter initializes routes
func SetupRouter() *mux.Router {
	r := mux.NewRouter()
	r.HandleFunc("/notes", handlers.GetNotes).Methods("GET")
	r.HandleFunc("/notes", handlers.CreateNote).Methods("POST")
	r.HandleFunc("/notes/{id}", handlers.GetNoteByID).Methods("GET")
	r.HandleFunc("/notes/{id}", handlers.UpdateNote).Methods("PUT")
	r.HandleFunc("/notes/{id}", handlers.DeleteNote).Methods("DELETE")
	return r
}
```

#### 9. Membuat `main.go`

Sekarang buat file `main.go` untuk mengatur server HTTP dan koneksi database.

```go
// main.go
package main

import (
	"fmt"
	"log"
	"net/http"
	"golang-rest-api-notes/models"
	"golang-rest-api-notes/routes"
)

func main() {
	// Initialize DB
	models.InitializeDB()

	// Setup routes
	r := routes.SetupRouter()

	// Start server
	fmt.Println("Starting server on :8000...")
	log.Fatal(http.ListenAndServe(":8000", r))
}
```

#### 10. Menjalankan Server

Untuk menjalankan server API:

```bash
go run main.go
```

Server akan berjalan di `http://localhost:8000`. Anda bisa mengakses API menggunakan tools seperti Postman atau curl.

#### 11. Endpoint API

Berikut adalah daftar endpoint yang telah dibuat:

1. **GET /notes** – Mengambil semua catatan.
2. **POST /notes** – Membuat catatan baru.
3. **GET /notes/{id}** – Mengambil catatan berdasarkan ID.
4. **PUT /notes/{id}** – Memperbarui catatan berdasarkan ID.
5. **DELETE /notes/{id}** – Menghapus catatan berdasarkan ID.

#### 12. Testing API

*   **POST Request** untuk membuat catatan:

    * URL: `http://localhost:8000/notes`
    * Body (JSON):

    ```json
    {
      "title": "Catatan Pertama",
      "content": "Ini adalah konten catatan pertama."
    }
    ```
* **GET Request** untuk mendapatkan semua catatan:
  * URL: `http://localhost:8000/notes`
* **GET Request** untuk mendapatkan catatan dengan ID tertentu:
  * URL: `http://localhost:8000/notes/1`
*   **PUT Request** untuk memperbarui catatan dengan ID tertentu:

    * URL: `http://localhost:8000/notes/1`
    * Body (JSON):

    ```json
    {
      "title": "Catatan Terbaru",
      "content": "Konten telah diperbarui."
    }
    ```
* **DELETE Request** untuk menghapus catatan dengan ID tertentu:
  * URL: `http://localhost:8000/notes/1`

#### 13. Kesimpulan

Dengan mengikuti langkah-langkah ini, Anda telah berhasil membangun RESTful API sederhana menggunakan Golang untuk mengelola catatan. Anda bisa mengembangkan lebih lanjut dengan menambahkan fitur-fitur lain seperti autentikasi, validasi input, atau menggunakan database lain seperti PostgreSQL.

Semoga ini membantu! Jika ada pertanyaan lebih lanjut, jangan ragu untuk bertanya!
