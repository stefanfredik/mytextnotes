# Notes Restful API

## Tutorial Lengkap: RESTful API Notes dengan Go

Tutorial ini akan memandu Anda membangun RESTful API yang robust untuk mengelola catatan menggunakan Go (Golang) dengan best practices yang tepat.

### 1. Persiapan dan Prasyarat

#### Instalasi Go

Pastikan Go versi 1.19 atau lebih baru sudah terinstal:

```bash
go version
```

Jika belum terinstal, unduh dari [situs resmi Go](https://golang.org/dl/).

#### Membuat Struktur Proyek

```bash
mkdir golang-notes-api
cd golang-notes-api
```

### 2. Inisialisasi Proyek

```bash
go mod init golang-notes-api
```

### 3. Instalasi Dependensi

Kita akan menggunakan dependensi modern dan aktif:

```bash
# Router HTTP
go get github.com/gorilla/mux

# ORM Database (GORM v2) dengan MySQL driver
go get gorm.io/gorm
go get gorm.io/driver/mysql

# Validasi input
go get github.com/go-playground/validator/v10

# Logging
go get github.com/sirupsen/logrus

# CORS middleware
go get github.com/gorilla/handlers

# Environment variables
go get github.com/joho/godotenv
```

### 4. Struktur Proyek yang Disarankan

```
golang-notes-api/
├── cmd/
│   └── server/
│       └── main.go
├── internal/
│   ├── config/
│   │   └── config.go
│   ├── database/
│   │   └── database.go
│   ├── handlers/
│   │   └── note_handler.go
│   ├── models/
│   │   └── note.go
│   ├── middleware/
│   │   └── middleware.go
│   └── routes/
│       └── routes.go
├── pkg/
│   └── utils/
│       └── response.go
├── .env
├── .gitignore
├── docker-compose.yml
├── Dockerfile
├── go.mod
└── go.sum
```

### 5. Konfigurasi Aplikasi

#### .env

```env
APP_PORT=8080
APP_ENV=development

# MySQL Database Configuration
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=notes_db
DB_CHARSET=utf8mb4
DB_PARSE_TIME=true
DB_LOC=Local

# Connection Pool Settings
DB_MAX_IDLE_CONNS=10
DB_MAX_OPEN_CONNS=100
DB_CONN_MAX_LIFETIME=3600

LOG_LEVEL=info
```

#### internal/config/config.go

```go
package config

import (
    "os"
    "strconv"
    "github.com/joho/godotenv"
)

type Config struct {
    Port     string
    LogLevel string
    Env      string
    Database DatabaseConfig
}

type DatabaseConfig struct {
    Host            string
    Port            string
    User            string
    Password        string
    Name            string
    Charset         string
    ParseTime       bool
    Loc             string
    MaxIdleConns    int
    MaxOpenConns    int
    ConnMaxLifetime int
}

func Load() (*Config, error) {
    // Load .env file
    if err := godotenv.Load(); err != nil {
        // It's okay if .env doesn't exist in production
    }

    return &Config{
        Port:     getEnv("APP_PORT", "8080"),
        LogLevel: getEnv("LOG_LEVEL", "info"),
        Env:      getEnv("APP_ENV", "development"),
        Database: DatabaseConfig{
            Host:            getEnv("DB_HOST", "localhost"),
            Port:            getEnv("DB_PORT", "3306"),
            User:            getEnv("DB_USER", "root"),
            Password:        getEnv("DB_PASSWORD", ""),
            Name:            getEnv("DB_NAME", "notes_db"),
            Charset:         getEnv("DB_CHARSET", "utf8mb4"),
            ParseTime:       getEnvBool("DB_PARSE_TIME", true),
            Loc:             getEnv("DB_LOC", "Local"),
            MaxIdleConns:    getEnvInt("DB_MAX_IDLE_CONNS", 10),
            MaxOpenConns:    getEnvInt("DB_MAX_OPEN_CONNS", 100),
            ConnMaxLifetime: getEnvInt("DB_CONN_MAX_LIFETIME", 3600),
        },
    }, nil
}

func getEnv(key, defaultValue string) string {
    if value := os.Getenv(key); value != "" {
        return value
    }
    return defaultValue
}

func getEnvBool(key string, defaultValue bool) bool {
    if value := os.Getenv(key); value != "" {
        if parsed, err := strconv.ParseBool(value); err == nil {
            return parsed
        }
    }
    return defaultValue
}

func getEnvInt(key string, defaultValue int) int {
    if value := os.Getenv(key); value != "" {
        if parsed, err := strconv.Atoi(value); err == nil {
            return parsed
        }
    }
    return defaultValue
}
```

### 6. Model Database yang Diperbaiki

#### internal/models/note.go

```go
package models

import (
    "time"
    "gorm.io/gorm"
)

// Note represents a note entity
type Note struct {
    ID        uint           `json:"id" gorm:"primaryKey"`
    Title     string         `json:"title" validate:"required,min=1,max=200" gorm:"not null"`
    Content   string         `json:"content" validate:"required,min=1" gorm:"type:text"`
    CreatedAt time.Time      `json:"created_at"`
    UpdatedAt time.Time      `json:"updated_at"`
    DeletedAt gorm.DeletedAt `json:"-" gorm:"index"`
}

// CreateNoteRequest represents the request payload for creating a note
type CreateNoteRequest struct {
    Title   string `json:"title" validate:"required,min=1,max=200"`
    Content string `json:"content" validate:"required,min=1"`
}

// UpdateNoteRequest represents the request payload for updating a note
type UpdateNoteRequest struct {
    Title   string `json:"title" validate:"omitempty,min=1,max=200"`
    Content string `json:"content" validate:"omitempty,min=1"`
}

// NoteResponse represents the response format for a note
type NoteResponse struct {
    ID        uint      `json:"id"`
    Title     string    `json:"title"`
    Content   string    `json:"content"`
    CreatedAt time.Time `json:"created_at"`
    UpdatedAt time.Time `json:"updated_at"`
}

// ToResponse converts Note to NoteResponse
func (n *Note) ToResponse() NoteResponse {
    return NoteResponse{
        ID:        n.ID,
        Title:     n.Title,
        Content:   n.Content,
        CreatedAt: n.CreatedAt,
        UpdatedAt: n.UpdatedAt,
    }
}
```

### 7. Database Connection yang Robust

#### internal/database/database.go

```go
package database

import (
    "fmt"
    "time"
    "golang-notes-api/internal/config"
    "golang-notes-api/internal/models"
    
    "gorm.io/driver/mysql"
    "gorm.io/gorm"
    "gorm.io/gorm/logger"
)

var DB *gorm.DB

// Initialize sets up MySQL database connection
func Initialize(cfg *config.Config) error {
    var err error
    
    // Configure GORM logger level
    logLevel := logger.Silent
    if cfg.Env == "development" {
        logLevel = logger.Info
    }
    
    // Create MySQL DSN (Data Source Name)
    dsn := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s?charset=%s&parseTime=%t&loc=%s",
        cfg.Database.User,
        cfg.Database.Password,
        cfg.Database.Host,
        cfg.Database.Port,
        cfg.Database.Name,
        cfg.Database.Charset,
        cfg.Database.ParseTime,
        cfg.Database.Loc,
    )
    
    // Open database connection
    DB, err = gorm.Open(mysql.Open(dsn), &gorm.Config{
        Logger: logger.Default.LogMode(logLevel),
        NowFunc: func() time.Time {
            return time.Now().Local()
        },
    })
    if err != nil {
        return fmt.Errorf("failed to connect to MySQL database: %w", err)
    }
    
    // Get underlying sql.DB
    sqlDB, err := DB.DB()
    if err != nil {
        return fmt.Errorf("failed to get underlying sql.DB: %w", err)
    }
    
    // Configure connection pool
    sqlDB.SetMaxIdleConns(cfg.Database.MaxIdleConns)
    sqlDB.SetMaxOpenConns(cfg.Database.MaxOpenConns)
    sqlDB.SetConnMaxLifetime(time.Duration(cfg.Database.ConnMaxLifetime) * time.Second)
    
    // Test the connection
    if err := sqlDB.Ping(); err != nil {
        return fmt.Errorf("failed to ping database: %w", err)
    }
    
    // Auto migrate schemas
    if err := DB.AutoMigrate(&models.Note{}); err != nil {
        return fmt.Errorf("failed to migrate database: %w", err)
    }
    
    return nil
}

// Close closes database connection
func Close() error {
    if DB != nil {
        sqlDB, err := DB.DB()
        if err != nil {
            return err
        }
        return sqlDB.Close()
    }
    return nil
}

// Health check for database connection
func HealthCheck() error {
    if DB == nil {
        return fmt.Errorf("database connection is nil")
    }
    
    sqlDB, err := DB.DB()
    if err != nil {
        return fmt.Errorf("failed to get underlying sql.DB: %w", err)
    }
    
    if err := sqlDB.Ping(); err != nil {
        return fmt.Errorf("database ping failed: %w", err)
    }
    
    return nil
}
```

### 8. Utility untuk Response

#### pkg/utils/response.go

```go
package utils

import (
    "encoding/json"
    "net/http"
)

// ErrorResponse represents an error response
type ErrorResponse struct {
    Error   string `json:"error"`
    Message string `json:"message,omitempty"`
}

// SuccessResponse represents a success response
type SuccessResponse struct {
    Data    interface{} `json:"data,omitempty"`
    Message string      `json:"message,omitempty"`
}

// WriteJSON writes JSON response
func WriteJSON(w http.ResponseWriter, status int, data interface{}) {
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(status)
    json.NewEncoder(w).Encode(data)
}

// WriteError writes error response
func WriteError(w http.ResponseWriter, status int, message string) {
    WriteJSON(w, status, ErrorResponse{
        Error:   http.StatusText(status),
        Message: message,
    })
}

// WriteSuccess writes success response
func WriteSuccess(w http.ResponseWriter, data interface{}, message string) {
    WriteJSON(w, http.StatusOK, SuccessResponse{
        Data:    data,
        Message: message,
    })
}
```

### 9. Middleware yang Diperlukan

#### internal/middleware/middleware.go

```go
package middleware

import (
    "net/http"
    "time"
    
    "github.com/gorilla/handlers"
    "github.com/sirupsen/logrus"
)

// Logger middleware for request logging
func Logger() func(http.Handler) http.Handler {
    return handlers.LoggingHandler(logrus.StandardLogger().Writer(), nil)
}

// CORS middleware
func CORS() func(http.Handler) http.Handler {
    return func(next http.Handler) http.Handler {
        return handlers.CORS(
            handlers.AllowedOrigins([]string{"*"}),
            handlers.AllowedMethods([]string{"GET", "POST", "PUT", "DELETE", "OPTIONS"}),
            handlers.AllowedHeaders([]string{"Content-Type", "Authorization"}),
        )(next)
    }
}

// Recovery middleware for panic recovery
func Recovery() func(http.Handler) http.Handler {
    return func(next http.Handler) http.Handler {
        return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            defer func() {
                if err := recover(); err != nil {
                    logrus.Errorf("Panic recovered: %v", err)
                    http.Error(w, "Internal Server Error", http.StatusInternalServerError)
                }
            }()
            next.ServeHTTP(w, r)
        })
    }
}

// Timeout middleware
func Timeout(timeout time.Duration) func(http.Handler) http.Handler {
    return func(next http.Handler) http.Handler {
        return http.TimeoutHandler(next, timeout, "Request timeout")
    }
}
```

### 10. Handler yang Diperbaiki dan Lengkap

#### internal/handlers/note\_handler.go

```go
package handlers

import (
    "encoding/json"
    "net/http"
    "strconv"
    
    "golang-notes-api/internal/database"
    "golang-notes-api/internal/models"
    "golang-notes-api/pkg/utils"
    
    "github.com/go-playground/validator/v10"
    "github.com/gorilla/mux"
    "github.com/sirupsen/logrus"
    "gorm.io/gorm"
)

var validate = validator.New()

// GetNotes retrieves all notes with pagination
func GetNotes(w http.ResponseWriter, r *http.Request) {
    // Parse query parameters for pagination
    page, _ := strconv.Atoi(r.URL.Query().Get("page"))
    if page <= 0 {
        page = 1
    }
    
    limit, _ := strconv.Atoi(r.URL.Query().Get("limit"))
    if limit <= 0 || limit > 100 {
        limit = 10
    }
    
    offset := (page - 1) * limit
    
    var notes []models.Note
    var total int64
    
    // Count total records
    if err := database.DB.Model(&models.Note{}).Count(&total).Error; err != nil {
        logrus.Errorf("Failed to count notes: %v", err)
        utils.WriteError(w, http.StatusInternalServerError, "Failed to retrieve notes")
        return
    }
    
    // Retrieve notes with pagination
    if err := database.DB.Offset(offset).Limit(limit).Find(&notes).Error; err != nil {
        logrus.Errorf("Failed to retrieve notes: %v", err)
        utils.WriteError(w, http.StatusInternalServerError, "Failed to retrieve notes")
        return
    }
    
    // Convert to response format
    var responses []models.NoteResponse
    for _, note := range notes {
        responses = append(responses, note.ToResponse())
    }
    
    // Prepare paginated response
    response := map[string]interface{}{
        "notes": responses,
        "pagination": map[string]interface{}{
            "page":       page,
            "limit":      limit,
            "total":      total,
            "total_pages": (total + int64(limit) - 1) / int64(limit),
        },
    }
    
    utils.WriteJSON(w, http.StatusOK, response)
}

// CreateNote creates a new note
func CreateNote(w http.ResponseWriter, r *http.Request) {
    var req models.CreateNoteRequest
    
    // Parse JSON request
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        utils.WriteError(w, http.StatusBadRequest, "Invalid JSON format")
        return
    }
    
    // Validate request
    if err := validate.Struct(req); err != nil {
        utils.WriteError(w, http.StatusBadRequest, "Validation failed: "+err.Error())
        return
    }
    
    // Create note
    note := models.Note{
        Title:   req.Title,
        Content: req.Content,
    }
    
    if err := database.DB.Create(&note).Error; err != nil {
        logrus.Errorf("Failed to create note: %v", err)
        utils.WriteError(w, http.StatusInternalServerError, "Failed to create note")
        return
    }
    
    utils.WriteJSON(w, http.StatusCreated, note.ToResponse())
}

// GetNoteByID retrieves a note by ID
func GetNoteByID(w http.ResponseWriter, r *http.Request) {
    vars := mux.Vars(r)
    id, err := strconv.ParseUint(vars["id"], 10, 32)
    if err != nil {
        utils.WriteError(w, http.StatusBadRequest, "Invalid note ID")
        return
    }
    
    var note models.Note
    if err := database.DB.First(&note, uint(id)).Error; err != nil {
        if err == gorm.ErrRecordNotFound {
            utils.WriteError(w, http.StatusNotFound, "Note not found")
            return
        }
        logrus.Errorf("Failed to retrieve note: %v", err)
        utils.WriteError(w, http.StatusInternalServerError, "Failed to retrieve note")
        return
    }
    
    utils.WriteJSON(w, http.StatusOK, note.ToResponse())
}

// UpdateNote updates an existing note
func UpdateNote(w http.ResponseWriter, r *http.Request) {
    vars := mux.Vars(r)
    id, err := strconv.ParseUint(vars["id"], 10, 32)
    if err != nil {
        utils.WriteError(w, http.StatusBadRequest, "Invalid note ID")
        return
    }
    
    var req models.UpdateNoteRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        utils.WriteError(w, http.StatusBadRequest, "Invalid JSON format")
        return
    }
    
    // Validate request
    if err := validate.Struct(req); err != nil {
        utils.WriteError(w, http.StatusBadRequest, "Validation failed: "+err.Error())
        return
    }
    
    // Find existing note
    var note models.Note
    if err := database.DB.First(&note, uint(id)).Error; err != nil {
        if err == gorm.ErrRecordNotFound {
            utils.WriteError(w, http.StatusNotFound, "Note not found")
            return
        }
        logrus.Errorf("Failed to find note: %v", err)
        utils.WriteError(w, http.StatusInternalServerError, "Failed to update note")
        return
    }
    
    // Update fields if provided
    if req.Title != "" {
        note.Title = req.Title
    }
    if req.Content != "" {
        note.Content = req.Content
    }
    
    if err := database.DB.Save(&note).Error; err != nil {
        logrus.Errorf("Failed to update note: %v", err)
        utils.WriteError(w, http.StatusInternalServerError, "Failed to update note")
        return
    }
    
    utils.WriteJSON(w, http.StatusOK, note.ToResponse())
}

// DeleteNote deletes a note (soft delete)
func DeleteNote(w http.ResponseWriter, r *http.Request) {
    vars := mux.Vars(r)
    id, err := strconv.ParseUint(vars["id"], 10, 32)
    if err != nil {
        utils.WriteError(w, http.StatusBadRequest, "Invalid note ID")
        return
    }
    
    var note models.Note
    if err := database.DB.First(&note, uint(id)).Error; err != nil {
        if err == gorm.ErrRecordNotFound {
            utils.WriteError(w, http.StatusNotFound, "Note not found")
            return
        }
        logrus.Errorf("Failed to find note: %v", err)
        utils.WriteError(w, http.StatusInternalServerError, "Failed to delete note")
        return
    }
    
    if err := database.DB.Delete(&note).Error; err != nil {
        logrus.Errorf("Failed to delete note: %v", err)
        utils.WriteError(w, http.StatusInternalServerError, "Failed to delete note")
        return
    }
    
    utils.WriteJSON(w, http.StatusOK, map[string]string{
        "message": "Note deleted successfully",
    })
}

// SearchNotes searches notes by title or content
func SearchNotes(w http.ResponseWriter, r *http.Request) {
    query := r.URL.Query().Get("q")
    if query == "" {
        utils.WriteError(w, http.StatusBadRequest, "Search query is required")
        return
    }
    
    var notes []models.Note
    searchPattern := "%" + query + "%"
    
    if err := database.DB.Where("title LIKE ? OR content LIKE ?", searchPattern, searchPattern).Find(&notes).Error; err != nil {
        logrus.Errorf("Failed to search notes: %v", err)
        utils.WriteError(w, http.StatusInternalServerError, "Failed to search notes")
        return
    }
    
    var responses []models.NoteResponse
    for _, note := range notes {
        responses = append(responses, note.ToResponse())
    }
    
    utils.WriteJSON(w, http.StatusOK, map[string]interface{}{
        "notes": responses,
        "query": query,
        "count": len(responses),
    })
}
```

### 11. Routing yang Diperbaiki

#### internal/routes/routes.go

```go
package routes

import (
    "net/http"
    "time"
    
    "golang-notes-api/internal/handlers"
    "golang-notes-api/internal/middleware"
    
    "github.com/gorilla/mux"
)

// SetupRouter initializes and configures routes
func SetupRouter() *mux.Router {
    r := mux.NewRouter()
    
    // Apply global middleware
    r.Use(middleware.Recovery())
    r.Use(middleware.Logger())
    r.Use(middleware.CORS())
    r.Use(middleware.Timeout(30 * time.Second))
    
    // API routes
    api := r.PathPrefix("/api/v1").Subrouter()
    
    // Notes routes
    api.HandleFunc("/notes", handlers.GetNotes).Methods("GET")
    api.HandleFunc("/notes", handlers.CreateNote).Methods("POST")
    api.HandleFunc("/notes/search", handlers.SearchNotes).Methods("GET")
    api.HandleFunc("/notes/{id:[0-9]+}", handlers.GetNoteByID).Methods("GET")
    api.HandleFunc("/notes/{id:[0-9]+}", handlers.UpdateNote).Methods("PUT")
    api.HandleFunc("/notes/{id:[0-9]+}", handlers.DeleteNote).Methods("DELETE")
    
    // Health check endpoint
    api.HandleFunc("/health", healthCheck).Methods("GET")
    
    return r
}

func healthCheck(w http.ResponseWriter, r *http.Request) {
    // Check database health
    if err := database.HealthCheck(); err != nil {
        w.Header().Set("Content-Type", "application/json")
        w.WriteHeader(http.StatusServiceUnavailable)
        w.Write([]byte(`{"status":"error","message":"Database connection failed"}`))
        return
    }
    
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusOK)
    w.Write([]byte(`{"status":"ok","message":"Server is running","database":"connected"}`))
}
```

### 12. Main Application

#### cmd/server/main.go

```go
package main

import (
    "context"
    "net/http"
    "os"
    "os/signal"
    "syscall"
    "time"
    
    "golang-notes-api/internal/config"
    "golang-notes-api/internal/database"
    "golang-notes-api/internal/routes"
    
    "github.com/sirupsen/logrus"
)

func main() {
    // Load configuration
    cfg, err := config.Load()
    if err != nil {
        logrus.Fatalf("Failed to load config: %v", err)
    }
    
    // Configure logging
    setupLogging(cfg)
    
    // Initialize database
    if err := database.Initialize(cfg); err != nil {
        logrus.Fatalf("Failed to initialize database: %v", err)
    }
    defer database.Close()
    
    // Setup routes
    router := routes.SetupRouter()
    
    // Create HTTP server
    server := &http.Server{
        Addr:         ":" + cfg.Port,
        Handler:      router,
        ReadTimeout:  15 * time.Second,
        WriteTimeout: 15 * time.Second,
        IdleTimeout:  60 * time.Second,
    }
    
    // Start server in a goroutine
    go func() {
        logrus.Infof("Starting server on port %s", cfg.Port)
        if err := server.ListenAndServe(); err != nil && err != http.ErrServerClosed {
            logrus.Fatalf("Server failed to start: %v", err)
        }
    }()
    
    // Wait for interrupt signal to gracefully shutdown
    quit := make(chan os.Signal, 1)
    signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
    <-quit
    
    logrus.Info("Shutting down server...")
    
    // Graceful shutdown with timeout
    ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
    defer cancel()
    
    if err := server.Shutdown(ctx); err != nil {
        logrus.Errorf("Server forced to shutdown: %v", err)
    }
    
    logrus.Info("Server exiting")
}

func setupLogging(cfg *config.Config) {
    // Set log level
    level, err := logrus.ParseLevel(cfg.LogLevel)
    if err != nil {
        level = logrus.InfoLevel
    }
    logrus.SetLevel(level)
    
    // Set formatter
    if cfg.Env == "production" {
        logrus.SetFormatter(&logrus.JSONFormatter{})
    } else {
        logrus.SetFormatter(&logrus.TextFormatter{
            FullTimestamp: true,
        })
    }
}
```

### 13. File Konfigurasi Tambahan

#### .gitignore

```
# Binaries
*.exe
*.exe~
*.dll
*.so
*.dylib
*.test

# Output files
*.out

# Go workspace files
go.work

# Environment files
.env
.env.local

# Database files
*.db
*.sqlite
*.sqlite3

# Logs
*.log

# IDE files
.vscode/
.idea/
*.swp
*.swo

# OS files
.DS_Store
Thumbs.db

# MySQL dumps
*.sql
```

#### Dockerfile

```dockerfile
# Build stage
FROM golang:1.21-alpine AS builder

WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main ./cmd/server

# Run stage
FROM alpine:latest

RUN apk --no-cache add ca-certificates tzdata
WORKDIR /root/

COPY --from=builder /app/main .
COPY --from=builder /app/.env .

EXPOSE 8080

CMD ["./main"]
```

#### docker-compose.yml

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    container_name: notes_mysql
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: notes_db
      MYSQL_USER: notes_user
      MYSQL_PASSWORD: notes_password
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql
      - ./scripts/init.sql:/docker-entrypoint-initdb.d/init.sql
    command: --default-authentication-plugin=mysql_native_password
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      timeout: 20s
      retries: 10

  notes-api:
    build: .
    container_name: notes_api
    restart: unless-stopped
    ports:
      - "8080:8080"
    environment:
      - APP_PORT=8080
      - DB_HOST=mysql
      - DB_PORT=3306
      - DB_USER=notes_user
      - DB_PASSWORD=notes_password
      - DB_NAME=notes_db
    depends_on:
      mysql:
        condition: service_healthy
    volumes:
      - ./logs:/root/logs

volumes:
  mysql_data:
```

### 14. Setup MySQL Database

#### Instalasi MySQL

**Ubuntu/Debian:**

```bash
sudo apt update
sudo apt install mysql-server
sudo mysql_secure_installation
```

**macOS (menggunakan Homebrew):**

```bash
brew install mysql
brew services start mysql
```

**Windows:** Download MySQL installer dari [mysql.com](https://dev.mysql.com/downloads/installer/)

#### Membuat Database dan User

Login ke MySQL sebagai root:

```bash
mysql -u root -p
```

Buat database dan user:

```sql
-- Membuat database
CREATE DATABASE notes_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Membuat user dan memberikan privileges
CREATE USER 'notes_user'@'localhost' IDENTIFIED BY 'notes_password';
GRANT ALL PRIVILEGES ON notes_db.* TO 'notes_user'@'localhost';

-- Untuk Docker (user dari any host)
CREATE USER 'notes_user'@'%' IDENTIFIED BY 'notes_password';
GRANT ALL PRIVILEGES ON notes_db.* TO 'notes_user'@'%';

FLUSH PRIVILEGES;
EXIT;
```

#### Script Inisialisasi Database (Opsional)

Buat file `scripts/init.sql`:

```sql
-- scripts/init.sql
USE notes_db;

-- Membuat table notes (akan dibuat otomatis oleh GORM, ini hanya contoh)
-- CREATE TABLE IF NOT EXISTS notes (
--     id bigint unsigned NOT NULL AUTO_INCREMENT,
--     title varchar(200) NOT NULL,
--     content text NOT NULL,
--     created_at datetime(3) NULL,
--     updated_at datetime(3) NULL,
--     deleted_at datetime(3) NULL,
--     PRIMARY KEY (id),
--     KEY idx_notes_deleted_at (deleted_at)
-- ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Insert sample data (opsional)
-- INSERT INTO notes (title, content, created_at, updated_at) VALUES 
-- ('Welcome Note', 'Welcome to Notes API with MySQL!', NOW(), NOW()),
-- ('Sample Note', 'This is a sample note to test the API.', NOW(), NOW());
```

### 15. Testing API

#### Menjalankan dengan MySQL Lokal

1. **Setup MySQL database** (lihat section 14)
2. **Update file .env** dengan kredensial MySQL Anda
3. **Jalankan server:**

```bash
go run cmd/server/main.go
```

#### Menjalankan dengan Docker Compose

```bash
# Jalankan MySQL dan API sekaligus
docker-compose up -d

# Melihat logs
docker-compose logs -f notes-api

# Menghentikan services
docker-compose down
```

#### Testing dengan cURL

**Health Check:**

```bash
curl http://localhost:8080/api/v1/health
```

**Membuat Note:**

```bash
curl -X POST http://localhost:8080/api/v1/notes \
  -H "Content-Type: application/json" \
  -d '{
    "title": "My First Note",
    "content": "This is the content of my first note."
  }'
```

**Mendapatkan Semua Notes:**

```bash
curl http://localhost:8080/api/v1/notes
```

**Mendapatkan Note dengan Pagination:**

```bash
curl "http://localhost:8080/api/v1/notes?page=1&limit=5"
```

**Mendapatkan Note by ID:**

```bash
curl http://localhost:8080/api/v1/notes/1
```

**Update Note:**

```bash
curl -X PUT http://localhost:8080/api/v1/notes/1 \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Updated Note Title",
    "content": "Updated content."
  }'
