
## Authors

- Alexandro Nesta
- NIM 202131148
- Pengolahan Citra Kelas A

## Landasan Teori

Pengolahan Citra: Deteksi plat nomor seringkali melibatkan pengolahan citra untuk mengidentifikasi dan memisahkan plat nomor dari latar belakang. Teknik pengolahan citra yang umum digunakan termasuk operasi morfologi, deteksi tepi, segmentasi warna, dan filtrasi.

Segmentasi: Salah satu langkah penting dalam deteksi plat nomor adalah segmentasi, yaitu proses memisahkan plat nomor dari latar belakang atau objek lainnya dalam citra. Metode segmentasi dapat mencakup segmentasi berdasarkan warna, teksur, atau metode berbasis kontur.

Selain itu landasan teori untuk codingan dibawah meliputi beberapa konsep dasar dalam pengolahan citra dan pemrosesan citra menggunakan library OpenCV (cv2) dan matplotlib.

1. OpenCV (Open Source Computer Vision Library):
- OpenCV adalah library populer yang digunakan untuk pengolahan gambar dan pemrosesan citra.
- Mendukung berbagai operasi pengolahan citra seperti transformasi, deteksi tepi, segmentasi, deteksi objek, dan banyak lagi.
- Menyediakan fungsi dan algoritma efisien untuk manipulasi dan analisis gambar.
- Dalam codingan tersebut, cv2 digunakan untuk membaca gambar, mengubah ke skala abu-abu, deteksi tepi, operasi morfologi, pencarian kontur, dan manipulasi gambar lainnya.

2. Pengolahan Citra Skala Abu-abu (Grayscale):
- Representasi gambar dalam skala abu-abu hanya menggunakan satu saluran warna (intensitas kecerahan) daripada tiga saluran warna RGB.
- Mengubah gambar ke skala abu-abu dapat mempermudah operasi seperti deteksi tepi, segmentasi, dan analisis citra.
- Dalam codingan tersebut, gambar asli diubah ke skala abu-abu menggunakan fungsi `cv2.cvtColor()`.

3. Deteksi Tepi (Edge Detection):
- Deteksi tepi merupakan langkah penting dalam analisis citra yang bertujuan untuk menemukan perubahan tajam dalam intensitas citra.
- Metode Canny adalah salah satu algoritma deteksi tepi yang populer yang digunakan dalam codingan tersebut.
- Fungsi `cv2.Canny()` digunakan untuk mendeteksi tepi dalam gambar skala abu-abu.

4. Operasi Morfologi:
- Operasi morfologi adalah operasi pengolahan citra yang dilakukan pada representasi biner (hitam dan putih) dari gambar.
- Digunakan untuk memanipulasi bentuk dan struktur objek dalam gambar.
- Dalam codingan tersebut, operasi morfologi dilakukan untuk meningkatkan tepi hasil deteksi menggunakan fungsi `cv2.morphologyEx()`.

5. Kontur (Contours):
- Kontur adalah kurva yang membentuk batas objek yang terpisah dalam gambar.
- Dalam codingan tersebut, fungsi `cv2.findContours()` digunakan untuk menemukan kontur dalam gambar biner hasil deteksi tepi.

6. Bounding Rectangle:
- Bounding rectangle adalah persegi panjang yang melingkupi objek berdasarkan konturnya.
- Digunakan untuk membatasi area objek yang terdeteksi.
- Dalam codingan tersebut, fungsi `cv2.rectangle()` digunakan untuk menggambar bounding rectangle di sekitar plat nomor.

7. Slicing:
- Slicing adalah teknik dalam Python untuk memotong dan mengambil bagian dari array atau list.
- Dalam codingan tersebut, slicing digunakan untuk memotong wilayah gambar yang berisi plat nomor menggunakan koordinat bounding rectangle.

8. Visualisasi dengan matplotlib:
- Matplotlib adalah library visualisasi data yang populer di Python.
- Digunakan untuk membuat plot dan menampilkan gambar secara interaktif.
- Dalam codingan tersebut, matplotlib digunakan untuk menampilkan gambar asli, plat nomor terdeteksi, gambar biner, dan gambar tepi dalam satu tampilan menggunakan fungsi `plt.subplot()` dan `plt.imshow()`.

Dengan menggunakan konsep-konsep di atas, codingan tersebut mengimplementasikan alur pengolahan gambar untuk mendeteksi plat nomor dalam sebuah gambar. Langkah-langkahnya meliputi konversi gambar ke skala abu-abu, deteksi tepi, operasi morfologi, pencarian kontur, pemilihan kontur terbesar, dan pembuatan bounding rectangle di sekitar plat nomor. Selanjutnya, gambar-gambar yang relevan ditampilkan menggunakan matplotlib untuk visualisasi dan analisis lebih lanjut.
## Tutorial Untuk Menjalankan Program

- Ambil foto plat nomor kendaraan yang ingin dideteksi
- Sertakan rincian foto yang telah diambil
- Pindahkan foto dalam satu folder yang sama agar gambar mudah untuk dibaca 
- Kemudian copy codingan yang ada dibawah ini ke dalam Jupyter Notebook
- Pada bagian image_path = "plat22.jpg" bisa digantikan dengan nama foto yang kalian pakai
- Kemudian Run Program
## Codingan

