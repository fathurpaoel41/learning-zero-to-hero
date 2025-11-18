# PERTEMUAN 14 — JavaScript DOM

## 📚 Materi

### 1. Pengenalan DOM
DOM (Document Object Model) adalah representasi HTML sebagai object yang bisa dimanipulasi dengan JavaScript.

**Struktur DOM:**
```
Document
└── HTML
    ├── Head
    └── Body
        ├── H1
        ├── P
        └── Button
```

### 2. Query Selector

**Mengambil Element:**
```javascript
// getElementById (satu element)
let header = document.getElementById('header');

// getElementsByClassName (array-like)
let items = document.getElementsByClassName('item');

// getElementsByTagName (array-like)
let paragraphs = document.getElementsByTagName('p');

// querySelector (satu element, CSS selector)
let title = document.querySelector('.title');
let firstItem = document.querySelector('#list li:first-child');

// querySelectorAll (semua element, array)
let allItems = document.querySelectorAll('.item');
let listItems = document.querySelectorAll('#list li');
```

**Contoh HTML:**
```html
<div id="header">Header</div>
<ul id="list">
    <li class="item">Item 1</li>
    <li class="item">Item 2</li>
    <li class="item">Item 3</li>
</ul>
```

**Contoh JavaScript:**
```javascript
// Ambil element
let header = document.getElementById('header');
let items = document.querySelectorAll('.item');

// Manipulasi
header.textContent = "New Header";
items[0].textContent = "Updated Item 1";
```

### 3. Manipulasi Element

**Mengubah Konten:**
```javascript
let element = document.querySelector('#title');

// textContent (hanya teks)
element.textContent = "New Title";

// innerHTML (bisa HTML)
element.innerHTML = "<strong>New Title</strong>";

// innerText (hanya teks yang terlihat)
element.innerText = "New Title";
```

**Mengubah Attribute:**
```javascript
let image = document.querySelector('img');

// Set attribute
image.setAttribute('src', 'new-image.jpg');
image.setAttribute('alt', 'Description');

// Get attribute
let src = image.getAttribute('src');

// Remove attribute
image.removeAttribute('alt');

// Direct property (lebih cepat)
image.src = 'new-image.jpg';
image.alt = 'Description';
```

**Mengubah Style:**
```javascript
let element = document.querySelector('.box');

// Inline style
element.style.color = 'red';
element.style.backgroundColor = 'blue';
element.style.fontSize = '20px';

// Menggunakan class
element.classList.add('active');
element.classList.remove('inactive');
element.classList.toggle('hidden');
element.classList.contains('active'); // true/false
```

### 4. Membuat dan Menghapus Element

**Membuat Element:**
```javascript
// Buat element
let newDiv = document.createElement('div');
newDiv.textContent = "New Div";
newDiv.className = "box";

// Tambahkan ke DOM
let container = document.querySelector('#container');
container.appendChild(newDiv);

// Atau insertBefore
let firstChild = container.firstChild;
container.insertBefore(newDiv, firstChild);
```

**Menghapus Element:**
```javascript
let element = document.querySelector('.item');
element.remove(); // Modern way

// Atau
element.parentNode.removeChild(element); // Old way
```

**Contoh Lengkap:**
```javascript
// Buat <li> baru
let newItem = document.createElement('li');
newItem.textContent = "Item Baru";
newItem.className = "item";

// Tambahkan ke <ul>
let list = document.querySelector('#list');
list.appendChild(newItem);
```

### 5. Event Listener

**Cara 1: Inline (tidak disarankan):**
```html
<button onclick="alert('Clicked!')">Click Me</button>
```

**Cara 2: addEventListener (disarankan):**
```javascript
let button = document.querySelector('#myButton');

button.addEventListener('click', function() {
    alert('Button clicked!');
});

// Arrow function
button.addEventListener('click', () => {
    console.log('Clicked!');
});
```

**Event Types:**
```javascript
// Click
element.addEventListener('click', handler);

// Double click
element.addEventListener('dblclick', handler);

// Mouse events
element.addEventListener('mouseenter', handler);
element.addEventListener('mouseleave', handler);
element.addEventListener('mousemove', handler);

// Keyboard events
element.addEventListener('keydown', handler);
element.addEventListener('keyup', handler);
element.addEventListener('keypress', handler);

// Form events
element.addEventListener('submit', handler);
element.addEventListener('change', handler);
element.addEventListener('input', handler);

// Window events
window.addEventListener('load', handler);
window.addEventListener('resize', handler);
```

**Event Object:**
```javascript
button.addEventListener('click', function(event) {
    console.log(event.type);      // "click"
    console.log(event.target);    // Element yang diklik
    console.log(event.clientX);   // Posisi X mouse
    console.log(event.clientY);   // Posisi Y mouse
});
```

### 6. preventDefault() dan stopPropagation()

**preventDefault():**
```javascript
let form = document.querySelector('#myForm');

form.addEventListener('submit', function(event) {
    event.preventDefault(); // Mencegah form submit default
    // Lakukan sesuatu
    console.log('Form submitted!');
});
```

**stopPropagation():**
```javascript
let button = document.querySelector('#button');
let div = document.querySelector('#container');

button.addEventListener('click', function(event) {
    event.stopPropagation(); // Mencegah event bubbling
    console.log('Button clicked');
});

div.addEventListener('click', function() {
    console.log('Div clicked'); // Tidak akan dipanggil jika stopPropagation
});
```

### 7. Contoh Praktis: Dynamic List

**HTML:**
```html
<div>
    <input type="text" id="itemInput" placeholder="Masukkan item">
    <button id="addButton">Tambah Item</button>
    <ul id="itemList"></ul>
</div>
```