```

**Search Notes:**

```bash
curl "http://localhost:8080/api/v1/notes/search?q=first"
```

**Delete Note:**

```bash
curl -X DELETE http://localhost:8080/api/v1/notes/1
```

### 16. Troubleshooting MySQL Connection

#### Error: "Access denied for user"

```bash
# Reset MySQL root password
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new_password';
FLUSH PRIVILEGES;
EXIT;
```

#### Error: "Connection refused"

```bash
# Cek status MySQL service
sudo systemctl status mysql
sudo systemctl start mysql

# Atau untuk macOS
brew services list | grep mysql
brew services start mysql
```

#### Error: "Unknown database"

Pastikan database `notes_db` sudah dibuat:

```sql
SHOW DATABASES;
CREATE DATABASE IF NOT EXISTS notes_db;
```

#### Connection Pool Issues

Jika mengalami "too many connections", sesuaikan nilai di `.env`:

```env
DB_MAX_IDLE_CONNS=5
DB_MAX_OPEN_CONNS=50
```

### 17. Fitur Tambahan dan Improvement

#### Rate Limiting

Anda dapat menambahkan rate limiting menggunakan `golang.org/x/time/rate`:

```go
// internal/middleware/ratelimit.go
package middleware

import (
    "net/http"
    "golang.org/x/time/rate"
)

