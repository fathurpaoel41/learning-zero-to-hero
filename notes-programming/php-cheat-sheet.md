# PHP Cheat Sheet

## Quick Reference untuk PHP

### Variabel & Tipe Data
```php
$nama = "Budi";           // String
$umur = 20;               // Integer
$tinggi = 170.5;          // Float
$isActive = true;         // Boolean
$hobi = ["Membaca", "Coding"]; // Array
```

### Array
```php
// Indexed Array
$buah = ["Apel", "Jeruk", "Mangga"];
echo $buah[0]; // Apel

// Associative Array
$mahasiswa = [
    "nama" => "Budi",
    "umur" => 20
];
echo $mahasiswa["nama"]; // Budi

// Array Functions
count($array);              // Jumlah elemen
array_push($array, $item);  // Tambah di akhir
array_pop($array);          // Hapus dari akhir
in_array($value, $array);  // Cek apakah ada
```

### String Functions
```php
strlen($string);                    // Panjang string
strtoupper($string);                // Uppercase
strtolower($string);                // Lowercase
substr($string, 0, 5);             // Substring
str_replace("old", "new", $string); // Replace
trim($string);                      // Hapus spasi
explode(",", $string);             // String ke array
implode(",", $array);              // Array ke string
```

### Control Flow
```php
// If/Else
if ($umur >= 18) {
    echo "Dewasa";
} else {
    echo "Anak-anak";
}

// Switch
switch ($hari) {
    case "Senin":
        echo "Hari kerja";
        break;
    default:
        echo "Hari lain";
}

// Ternary
$status = $umur >= 18 ? "Dewasa" : "Anak-anak";
```

### Loop
```php
// For
for ($i = 0; $i < 10; $i++) {
    echo $i;
}

// While
while ($i < 10) {
    echo $i;
    $i++;
}

// Foreach
foreach ($array as $item) {
    echo $item;
}

foreach ($array as $key => $value) {
    echo "$key: $value";
}
```

### Function
```php
function sapa($nama) {
    return "Halo, " . $nama;
}

// Default parameter
function sapa($nama = "Guest") {
    return "Halo, " . $nama;
}
```

### OOP
```php
class User {
    public $name;
    private $email;
    
    public function __construct($name) {
        $this->name = $name;
    }
    
    public function sayHello() {
        return "Hello, " . $this->name;
    }
}

$user = new User("Budi");
echo $user->sayHello();
```

### MySQLi
```php
// Koneksi
$conn = mysqli_connect("localhost", "user", "pass", "db");

// Query
$result = mysqli_query($conn, "SELECT * FROM users");

// Fetch
while ($row = mysqli_fetch_assoc($result)) {
    echo $row['name'];
}

// Insert
mysqli_query($conn, "INSERT INTO users (name) VALUES ('Budi')");

// Escape
$name = mysqli_real_escape_string($conn, $_POST['name']);

// Close
mysqli_close($conn);
```

### Superglobals
```php
$_GET['id'];        // URL parameter
$_POST['name'];     // Form POST
$_SESSION['user'];  // Session
$_COOKIE['lang'];   // Cookie
$_SERVER['REQUEST_METHOD']; // Request method
```

### Useful Functions
```php
isset($var);        // Cek apakah variabel ada
empty($var);        // Cek apakah kosong
var_dump($var);     // Debug variabel
print_r($var);      // Print array/object
date('Y-m-d');      // Tanggal sekarang
strtotime('2024-01-01'); // String ke timestamp
```

