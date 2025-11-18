# JavaScript Cheat Sheet

## Quick Reference untuk JavaScript

### Variabel
```javascript
let name = "Budi";      // Bisa diubah
const age = 20;         // Tidak bisa diubah
var old = "old way";    // Tidak disarankan
```

### Tipe Data
```javascript
// Primitive
let str = "Hello";
let num = 42;
let bool = true;
let undef = undefined;
let nul = null;

// Object
let obj = {
    name: "Budi",
    age: 20
};

// Array
let arr = [1, 2, 3];
```

### Array Methods
```javascript
arr.push(item);         // Tambah di akhir
arr.pop();              // Hapus dari akhir
arr.shift();            // Hapus dari awal
arr.unshift(item);      // Tambah di awal
arr.indexOf(item);      // Cari index
arr.includes(item);      // Cek apakah ada
arr.join(", ");         // Array ke string
arr.slice(0, 2);        // Potong array
arr.splice(1, 1);       // Hapus/insert

// Functional
arr.forEach(item => console.log(item));
arr.map(item => item * 2);
arr.filter(item => item > 5);
arr.find(item => item > 5);
arr.reduce((acc, item) => acc + item, 0);
```

### String Methods
```javascript
str.length;                    // Panjang
str.toUpperCase();             // Uppercase
str.toLowerCase();             // Lowercase
str.substring(0, 5);           // Substring
str.replace("old", "new");     // Replace
str.trim();                    // Hapus spasi
str.split(",");                // String ke array
str.includes("text");          // Cek apakah ada
```

### Control Flow
```javascript
// If/Else
if (age >= 18) {
    console.log("Dewasa");
} else {
    console.log("Anak-anak");
}

// Ternary
let status = age >= 18 ? "Dewasa" : "Anak-anak";

// Switch
switch (day) {
    case "Monday":
        console.log("Hari kerja");
        break;
    default:
        console.log("Hari lain");
}
```

### Loop
```javascript
// For
for (let i = 0; i < 10; i++) {
    console.log(i);
}

// While
while (i < 10) {
    console.log(i);
    i++;
}

// For...of (Array)
for (let item of array) {
    console.log(item);
}

// For...in (Object)
for (let key in obj) {
    console.log(key, obj[key]);
}
```

### Function
```javascript
// Function declaration
function greet(name) {
    return "Hello, " + name;
}

// Arrow function
const greet = (name) => {
    return "Hello, " + name;
};

// Single line
const greet = name => "Hello, " + name;
```

### DOM Manipulation
```javascript
// Select
document.getElementById('id');
document.querySelector('.class');
document.querySelectorAll('.class');

// Manipulate
element.textContent = "Text";
element.innerHTML = "<strong>Text</strong>";
element.style.color = "red";
element.classList.add('class');
element.classList.remove('class');
element.classList.toggle('class');

// Create/Remove
let div = document.createElement('div');
parent.appendChild(div);
element.remove();
```

### Event Listener
```javascript
element.addEventListener('click', function() {
    console.log('Clicked');
});

// Arrow function
element.addEventListener('click', () => {
    console.log('Clicked');
});

// Event object
element.addEventListener('click', (event) => {
    event.preventDefault();
    event.stopPropagation();
    console.log(event.target);
});
```

### Common Events
```javascript
'click'      // Klik mouse
'dblclick'   // Double click
'mouseenter' // Mouse masuk
'mouseleave' // Mouse keluar
'keydown'    // Tekan tombol
'keyup'      // Lepas tombol
'submit'     // Form submit
'change'     // Input berubah
'input'      // Input typing
'load'       // Page loaded
```

### Fetch API
```javascript
fetch('/api/users')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));

// POST
fetch('/api/users', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify({ name: 'Budi' })
});
```

### Object Methods
```javascript
Object.keys(obj);      // Array of keys
Object.values(obj);    // Array of values
Object.entries(obj);   // Array of [key, value]
Object.assign({}, obj); // Copy object
```

### Useful Functions
```javascript
parseInt("42");        // String ke integer
parseFloat("3.14");    // String ke float
JSON.stringify(obj);   // Object ke JSON
JSON.parse(json);      // JSON ke object
typeof variable;       // Tipe data
Array.isArray(arr);    // Cek apakah array
```

