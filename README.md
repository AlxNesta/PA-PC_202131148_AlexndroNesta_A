
## Authors

- Alexandro Nesta
- NIM 202131148
- Pengolahan Citra Kelas A


# Deteksi Plat Nomor

Pendeteksi plat nomor adalah sebuah aplikasi yang menggunakan teknologi pengolahan gambar dan pengenalan karakter untuk mengenali dan mendeteksi plat nomor pada gambar atau video.

Proyek ini didasarkan pada penggunaan pemrosesan gambar dan teknik deteksi objek untuk mendeteksi plat nomor motor pada sebuah gambar. Berikut adalah beberapa teori yang mendukung proyek ini:

1. **Pemrosesan Gambar**: Pemrosesan gambar adalah proses memanipulasi gambar digital untuk menghasilkan informasi yang lebih berguna. Dalam proyek ini, kita menggunakan library OpenCV untuk memproses gambar, seperti mengubah gambar ke skala abu-abu, menerapkan thresholding, dan mendeteksi tepi pada gambar.

2. **Deteksi Objek**: Deteksi objek adalah salah satu cabang dari visi komputer yang bertujuan untuk mengidentifikasi dan melokalisasi objek tertentu dalam gambar atau video. Dalam proyek ini, kita menggunakan pendekatan deteksi objek menggunakan Cascade Classifier, yaitu dengan menggunakan model yang telah dilatih sebelumnya untuk mendeteksi plat nomor motor berdasarkan fitur-fitur yang ada pada plat nomor.

3. **Thresholding**: Thresholding adalah teknik pemrosesan gambar yang digunakan untuk mengubah gambar menjadi gambar biner (hitam-putih) dengan memilih nilai ambang (threshold) tertentu. Dalam proyek ini, kita menggunakan thresholding untuk mengubah gambar plat nomor menjadi gambar biner sehingga memudahkan dalam mendeteksi tepi pada plat nomor.

4. **Deteksi Tepi (Edge Detection)**: Deteksi tepi adalah teknik yang digunakan untuk menemukan perubahan yang tajam dalam intensitas piksel pada gambar. Dalam proyek ini, kita menggunakan algoritma Canny Edge Detection untuk mendeteksi tepi pada gambar plat nomor. Teknik ini membantu dalam mengisolasi garis tepi plat nomor motor yang berguna untuk proses pengenalan karakter plat nomor lebih lanjut.

Dalam keseluruhan, proyek ini menggabungkan pemrosesan gambar, deteksi objek, dan teknik pengolahan lainnya untuk mendeteksi plat nomor motor pada gambar. Dengan menggunakan metode ini, kita dapat mengambil gambar plat nomor, mengubahnya menjadi gambar biner, dan mendeteksi tepi plat nomor untuk langkah-langkah selanjutnya seperti pengenalan karakter plat nomor.

## Bukti Gambar

