# Bash Scripting

## First Line On Bash Script File

```bash
#!/bin/bash
```

## Var

### Membuat Variabel

Example :

```bash
nama = "Kopi Flores"
```

### Akses Variabel

Menggunakan tanda $var (tanda dolar dan diikuti nama variabel);

Example :

```bash
echo "Hallo nama saya $nama";
```

## Function

Create Function :

```bash
 hay(){
    return "Hello Word";
 }
```

Akses Function Pada bash fungsi dapat return value dan non return.

```bash
 hay(){
    return "Hello Word";
 }

 echo hay
```

## Input Output

### Input

Untuk menginput nilai variabel dapat menggunakan kata kunci read lalu diikuti nama variabel

```bash
read nama
```

Example :

```bash
echo "Enter filename"
read filename

if [ -e $filename ]
then
echo "$filename is exits on the directory"
cat $filename

else
    cat > $filename
    echo "File created"
fi
```
