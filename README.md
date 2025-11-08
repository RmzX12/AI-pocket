# AI Pocket v1

Perangkat AI portabel bertenaga ESP32-C3 dengan fitur lengkap dan antarmuka intuitif.

## 🌟 Fitur Utama

### 🤖 Integrasi AI Chat
- **Dukungan Dual Gemini API**: Beralih antara dua API key Google Gemini
- Respons AI real-time dengan animasi loading
- Viewer respons yang bisa di-scroll
- Pembungkus teks otomatis dan formatting pintar

### 📡 Manajemen WiFi
- Scanner jaringan dengan indikator kekuatan sinyal
- Koneksi otomatis ke jaringan tersimpan
- Input password dengan keyboard virtual
- Timer auto-off (dapat dikonfigurasi, default 5 menit)
- Kemampuan untuk melupakan jaringan

### ⚡ Manajemen Daya
- **Visual Baterai**: Tampilan baterai animasi dengan efek charging
- **Statistik**: Detail voltase dan estimasi waktu
- **Grafik Daya**: Visualisasi data baterai historis
- Deteksi charging real-time
- Peringatan baterai kritis

### 🧮 Kalkulator
- Kalkulator lengkap
- Operasi: +, -, *, /
- Fungsi clear dan equals
- Penanganan error (pembagian dengan nol)

### ⚙️ Fitur Sistem
- **Info Sistem**: Monitoring heap memory, frekuensi CPU, loop rate
- **Menu Pengaturan**: Toggle WiFi auto-off, akses info sistem
- **Mode Tidur**: Deep sleep dengan wake-up tombol
- Pola LED interaktif untuk berbagai state

### 🎨 Antarmuka Pengguna
- Layar OLED (128x64)
- Keyboard virtual dengan 3 mode (huruf kecil, besar, angka)
- Indikator baterai dan WiFi di semua layar
- Progress bar untuk operasi
- Layar loading beranimasi

## 🔧 Kebutuhan Hardware

### Komponen
- Mikrokontroler ESP32-C3
- Layar OLED SSD1306 (128x64, I2C)
- 5 Tombol tekan (navigasi)
- LED built-in
- Baterai dengan rangkaian voltage divider
- Rangkaian deteksi charging

### Konfigurasi Pin
```
Layar OLED:
- SDA: GPIO 20
- SCL: GPIO 21

Tombol:
- UP: GPIO 5
- DOWN: GPIO 6
- LEFT: GPIO 3
- RIGHT: GPIO 4
- SELECT: GPIO 9

Lainnya:
- LED: GPIO 8
- BATTERY ADC: GPIO 0
- CHARGING DETECT: GPIO 1
```

### Rangkaian Baterai
- Rasio voltage divider: 2.0
- Range baterai: 3.3V - 4.2V
- Resolusi ADC: 12-bit

## 📦 Library yang Dibutuhkan

Install library ini via Arduino Library Manager:

```
- Wire (bawaan)
- WiFi (bawaan)
- HTTPClient (bawaan)
- Preferences (bawaan)
- Adafruit_GFX
- Adafruit_SSD1306
- ArduinoJson
```

## 🚀 Petunjuk Setup

### 1. Perakitan Hardware
1. Hubungkan layar OLED ke pin I2C (SDA=20, SCL=21)
2. Pasang semua tombol dengan konfigurasi pull-up
3. Hubungkan rangkaian monitoring baterai dengan voltage divider
4. Hubungkan rangkaian deteksi charging

### 2. Setup Software