![alt text](https://github.com/AlxNesta/PA-PC_202131148_AlexndroNesta_A/blob/main/PlatMotor1.jpg?raw=true)

![alt text](https://github.com/AlxNesta/PA-PC_202131148_AlexndroNesta_A/blob/main/Screenshot_20230705-054425_Gallery.jpg?raw=true)


## Codingan

Jalankan codingan dibawah ini

```
# Untuk Import Library
import cv2
import matplotlib.pyplot as plt
```
```
# Fungsi untuk menampilkan gambar menggunakan Matplotlib
def show_image(title, image, position):
    plt.subplot(2, 2, position)
    plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
    plt.title(title)
    plt.axis('on')
```
```
# Membaca gambar asli
image = cv2.imread('plat12.jpg')
```
```
# Mengubah gambar menjadi ke skala abu-abu
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
```
```
# Menggunakan CascadeClassifier untuk mendeteksi wajah pada gambar
classifier = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_russian_plate_number.xml')
plates = classifier.detectMultiScale(gray, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30))

# Jika plat nomor terdeteksi
if len(plates) > 0:
    # Mengambil koordinat dari area plat nomor
    (x, y, w, h) = plates[0]
    
    # Crop area plat nomor
    plate_image = image[y:y+h, x:x+w]
    
    # Konversi ke skala abu-abu
    gray_plate = cv2.cvtColor(plate_image, cv2.COLOR_BGR2GRAY)
    
    # Thresholding gambar plat nomor menjadi biner
    _, binary_plate = cv2.threshold(gray_plate, 127, 255, cv2.THRESH_BINARY)
    
    # Mendeteksi tepi pada gambar plat nomor
    edges = cv2.Canny(binary_plate, 100, 200)
    
    # Menampilkan gambar-gambar hasil dengan susunan 2x2
    plt.figure(figsize=(12, 8))
    
    show_image('Gambar Asli', image, 1)
    show_image('Gambar Plat Nomor', plate_image, 2)
    show_image('Gambar Binary', binary_plate, 3)
    show_image('Gambar Tepi', edges, 4)
    
    plt.tight_layout()
    plt.show()
else:
    print("Plat nomor tidak terdeteksi pada gambar.")
```


## Penjelasan

Kodingan di atas adalah contoh implementasi pengolahan gambar menggunakan library OpenCV dan Matplotlib dalam bahasa pemrograman Python.

- Pertama, kita mengimpor library yang diperlukan, yaitu `cv2` untuk OpenCV dan `matplotlib.pyplot` untuk Matplotlib. Kemudian, kita mendefinisikan sebuah fungsi bernama `show_image` yang akan digunakan untuk menampilkan gambar menggunakan Matplotlib. Fungsi ini akan menerima tiga parameter: judul gambar, gambar yang akan ditampilkan, dan posisi gambar pada susunan 2x2.

- Selanjutnya, kita membaca gambar asli dengan menggunakan fungsi `cv2.imread()`. Gambar tersebut disimpan dalam variabel `image`.

- Kemudian, kita mengubah gambar menjadi skala abu-abu dengan menggunakan fungsi `cv2.cvtColor()` dan menyimpannya dalam variabel `gray`. Skala abu-abu digunakan untuk mempermudah pemrosesan gambar selanjutnya.

- Lalu, kita melakukan pengecekan jika terdapat plat nomor yang terdeteksi. Variabel `plates` yang tidak terdefinisi dalam kodingan ini seharusnya berisi informasi mengenai deteksi plat nomor. Jika `len(plates) > 0`, artinya terdapat plat nomor yang terdeteksi.

- Jika ada plat nomor yang terdeteksi, kita mengambil koordinat (x, y, w, h) dari area plat nomor pertama. Koordinat ini menunjukkan posisi plat nomor dalam gambar.

- Selanjutnya, kita melakukan cropping atau pemotongan gambar asli (`image`) berdasarkan koordinat plat nomor yang didapatkan sebelumnya. Hasil cropping disimpan dalam variabel `plate_image`.

- Selanjutnya, kita mengubah gambar plat nomor menjadi skala abu-abu dengan menggunakan fungsi `cv2.cvtColor()` dan menyimpannya dalam variabel `gray_plate`.

- Kemudian, kita melakukan thresholding pada gambar plat nomor untuk menghasilkan gambar biner menggunakan fungsi `cv2.threshold()`. Thresholding adalah proses mengubah gambar menjadi gambar biner (hitam putih) berdasarkan nilai ambang tertentu. Dalam contoh ini, kita menggunakan nilai ambang 127, yang berarti piksel dengan intensitas di atas 127 akan diubah menjadi putih (255), sedangkan piksel dengan intensitas di bawah 127 akan diubah menjadi hitam (0). Hasil thresholding disimpan dalam variabel `binary_plate`.

- Selanjutnya, kita mendeteksi tepi pada gambar plat nomor menggunakan metode Canny dengan fungsi `cv2.Canny()`. Deteksi tepi (edge detection) adalah proses untuk menemukan garis tepi atau batas objek dalam gambar. Hasil deteksi tepi disimpan dalam variabel `edges`.

- Terakhir, kita menggunakan Matplotlib untuk menampilkan gambar-gambar hasil dalam susunan 2x2. Kita membuat sebuah figure dengan ukuran 12x8 piksel menggunakan `plt.figure(figsize=(12, 8))`. Kemudian, kita menggunakan fungsi `show_image()` yang telah kita definisikan sebelumnya untuk menampilkan gambar asli, gambar plat nomor, gambar biner plat nomor, dan gambar tepi. Setiap gambar ditampilkan dengan judul yang sesuai. 

- Setelah itu, kita menggunakan `plt.tight_layout()` untuk mengatur tata letak gambar agar lebih rapi, dan `plt.show()` untuk menampilkan keseluruhan gambar.

- Jika tidak ada plat nomor yang terdeteksi pada gambar, maka akan ditampilkan pesan "Plat nomor tidak terdeteksi pada gambar."
## Hasil Output

![alt text](https://github.com/AlxNesta/PA-PC_202131148_AlexndroNesta_A/blob/main/HasilOutput.PNG?raw=true)