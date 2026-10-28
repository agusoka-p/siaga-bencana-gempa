# Siaga Gempa

Website Edukasi Mitigasi Bencana Gempa Bumi
Tugas UTS Mata Kuliah Pemrograman Web.


## Anggota Kelompok

| No | Nama | NIM | Tugas |
|:--:|------|:---:|-------|
| 1 | I Putu Agus Oka Pradana | 2515091002 | Membuat page Panduan |
| 2 | I Nyoman Adi Murdana Putra | 2515091040 | Membuat page Edukasi dan membuat CSS global |
| 3 | Muhamad Rizki Alhamid | 2515091078 | Membuat page Fakta/Artikel |
| 4 | I Made Segara Putra | 2515091097 | Membuat page Beranda dan Tentang |


## Deskripsi Website

Siaga Gempa adalah website edukasi kesiapsiagaan dan mitigasi bencana dengan fokus pada gempa bumi. Informasi disusun dari sumber resmi seperti BMKG dan BNPB agar mudah dipahami dan dipraktikkan.

**Tujuan**
- Menyajikan panduan singkat tentang tindakan sebelum, saat, dan sesudah gempa.
- Menjelaskan konsep dasar gempa bumi dengan bahasa yang mudah dipahami.
- Mendukung literasi kebencanaan yang selaras dengan SDG 11 dan SDG 13.

**Manfaat**
- Pengguna tahu langkah yang harus dilakukan pada setiap fase gempa.
- Pengguna tahu sumber informasi resmi, yaitu aplikasi Info BMKG, InaTEWS, dan nomor darurat 112, 115, dan 117.
- Pengguna dapat belajar dari studi kasus gempa besar di Indonesia dan berita gempa terbaru.

**Sasaran pengguna:** pelajar, keluarga, dan komunitas.


## Halaman Website

| Halaman | File | Isi |
|---------|------|-----|
| Beranda | `index.html` | Hero, ringkasan edukasi dan panduan, data gempa Indonesia 2025, video edukasi, teaser tentang |
| Edukasi | `edukasi.html` | Konsep dasar, isu, permasalahan, dan fakta ilmiah kegempaan |
| Panduan | `panduan.html` | Tindakan sebelum, saat, dan sesudah gempa, peringatan dini resmi, nomor darurat |
| Artikel | `fakta.html` | Berita gempa terbaru, fakta dan solusi, studi kasus gempa besar, kaitan dengan SDG |
| Tentang | `tentang.html` | Profil website, tujuan, konsep risiko, sasaran, anggota tim, daftar referensi |


## Teknologi

- HTML5
- CSS3 (satu berkas `style.css` untuk semua halaman)
- Flexbox dan CSS Grid
- CSS Media Queries
- JavaScript dasar (menu hamburger)
- Google Fonts (DM Sans dan Manrope)
  

## Penjelasan Implementasi

### Navigasi

Semua halaman memakai header yang sama dengan lima menu. Menu halaman yang sedang dibuka diberi class `active`. Header bersifat `sticky` sehingga selalu terlihat saat halaman di-scroll.

```html
<ul class="nav-menu">
    <li><a href="index.html">Beranda</a></li>
    <li><a href="panduan.html" class="active" aria-current="page">Panduan</a></li>
    ...
</ul>
```

Pada layar mobile, menu disembunyikan dan dibuka lewat tombol hamburger.

```js
toggle.addEventListener("click", () => {
    const isOpen = panel.classList.toggle("open");
    toggle.setAttribute("aria-expanded", String(isOpen));
});
```

Fungsinya menambah atau menghapus class `open` pada panel menu dan memperbarui `aria-expanded` agar tetap sesuai statusnya.

### Layout

Warna disimpan sebagai variabel di `:root` sehingga konsisten di semua halaman. Lebar konten dibatasi dengan `.container`, dan seksi dibuat selang-seling putih (`.section`) dan abu kehijauan (`.section-alt`).

```css
:root {
    --green: #A3FF12;
    --blue-dark: #0A2033;
    --blue: #176A8A;
}

.container { width: min(1100px, 90%); margin: 0 auto; }
```

Komponen yang dipakai ulang di banyak halaman:

- `.card-grid` dan `.info-card`: kartu dalam grid 3 kolom (4 kolom untuk `.fact-grid`).
- `.split-layout`: Flexbox untuk gambar di satu sisi dan teks di sisi lain.
- `.hero`: grid dua kolom dengan latar gradient.

```css
.split-layout { display: flex; align-items: flex-start; gap: 40px; }
.split-content { flex: 1 1 0; min-width: 0; }
.split-media { flex: 0 0 min(42%, 420px); }
```

### Responsive Design

Tampilan menyesuaikan lebar layar dengan media query `max-width`.

| Lebar layar | Perubahan |
|-------------|-----------|
| 900px ke bawah (tablet) | Grid kartu menjadi 2 kolom, video menjadi 1 kolom, footer 2 kolom, ukuran judul hero dikecilkan |
| 680px ke bawah (mobile) | Menu hamburger aktif, semua grid menjadi 1 kolom, `split-layout` menjadi susunan vertikal, footer 1 kolom |
| 480px dan 400px ke bawah | Penyesuaian kecil pada ukuran judul dan padding kartu |

