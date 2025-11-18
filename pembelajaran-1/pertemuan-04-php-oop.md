# PERTEMUAN 4 — PHP OOP

## 📚 Materi

### 1. Pengenalan OOP (Object-Oriented Programming)
OOP adalah paradigma pemrograman yang menggunakan konsep "objek" yang memiliki data (property) dan fungsi (method).

**Konsep Dasar:**
- **Class** - Blueprint/template untuk membuat objek
- **Object** - Instance dari class
- **Property** - Variabel dalam class
- **Method** - Function dalam class

### 2. Class dan Object

**Membuat Class:**
```php
class User {
    // Property
    public $name;
    public $email;
    
    // Method
    public function sayHello() {
        echo "Hello, my name is " . $this->name;
    }
}
```

**Membuat Object (Instance):**
```php
// Membuat object dari class User
$user1 = new User();
$user1->name = "Budi";
$user1->email = "budi@example.com";
$user1->sayHello(); // Output: Hello, my name is Budi
```

### 3. Property
Property adalah variabel yang dimiliki oleh class/object.

**Visibility:**
- `public` - Bisa diakses dari mana saja
- `private` - Hanya bisa diakses dari dalam class
- `protected` - Bisa diakses dari class dan child class

**Contoh:**
```php
class User {
    public $name;        // Public property
    private $password;   // Private property
    protected $email;    // Protected property
}
```

### 4. Method
Method adalah function yang ada dalam class.

**Contoh:**
```php
class User {
    public $name;
    
    // Method public
    public function sayHello() {
        return "Hello, " . $this->name;
    }
    
    // Method private
    private function validateEmail($email) {
        return filter_var($email, FILTER_VALIDATE_EMAIL);
    }
    
    // Method dengan parameter
    public function setEmail($email) {
        if ($this->validateEmail($email)) {
            $this->email = $email;
            return true;
        }
        return false;
    }
}
```

**Menggunakan `$this`:**
- `$this` merujuk pada object saat ini
- Digunakan untuk mengakses property dan method dalam class

### 5. Constructor
Constructor adalah method khusus yang otomatis dipanggil saat object dibuat.

**Sintaks:**
```php
class User {
    public $name;
    public $email;
    
    // Constructor
    public function __construct($name, $email) {
        $this->name = $name;
        $this->email = $email;
        echo "User created: " . $this->name;
    }
}

// Membuat object (constructor otomatis dipanggil)
$user = new User("Budi", "budi@example.com");
// Output: User created: Budi
```

**Constructor dengan Default Value:**
```php
class User {
    public $name;
    public $email;
    public $status;
    
    public function __construct($name, $email, $status = "active") {
        $this->name = $name;
        $this->email = $email;
        $this->status = $status;
    }
}

$user1 = new User("Budi", "budi@example.com"); // status = "active"
$user2 = new User("Siti", "siti@example.com", "inactive");
```

### 6. Destructor
Destructor adalah method yang otomatis dipanggil saat object dihapus.

```php
class User {
    public function __construct($name) {
        $this->name = $name;
        echo "User created: " . $this->name . "<br>";
    }
    
    public function __destruct() {
        echo "User destroyed: " . $this->name . "<br>";
    }
}

$user = new User("Budi");
// Output: User created: Budi
// Ketika script selesai: User destroyed: Budi
```

### 7. Contoh Lengkap

```php
class User {
    // Properties
    public $name;
    public $email;
    private $password;
    
    // Constructor
    public function __construct($name, $email) {
        $this->name = $name;
        $this->email = $email;
    }
    
    // Method untuk set password
    public function setPassword($password) {
        $this->password = password_hash($password, PASSWORD_DEFAULT);
    }
    
    // Method untuk get info
    public function getInfo() {
        return [
            "name" => $this->name,
            "email" => $this->email
        ];
    }
    
    // Method sayHello
    public function sayHello() {
        echo "Hello, my name is " . $this->name . " and my email is " . $this->email;
    }
}

// Membuat object
$user1 = new User("Budi", "budi@example.com");
$user1->setPassword("secret123");
$user1->sayHello();
// Output: Hello, my name is Budi and my email is budi@example.com
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa itu class?**
   - Jawab: Class adalah blueprint/template untuk membuat objek. Class mendefinisikan property (data) dan method (fungsi) yang akan dimiliki oleh objek.

2. **Apa beda function dan method?**
   - Jawab: Function adalah fungsi yang berdiri sendiri, sedangkan method adalah function yang ada di dalam class. Method bisa mengakses property class menggunakan `$this`.

3. **Apa itu constructor?**
   - Jawab: Constructor adalah method khusus (dengan nama `__construct()`) yang otomatis dipanggil saat object dibuat. Biasanya digunakan untuk inisialisasi property object.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Class User dengan Method sayHello()
Buat class `User` dengan:
- Property `name` (public)
- Method `sayHello()` yang menampilkan "Hello, my name is [name]"

**Contoh:**
```php
<?php
class User {
    public $name;
    
    public function sayHello() {
        echo "Hello, my name is " . $this->name;
    }
}

// Buat object dan test
$user1 = new User();
$user1->name = "Budi";
$user1->sayHello(); // Output: Hello, my name is Budi

$user2 = new User();
$user2->name = "Siti";
$user2->sayHello(); // Output: Hello, my name is Siti
?>
```

**File:** `user.php`

### Tugas 2: Constructor yang Mengisi Name Otomatis
Buat constructor yang mengisi `name` otomatis saat object dibuat.

**Contoh:**
```php
<?php
class User {
    public $name;
    public $email;
    
    // Constructor
    public function __construct($name, $email = "") {
        $this->name = $name;
        $this->email = $email;
        echo "User created: " . $this->name . "<br>";
    }
    
    public function sayHello() {
        echo "Hello, my name is " . $this->name;
        if (!empty($this->email)) {
            echo " and my email is " . $this->email;
        }
        echo "<br>";
    }
}

// Test
$user1 = new User("Budi", "budi@example.com");
// Output: User created: Budi
$user1->sayHello();
// Output: Hello, my name is Budi and my email is budi@example.com

$user2 = new User("Siti");
// Output: User created: Siti
$user2->sayHello();
// Output: Hello, my name is Siti
?>
```

**File:** `user_constructor.php`

---

## 💡 Tips

- Gunakan `$this->` untuk mengakses property dan method dalam class
- Constructor sangat berguna untuk inisialisasi property
- Gunakan visibility (public, private, protected) dengan bijak
- Nama class biasanya menggunakan PascalCase (User, Car, Product)
- Object adalah instance dari class, bisa dibuat banyak object dari satu class

---

**Selamat belajar! 🚀**

