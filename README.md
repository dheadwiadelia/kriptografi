# Tugas Kriptografi

Nama : Dhea Dwi Adelia

NIM : 312210116

Kelas : TI.22.A.1

![tugas](https://github.com/user-attachments/assets/aa108798-d8aa-4e91-b890-6d5264c9804b)

- Dari kunci "TEKNIK INFORMATIKA", kita akan menghilangkan karakter yang berulang dan menuliskannya ke dalam matriks 5x5, menggabungkan huruf I dan J (sebagai satu entitas, karena alfabet hanya memiliki 25 huruf untuk tabel Playfair). Sisanya diisi dengan huruf-huruf alfabet lainnya yang belum digunakan.

- Kunci yang diberikan: TEKNIK INFORMATIKA

    - Hilangkan huruf berulang: TEKNIKFORMA
    - Matriks 5x5:

 ```
T E K N I
K F O R M
A B C D G
H L P Q S
U V W X Y
```


### Code 

```
def generate_playfair_matrix(key):
    # Menghilangkan huruf berulang dan menyusun tabel Playfair
    key = key.upper().replace("J", "I")
    seen = []
    matrix = []

    for char in key:
        if char not in seen and char.isalpha():
            seen.append(char)

    # Menambahkan huruf-huruf alfabet yang belum ada dalam kunci
    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"
    for char in alphabet:
        if char not in seen:
            seen.append(char)

    # Membuat matriks 5x5
    matrix = [seen[i:i + 5] for i in range(0, 25, 5)]
    return matrix

def find_position(matrix, char):
    # Menemukan posisi karakter dalam matriks
    for i, row in enumerate(matrix):
        if char in row:
            return i, row.index(char)
    return None

def playfair_encrypt(plaintext, key):
    matrix = generate_playfair_matrix(key)
    
    # Preprocessing plaintext
    plaintext = plaintext.upper().replace("J", "I").replace(" ", "")
    pairs = []
    i = 0
    while i < len(plaintext):
        # Jika pasangan huruf sama, tambahkan X
        if i + 1 < len(plaintext) and plaintext[i] == plaintext[i + 1]:
            pairs.append(plaintext[i] + 'X')
            i += 1
        elif i + 1 < len(plaintext):
            pairs.append(plaintext[i] + plaintext[i + 1])
            i += 2
        else:
            pairs.append(plaintext[i] + 'X')  # Jika jumlah huruf ganjil, tambahkan X
            i += 1
    
    ciphertext = ''
    
    for pair in pairs:
        row1, col1 = find_position(matrix, pair[0])
        row2, col2 = find_position(matrix, pair[1])

        # Aturan Playfair
        if row1 == row2:  # Jika di baris yang sama
            ciphertext += matrix[row1][(col1 + 1) % 5] + matrix[row2][(col2 + 1) % 5]
        elif col1 == col2:  # Jika di kolom yang sama
            ciphertext += matrix[(row1 + 1) % 5][col1] + matrix[(row2 + 1) % 5][col2]
        else:  # Jika di baris dan kolom yang berbeda
            ciphertext += matrix[row1][col2] + matrix[row2][col1]
    
    return ciphertext

# Input plaintext dan kunci
plaintexts = [
    "GOOD BROOM SWEEP CLEAN",
    "REDWOOD NATIONAL STATE PARK",
    "JUNK FOOD AND HEALTH PROMBLEMS"
]
key = "TEKNIK INFORMATIKA"

# Enkripsi tiap plaintext
for plaintext in plaintexts:
    ciphertext = playfair_encrypt(plaintext, key)
    print(f"Plaintext: {plaintext}")
    print(f"Ciphertext: {ciphertext}")
    print()
```

![hasil](https://github.com/user-attachments/assets/e4faa7a4-c5a3-491a-8756-b73abd61e5fb)


### Penjelasan:

1. Fungsi ```generate_playfair_matrix(key)```:
   
   Menghasilkan tabel 5x5 berdasarkan kunci yang diberikan, menggabungkan huruf "I" dan "J" serta menghilangkan karakter yang berulang.

2. Fungsi ```find_position(matrix, char)```:
   
   Menemukan posisi suatu karakter di dalam tabel Playfair.

3. Fungsi ```playfair_encrypt(plaintext, key)```:
   
   Melakukan enkripsi menggunakan aturan Playfair Cipher. Teks dienkripsi dengan pasangan huruf sesuai dengan aturan enkripsi Playfair (baris sama, kolom sama, baris-kolom berbeda).
