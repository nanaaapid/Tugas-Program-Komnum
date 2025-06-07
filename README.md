# 🧮 Tugas Program Komputasi Numerik – Stirling Interpolation

> Disusun oleh Kelompok A18 untuk menyelesaikan Tugas Program Mata Kuliah Komputasi Numerik  
> Dosen Pengampu: **Ibu Bilqis**

## 👥 Anggota Kelompok A18

1. **Umi Lailatul Khotimah** (5025241062)
2. **Nashwa Aulia Putri Diansyah** (5025241064)
3. **Nabilah Bunga Sulistia** (5025241073)
4. **Rhea Debora Sianturi** (5025241095)
5. **Mahirah Yasmin Aulia Mawahib** (5025241095)

---

## 📌 Deskripsi Singkat

Pada tugas ini, kelompok kami mendapat **Soal Nomor 28** yang membahas **Interpolasi Stirling**.
---

## 🛠️ Alur Program

### 1. Import Library

```python
import numpy as np
from sympy import Rational, factorial, simplify
```

### 2. Menyusun Tabel Selisih

Fungsi berikut akan menghitung **tabel selisih terhingga** (∆f(x), ∆²f(x), dst.) yang nanti digunakan dalam perhitungan utama.

```python
def compute_diff_table(y_vals):
    n = len(y_vals)
    diff = np.zeros((n, n))
    diff[:, 0] = y_vals
    for j in range(1, n):
        for i in range(n - j):
            diff[i, j] = diff[i + 1, j - 1] - diff[i, j - 1]
            if j <= 4:
                print(f"diff[{i}, {j}] = {round(diff[i, j], 2)}")
    return diff
```

   hasilnya nanti akan seperti isi pada tabel soal :

   ![Screenshot 2025-06-07 192131](https://github.com/user-attachments/assets/398e7134-8275-419f-a23f-659383b7ce3c)


### 3. Fungsi Utama: Stirling Interpolation
untuk mencari nilai h, s, dan setiap delta

```python
def stirling_interpolation(x_vals, y_vals, x0_val, x_target_val):
  x_vals = np.array(x_vals, dtype=float)
  y_vals = np.array(y_vals, dtype=float)
  
  h = x_vals[1] - x_vals[0]
  print(f"\nAll h are equal: h = {h}")
  
  idx = np.where(x_vals == x0_val)[0][0]
  S = round((x_target_val - x0_val) / h, 2)
  print(f"S = {S}")
  
  diff = compute_diff_table(y_vals)
  
  print("\nCalculating Stirling Interpolation")
  result = y_vals[idx]
  print(f"f0 = {result}")
  
  delta1 = (diff[idx][1] + diff[idx - 1][1]) / 2
  term1 = round(S * delta1,2)
  print(f"Term i=1: {term1}")
  result += term1
  
  delta2 = diff[idx - 1][2]
  term2 = round((round((S * (S - 1))/2, 2) * delta2),2)
  print(f"Term i=2: {term2}")
  result += term2
  
  delta3 = (diff[idx - 1][3] + diff[idx - 2][3]) / 2
  term3 = round((round((S * (S - 1) * (S - 2))/6, 2) * delta3),2)
  print(f"Term i=3: {term3}")
  result += term3
  
  delta4 = diff[idx - 2][4]
  term4 = round(((x_target_val/4) * round((S * (S - 1) * (S - 2))/6,2) * delta4),2)
  print(f"Term i=4: {term4}")
  result += term4
  
  y_interp = round(result, 2)
  return y_interp
```

   hasil :

   ![Screenshot 2025-06-07 192138](https://github.com/user-attachments/assets/62a92b80-06b3-489c-9179-2d1707751116)


### 4. Input Data & Eksekusi Interpolasi
kita input data yang diketahui, kemudian panggil fungsi `stirling_interpolation` untuk mencari hasil

```python
x_vals = [3, 6, 9, 12, 15, 18, 21, 24, 27]
y_vals = [-741, -186, 32121, 184956, 634575, 1673874, 3741549, 7451256, 13620771]

x0 = 15
xt = 16
yt = 897104

hasil = stirling_interpolation(x_vals, y_vals, x0, xt)

print(f"\nf({xt}) = {hasil}")
print(f"Et ={abs((yt - hasil)/yt) * 100 : .2f}")
```

Output hasil dari f(16) dan nilai Et seperti ini :

![Screenshot 2025-06-07 192146](https://github.com/user-attachments/assets/2e16ed28-1f1c-4c32-92b2-77de53813ad5)

   
