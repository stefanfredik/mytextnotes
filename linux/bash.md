---
icon: square-terminal
---

# Bash Scripting

## Modul Lengkap Bash Scripting - Dasar hingga Mahir

### Daftar Isi

1. [Pengenalan Bash](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#1-pengenalan-bash)
2. [Dasar-dasar Bash Scripting](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#2-dasar-dasar-bash-scripting)
3. [Variabel dan Parameter](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#3-variabel-dan-parameter)
4. [Input dan Output](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#4-input-dan-output)
5. [Kondisional dan Perulangan](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#5-kondisional-dan-perulangan)
6. [Fungsi](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#6-fungsi)
7. [Array](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#7-array)
8. [Manipulasi String](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#8-manipulasi-string)
9. [File Operations](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#9-file-operations)
10. [Advanced Bash Features](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#10-advanced-bash-features)
11. [Error Handling dan Debugging](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#11-error-handling-dan-debugging)
12. [Best Practices](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#12-best-practices)
13. [Proyek Praktis](https://claude.ai/chat/0dfa7b2d-9f34-4998-a1d0-cf6d975afc34#13-proyek-praktis)

***

### 1. Pengenalan Bash

#### Apa itu Bash?

Bash (Bourne Again Shell) adalah command-line interpreter dan scripting language yang berjalan di sistem Unix/Linux. Bash script memungkinkan otomatisasi tugas-tugas sistem operasi.

#### Keunggulan Bash Scripting:

* Otomatisasi tugas repetitif
* Manajemen sistem
* Pemrosesan file batch
* Integrasi dengan tools Linux/Unix
* Portabilitas antar sistem Unix-like

#### Cara Memulai:

```bash
# Cek versi bash
bash --version

# Membuat file script pertama
touch hello.sh
chmod +x hello.sh
```

***

### 2. Dasar-dasar Bash Scripting

#### Struktur Dasar Script

```bash
#!/bin/bash
# Ini adalah komentar
# Script pertama saya

echo "Hello, World!"
```

#### Shebang (#!/bin/bash)

* Menentukan interpreter yang digunakan
* Harus berada di baris pertama
* Alternatif: `#!/usr/bin/env bash`

#### Menjalankan Script

```bash
# Metode 1: Dengan permission execute
chmod +x script.sh
./script.sh

# Metode 2: Langsung dengan bash
bash script.sh

# Metode 3: Source (menjalankan dalam shell saat ini)
source script.sh
# atau
. script.sh
```

#### Contoh Script Dasar

```bash
#!/bin/bash

# Script informasi sistem
echo "=== Informasi Sistem ==="
echo "Hostname: $(hostname)"
echo "User: $(whoami)"
echo "Date: $(date)"
echo "Uptime: $(uptime)"
```

***

### 3. Variabel dan Parameter

#### Deklarasi Variabel

```bash
#!/bin/bash

# Variabel string
nama="John Doe"
umur=25

# Variabel sistem
current_user=$(whoami)
today=$(date +%Y-%m-%d)

# Konstanta (readonly)
readonly PI=3.14159

echo "Nama: $nama"
echo "Umur: $umur"
echo "User: $current_user"
echo "Tanggal: $today"
```

#### Environment Variables

```bash
#!/bin/bash

# Menampilkan environment variables
echo "HOME: $HOME"
echo "PATH: $PATH"
echo "USER: $USER"
echo "SHELL: $SHELL"

# Membuat environment variable
export MY_VAR="Hello World"
echo "MY_VAR: $MY_VAR"
```

#### Parameter Script

```bash
#!/bin/bash

# Parameter posisi
echo "Nama script: $0"
echo "Parameter 1: $1"
echo "Parameter 2: $2"
echo "Semua parameter: $@"
echo "Jumlah parameter: $#"
echo "Process ID: $$"
echo "Exit status terakhir: $?"
```

#### Special Variables

```bash
#!/bin/bash

# $? - Exit status command terakhir
ls /nonexistent 2>/dev/null
echo "Exit status: $?"

# $$ - Process ID shell saat ini
echo "PID: $$"

# $! - Process ID background job terakhir
sleep 5 &
echo "Background PID: $!"
```

***

### 4. Input dan Output

#### Input dari User

```bash
#!/bin/bash

# Basic input
echo -n "Masukkan nama Anda: "
read nama
echo "Halo, $nama!"

# Input dengan prompt
read -p "Masukkan umur: " umur
echo "Anda berumur $umur tahun"

# Input password (tersembunyi)
read -s -p "Masukkan password: " password
echo
echo "Password tersimpan"

# Input dengan timeout
read -t 10 -p "Anda punya 10 detik untuk menjawab: " jawaban
echo "Jawaban Anda: $jawaban"
```

#### Output dan Redirection

```bash
#!/bin/bash

# Output dasar
echo "Pesan normal"
echo "Pesan error" >&2

# Redirection
echo "Ini akan disimpan ke file" > output.txt
echo "Ini akan ditambahkan" >> output.txt

# Mengarahkan error
ls /nonexistent 2> error.log

# Mengarahkan output dan error
command > output.log 2>&1

# Here document
cat << EOF
Ini adalah here document
Bisa multiline
Variabel: $USER
EOF
```

***

### 5. Kondisional dan Perulangan

#### If Statement

```bash
#!/bin/bash

# If sederhana
if [ $# -eq 0 ]; then
    echo "Tidak ada parameter"
fi

# If-else
read -p "Masukkan angka: " angka
if [ $angka -gt 10 ]; then
    echo "Angka lebih besar dari 10"
else
    echo "Angka kurang dari atau sama dengan 10"
fi

# If-elif-else
if [ $angka -lt 5 ]; then
    echo "Kecil"
elif [ $angka -lt 15 ]; then
    echo "Sedang"
else
    echo "Besar"
fi
```

#### Test Conditions

```bash
#!/bin/bash

file="test.txt"

# File tests
if [ -f "$file" ]; then
    echo "File exists"
fi

if [ -d "/home" ]; then
    echo "Directory exists"
fi

if [ -r "$file" ]; then
    echo "File is readable"
fi

# String tests
string1="hello"
string2="world"

if [ "$string1" = "$string2" ]; then
    echo "Strings are equal"
fi

if [ -z "$string1" ]; then
    echo "String is empty"
fi

if [ -n "$string1" ]; then
    echo "String is not empty"
fi

# Numeric tests
num1=10
num2=20

if [ $num1 -eq $num2 ]; then
    echo "Numbers are equal"
elif [ $num1 -lt $num2 ]; then
    echo "num1 is less than num2"
fi
```

#### Case Statement

```bash
#!/bin/bash

read -p "Masukkan pilihan (1-3): " pilihan

case $pilihan in
    1)
        echo "Anda memilih opsi 1"
        ;;
    2)
        echo "Anda memilih opsi 2"
        ;;
    3)
        echo "Anda memilih opsi 3"
        ;;
    *)
        echo "Pilihan tidak valid"
        ;;
esac

# Case dengan pattern
read -p "Masukkan file: " filename
case $filename in
    *.txt)
        echo "File teks"
        ;;
    *.jpg|*.png|*.gif)
        echo "File gambar"
        ;;
    *.sh)
        echo "Script bash"
        ;;
    *)
        echo "File tidak dikenal"
        ;;
esac
```

#### For Loop

```bash
#!/bin/bash

# For loop sederhana
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# For loop dengan range
for i in {1..10}; do
    echo "Count: $i"
done

# For loop dengan step
for i in {0..20..2}; do
    echo "Even number: $i"
done

# For loop dengan array
fruits=("apple" "banana" "orange")
for fruit in "${fruits[@]}"; do
    echo "Fruit: $fruit"
done

# For loop dengan files
for file in *.txt; do
    if [ -f "$file" ]; then
        echo "Processing: $file"
    fi
done

# C-style for loop
for (( i=1; i<=5; i++ )); do
    echo "Loop $i"
done
```

#### While Loop

```bash
#!/bin/bash

# While loop sederhana
counter=1
while [ $counter -le 5 ]; do
    echo "Counter: $counter"
    counter=$((counter + 1))
done

# While loop dengan kondisi
while true; do
    read -p "Masukkan 'quit' untuk keluar: " input
    if [ "$input" = "quit" ]; then
        break
    fi
    echo "Anda mengetik: $input"
done

# Reading file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < "input.txt"
```

#### Until Loop

```bash
#!/bin/bash

# Until loop
counter=1
until [ $counter -gt 5 ]; do
    echo "Counter: $counter"
    counter=$((counter + 1))
done
```

***

### 6. Fungsi

#### Deklarasi dan Pemanggilan Fungsi

```bash
#!/bin/bash

# Fungsi sederhana
greet() {
    echo "Hello, World!"
}

# Pemanggilan fungsi
greet

# Fungsi dengan parameter
greet_user() {
    local name=$1
    local age=$2
    echo "Hello, $name! You are $age years old."
}

greet_user "John" 25

# Fungsi dengan return value
add_numbers() {
    local num1=$1
    local num2=$2
    local result=$((num1 + num2))
    echo $result
}

result=$(add_numbers 5 3)
echo "5 + 3 = $result"
```

#### Variabel Lokal dan Global

```bash
#!/bin/bash

# Variabel global
global_var="I'm global"

test_scope() {
    local local_var="I'm local"
    global_var="Modified global"
    
    echo "Inside function:"
    echo "Local: $local_var"
    echo "Global: $global_var"
}

echo "Before function call:"
echo "Global: $global_var"

test_scope

echo "After function call:"
echo "Global: $global_var"
# echo "Local: $local_var"  # Ini akan error
```

#### Advanced Function Features

```bash
#!/bin/bash

# Fungsi dengan multiple return values
get_system_info() {
    local hostname=$(hostname)
    local kernel=$(uname -r)
    local uptime=$(uptime -p)
    
    echo "$hostname|$kernel|$uptime"
}

# Menggunakan IFS untuk split result
IFS='|' read -r host kern up <<< $(get_system_info)
echo "Hostname: $host"
echo "Kernel: $kern"
echo "Uptime: $up"

# Recursive function
factorial() {
    local n=$1
    if [ $n -le 1 ]; then
        echo 1
    else
        local prev=$(factorial $((n-1)))
        echo $((n * prev))
    fi
}

echo "Factorial of 5: $(factorial 5)"
```

***

### 7. Array

#### Array Dasar

```bash
#!/bin/bash

# Deklarasi array
fruits=("apple" "banana" "orange" "grape")

# Mengakses elemen
echo "First fruit: ${fruits[0]}"
echo "Second fruit: ${fruits[1]}"

# Semua elemen
echo "All fruits: ${fruits[@]}"
echo "All fruits (quoted): ${fruits[*]}"

# Panjang array
echo "Number of fruits: ${#fruits[@]}"

# Index array
echo "Indices: ${!fruits[@]}"
```

#### Manipulasi Array

```bash
#!/bin/bash

# Array kosong
numbers=()

# Menambah elemen
numbers+=(1)
numbers+=(2 3 4)
numbers[10]=100

echo "Numbers: ${numbers[@]}"
echo "Indices: ${!numbers[@]}"

# Menghapus elemen
unset numbers[2]
echo "After deletion: ${numbers[@]}"

# Slice array
all_numbers=(1 2 3 4 5 6 7 8 9 10)
echo "Slice [2:5]: ${all_numbers[@]:2:3}"

# Looping through array
for i in "${!numbers[@]}"; do
    echo "Index $i: ${numbers[i]}"
done
```

#### Associative Array

```bash
#!/bin/bash

# Deklarasi associative array
declare -A colors
colors[red]="#FF0000"
colors[green]="#00FF00"
colors[blue]="#0000FF"

# Mengakses nilai
echo "Red color code: ${colors[red]}"

# Semua keys dan values
echo "All colors:"
for color in "${!colors[@]}"; do
    echo "$color: ${colors[$color]}"
done

# Cek apakah key exists
if [[ -v colors[yellow] ]]; then
    echo "Yellow exists"
else
    echo "Yellow doesn't exist"
fi
```

***

### 8. Manipulasi String

#### Operasi String Dasar

```bash
#!/bin/bash

text="Hello World"

# Panjang string
echo "Length: ${#text}"

# Substring
echo "Substring: ${text:0:5}"    # "Hello"
echo "From index 6: ${text:6}"  # "World"

# Uppercase/Lowercase
echo "Uppercase: ${text^^}"
echo "Lowercase: ${text,,}"
echo "First letter uppercase: ${text^}"
```

#### Pattern Matching dan Replacement

```bash
#!/bin/bash

filename="document.backup.txt"

# Remove from beginning
echo "Remove doc*: ${filename#doc*}"        # "ument.backup.txt"
echo "Remove doc* (greedy): ${filename##doc*}" # ".backup.txt"

# Remove from end
echo "Remove .txt: ${filename%.txt}"        # "document.backup"
echo "Remove .* (greedy): ${filename%%.*}" # "document"

# String replacement
echo "Replace backup with old: ${filename/backup/old}"
echo "Replace all dots: ${filename//./_}"

# Default values
unset optional_var
echo "With default: ${optional_var:-"default value"}"
echo "Assign default: ${optional_var:="assigned default"}"
```

#### Advanced String Operations

```bash
#!/bin/bash

# String splitting
IFS=',' read -ra fields <<< "apple,banana,orange"
for field in "${fields[@]}"; do
    echo "Field: $field"
done

# String validation
email="user@example.com"
if [[ $email =~ ^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$ ]]; then
    echo "Valid email"
else
    echo "Invalid email"
fi

# Multiple pattern matching
case $filename in
    *.txt|*.doc|*.pdf)
        echo "Document file"
        ;;
    *.jpg|*.png|*.gif)
        echo "Image file"
        ;;
esac
```

***

### 9. File Operations

#### File Testing dan Information

```bash
#!/bin/bash

file="example.txt"

# File tests
if [ -f "$file" ]; then
    echo "File exists and is a regular file"
    echo "Size: $(stat -f%z "$file" 2>/dev/null || stat -c%s "$file" 2>/dev/null) bytes"
    echo "Modified: $(stat -f%Sm "$file" 2>/dev/null || stat -c%y "$file" 2>/dev/null)"
    echo "Permissions: $(stat -f%Sp "$file" 2>/dev/null || stat -c%A "$file" 2>/dev/null)"
fi

# Directory operations
if [ -d "/tmp" ]; then
    echo "Directory exists"
    echo "Contents:"
    ls -la /tmp | head -5
fi
```

#### File Reading dan Writing

```bash
#!/bin/bash

# Writing to file
cat > sample.txt << EOF
Line 1
Line 2
Line 3
EOF

# Reading file line by line
echo "Reading file:"
while IFS= read -r line || [ -n "$line" ]; do
    echo "Line: $line"
done < sample.txt

# Reading entire file into variable
content=$(<sample.txt)
echo "File content: $content"

# Processing CSV file
echo "name,age,city" > data.csv
echo "John,25,New York" >> data.csv
echo "Jane,30,London" >> data.csv

while IFS=',' read -r name age city; do
    echo "Name: $name, Age: $age, City: $city"
done < data.csv
```

#### File Manipulation

```bash
#!/bin/bash

# File operations
source_file="source.txt"
backup_file="source_backup.txt"

# Create test file
echo "Original content" > "$source_file"

# Copy file
cp "$source_file" "$backup_file"

# Append to file
echo "Additional content" >> "$source_file"

# Find and replace in file
sed -i 's/Original/Modified/' "$source_file"

# Count lines, words, characters
wc_output=$(wc "$source_file")
echo "File statistics: $wc_output"

# Find files
echo "Text files in current directory:"
find . -name "*.txt" -type f

# File permissions
chmod 644 "$source_file"
echo "File permissions changed"
```

***

### 10. Advanced Bash Features

#### Command Substitution dan Process Substitution

```bash
#!/bin/bash

# Command substitution
current_date=$(date +%Y-%m-%d)
echo "Today is: $current_date"

# Nested command substitution
user_home=$(eval echo ~$(whoami))
echo "User home: $user_home"

# Process substitution
# Compare output of two commands
diff <(ls /usr/bin) <(ls /bin)

# Multiple inputs
while read -r line; do
    echo "Processing: $line"
done < <(find /etc -name "*.conf" | head -5)
```

#### Advanced Parameter Expansion

```bash
#!/bin/bash

# Parameter expansion with transformation
files=("Document.TXT" "Image.JPG" "script.SH")

for file in "${files[@]}"; do
    # Convert to lowercase
    lowercase="${file,,}"
    
    # Extract name and extension
    name="${file%.*}"
    extension="${file##*.}"
    
    echo "Original: $file"
    echo "Lowercase: $lowercase"
    echo "Name: $name, Extension: $extension"
    echo "---"
done

# Array parameter expansion
numbers=(1 2 3 4 5)
echo "All: ${numbers[*]}"
echo "Quoted all: ${numbers[@]}"
echo "Length: ${#numbers[@]}"
echo "Indices: ${!numbers[@]}"
```

#### Traps dan Signal Handling

```bash
#!/bin/bash

# Cleanup function
cleanup() {
    echo "Cleaning up..."
    rm -f temp_file.txt
    exit 0
}

# Set trap for signals
trap cleanup INT TERM

# Create temporary file
echo "Creating temporary file..."
touch temp_file.txt

# Simulate work
echo "Working... (Press Ctrl+C to interrupt)"
for i in {1..30}; do
    echo "Step $i"
    sleep 1
done

cleanup
```

#### Background Jobs dan Job Control

```bash
#!/bin/bash

# Start background job
echo "Starting background job..."
sleep 10 &
job1_pid=$!

echo "Background job PID: $job1_pid"

# Start another background job
(
    for i in {1..5}; do
        echo "Background task: $i"
        sleep 2
    done
) &
job2_pid=$!

# Wait for specific job
wait $job1_pid
echo "Job 1 completed"

# Wait for all background jobs
wait
echo "All jobs completed"

# Job control
jobs
```

***

### 11. Error Handling dan Debugging

#### Error Handling

```bash
#!/bin/bash

# Set error handling options
set -e  # Exit on error
set -u  # Exit on undefined variable
set -o pipefail  # Exit on pipe failure

# Error handling function
handle_error() {
    local exit_code=$?
    local line_number=$1
    echo "Error occurred in script at line $line_number: exit code $exit_code"
    exit $exit_code
}

# Set error trap
trap 'handle_error $LINENO' ERR

# Function with error handling
safe_command() {
    local command="$1"
    local error_message="$2"
    
    if ! $command; then
        echo "Error: $error_message"
        return 1
    fi
}

# Example usage
safe_command "ls /nonexistent" "Directory not found"
```

#### Debugging Techniques

```bash
#!/bin/bash

# Debug mode
set -x  # Print commands before execution

# Debug function
debug() {
    if [ "${DEBUG:-}" = "1" ]; then
        echo "DEBUG: $*" >&2
    fi
}

# Usage
DEBUG=1
debug "Starting script"
debug "Processing data"

# Conditional debugging
if [ "${DEBUG:-}" = "1" ]; then
    echo "Debug information..."
    env | grep "^PATH"
fi

# Turn off debug mode
set +x

# Logging function
log() {
    local level=$1
    shift
    echo "$(date '+%Y-%m-%d %H:%M:%S') [$level] $*"
}

log "INFO" "Script started"
log "ERROR" "Something went wrong"
```

#### Testing dan Validation

```bash
#!/bin/bash

# Input validation
validate_number() {
    local input=$1
    if [[ $input =~ ^[0-9]+$ ]]; then
        return 0
    else
        return 1
    fi
}

# File validation
validate_file() {
    local file=$1
    if [ ! -f "$file" ]; then
        echo "Error: File '$file' not found"
        return 1
    fi
    
    if [ ! -r "$file" ]; then
        echo "Error: File '$file' not readable"
        return 1
    fi
    
    return 0
}

# Usage
read -p "Enter a number: " user_input
if validate_number "$user_input"; then
    echo "Valid number: $user_input"
else
    echo "Invalid number"
    exit 1
fi
```

***

### 12. Best Practices

#### Script Structure dan Organization

```bash
#!/bin/bash

# Script metadata
# Author: Your Name
# Date: 2024-01-01
# Description: Example script with best practices
# Version: 1.0

# Strict mode
set -euo pipefail

# Global variables (uppercase)
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly SCRIPT_NAME="$(basename "$0")"
readonly LOG_FILE="/tmp/${SCRIPT_NAME%.sh}.log"

# Configuration
readonly DEFAULT_TIMEOUT=30
readonly MAX_RETRIES=3

# Functions (lowercase with underscores)
usage() {
    cat << EOF
Usage: $SCRIPT_NAME [OPTIONS]

OPTIONS:
    -h, --help      Show this help message
    -v, --verbose   Enable verbose output
    -f, --file      Specify input file
    
Examples:
    $SCRIPT_NAME -f input.txt
    $SCRIPT_NAME --verbose --file data.csv
EOF
}

log_message() {
    local level=$1
    shift
    echo "$(date '+%Y-%m-%d %H:%M:%S') [$level] $*" | tee -a "$LOG_FILE"
}

cleanup_and_exit() {
    local exit_code=${1:-0}
    log_message "INFO" "Script finished with exit code: $exit_code"
    exit $exit_code
}

# Main function
main() {
    local input_file=""
    local verbose=false
    
    # Parse arguments
    while [[ $# -gt 0 ]]; do
        case $1 in
            -h|--help)
                usage
                exit 0
                ;;
            -v|--verbose)
                verbose=true
                shift
                ;;
            -f|--file)
                input_file="$2"
                shift 2
                ;;
            *)
                echo "Unknown option: $1"
                usage
                exit 1
                ;;
        esac
    done
    
    # Validate input
    if [ -z "$input_file" ]; then
        echo "Error: Input file required"
        usage
        exit 1
    fi
    
    # Main logic here
    log_message "INFO" "Processing file: $input_file"
    
    if [ "$verbose" = true ]; then
        log_message "DEBUG" "Verbose mode enabled"
    fi
    
    # Your script logic here
    
    cleanup_and_exit 0
}

# Trap for cleanup
trap 'cleanup_and_exit $?' INT TERM

# Run main function
main "$@"
```

#### Security Best Practices

```bash
#!/bin/bash

# Secure script practices

# 1. Use full paths for commands
readonly LS="/bin/ls"
readonly GREP="/bin/grep"
readonly AWK="/usr/bin/awk"

# 2. Validate input rigorously
validate_input() {
    local input=$1
    local pattern=$2
    
    if [[ ! $input =~ $pattern ]]; then
        echo "Invalid input format"
        return 1
    fi
}

# 3. Use temporary files securely
create_temp_file() {
    local temp_file
    temp_file=$(mktemp) || {
        echo "Failed to create temporary file"
        exit 1
    }
    echo "$temp_file"
}

# 4. Handle passwords securely
read_password() {
    local password
    echo -n "Enter password: "
    read -s password
    echo
    
    # Use the password
    # Don't store in variable longer than necessary
    unset password
}

# 5. Sanitize file paths
sanitize_path() {
    local path=$1
    # Remove dangerous characters
    path=${path//\.\./}
    path=${path//;/}
    path=${path//\|/}
    echo "$path"
}
```

***

### 13. Proyek Praktis

#### Project 1: System Monitor

```bash
#!/bin/bash

# System monitoring script
readonly LOG_DIR="/var/log/system-monitor"
readonly ALERT_THRESHOLD_CPU=80
readonly ALERT_THRESHOLD_MEM=85
readonly ALERT_THRESHOLD_DISK=90

# Ensure log directory exists
mkdir -p "$LOG_DIR"

# System information gathering
get_cpu_usage() {
    top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1}'
}

get_memory_usage() {
    free | grep Mem | awk '{printf("%.2f"), $3/$2 * 100.0}'
}

get_disk_usage() {
    df -h / | awk 'NR==2 {print $5}' | sed 's/%//'
}

# Alert function
send_alert() {
    local metric=$1
    local value=$2
    local threshold=$3
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    
    echo "[$timestamp] ALERT: $metric usage is ${value}% (threshold: ${threshold}%)" >> "$LOG_DIR/alerts.log"
}

# Main monitoring function
monitor_system() {
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    local cpu_usage=$(get_cpu_usage)
    local mem_usage=$(get_memory_usage)
    local disk_usage=$(get_disk_usage)
    
    # Log metrics
    echo "$timestamp,CPU,$cpu_usage" >> "$LOG_DIR/metrics.csv"
    echo "$timestamp,MEM,$mem_usage" >> "$LOG_DIR/metrics.csv"
    echo "$timestamp,DISK,$disk_usage" >> "$LOG_DIR/metrics.csv"
    
    # Check thresholds
    if (( $(echo "$cpu_usage > $ALERT_THRESHOLD_CPU" | bc -l) )); then
        send_alert "CPU" "$cpu_usage" "$ALERT_THRESHOLD_CPU"
    fi
    
    if (( $(echo "$mem_usage > $ALERT_THRESHOLD_MEM" | bc -l) )); then
        send_alert "Memory" "$mem_usage" "$ALERT_THRESHOLD_MEM"
    fi
    
    if [ "$disk_usage" -gt "$ALERT_THRESHOLD_DISK" ]; then
        send_alert "Disk" "$disk_usage" "$ALERT_THRESHOLD_DISK"
    fi
    
    echo "[$timestamp] System check completed - CPU: ${cpu_usage}%, MEM: ${mem_usage}%, DISK: ${disk_usage}%"
}

# Run monitoring
monitor_system
```

#### Project 2: Backup Script

```bash
#!/bin/bash

# Advanced backup script
readonly BACKUP_ROOT="/backups"
readonly CONFIG_FILE="/etc/backup.conf"
readonly LOG_FILE="/var/log/backup.log"
readonly MAX_BACKUPS=7

# Configuration variables
declare -A BACKUP_SOURCES
declare -A BACKUP_DESTINATIONS
declare -a EXCLUDE_PATTERNS

# Load configuration
load_config() {
    if [ ! -f "$CONFIG_FILE" ]; then
        echo "Configuration file not found: $CONFIG_FILE"
        exit 1
    fi
    
    source "$CONFIG_FILE"
}

# Logging function
log() {
    local level=$1
    shift
    echo "$(date '+%Y-%m-%d %H:%M:%S') [$level] $*" | tee -a "$LOG_FILE"
}

# Create backup
create_backup() {
    local source=$1
    local destination=$2
    local backup_name="backup_$(date +%Y%m%d_%H%M%S)"
    local backup_path="$destination/$backup_name"
    
    log "INFO" "Starting backup: $source -> $backup_path"
    
    # Create destination directory
    mkdir -p "$backup_path"
    
    # Build exclude options
    local exclude_opts=""
```