1. **Dapatkan API Key Gemini**:
   - Kunjungi [Google AI Studio](https://aistudio.google.com)
   - Buat dua API key
   - Ganti di kode:
     ```cpp
     const char* geminiApiKey1 = "API_KEY_ANDA_1";
     const char* geminiApiKey2 = "API_KEY_ANDA_2";
     ```

2. **Konfigurasi Arduino IDE**:
   - Board: ESP32C3 Dev Module
   - Upload Speed: 921600
   - CPU Frequency: 160MHz
   - Flash Size: 4MB
   - Partition Scheme: Default

3. **Upload Kode**:
   - Verify dan compile
   - Upload ke ESP32-C3
   - Buka Serial Monitor (115200 baud)

### 3. Boot Pertama
1. Device akan menampilkan menu WiFi jika belum ada jaringan tersimpan
2. Scan dan koneksi ke WiFi
3. Kredensial tersimpan otomatis
4. Akses menu utama

## 📱 Panduan Penggunaan

### Navigasi Menu Utama
- **UP/DOWN**: Navigasi item menu
- **SELECT**: Pilih opsi

### Opsi Menu
1. **Chat AI**: Mulai percakapan AI
2. **WiFi Settings**: Kelola koneksi
3. **Calculator**: Buka kalkulator
4. **Power Central**: Manajemen baterai
5. **Settings**: Konfigurasi sistem

### Keyboard Virtual
- **Tombol Arah**: Gerakkan kursor
- **SELECT**: Masukkan karakter
- **Tombol #**: Ganti mode (a/A/#)
- **Tombol <**: Backspace
- **OK**: Kirim input

### Kalkulator
- **Tombol Arah**: Navigasi tombol
- **SELECT**: Tekan tombol
- **C**: Clear
- **=**: Hitung hasil

### Power Central
1. **Battery Visual**: Tampilan baterai beranimasi
2. **Statistics**: Info daya detail
3. **Power Graph**: Data historis (50 data poin)
4. **Back**: Kembali ke menu utama

### Pengaturan
- **WiFi Auto-Off**: Toggle timeout 5 menit
- **System Info**: Lihat statistik memori dan CPU
- **Sleep Mode**: Masuk deep sleep
- **Back**: Kembali ke menu utama

## 🎨 Pola LED

Setiap state memiliki perilaku LED unik:
- **Menu Utama**: Pola heartbeat
- **Loading**: Pulsing
- **Chat Response**: Efek breathing
- **Calculator**: Blink lambat
- **Layar Power**: Dinamis berdasarkan status baterai
- **System Info**: Pola seperti pelangi
- **Menu Sleep**: Fade lambat
- **Keyboard**: Blink sedang
- **WiFi**: Terkoneksi (lambat) / Terputus (cepat)
- **API Select**: Pola kompleks

## ⚡ Manajemen Daya

### Monitoring Baterai
- Update setiap 2 detik
- Rata-rata 10 sampel untuk stabilitas
- Kalkulasi persentase otomatis
- Deteksi charging dengan hysteresis

### WiFi Auto-Off
- Timeout dapat dikonfigurasi (default: 5 menit)
- Reset saat tombol ditekan
- Hemat daya saat idle
- Aktifkan/nonaktifkan di Settings

### Deep Sleep
- Wake-up via tombol SELECT
- Disconnect WiFi sebelum tidur
- Animasi fadeout LED
- Konsumsi daya ultra-rendah

## 🔧 Kustomisasi

### Modifikasi Timeout
```cpp
const unsigned long wifiTimeout = 300000; // WiFi (5 menit)
const unsigned long batteryCheckInterval = 2000; // Baterai (2 detik)
```

### Sesuaikan Kalibrasi Baterai
```cpp
#define BATTERY_MAX_VOLTAGE 4.2
#define BATTERY_MIN_VOLTAGE 3.3
#define VOLTAGE_DIVIDER_RATIO 2.0
```

### Ubah Pengaturan Display
```cpp
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define SCREEN_ADDRESS 0x3C
```

## 🐛 Troubleshooting

### Masalah Display
- Cek alamat I2C (default: 0x3C)
- Verifikasi koneksi SDA/SCL
- Coba resistor pull-up berbeda

### Koneksi WiFi Gagal
- Cek keakuratan password
- Pastikan jaringan 2.4GHz
- Pindah lebih dekat ke router
- Cek koneksi antena

### Pembacaan Baterai Salah
- Kalibrasi rasio voltage divider
- Cek koneksi ADC
- Verifikasi range voltase baterai

### Error API
- Verifikasi validitas API key
- Cek koneksi internet
- Pastikan kuota cukup
- Coba API key alternatif

### Tombol Tidak Responsif
- Cek debounce delay (default 200ms)
- Verifikasi konfigurasi pull-up
- Test dengan multimeter

## 📊 Spesifikasi Teknis

- **MCU**: ESP32-C3 (RISC-V, 160MHz)
- **RAM**: ~400KB tersedia
- **Flash**: 4MB
- **Display**: OLED 128x64 (I2C)
- **WiFi**: 802.11 b/g/n (2.4GHz)
- **Baterai**: Li-Po/Li-Ion (3.3V-4.2V)
- **ADC**: Resolusi 12-bit
- **Tombol**: 5x tactile switch
- **LED**: 1x programmable

## 🔐 Catatan Keamanan

- API key tersimpan di kode (pertimbangkan enkripsi)
- Kredensial WiFi tersimpan di Preferences
- Clear preferences untuk menghapus data tersimpan
- Tidak ada telemetri atau logging eksternal

## 📄 Lisensi

Proyek ini disediakan apa adanya untuk penggunaan edukasi dan personal.

## 🤝 Kontribusi

Kontribusi dipersilakan! Area untuk perbaikan:
- Provider AI tambahan
- Lebih banyak fungsi kalkulator
- Animasi kustom
- Optimasi daya
- Recovery error
- Dukungan lokalisasi
- Fitur deep sleep
## 📞 Dukungan

Untuk masalah dan pertanyaan:
1. Cek bagian troubleshooting
2. Verifikasi koneksi hardware
3. Test dengan konfigurasi minimal
4. Cek output serial monitor

---

**Proyek**: AI Pocket v1  
**Versi**: 1.0  
**Terakhir Update**: 2025 
**Status**: Rilis BETA 

Dibuat dengan ❤️ untuk penggemar ESP32-C3 dan AI enthusiast
