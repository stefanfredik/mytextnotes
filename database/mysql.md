---
icon: laptop-code
---

# Mysql

### Database User

#### Create User

Add User&#x20;

```bash
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
```

Grant all privileges to new user.

```bash
GRANT ALL PRIVILEGES ON mydatabase.* TO 'newuser'@'localhost';
```

Flush Privileges

```bash
FLUSH PRIVILEGES;
```
