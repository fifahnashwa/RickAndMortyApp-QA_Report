# Testing Documentation — Rick and Morty App

Folder ini berisi dokumentasi lengkap proses **Software Quality Assurance (SQA)** terhadap aplikasi Android [RickAndMortyApp](https://github.com/JohanArizona/RickAndMortyApp).

---

## Dokumen

| Dokumen | Deskripsi | File |
|---|---|---|
| **STP** – Software Testing Plan | Perencanaan pengujian: scope, environment, jadwal, dan rincian test case per modul | [STP.md](./STP.md) |
| **STD** – Software Test Design | Desain pengujian detail: precondition, input, intermediate result, dan expected result setiap TC | [STD.md](./STD.md) |
| **STR** – Software Test Report | Rangkuman hasil eksekusi pengujian: status PASS/FAIL, defect, dan evaluasi metrik | [STR.md](./STR.md) |

---

## 🎯 Aplikasi yang Diuji

| Atribut | Detail |
|---|---|
| **Nama** | Rick and Morty App |
| **Versi** | 1.0 (versionCode 1) |
| **Application ID** | `com.takehomechallenge.arizona` |
| **Platform** | Android (minSdk 24 – targetSdk 36) |
| **Bahasa** | Kotlin + Jetpack Compose |
| **Arsitektur** | Clean Architecture + MVVM |
| **Repository** | [github.com/JohanArizona/RickAndMortyApp](https://github.com/JohanArizona/RickAndMortyApp) |

---

## Modul yang Diuji

| ID | Modul | Jenis | Jumlah TC |
|---|---|---|---|
| TEST-HOME | Home Screen — Character List | Manual (Black Box) | 5 |
| TEST-DETAIL | Character Detail Screen | Manual (Black Box) | 5 |
| TEST-SEARCH | Search Screen — Advanced Search | Manual (Black Box) | 11 |
| TEST-FAVORITE | Favorite Screen — Favorites System | Manual (Black Box) | 7 |
| TEST-OFFLINE | Offline Mode — Room DB Caching | Manual (Black Box) | 3 |
| TEST-UNIT | Unit Tests — Domain / Data / Presentation | Automated (JUnit + MockK) | 11 |
| | **Total** | | **42** |

---

## SQA Metrics

| Metrik | Formula | Target |
|---|---|---|
| **Test Execution Rate** | (TC tereksekusi / Total TC direncanakan) × 100% | ≥ 90% |
| **Defect Density** | Jumlah defect / Jumlah fitur utama (5 fitur) | ≤ 2 defect/fitur |
| **API Response Time** | Rata-rata waktu request → data tampil di UI | ≤ 3 detik |

---

## Testing Environment

| Komponen | Keterangan |
|---|---|
| **Hardware** | Perangkat Android fisik / emulator, min. API Level 24 (Android 7.0) |
| **Software** | Android Studio, JDK 11+ |
| **Online testing** | Koneksi internet aktif |
| **Offline testing** | Airplane mode aktif, cache dibersihkan |
| **Automated testing** | JVM (tidak perlu perangkat fisik) |
| **Monitoring** | Android Studio Logcat |
