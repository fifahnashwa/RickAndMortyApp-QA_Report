# Software Test Design (STD)

> **Aplikasi:** Rick and Morty App v1.0 — Android (minSdk 24 – targetSdk 36)  
> **Referensi Kode:** [github.com/JohanArizona/RickAndMortyApp](https://github.com/JohanArizona/RickAndMortyApp)

---

## TEST-HOME — Home Screen · Character List

**Environment:**

| Komponen | Detail |
|---|---|
| OS & Hardware | Android fisik/emulator, min. API Level 24 · PC/Laptop + Android Studio |
| Koneksi | Internet aktif (TC-HOME-001–003, 005) · Airplane mode + clear cache (TC-HOME-004) |
| Monitoring | Android Studio Logcat |

**Testing Process:**
1. Buka aplikasi dari launcher, amati grid karakter dan loading indicator
2. Setelah data muat, verifikasi hero header tampil di bagian atas
3. Scroll perlahan ke bawah, amati infinite scroll tanpa aksi tambahan
4. Untuk TC-HOME-004: aktifkan airplane mode, bersihkan cache, buka ulang aplikasi
5. Catat semua hasil pada STR

---

### TC-HOME-001 — Tampilan grid karakter saat pertama buka app

| Field | Detail |
|---|---|
| **Precondition** | Aplikasi terinstall · Koneksi internet: aktif · Cache: bersih |
| **Input** | User membuka aplikasi dari launcher |
| **System** | API call ke `/api/character/?page=1` |
| **Intermediate Result** | Loading indicator tampil selama data dimuat |
| **Expected Result** | Hero header dan karakter muncul · Tidak ada pesan error |
| **Type** | Manual |

---

### TC-HOME-002 — Infinite scroll memuat halaman berikutnya

| Field | Detail |
|---|---|
| **Precondition** | Home Screen menampilkan halaman pertama · Koneksi: aktif |
| **Input** | User scroll ke bawah hingga mendekati item terakhir |
| **System** | API call ke halaman berikutnya dipicu otomatis |
| **Intermediate Result** | Loading indicator muncul di bagian bawah daftar |
| **Expected Result** | Data halaman berikutnya termuat otomatis · Total item bertambah · Tidak ada duplikasi |
| **Type** | Manual |

---

### TC-HOME-003 — Loading indicator saat data dimuat

| Field | Detail |
|---|---|
| **Precondition** | Aplikasi dibuka · Koneksi: aktif · Data sedang dalam proses fetch |
| **Input** | User mengamati layar saat aplikasi pertama dibuka |
| **Expected Result** | Loading indicator tampil selama fetch · Menghilang setelah data selesai dimuat |
| **Type** | Manual |

---

### TC-HOME-004 — Error state saat koneksi gagal

| Field | Detail |
|---|---|
| **Precondition** | Airplane mode: aktif · Cache: bersih · Tidak ada data lokal |
| **Input** | User membuka aplikasi tanpa koneksi jaringan |
| **Expected Result** | Pesan error tampil (misal: *'No internet connection and no cached data'*) · Tombol **Retry** tampil dan berfungsi · Aplikasi tidak crash |
| **Type** | Manual |

---

### TC-HOME-005 — Posisi halaman tidak berubah setelah kembali dari Detail

| Field | Detail |
|---|---|
| **Precondition** | Home Screen sudah menampilkan karakter dan di-scroll ke halaman tertentu · Koneksi: aktif |
| **Input** | User tap karakter → masuk Detail Screen → tap back |
| **Expected Result** | Posisi scroll tidak berubah · Halaman tidak auto-refresh |
| **Type** | Manual |

---

## TEST-DETAIL — Character Detail Screen

**Environment:**

| Komponen | Detail |
|---|---|
| OS & Hardware | Android fisik/emulator, min. API Level 24 |
| Koneksi | Internet aktif (TC-DETAIL-001–004) · Airplane mode (TC-DETAIL-005) |
| Persiapan offline | Kunjungi detail karakter terlebih dahulu saat online agar ter-cache |

**Testing Process:**
1. Dari Home Screen, tap karakter untuk membuka Detail Screen
2. Verifikasi kelengkapan data: nama, status, species, gender, origin, image
3. Scroll ke section episode → swipe horizontal
4. Scroll ke section 'YOU MIGHT ALSO LIKE' → verifikasi kartu dan tap
5. Tap ikon favorite (add) → amati perubahan ikon → tap lagi (remove)
6. Untuk TC-DETAIL-005: aktifkan airplane mode, navigasi ke karakter yang sama

---

### TC-DETAIL-001 — Detail screen terbuka dengan data lengkap

| Field | Detail |
|---|---|
| **Precondition** | Home Screen menampilkan karakter · Koneksi: aktif |
| **Input** | User men-tap salah satu karakter dari grid Home |
| **Expected Result** | Detail Screen terbuka · Semua field tampil lengkap: nama, status, species, gender, origin, image · Tidak ada field kosong atau placeholder |
| **Type** | Manual |

---

### TC-DETAIL-002 — Daftar episode dapat di-scroll horizontal

| Field | Detail |
|---|---|
| **Precondition** | Detail Screen terbuka · Koneksi: aktif |
| **Input** | User scroll ke section episode → swipe horizontal pada LazyRow |
| **Expected Result** | Episode tampil dalam LazyRow · User dapat swipe kiri/kanan · Tidak ada crash |
| **Type** | Manual |

---

### TC-DETAIL-003 — Section 'YOU MIGHT ALSO LIKE' tampil dan berfungsi

| Field | Detail |
|---|---|
| **Precondition** | Detail Screen terbuka · Koneksi: aktif |
| **Input** | User scroll ke section 'YOU MIGHT ALSO LIKE' → tap salah satu kartu |
| **Expected Result** | Section tampil · Kartu karakter dalam LazyRow horizontal · Tap kartu membuka Detail Screen karakter tersebut |
| **Type** | Manual |

---

### TC-DETAIL-004 — Tombol Favorite berfungsi (add/remove)

| Field | Detail |
|---|---|
| **Precondition** | Detail Screen terbuka · Karakter belum difavoritkan (ikon `FavoriteBorder`) |
| **Input** | User men-tap ikon favorite (add) → tap lagi (remove) |
| **Intermediate Result** | Ikon: `FavoriteBorder` → `Favorite` saat add · `Favorite` → `FavoriteBorder` saat remove · Tidak ada toast/snackbar |
| **Expected Result** | Add: ikon menjadi filled · Karakter muncul di Favorite Screen tanpa restart · Remove: ikon kembali ke border · Karakter hilang dari Favorite Screen tanpa restart |
| **Type** | Manual |

---

### TC-DETAIL-005 — Data detail tersedia saat offline (dari cache)

| Field | Detail |
|---|---|
| **Precondition** | Karakter sudah pernah dibuka saat online (ter-cache di Room DB) · Airplane mode: aktif |
| **Input** | User navigasi ke Detail Screen karakter yang sudah ter-cache |
| **Expected Result** | Detail Screen terbuka dengan data lengkap dari Room DB · Tidak ada pesan error koneksi · Semua field tampil benar |
| **Type** | Manual |

---

## TEST-SEARCH — Search Screen · Advanced Search

**Environment:**

| Komponen | Detail |
|---|---|
| OS & Hardware | Android fisik/emulator, min. API Level 24 · Keyboard virtual aktif |
| Koneksi | Internet aktif (TC-SEARCH-001–010) · Airplane mode (TC-SEARCH-011) |
| Monitoring | Logcat untuk verifikasi penyimpanan riwayat di Room DB |

**Testing Process:**
1. Buka Search Screen dari bottom navigation bar
2. Ketik perlahan di search field → amati debounce (hasil muncul setelah jeda)
3. Hapus teks hingga kosong → amati riwayat pencarian · Tap riwayat
4. Aktifkan filter satu per satu, amati hasil · Aktifkan kombinasi filter
5. Klik Reset → amati filter kembali ke awal, recent searches tetap
6. Hapus entri recent searches via tombol ×
7. Ketik kata kunci tidak ada → amati empty state
8. Aktifkan airplane mode → ketik kata kunci → amati pesan error

---

### TC-SEARCH-001 — Pencarian dengan debounce

| Field | Detail |
|---|---|
| **Precondition** | Search Screen terbuka · Koneksi: aktif |
| **Input** | User mengetik `rick` satu karakter per karakter secara perlahan |
| **System** | API call ke `/api/character/?name=rick` hanya dipicu setelah debounce delay |
| **Intermediate Result** | API call tidak dipicu pada setiap keystroke |
| **Expected Result** | Hasil menampilkan karakter dengan nama mengandung 'rick' · Tidak ada hasil duplikat atau tidak relevan |
| **Type** | Manual |

---

### TC-SEARCH-002 — Riwayat pencarian tampil saat field kosong

| Field | Detail |
|---|---|
| **Precondition** | Minimal satu pencarian sudah dilakukan · Riwayat tersimpan di Room DB |
| **Input** | User menghapus semua teks di search field hingga kosong |
| **Expected Result** | Riwayat pencarian tampil di bawah search field · Tap riwayat → search field terisi otomatis |
| **Type** | Manual |

---

### TC-SEARCH-003 — Filter Status

| Field | Detail |
|---|---|
| **Precondition** | Search Screen terbuka · Koneksi: aktif |
| **Input** | User mengaktifkan filter Status → pilih `Alive` |
| **System** | API call dengan parameter `?status=Alive` |
| **Expected Result** | Semua karakter tampil berstatus `Alive` · Tidak ada karakter berstatus `Dead` atau `Unknown` |
| **Type** | Manual |

---

### TC-SEARCH-004 — Filter Gender

| Field | Detail |
|---|---|
| **Precondition** | Search Screen terbuka · Koneksi: aktif |
| **Input** | User mengaktifkan filter Gender → pilih `Male` |
| **System** | API call dengan parameter `?gender=Male` |
| **Expected Result** | Semua karakter tampil bergender `Male` |
| **Type** | Manual |

---

### TC-SEARCH-005 — Filter Species

| Field | Detail |
|---|---|
| **Precondition** | Search Screen terbuka · Koneksi: aktif |
| **Input** | User mengaktifkan filter Species → pilih `Human` |
| **System** | API call dengan parameter `?species=Human` |
| **Expected Result** | Semua karakter tampil berspesies `Human` |
| **Type** | Manual |

---

### TC-SEARCH-006 — Kombinasi filter (AND logic)

| Field | Detail |
|---|---|
| **Precondition** | Search Screen terbuka · Koneksi: aktif |
| **Input** | User mengaktifkan filter Status: `Alive` DAN Gender: `Male` secara bersamaan |
| **System** | API call dengan parameter `?status=Alive&gender=Male` |
| **Expected Result** | Hasil menampilkan karakter yang memenuhi SEMUA filter (AND logic) · Tidak ada yang hanya memenuhi satu filter |
| **Type** | Manual |

---

### TC-SEARCH-007 — Filter Type

| Field | Detail |
|---|---|
| **Precondition** | Search Screen terbuka · Koneksi: aktif |
| **Input** | User mengaktifkan filter Type → pilih `Human with antennae` |
| **System** | API call dengan parameter `?type=Human with antennae` |
| **Expected Result** | Semua karakter tampil dengan Type `Human with antennae` |
| **Type** | Manual |

---

### TC-SEARCH-008 — Empty state untuk pencarian tidak ditemukan

| Field | Detail |
|---|---|
| **Precondition** | Search Screen terbuka · Koneksi: aktif |
| **Input** | User mengetik `XYZabc123notexist` |
| **Expected Result** | Empty state tampil dengan pesan yang sesuai · Tidak ada crash atau blank screen |
| **Type** | Manual |

---

### TC-SEARCH-009 — Reset filter

| Field | Detail |
|---|---|
| **Precondition** | Search Screen terbuka · Filter aktif · Koneksi: aktif |
| **Input** | User klik tombol **Reset** |
| **Expected Result** | Filter kembali ke kondisi awal · Recent Searches tetap tampil normal |
| **Type** | Manual |

---

### TC-SEARCH-010 — Penghapusan individual recent searches

| Field | Detail |
|---|---|
| **Precondition** | Search Screen terbuka · Terdapat beberapa item recent searches · Koneksi: aktif |
| **Input** | User menghapus beberapa entri via tombol ×, satu per satu |
| **Expected Result** | Penghapusan individual bekerja dengan benar · Entri lain tetap ada |
| **Type** | Manual |

---

### TC-SEARCH-011 — Pesan error tanpa koneksi internet

| Field | Detail |
|---|---|
| **Precondition** | Search Screen terbuka · Airplane mode: aktif |
| **Input** | User mengetik `beth` di search field |
| **Expected Result** | Pesan `'No internet connection'` tampil berwarna merah di tengah layar · Tidak ada crash atau blank screen |
| **Type** | Manual |

---

## TEST-FAVORITE — Favorite Screen · Favorites System

**Environment:**

| Komponen | Detail |
|---|---|
| OS & Hardware | Android fisik/emulator, min. API Level 24 · Koneksi: aktif |
| Persiapan | Home Screen sudah menampilkan karakter · Navigasi via bottom navigation bar |

**Testing Process:**
1. Tap ikon `FavoriteBorder` pada karakter di Home → amati perubahan ikon → cek Favorite Screen
2. Dari Detail Screen: tap ikon favorite → cek Favorite Screen
3. Dari Search Screen: tap ikon favorite → cek Favorite Screen
4. Hapus karakter dari favorit → verifikasi ikon berubah di semua screen
5. Hapus semua favorit → verifikasi empty state
6. Bersihkan cache aplikasi → buka kembali → cek apakah favorit tetap ada

---

### TC-FAV-001 — Tambah favorit dari Home Screen

| Field | Detail |
|---|---|
| **Precondition** | Home Screen menampilkan karakter · Karakter target: belum difavoritkan |
| **Input** | User men-tap ikon `FavoriteBorder` pada karakter di Home |
| **Intermediate Result** | Ikon berubah ke `Favorite` (filled) · Tidak ada toast/snackbar |
| **Expected Result** | Ikon berubah ke filled · Karakter muncul di Favorite Screen tanpa restart |
| **Type** | Manual |

---

### TC-FAV-002 — Tambah favorit dari Detail Screen

| Field | Detail |
|---|---|
| **Precondition** | Detail Screen karakter terbuka · Karakter: belum difavoritkan |
| **Input** | User men-tap ikon `FavoriteBorder` di Detail Screen |
| **Intermediate Result** | Ikon berubah ke `Favorite` · Tidak ada toast/snackbar |
| **Expected Result** | Ikon berubah ke filled · Karakter muncul di Favorite Screen tanpa restart |
| **Type** | Manual |

---

### TC-FAV-003 — Tambah favorit dari Search Screen

| Field | Detail |
|---|---|
| **Precondition** | Search Screen menampilkan hasil pencarian · Karakter target: belum difavoritkan |
| **Input** | User men-tap ikon `FavoriteBorder` pada karakter di hasil pencarian |
| **Intermediate Result** | Ikon berubah ke `Favorite` · Tidak ada toast/snackbar |
| **Expected Result** | Ikon berubah ke filled · Karakter muncul di Favorite Screen tanpa restart |
| **Type** | Manual |

---

### TC-FAV-004 — Hapus favorit secara real-time

| Field | Detail |
|---|---|
| **Precondition** | Minimal satu karakter difavoritkan · Favorite Screen terbuka |
| **Input** | User men-tap ikon `Favorite` (filled) pada karakter di Favorite Screen |
| **Intermediate Result** | Ikon berubah ke `FavoriteBorder` · Tidak ada toast/snackbar |
| **Expected Result** | Karakter langsung hilang dari Favorite Screen secara real-time · Tidak perlu refresh atau restart |
| **Type** | Manual |

---

### TC-FAV-005 — Sinkronisasi status favorit lintas layar

| Field | Detail |
|---|---|
| **Precondition** | Karakter X ditambahkan ke favorit dari Home Screen · Ikon karakter X di Home: `Favorite` (filled) |
| **Input** | User navigasi ke Search Screen → cari karakter X → navigasi ke Detail Screen karakter X |
| **Expected Result** | Ikon karakter X di Search Screen: `Favorite` (filled) · Ikon di Detail Screen: `Favorite` (filled) · Sinkronisasi terjadi tanpa refresh manual (via StateFlow/observer) |
| **Type** | Manual |

---

### TC-FAV-006 — Empty state saat tidak ada favorit

| Field | Detail |
|---|---|
| **Precondition** | Semua karakter dihapus dari favorit · Favorite Screen dibuka |
| **Input** | User membuka Favorite Screen dalam kondisi daftar kosong |
| **Expected Result** | Empty state tampil dengan pesan yang sesuai · Tidak ada crash atau blank screen |
| **Type** | Manual |

---

### TC-FAV-007 — Favorit tetap ada setelah clear cache

| Field | Detail |
|---|---|
| **Precondition** | Terdapat karakter pada Favorite Screen |
| **Input** | User membersihkan cache aplikasi → membuka kembali aplikasi |
| **Expected Result** | Karakter pada Favorite Screen tetap ada, tidak berubah, tidak terpengaruh oleh clear cache |
| **Type** | Manual |

---

## TEST-OFFLINE — Offline Mode · Room DB Caching

**Environment:**

| Komponen | Detail |
|---|---|
| OS & Hardware | Android fisik/emulator, min. API Level 24 · Airplane mode aktif saat sesi offline |
| Persiapan | Buka app online → scroll beberapa halaman → buka Detail 2–3 karakter → aktifkan airplane mode |
| Monitoring | Logcat untuk verifikasi sumber data (Room DB vs API) |

**Testing Process:**
1. Buka aplikasi dalam kondisi online, scroll Home beberapa halaman
2. Buka Detail Screen 2–3 karakter berbeda
3. Aktifkan airplane mode
4. Kembali ke Home → amati karakter yang sudah dimuat
5. Navigasi ke Detail karakter yang sudah dikunjungi → amati data dari cache
6. Cek Logcat: verifikasi data diambil dari Room DB

---

### TC-OFFLINE-001 — Home Screen tetap tampil saat offline

| Field | Detail |
|---|---|
| **Precondition** | Beberapa halaman sudah dimuat saat online · Airplane mode: aktif |
| **Input** | User membuka atau scroll Home Screen dalam kondisi offline |
| **Expected Result** | Karakter yang sudah ter-cache tetap tampil · Tidak ada pesan error untuk data yang sudah ada · Logcat mengkonfirmasi data dari Room DB |
| **Type** | Manual |

---

### TC-OFFLINE-002 — Detail karakter dari cache (sudah pernah dibuka)

| Field | Detail |
|---|---|
| **Precondition** | Detail karakter X sudah pernah dibuka saat online · Airplane mode: aktif |
| **Input** | User navigasi ke Detail Screen karakter X |
| **Expected Result** | Detail Screen tampil dengan data lengkap dari cache · Tidak ada blank field atau error koneksi · Logcat mengkonfirmasi data dari Room DB (`getCharacterById` dari DAO) |
| **Type** | Manual |

---

### TC-OFFLINE-003 — Detail karakter dari cache ('YOU MIGHT ALSO LIKE')

| Field | Detail |
|---|---|
| **Precondition** | Karakter X belum pernah dibuka, tetapi pernah muncul di section 'YOU MIGHT ALSO LIKE' · Airplane mode: aktif |
| **Input** | User navigasi ke Detail Screen karakter X |
| **Expected Result** | Detail Screen berhasil terbuka dengan data lengkap dari Room DB cache · Tidak ada pesan error koneksi |
| **Type** | Manual |

---

## TEST-UNIT — Unit Tests · Automated (JUnit + MockK)

**Environment:**

| Komponen | Detail |
|---|---|
| OS & Hardware | PC/Laptop + Android Studio + JDK 11+ · Tidak memerlukan perangkat fisik |
| Runtime | JVM |
| Dependencies | JUnit 4.13.2 · MockK 1.13.8 · kotlinx-coroutines-test · Google Truth 1.1.5 |
| Run Command | `./gradlew test` |
| Report | `app/build/reports/tests/testDebugUnitTest/index.html` |

**Testing Process:**
1. Buka terminal di root directory project
2. Jalankan: `./gradlew test`
3. Tunggu hingga seluruh test selesai
4. Buka laporan HTML untuk detail PASS/FAIL
5. Catat jumlah PASS/FAIL dan test coverage per layer

---

### TC-UNIT-001 — CharacterMapper: entity ke domain

| Field | Detail |
|---|---|
| **Layer** | Data (Mapper) |
| **Input** | `CharacterEntity(id=1, name='Morty', status='Alive', species='Human', gender='Male', ...)` · `isFavorite = true` · Method: `CharacterMapper.mapEntityToDomain(entity, isFavorite)` |
| **Expected Result** | `domain.name == 'Morty'` · `domain.status == CharacterStatus.Alive` · `domain.gender == CharacterGender.Male` · `domain.isFavorite == true` |
| **Type** | Automation |

---

### TC-UNIT-002 — CharacterStatus.fromString() dengan nilai tidak dikenal

| Field | Detail |
|---|---|
| **Layer** | Data (Mapper) |
| **Input** | `statusString = 'Mati Suri'` · Method: `CharacterStatus.fromString(statusString)` |
| **Expected Result** | `result == CharacterStatus.Unknown` |
| **Type** | Automation |

---

### TC-UNIT-003 — GetCharactersUseCase: ResourceState.Success

| Field | Detail |
|---|---|
| **Layer** | Domain (UseCase) |
| **Input** | `coEvery { characterRepo.getCharacters(1) } returns flowOf(ResourceState.Success(...))` |
| **Expected Result** | `result is ResourceState.Success` · `data.size == 1` |
| **Type** | Automation |

---

### TC-UNIT-004 — SearchCharactersUseCase: ResourceState.Empty

| Field | Detail |
|---|---|
| **Layer** | Domain (UseCase) |
| **Input** | `query = 'Zorglub'` · repo mengembalikan `ResourceState.Empty` |
| **Expected Result** | `result is ResourceState.Empty` |
| **Type** | Automation |

---

### TC-UNIT-005 — AddFavoriteUseCase: dipanggil tepat 1 kali

| Field | Detail |
|---|---|
| **Layer** | Domain (UseCase) |
| **Input** | `favoriteRepo = mockk(relaxed = true)` · `AddFavoriteUseCase(favoriteRepo).invoke(character)` |
| **Expected Result** | `coVerify { favoriteRepo.addFavorite(character) }` → dipanggil tepat 1 kali |
| **Type** | Automation |

---

### TC-UNIT-006 — RemoveFavoriteUseCase: dipanggil tepat 1 kali

| Field | Detail |
|---|---|
| **Layer** | Domain (UseCase) |
| **Input** | `characterId = 42` · `RemoveFavoriteUseCase(favoriteRepo).invoke(42)` |
| **Expected Result** | `coVerify { favoriteRepo.removeFavorite(42) }` → dipanggil tepat 1 kali |
| **Type** | Automation |

---

### TC-UNIT-007 — GetFavoritesUseCase: Flow memancarkan list

| Field | Detail |
|---|---|
| **Layer** | Domain (UseCase) |
| **Input** | `coEvery { favoriteRepo.getAllFavorites() } returns flowOf(listOf(mockk()))` |
| **Expected Result** | Flow memancarkan list dengan `size == 1` · Tidak ada exception |
| **Type** | Automation |

---

### TC-UNIT-008 — SearchHistory: add, get, remove

| Field | Detail |
|---|---|
| **Layer** | Domain (UseCase) |
| **Input** | `query = 'Rick'` · Method: `addToHistory('Rick')` → `getHistory()` → `removeFromHistory('Rick')` |
| **Expected Result** | `addSearchHistory('Rick')` dipanggil 1 kali · `removeSearchHistory('Rick')` dipanggil 1 kali |
| **Type** | Automation |

---

### TC-UNIT-009 — HomeViewModel: success case

| Field | Detail |
|---|---|
| **Layer** | Presentation (ViewModel) |
| **Input** | Mock success: `getCharactersUseCase(1)` → `ResourceState.Success` · Mock `getFavoritesUseCase()` → empty list |
| **Expected Result** | `uiState.isLoading == false` · `uiState.characters.isNotEmpty()` · `uiState.page == 2` |
| **Type** | Automation |

---

### TC-UNIT-010 — HomeViewModel: error case

| Field | Detail |
|---|---|
| **Layer** | Presentation (ViewModel) |
| **Input** | Mock error: `getCharactersUseCase(1)` → `ResourceState.Error('Server Down')` |
| **Expected Result** | `uiState.isLoading == false` · `uiState.characters.isEmpty()` · `uiState.error == 'Server Down'` |
| **Type** | Automation |

---

### TC-UNIT-011 — DetailViewModel: uiState terupdate sesuai mock

| Field | Detail |
|---|---|
| **Layer** | Presentation (ViewModel) |
| **Input** | Inisialisasi `DetailViewModel` · Simulasi load detail karakter |
| **Expected Result** | `uiState.isLoading == false` setelah data dimuat · Data detail terisi sesuai mock · Tidak ada exception |
| **Type** | Automation |

---

## Actions in Case of Program Failure

| Tindakan | Detail |
|---|---|
| **1. Dokumentasi** | Catat TC FAIL beserta pesan error, screenshot, Logcat, dan langkah reproduksi pada STR |
| **2. Hentikan Sesi** | Jika crash atau state tidak dapat dipulihkan, hentikan sesi dan dokumentasikan kondisi terakhir |
| **3. Eskalasi** | Sampaikan ke developer: TC ID, langkah reproduksi, expected vs actual result, environment detail |
| **4. Reset** | Sebelum lanjut ke TC berikutnya, reset state app (clear data / reinstall APK) |
| **5. Unit Test FAIL** | Catat stack trace lengkap · Identifikasi apakah dari logika bisnis atau konfigurasi mock |

## Procedures According to Test Results

| Prosedur | Detail |
|---|---|
| **Evaluasi Metrik** | Bandingkan dengan target STP: Execution Rate ≥ 90% · Defect Density ≤ 2 · API Time ≤ 3 detik |
