# Software Testing Plan (STP)

> **Aplikasi:** Rick and Morty App v1.0  
> **Referensi Kode:** [github.com/JohanArizona/RickAndMortyApp](https://github.com/JohanArizona/RickAndMortyApp)

---

## 1. Deskripsi Aplikasi

Rick and Morty App adalah aplikasi Android yang dibangun dengan **Kotlin + Jetpack Compose**, menggunakan Clean Architecture dan pola MVVM. Aplikasi memungkinkan pengguna menelusuri, mencari, dan mengelola daftar karakter favorit dari serial animasi *Rick and Morty* secara online maupun offline.

**Fitur utama:**
- Character list dengan infinite scrolling grid dan hero header
- Character detail dengan sticky nav bar, animasi fade-in, horizontal scroll episode & rekomendasi
- Advanced search: real-time debouncing, search history (Room DB), filter status/gender/species/type
- Favorites system dengan sinkronisasi real-time lintas layar (Home, Search, Detail)
- Offline-first menggunakan Room Database sebagai cache lokal
- Unit test pada layer Domain, Data, dan Presentation (JUnit + MockK)
- Native Splash Screen (Android 12+ API)

---

## 2. Software Quality Metrics

| Kategori | Nama Metrik | Formula | Target |
|---|---|---|---|
| **Process** | Test Execution Rate | (TC tereksekusi / Total TC direncanakan) × 100% | ≥ 90% |
| **Product** | Defect Density | Jumlah defect / Jumlah fitur utama (5 fitur) | ≤ 2 defect per fitur |
| **Product** | API Response Time | Waktu rata-rata request → data tampil di UI | ≤ 3 detik (koneksi normal) |

---

## 3. Testing Environment

| Komponen | Detail |
|---|---|
| **Hardware** | Perangkat Android fisik / emulator, min. API Level 24 (Android 7.0 Nougat) |
| **Software** | PC/Laptop + Android Studio, JDK 11+ |
| **Koneksi** | Internet aktif (online test) · Airplane mode (offline test) |
| **Monitoring** | Android Studio Logcat |

**Persiapan sebelum pengujian:**
1. Instalasi Android Studio dan setup emulator / perangkat fisik
2. Clone repository: `git clone https://github.com/JohanArizona/RickAndMortyApp`
3. Familiarisasi codebase: Clean Architecture (data / domain / presentation layer)
4. Review `README.md` dan `build.gradle.kts`
5. Pemahaman tools: JUnit 4.13.2, MockK 1.13.8, Google Truth 1.1.5

---

## 4. Test Details per Modul

### 4.1 Home Screen — Character List (`TEST-HOME`)

| Atribut | Detail |
|---|---|
| **Objective** | Memverifikasi daftar karakter tampil benar dalam grid dengan infinite scrolling, hero header, loading state, dan error state |
| **Test Class** | Functional Testing (Black Box) |
| **Test Level** | System Test |
| **Special Req.** | Internet aktif untuk TC-HOME-001–003 & 005 · Airplane mode untuk TC-HOME-004 |

**Test Cases:**

| TC ID | Deskripsi |
|---|---|
| TC-HOME-001 | Grid karakter tampil saat pertama kali buka app (online) |
| TC-HOME-002 | Infinite scroll memuat halaman berikutnya saat user scroll ke bawah |
| TC-HOME-003 | Loading indicator tampil saat data sedang dimuat |
| TC-HOME-004 | Error state + tombol Retry tampil saat koneksi gagal |
| TC-HOME-005 | Halaman tidak refresh ke default setelah kembali dari Detail Screen |

---

### 4.2 Character Detail Screen (`TEST-DETAIL`)

| Atribut | Detail |
|---|---|
| **Objective** | Memverifikasi halaman detail menampilkan info lengkap dengan UI sesuai: sticky nav, animasi fade, horizontal scroll episode & rekomendasi |
| **Test Class** | Functional Testing (Black Box) |
| **Test Level** | System Test |
| **Special Req.** | TC-DETAIL-005 memerlukan pengujian offline (karakter harus sudah dikunjungi saat online) |

**Test Cases:**

| TC ID | Deskripsi |
|---|---|
| TC-DETAIL-001 | Detail screen terbuka dengan data lengkap saat karakter di-tap dari Home |
| TC-DETAIL-002 | Daftar episode tampil dan dapat di-scroll horizontal |
| TC-DETAIL-003 | Section 'YOU MIGHT ALSO LIKE' tampil dan kartu dapat di-tap |
| TC-DETAIL-004 | Tombol Favorite berfungsi add/remove dari halaman detail |
| TC-DETAIL-005 | Data detail tersedia saat offline (karakter sudah pernah dibuka sebelumnya) |

---

### 4.3 Search Screen — Advanced Search (`TEST-SEARCH`)

| Atribut | Detail |
|---|---|
| **Objective** | Memverifikasi pencarian karakter dengan debouncing, riwayat pencarian (Room DB), dan filter lanjutan (Status, Gender, Species, Type) beserta kombinasinya |
| **Test Class** | Functional Testing (Black Box) |
| **Test Level** | System Test & Integration Test |
| **Special Req.** | Verifikasi debounce via Logcat · Internet aktif TC-001–010 · Offline TC-011 |

**Test Cases:**

| TC ID | Deskripsi |
|---|---|
| TC-SEARCH-001 | Pencarian berjalan setelah debounce delay, hasil relevan |
| TC-SEARCH-002 | Riwayat pencarian tersimpan dan dapat dipilih kembali |
| TC-SEARCH-003 | Filter Status menampilkan karakter sesuai status yang dipilih |
| TC-SEARCH-004 | Filter Gender menampilkan karakter sesuai gender yang dipilih |
| TC-SEARCH-005 | Filter Species menampilkan karakter sesuai species yang dipilih |
| TC-SEARCH-006 | Kombinasi filter menggunakan AND logic dengan hasil tepat |
| TC-SEARCH-007 | Filter Type menampilkan karakter sesuai type yang dipilih |
| TC-SEARCH-008 | Pencarian tidak ditemukan → empty state tampil |
| TC-SEARCH-009 | Reset filter mengembalikan kondisi awal, recent searches tetap |
| TC-SEARCH-010 | Penghapusan individual recent searches bekerja benar |
| TC-SEARCH-011 | Tanpa internet → pesan "No internet connection" tampil, tidak crash |

---

### 4.4 Favorite Screen — Favorites System (`TEST-FAVORITE`)

| Atribut | Detail |
|---|---|
| **Objective** | Memverifikasi sistem favorit: add/remove karakter, sinkronisasi real-time lintas layar (Home, Search, Detail, Favorite) |
| **Test Class** | Functional Testing (Black Box) & Integration Test |
| **Test Level** | System Test |
| **Special Req.** | Sinkronisasi real-time diverifikasi tanpa restart app · Data favorit diverifikasi di Room DB |

**Test Cases:**

| TC ID | Deskripsi |
|---|---|
| TC-FAV-001 | Tambah favorit dari Home Screen → muncul di Favorite Screen |
| TC-FAV-002 | Tambah favorit dari Detail Screen → muncul di Favorite Screen |
| TC-FAV-003 | Tambah favorit dari Search Screen → muncul di Favorite Screen |
| TC-FAV-004 | Hapus favorit → karakter hilang dari Favorite Screen secara real-time |
| TC-FAV-005 | Status favorit sinkron di semua layar tanpa perlu refresh |
| TC-FAV-006 | Empty state tampil saat tidak ada karakter difavoritkan |
| TC-FAV-007 | Data favorit tetap ada setelah cache aplikasi dibersihkan |

---

### 4.5 Offline Mode — Room DB Caching (`TEST-OFFLINE`)

| Atribut | Detail |
|---|---|
| **Objective** | Memverifikasi aplikasi berjalan tanpa internet untuk konten yang sudah dikunjungi, menggunakan Room DB sebagai cache, serta perilaku transisi online ↔ offline |
| **Test Class** | Functional Testing (Black Box) |
| **Test Level** | System Test |
| **Special Req.** | Airplane mode aktif saat pengujian · Verifikasi sumber data via Logcat (Room DB vs API) |

**Test Cases:**

| TC ID | Deskripsi |
|---|---|
| TC-OFFLINE-001 | Karakter yang sudah dimuat tetap tampil di Home Screen saat offline |
| TC-OFFLINE-002 | Detail karakter yang sudah pernah dibuka tampil dari cache saat offline |
| TC-OFFLINE-003 | Detail karakter yang muncul di 'YOU MIGHT ALSO LIKE' dapat dibuka saat offline |

---

### 4.6 Unit Tests — Automated (`TEST-UNIT`)

| Atribut | Detail |
|---|---|
| **Objective** | Memverifikasi logika bisnis layer Domain (UseCases), Data (Mappers), dan Presentation (ViewModels) melalui unit test terisolasi |
| **Test Class** | White Box Testing (Structural) |
| **Test Level** | Unit Test |
| **Special Req.** | Tidak memerlukan perangkat fisik — dijalankan di JVM |
| **Tools** | JUnit 4.13.2 · MockK 1.13.8 · kotlinx-coroutines-test · Google Truth 1.1.5 |
| **Run Command** | `./gradlew test` |
| **Report** | `app/build/reports/tests/testDebugUnitTest/index.html` |

**Test Cases:**

| TC ID | Layer | Deskripsi |
|---|---|---|
| TC-UNIT-001 | Data (Mapper) | `CharacterMapper.mapEntityToDomain()` — konversi entity ke domain, semua field + isFavorite |
| TC-UNIT-002 | Data (Mapper) | `CharacterStatus.fromString()` dengan nilai tidak dikenal → `Unknown` |
| TC-UNIT-003 | Domain (UseCase) | `GetCharactersUseCase` → `ResourceState.Success` dengan data |
| TC-UNIT-004 | Domain (UseCase) | `SearchCharactersUseCase` → `ResourceState.Empty` saat tidak ada hasil |
| TC-UNIT-005 | Domain (UseCase) | `AddFavoriteUseCase` → `favoriteRepo.addFavorite()` dipanggil tepat 1 kali |
| TC-UNIT-006 | Domain (UseCase) | `RemoveFavoriteUseCase` → `favoriteRepo.removeFavorite()` dipanggil tepat 1 kali |
| TC-UNIT-007 | Domain (UseCase) | `GetFavoritesUseCase` → Flow memancarkan list favorit dengan benar |
| TC-UNIT-008 | Domain (UseCase) | `SearchHistory`: add, get, dan remove riwayat pencarian |
| TC-UNIT-009 | Presentation (ViewModel) | `HomeViewModel` initial state — success case |
| TC-UNIT-010 | Presentation (ViewModel) | `HomeViewModel` — error state ditangani dengan benar |
| TC-UNIT-011 | Presentation (ViewModel) | `DetailViewModel` — uiState terupdate sesuai mock (success & error) |

---
