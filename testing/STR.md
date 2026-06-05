# Software Test Report (STR)

> **Aplikasi:** Rick and Morty App v1.0 — Android (minSdk 24 – targetSdk 36)
> **Referensi Kode:** [github.com/JohanArizona/RickAndMortyApp](https://github.com/JohanArizona/RickAndMortyApp)
> **Periode Pengujian:** 9-11 Mei 2026 

---

**Environment:**

| Komponen | Detail |
|---|---|
| **Perangkat** | Android fisik/emulator, min. API Level 24 (Android 7.0 Nougat) |
| **PC/Laptop** | Intel Core i7 · RAM 20 GB · Windows 11 |
| **JDK** | JDK 11 |
| **Android Studio** | Android Studio Panda 4 |
| **Test Framework** | JUnit 4.13.2 · MockK 1.13.8 · kotlinx-coroutines-test · Google Truth 1.1.5 |

---

## Ringkasan Eksekusi

| Metrik | Target | Aktual | Status |
|---|---|---|---|
| **Total Test Cases** | 42 | 42 | — |
| **Total PASS** | — | 42 | — |
| **Total FAIL** | — | 0 | — |
| **Test Execution Rate** | >= 90% | 100% | PASS |
| **Defect Density** | <= 2/fitur | 0.2 (1 defect / 5 fitur) | PASS |
| **API Response Time** | <= 3 detik | < 1 detik | PASS |

---

## Hasil per Modul

### TEST-HOME — Home Screen · Character List

| TC ID | Hasil Eksekusi | Status | Catatan |
|---|---|---|---|
| TC-HOME-001 | Aplikasi dibuka → hero header 'Rick and Morty' tampil beserta rating IMDb, genre, deskripsi. Grid karakter 2 kolom tampil. Tidak ada pesan error. | PASS | — |
| TC-HOME-002 | Scroll ke bawah → layar berhenti sebentar → karakter halaman berikutnya muncul otomatis tanpa tap. | PASS | — |
| TC-HOME-003 | Karakter halaman berikutnya otomatis dimuat saat scroll mendekati item terakhir. | PASS | Loading indicator tampil secara visual namun tidak berhasil di-capture karena durasinya sangat singkat. |
| TC-HOME-004 | Airplane mode aktif + cache bersih → hero header tetap tampil, section Characters menampilkan ikon peringatan merah + pesan *'No internet connection and no cached data'* + tombol **Retry** hijau. Tidak crash. | PASS | — |
| TC-HOME-005 | Dari Home yang sudah di-scroll ke halaman 2, tap *Blue Footprint Guy* → Detail Screen → back. Home kembali ke posisi scroll yang sama persis. | PASS | — |

**5/5 PASS**

---

### TEST-DETAIL — Character Detail Screen

| TC ID | Hasil Eksekusi | Status | Catatan |
|---|---|---|---|
| TC-DETAIL-001 | Detail *Blim Blam* terbuka. Semua field lengkap: nama, status (Alive), species (Alien), gender (Male), type (Korblock), origin (unknown), last location (Earth - Replacement Dimension), gambar. Tidak ada field kosong. | PASS | — |
| TC-DETAIL-002 | Section *Episode Appearances* tampil — Episode 47, 48, 49 terlihat. Swipe kiri/kanan lancar, tidak crash. | PASS | — |
| TC-DETAIL-003 | Section *YOU MIGHT ALSO LIKE* tampil di bawah Detail *Leonard Smith*. Tap *Crocubot* → Detail Crocubot terbuka (Dead, Animal, Male, Worldender's lair, Episode 25). | PASS | — |
| TC-DETAIL-004 | Tap pertama pada *Crocubot*: ikon `FavoriteBorder` → `Favorite`, muncul di Favorite Screen real-time. Tap kedua: ikon kembali ke border, hilang dari Favorite Screen. Tidak crash. | PASS | — |
| TC-DETAIL-005 | Airplane mode aktif → Detail *Rick Sanchez* terbuka dari cache: nama, status (Alive), species (Human), gender (Male), origin (Earth C-137), location (Citadel of Ricks), gambar, episode list, dan *YOU MIGHT ALSO LIKE* tampil. Tidak ada error koneksi. | PASS | — |

**5/5 PASS**

---

### TEST-SEARCH — Search Screen · Advanced Search

| TC ID | Hasil Eksekusi | Status | Catatan |
|---|---|---|---|
| TC-SEARCH-001 | Logcat mengkonfirmasi API hanya dipanggil 1 kali (`name=rick`) pada pukul 15:22:15 — tidak ada call untuk `r`, `ri`, `ric`. Hasil menampilkan karakter mengandung 'rick'. | PASS | Diverifikasi via Logcat OkHttp. |
| TC-SEARCH-002 | Search field kosong → *Recent Searches* tampil otomatis (rick, ric, ri, r, ri). Tap riwayat → field terisi 'rick' → hasil langsung muncul (Rick Sanchez, Adjudicator Rick, dll). | PASS | — |
| TC-SEARCH-003 | Filter Status 'Alive' + keyword 'beth' → hanya Beth Smith, Wasp Beth, Beth Smith (Alive) muncul. Beth's Mytholog & Evil Beth Clone (Dead) tidak muncul. | PASS | Filter hanya bekerja jika search field terisi. |
| TC-SEARCH-004 | Filter Gender 'Male' + keyword 'morty' → hanya karakter Male (Cyclops Morty, Eric Stoltz Mask Morty, Evil Morty, Fat Morty). Karakter gender Unknown tidak muncul. | PASS | Filter hanya bekerja jika search field terisi. |
| TC-SEARCH-005 | Filter Species 'Human' + keyword 'googah' → sebelum filter: Alien Googah muncul. Setelah filter: 'No character found' karena tidak ada Googah berspesies Human. | PASS | Filter hanya bekerja jika search field terisi. |
| TC-SEARCH-006 | Filter Status Alive + Gender Male + keyword 'jerry' → Jerry Smith, Jerry 5-126, Celebrity Jerry muncul (semua Alive & Male). Evil Jerry Clone & Ideal Jerry (Dead) tidak muncul. AND logic terkonfirmasi. | PASS | Filter hanya bekerja jika search field terisi. |
| TC-SEARCH-007 | Filter Type 'Human with antennae' + keyword 'morty' → dari banyak karakter Morty, menyempit ke 1 karakter: *Antenna Morty*. Filter Type terkonfirmasi bekerja. | PASS | Filter hanya bekerja jika search field terisi. |
| TC-SEARCH-008 | Keyword 'XYZabc123notexist' → empty state dengan ikon kaca pembesar dan pesan *'No character found'*. Tidak crash. | PASS | — |
| TC-SEARCH-009 | Filter Status Alive + Gender Male + Species Humanoid aktif → tap Reset → semua filter kembali kosong. Recent Searches tetap tampil normal. | PASS | — |
| TC-SEARCH-010 | Recent Searches 10 item (abra, albradolf, alien, morty, Jerry, J, je, jee, XYZabc123notexist, XYZabc123). Hapus satu per satu via tombol x → menyusut ke 5 item (v, jerry, googah, beth, eth). | PASS | — |
| TC-SEARCH-011 | Airplane mode aktif + keyword 'beth' → pesan *'No internet connection'* tampil merah di tengah layar. Tidak crash. | PASS | — |

**11/11 PASS**

---

### TEST-FAVORITE — Favorite Screen · Favorites System

| TC ID | Hasil Eksekusi | Status | Catatan |
|---|---|---|---|
| TC-FAV-001 | Tap `FavoriteBorder` pada *Abradolf Lincler* di Home → ikon jadi filled, muncul di Favorite Screen tanpa restart. | PASS | — |
| TC-FAV-002 | Tap `FavoriteBorder` di Detail *Agency Director* → ikon filled, muncul di Favorite Screen bersama Abradolf Lincler. Tidak perlu restart. | PASS | — |
| TC-FAV-003 | Keyword 'alien' di Search → tap `FavoriteBorder` pada *Arcade Alien* → muncul di Favorite Screen bersama 2 karakter sebelumnya. | PASS | — |
| TC-FAV-004 | Dari Favorite Screen (3 karakter), tap `Favorite` pada *Agency Director* → langsung hilang real-time. Tersisa Arcade Alien & Abradolf Lincler. | PASS | — |
| TC-FAV-005 | *Abradolf Lincler* difavoritkan dari Home (filled) → di Search Screen dengan keyword 'abra': ikon filled → di Detail Screen: ikon filled. Sinkronisasi di 3 layar terkonfirmasi tanpa refresh. | PASS | — |
| TC-FAV-006 | Semua favorit dihapus → Favorite Screen menampilkan ikon hati abu-abu + pesan *'No favorites yet. Mark characters to see them here.'* Tidak crash. | PASS | — |
| TC-FAV-007 | Sebelum clear cache: 4 karakter favorit tersimpan. Setelah clear cache (Cache: 0B) → buka ulang → keempat karakter favorit masih ada. Data favorit persisten di Room DB (User Data: 684kB), tidak terpengaruh clear cache. | PASS | — |

**7/7 PASS**

---

### TEST-OFFLINE — Offline Mode · Room DB Caching

| TC ID | Hasil Eksekusi | Status | Catatan |
|---|---|---|---|
| TC-OFFLINE-001 | Airplane mode aktif → Home Screen menampilkan hero header dan grid karakter ter-cache (Beth Sanchez, Beth Smith, Beth's Mytholog, dll). Tidak ada pesan error. | PASS | — |
| TC-OFFLINE-002 | Airplane mode aktif → Detail *Beth Smith* terbuka dari cache: nama, status (Alive), species (Human), gender (Female), origin, location, Episode 10, dan *YOU MIGHT ALSO LIKE* tampil. Tidak ada error. | PASS | [WARNING] Ikon gender menampilkan simbol laki-laki meskipun karakter bergender Female → **DEF-001** |
| TC-OFFLINE-003 | Airplane mode aktif → Detail beberapa karakter ter-cache yang belum pernah di-klik berhasil dibuka: Bootleg Portal Chemist Rick (Dead), Blue Shirt Morty (unknown), Rich Plutonian (Alive). Semua field tampil lengkap. | PASS | [WARNING] Ikon gender statis laki-laki → **DEF-001** |

**3/3 PASS**

---

### TEST-UNIT — Unit Tests · Automated (JUnit + MockK)

| TC ID | Layer | Hasil Eksekusi | Waktu | Status |
|---|---|---|---|---|
| TC-UNIT-001 | Data | `mapEntityToDomain` converts all fields correctly when `isFavorite=true`. Semua assertion lolos: name, status, gender, isFavorite. | 0.001s | PASS |
| TC-UNIT-002 | Data | `CharacterStatus.fromString()` returns `Unknown` for unrecognized value. Terkonfirmasi. | 0s | PASS |
| TC-UNIT-003 | Domain | `GetCharactersUseCase` → `ResourceState.Success` dengan `data.size == 1`. | 0.005s | PASS |
| TC-UNIT-004 | Domain | `SearchCharactersUseCase` → `ResourceState.Empty` saat tidak ada hasil. | 0.008s | PASS |
| TC-UNIT-005 | Domain | `AddFavoriteUseCase` → `coVerify { addFavorite(character) }` dipanggil tepat 1 kali. | 0.041s | PASS |
| TC-UNIT-006 | Domain | `RemoveFavoriteUseCase` → `coVerify { removeFavorite(42) }` dipanggil tepat 1 kali. | 0.060s | PASS |
| TC-UNIT-007 | Domain | `GetFavoritesUseCase` → Flow memancarkan list size == 1, tidak ada exception. | 0.099s | PASS |
| TC-UNIT-008 | Domain | `addToHistory`, `getHistory`, `removeFromHistory` — ketiga operasi search history terkonfirmasi. | 0.176s | PASS |
| TC-UNIT-009 | Presentation | `HomeViewModel` initial state: `isLoading=false`, `characters.isNotEmpty()`, `page=2`. | 0.007s | PASS |
| TC-UNIT-010 | Presentation | `HomeViewModel` error state: `isLoading=false`, `error='Server Down'`, `characters.isEmpty()`. | 0.006s | PASS |
| TC-UNIT-011 | Presentation | `DetailViewModel` uiState terupdate sesuai mock (success & error). | 0.155s | PASS |

**11/11 PASS**

---

## Defect Log

| Defect ID | TC ID | Modul | Deskripsi | Severity | Status |
|---|---|---|---|---|---|
| DEF-001 | TC-OFFLINE-002, TC-OFFLINE-003 | Character Detail Screen | Ikon gender bersifat statis — selalu menampilkan simbol laki-laki tanpa menyesuaikan nilai gender karakter. Ditemukan pada Detail Screen Beth Smith (gender: Female). | Minor | Open |

**Langkah Reproduksi DEF-001:**
1. Buka aplikasi dalam kondisi online
2. Buka Detail Screen karakter dengan gender Female (contoh: Beth Smith)
3. Amati ikon gender di UI
4. Ikon menampilkan simbol laki-laki meskipun karakter bergender Female

---

## Error Distribution

**By Module:**

| Module | Total TC | Executed | Pass | Fail | Blocked |
|---|---|---|---|---|---|
| Home Screen | 5 | 5 | 5 | 0 | 0 |
| Character Detail | 5 | 5 | 5 | 0 | 0 |
| Search Screen | 11 | 11 | 11 | 0 | 0 |
| Favorite Screen | 7 | 7 | 7 | 0 | 0 |
| Offline Mode | 3 | 3 | 3 | 0 | 0 |
| Unit Tests | 11 | 11 | 11 | 0 | 0 |
| **Total** | **42** | **42** | **42** | **0** | **0** |

**By Error Type:**

| Tipe Error | Jumlah | Persentase |
|---|---|---|
| Functional Error | 0 | 0% |
| UI / Display Error | 1 | 100% — DEF-001: Ikon gender statis (selalu laki-laki) |
| Performance Error | 0 | 0% |
| Offline / Caching Error | 0 | 0% |
| Automated Unit Test FAIL | 0 | 0% |

---

## Special Events & Proposals

**Kejadian Tidak Terduga:**

| TC ID | Keterangan |
|---|---|
| TC-HOME-003 | Loading indicator tampil secara visual namun tidak berhasil di-capture karena durasi sangat singkat. |
| TC-OFFLINE-002 | Ikon gender pada Detail Screen Beth Smith (gender: Female) menampilkan ikon laki-laki secara statis → dicatat sebagai DEF-001. |

**Rekomendasi Perbaikan:**

| Scope | Rekomendasi |
|---|---|
| TC-PROB-001 | Tambahkan unit test untuk Data layer (Repository Implementation, DAO) dan Presentation layer (SearchViewModel, FavoriteViewModel) pada siklus berikutnya agar coverage mencapai target >= 70%. |
| TC-SEARCH-003 s/d 007 | Perlu ditambahkan keterangan pada precondition di STD bahwa **filter hanya bekerja jika search field terisi** — temuan ini tidak tercantum di expected result STD sebelumnya. |

---

## Kesimpulan

Seluruh 42 test case berhasil dieksekusi dengan status **PASS**. Ditemukan 1 defect minor (DEF-001: ikon gender statis). Dengan metriks pengujian terpenuhi:

| Metrik | Hasil | Status |
|---|---|---|
| Test Execution Rate | 100% | PASS |
| Defect Density | 0.2 defect/fitur | PASS |
| API Response Time | < 1 detik | PASS |