func RateLimit(r rate.Limit, b int) func(http.Handler) http.Handler {
    limiter := rate.NewLimiter(r, b)
    
    return func(next http.Handler) http.Handler {
        return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            if !limiter.Allow() {
                http.Error(w, "Too Many Requests", http.StatusTooManyRequests)
                return
            }
            next.ServeHTTP(w, r)
        })
    }
}
```

#### Authentication (JWT)

Untuk menambahkan autentikasi, Anda bisa menggunakan JWT:

```bash
go get github.com/golang-jwt/jwt/v4
```

### 18. Best Practices yang Diterapkan

1. **Proper Project Structure**: Menggunakan struktur proyek standar Go
2. **Configuration Management**: Menggunakan environment variables
3. **Error Handling**: Proper error handling dan logging
4. **Validation**: Input validation menggunakan validator
5. **Middleware**: CORS, logging, recovery, timeout middleware
6. **Graceful Shutdown**: Proper server shutdown
7. **Pagination**: Support untuk pagination pada list endpoint
8. **Search Functionality**: Fitur pencarian notes
9. **MySQL Optimization**: Connection pooling dan proper indexing
10. **Database Health Check**: Health check endpoint yang memeriksa koneksi database
11. **Response Consistency**: Format response yang konsisten
12. **HTTP Status Codes**: Menggunakan HTTP status codes yang tepat
13. **Security**: Basic security practices

### 19. Deployment

#### Menggunakan Docker Compose (Recommended)

```bash
# Production deployment
docker-compose -f docker-compose.prod.yml up -d
```

**docker-compose.prod.yml:**

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/mysql_root_password
      MYSQL_DATABASE: notes_db
      MYSQL_USER: notes_user
      MYSQL_PASSWORD_FILE: /run/secrets/mysql_password
    volumes:
      - mysql_data:/var/lib/mysql
    secrets:
      - mysql_root_password
      - mysql_password
    command: --default-authentication-plugin=mysql_native_password

  notes-api:
    build: .
    restart: always
    ports:
      - "8080:8080"
    environment:
      - APP_ENV=production
      - DB_HOST=mysql
      - DB_USER=notes_user
    depends_on:
      - mysql
    secrets:
      - mysql_password

secrets:
  mysql_root_password:
    file: ./secrets/mysql_root_password.txt
  mysql_password:
    file: ./secrets/mysql_password.txt

volumes:
  mysql_data:
```

### 20. Monitoring dan Logging

Server ini sudah dilengkapi dengan:

* Structured logging menggunakan logrus
* Request logging middleware
* Error tracking
* Health check endpoint dengan database connection check
* MySQL connection monitoring

### 21. Kesimpulan

Tutorial ini memberikan implementasi lengkap RESTful API untuk mengelola catatan dengan fitur:

* ✅ CRUD operations lengkap
* ✅ Pagination dan search
* ✅ Input validation
* ✅ Error handling yang robust
* ✅ Middleware untuk logging, CORS, recovery
* ✅ Graceful shutdown
* ✅ MySQL database integration dengan connection pooling
* ✅ Database health monitoring
* ✅ Docker Compose setup dengan MySQL
* ✅ Production-ready MySQL configuration
* ✅ Best practices Go

API ini siap untuk production dengan beberapa tambahan seperti authentication, rate limiting, dan monitoring yang lebih advanced sesuai kebutuhan.

Untuk pengembangan lebih lanjut, Anda dapat menambahkan fitur seperti:

* Authentication dan authorization
* Rate limiting
* Caching dengan Redis
* Database replication setup
* Unit dan integration testing
* API documentation dengan Swagger
* Database backup strategies