**JavaScript:**
```javascript
let input = document.querySelector('#itemInput');
let button = document.querySelector('#addButton');
let list = document.querySelector('#itemList');

button.addEventListener('click', function() {
    let value = input.value.trim();
    
    if (value !== '') {
        // Buat <li> baru
        let li = document.createElement('li');
        li.textContent = value;
        
        // Buat tombol hapus
        let deleteBtn = document.createElement('button');
        deleteBtn.textContent = 'Hapus';
        deleteBtn.addEventListener('click', function() {
            li.remove();
        });
        
        li.appendChild(deleteBtn);
        list.appendChild(li);
        
        // Clear input
        input.value = '';
    }
});

// Tambahkan dengan Enter
input.addEventListener('keypress', function(event) {
    if (event.key === 'Enter') {
        button.click();
    }
});
```

### 8. Validasi Form dengan JavaScript

**HTML:**
```html
<form id="myForm">
    <input type="text" id="nama" placeholder="Nama" required>
    <span id="namaError" style="color: red;"></span>
    
    <input type="email" id="email" placeholder="Email" required>
    <span id="emailError" style="color: red;"></span>
    
    <button type="submit">Submit</button>
</form>
```

**JavaScript:**
```javascript
let form = document.querySelector('#myForm');
let namaInput = document.querySelector('#nama');
let emailInput = document.querySelector('#email');
let namaError = document.querySelector('#namaError');
let emailError = document.querySelector('#emailError');

form.addEventListener('submit', function(event) {
    event.preventDefault();
    
    // Reset errors
    namaError.textContent = '';
    emailError.textContent = '';
    
    let isValid = true;
    
    // Validasi nama
    if (namaInput.value.trim() === '') {
        namaError.textContent = 'Nama harus diisi!';
        isValid = false;
    }
    
    // Validasi email
    let emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailPattern.test(emailInput.value)) {
        emailError.textContent = 'Email tidak valid!';
        isValid = false;
    }
    
    if (isValid) {
        alert('Form valid! Data akan dikirim.');
        // form.submit(); // Uncomment untuk submit
    }
});
```

---

## 📝 Pretest (3 Soal Ringkas)

1. **Apa itu DOM?**
   - Jawab: DOM (Document Object Model) adalah representasi HTML sebagai object yang bisa dimanipulasi dengan JavaScript. DOM memungkinkan JavaScript untuk mengakses dan mengubah elemen HTML, atribut, dan style.

2. **Apa fungsi document.querySelector()?**
   - Jawab: `document.querySelector()` digunakan untuk mengambil satu elemen HTML pertama yang cocok dengan CSS selector. Contoh: `document.querySelector('.class')` atau `document.querySelector('#id')`. Untuk mengambil semua elemen, gunakan `querySelectorAll()`.

3. **Apa itu event?**
   - Jawab: Event adalah aksi yang terjadi di browser seperti click, submit, keypress, dll. JavaScript bisa "mendengarkan" event ini menggunakan `addEventListener()` dan menjalankan fungsi tertentu ketika event terjadi.

---

## 🏠 PR (Pekerjaan Rumah)

### Tugas 1: Tombol "Tambah Item" yang Menambah <li> ke <ul>
Buat tombol "Tambah Item" yang menambahkan item baru ke dalam list.

**HTML:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Tambah Item</title>
</head>
<body>
    <div>
        <input type="text" id="itemInput" placeholder="Masukkan item">
        <button id="addButton">Tambah Item</button>
        <ul id="itemList"></ul>
    </div>

    <script>
        // Kode JavaScript di sini
    </script>
</body>
</html>
```

**Fitur yang harus ada:**
- Input text untuk memasukkan item
- Tombol "Tambah Item"
- Setelah klik, item ditambahkan ke `<ul>` sebagai `<li>`
- Input dikosongkan setelah item ditambahkan
- Bisa menambah item dengan menekan Enter

**File:** `tugas1.html`

### Tugas 2: Validasi Form Input Kosong
Buat validasi form menggunakan JavaScript yang mengecek apakah input kosong.

**HTML:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Validasi Form</title>
    <style>
        .error { color: red; font-size: 14px; }
        input { padding: 8px; margin: 5px 0; }
        button { padding: 10px 20px; }
    </style>
</head>
<body>
    <form id="myForm">
        <div>
            <label>Nama:</label>
            <input type="text" id="nama" placeholder="Masukkan nama">
            <span id="namaError" class="error"></span>
        </div>
        
        <div>
            <label>Email:</label>
            <input type="email" id="email" placeholder="Masukkan email">
            <span id="emailError" class="error"></span>
        </div>
        
        <button type="submit">Submit</button>
    </form>

    <script>
        // Kode JavaScript di sini
    </script>
</body>
</html>
```

**Fitur yang harus ada:**
- Validasi saat form di-submit
- Tampilkan error jika input kosong
- Error ditampilkan di bawah input yang kosong
- Mencegah form submit jika ada error
- Reset error saat user mulai mengetik

**File:** `tugas2.html`

---

## 💡 Tips

- Gunakan `querySelector` dan `querySelectorAll` (modern, lebih fleksibel)
- `addEventListener` lebih baik dari inline event handler
- `preventDefault()` untuk mencegah behavior default (seperti form submit)
- `textContent` lebih aman dari `innerHTML` (mencegah XSS)
- `classList` lebih baik dari `className` untuk manipulasi class
- Selalu validasi input di client-side (tapi tetap validasi di server juga!)
- Gunakan `trim()` untuk menghapus spasi di awal/akhir

---

**Selamat belajar! 🚀**