```css
@media (max-width: 680px) {
    .nav-toggle { display: flex; }
    .nav-panel { display: none; }
    .nav-panel.open { display: block; }
    .split-layout { flex-direction: column; }
}
```

Gambar dibuat responsif dengan `img { max-width: 100%; height: auto; }`.

### Bagian Kode Penting

**Video responsif (Beranda).** `aspect-ratio` menjaga video tetap 16:9 di semua ukuran layar.

```css
.video-frame { position: relative; aspect-ratio: 16 / 9; overflow: hidden; }
.video-frame iframe { position: absolute; inset: 0; width: 100%; height: 100%; }
```

**Kartu berita dengan lapisan gelap di atas foto (Artikel).** Foto diletakkan di lapisan bawah dan gradient di atasnya agar teks putih tetap terbaca.

```css
.berita-img { position: absolute; inset: 0; z-index: -2; object-fit: cover; }
.berita-thumb::before {
    content: ""; position: absolute; inset: 0; z-index: -1;
    background: linear-gradient(135deg, rgba(10,32,51,.88), rgba(23,106,138,.62));
}
```

**Alur konsep bergaya timeline (Tentang).** Garis dibuat dengan `border-left` dan titik hijau dengan `::before`, tanpa elemen HTML tambahan.

```css
.concept-flow { list-style: none; border-left: 2px solid var(--border); }
.concept-flow li::before { content: ""; position: absolute; left: -6px; width: 10px; height: 10px; border-radius: 50%; background: var(--green); }
```


## Struktur Folder

```
siaga-gempa/
├── index.html
├── edukasi.html
├── panduan.html
├── fakta.html
├── tentang.html
├── style.css
├── README.md
└── images/
    ├── Hero.jpg, Tentang.jpg
    ├── bangunan-kokoh.jpeg, berlindung.jpg, titik-kumpul.jpg
    ├── diagram-mekanisme-gempa.jpg, diagram-hiposentrum-episentrum.jpg, diagram-gelombang-seismik.jpg
    ├── gempa-tektonik.jpg, gempa-vulkanik.jpg, gempa-runtuhan.jpg
    └── bogor.jpg, pasiman.jpg, pacitan.jpg, sumba.jpg
```

- `*.html`: lima halaman website.
- `style.css`: CSS global yang dipakai semua halaman.
- `images/`: seluruh gambar yang dipakai website.
  

## Website dan Repository

- Website (GitHub Pages): `https://agusoka-p.github.io/siaga-bencana-gempa/`
- Repository: `https://github.com/agusoka-p/siaga-bencana-gempa`
  

## Referensi

**Sumber materi**
- BMKG. [Edukasi Gempa Bumi dan Tsunami](https://www.bmkg.go.id/gempabumi/mitigasi/edukasi-gempabumi-tsunami)
- BNPB. [Siaga Bencana: Gempa](https://www.bnpb.go.id/siaga-bencana/gempa-saat-terjadi) dan [Buku Saku Bencana](https://www.bnpb.go.id/buku/buku-saku-bencana)
- Kemenko PMK. [Ebook Laporan IDMS 2025](https://www2.kemenkopmk.go.id/sites/default/files/artikel/2026-04/Ebook_Laporan%20IDMS%202025.pdf)
- Indonesiabaik.id. [Indonesia Tangguh Menghadapi Bencana 2025](https://indonesiabaik.id/infografis/indonesia-tangguh-menghadapi-bencana-2025)
- UU No. 24 Tahun 2007 tentang Penanggulangan Bencana. [Tautan](https://jdih.esdm.go.id/storage/document/uu-24-2007.pdf)
- PP No. 21 Tahun 2008 tentang Penyelenggaraan Penanggulangan Bencana. [Tautan](https://jdih.kemenkeu.go.id/fulltext/2008/21TAHUN2008PP.htm)
- PBB. [SDG 11](https://sdgs.un.org/goals/goal11) dan [SDG 13](https://sdgs.un.org/goals/goal13)
- Poster edukasi PREDIKT! (Preparedness for disaster toolkit)

Daftar lengkap 12 referensi ada di halaman Tentang.

**Video:** HUMAS BNPB dan kanal edukasi kebencanaan di YouTube (lihat halaman Beranda).

**Berita:** Republika, Databoks Katadata, dan Media Indonesia (lihat halaman Artikel).

**Gambar dan aset**
- Diagram dan ilustrasi halaman Edukasi: Katalogika, Harian Jogja, Kompas.com, Zenius, dan AI (Gemini).
- Ilustrasi halaman Artikel: dibuat sendiri dengan SVG.
- `Hero.jpg` dan `Tentang.jpg`:
   - Foto oleh Eemir Hannn (Pexels), dimuat di [Radar TV, PMI Siapkan Operasi Kemanusiaan Besar untuk Bantu Korban Gempa NTT](https://radartv.disway.id/nasional/read/38121/pmi-siapkan-operasi-kemanusiaan-besar-untuk-bantu-korban-gempa-ntt)
   - Ilustrasi bencana (Foto Canva), dimuat di [Berita UMS, Negeri di Atas Patahan: Benarkah Mitigasi Bencana Kita Sudah Memadai?](https://news.ums.ac.id/id/berita/negeri-di-atas-patahan-benarkah-mitigasi-bencana-kita-sudah-memadai/)
- Font: [Google Fonts](https://fonts.google.com/) (DM Sans dan Manrope).