Jalankan codingan dibawah ini

```
# Untuk Import Library
import cv2
import numpy as np
from matplotlib import pyplot as plt
```
```
def detect_license_plate(image_path):
    # Membaca gambar asli
    image = cv2.imread(image_path)

    # Mengubah gambar menjadi ke skala abu-abu
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

    # Perform edge detection
    edges = cv2.Canny(gray, 70, 500)

    # Melakukan operasi morfologi untuk meningkatkan tepi
    kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (5, 5))
    edges  = cv2.morphologyEx(edges, cv2.MORPH_CLOSE, kernel)

    # Untuk Temukan kontur plat nomor
    contours, _ = cv2.findContours(edges, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

    # Filter and sort the contours berdasarkan areanya
    contours = sorted(contours, key=cv2.contourArea, reverse=True)[:1]

    # Iterate over the contours and draw bounding rectangles around the license plates
    for contour in contours:
        x, y, w, h = cv2.boundingRect(contour)
        w = 300  # Panjang
        h = 150  # Lebar
        cv2.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 3)

        # Untuk Pangkas wilayah plat nomor
        plate_image = image[y:y + h, x:x + w]
        plate_binary = gray[y:y + h, x:x + w]
        plate_edges = edges[y:y + h, x:x + w]

    # Menampilkan the original image, detected license plate, binary image, and edges image
    plt.figure(figsize=(10, 10))

    # Original image
    plt.subplot(2, 2, 1)
    plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
    plt.title("Original Image")


    # Plat nomor terdeteksi
    plt.subplot(2, 2, 2)
    plt.imshow(cv2.cvtColor(plate_image, cv2.COLOR_BGR2RGB))
    plt.title("Plat Terdeteksi")


    # Binary image
    plt.subplot(2, 2, 3)
    plt.imshow(plate_binary, cmap="gray")
    plt.title("Binary Image")


    # Edges image
    plt.subplot(2, 2, 4)
    plt.imshow(cv2.cvtColor(plate_edges, cv2.COLOR_BGR2RGB))
    plt.title("Edges Image")


    # Show the plots
    plt.tight_layout()
    plt.show()

# Path to the image
image_path = "PlatMotorJauh.jpg"

# Call the function to detect the license plate
detect_license_plate(image_path)
```


## Penjelasan

Codingan di atas merupakan implementasi untuk mendeteksi plat nomor pada sebuah gambar menggunakan library OpenCV (cv2) dan matplotlib di Python.

Berikut adalah penjelasan langkah-langkah utama dalam codingan tersebut:

1. Mengimpor library yang diperlukan:
   - cv2: Library utama untuk pengolahan gambar (OpenCV).
   - numpy (np): Library untuk melakukan operasi matematika pada array.
   - pyplot dari matplotlib: Library untuk membuat plot dan visualisasi gambar.

2. Membuat fungsi `detect_license_plate` yang akan menerima path gambar sebagai argumen.
   - Fungsi ini akan membaca gambar asli menggunakan `cv2.imread()`.
   - Mengubah gambar menjadi skala abu-abu menggunakan `cv2.cvtColor()`.
   - Melakukan deteksi tepi pada gambar abu-abu menggunakan `cv2.Canny()`.
   - Meningkatkan tepi menggunakan operasi morfologi dengan kernel persegi (`cv2.getStructuringElement()` dan `cv2.morphologyEx()`).
   - Menggunakan `cv2.findContours()` untuk menemukan kontur objek pada gambar.
   - Mengurutkan kontur berdasarkan luasnya dan memilih kontur terbesar.
   - Menggambar persegi pembatas (bounding rectangle) di sekitar plat nomor menggunakan `cv2.rectangle()`.
   - Memotong wilayah gambar yang berisi plat nomor menggunakan slicing.
   - Menampilkan gambar asli, plat nomor terdeteksi, gambar biner (binary image), dan gambar tepi (edges image) menggunakan matplotlib.

3. Menentukan path gambar (`image_path`) yang akan digunakan sebagai input untuk fungsi `detect_license_plate`.

4. Memanggil fungsi `detect_license_plate(image_path)` untuk menjalankan proses deteksi plat nomor pada gambar.

Perlu diketahui bahwa codingan ini didesain untuk memproses suatu gambar dengan plat nomor yang jelas terlihat. Hasil deteksi dan pemotongan plat nomor mungkin tidak akurat jika diterapkan pada gambar dengan kondisi yang berbeda.
## Bukti Gambar

![alt text](https://github.com/AlxNesta/PA-PC_202131148_AlexndroNesta_A/blob/main/PlatMotorJauh.jpg?raw=true)

![alt text](https://github.com/AlxNesta/PA-PC_202131148_AlexndroNesta_A/blob/main/Screenshot_Rincian%20Gambar.jpg?raw=true)


## Hasil Output

![alt text](https://github.com/AlxNesta/PA-PC_202131148_AlexndroNesta_A/blob/main/Hasil.PNG?raw=true)