# redirect-link

Pemendek tautan pribadi berbasis file statis untuk **[go.kurniawanhafiz.web.id](https://go.kurniawanhafiz.web.id)**. Tautan pendek seperti `go.kurniawanhafiz.web.id/namalink` akan diarahkan otomatis ke tujuan aslinya (folder atau file Google Drive), tanpa server dan tanpa database.

## Cara Kerja

1. Pengunjung membuka `go.kurniawanhafiz.web.id/<slug>`.
2. Platform hosting membaca aturan di file `_redirects`.
3. Bila slug terdaftar, pengunjung langsung diarahkan ke URL tujuan dengan status `302` (redirect sementara).
4. Bila slug tidak terdaftar, pengunjung melihat halaman `404.html`.

## Struktur Repo

```
redirect-link/
├── _redirects   # daftar slug dan URL tujuan
├── 404.html     # halaman "Link Tidak Ditemukan"
└── README.md
```

| File | Fungsi |
| --- | --- |
| `_redirects` | Aturan redirect, satu baris per tautan. |
| `404.html` | Halaman error bertema gelap (Tailwind CSS CDN, font Inter, Font Awesome) yang tampil saat tautan tidak valid atau sudah kedaluwarsa. |

## Menambah Tautan Baru

Buka `_redirects`, lalu tambahkan satu baris dengan format:

```
/<slug> <url-tujuan> 302
```

Contoh:

```
/tugasbindo https://drive.google.com/drive/folders/ID_FOLDER 302
```

Setelah di-commit ke branch `main`, tautan `go.kurniawanhafiz.web.id/tugasbindo` aktif begitu proses deploy selesai.

### Aturan Penulisan

- Satu aturan per baris, dengan tiga bagian dipisah spasi: slug, tujuan, status.
- Slug diawali `/` dan sebaiknya huruf kecil tanpa spasi.
- Slug harus unik. Bila ada dua slug yang sama, umumnya aturan paling atas yang dipakai.
- Pakai `302` agar tujuan bisa diganti kapan saja tanpa tersangkut cache browser. Ganti ke `301` hanya untuk tautan permanen.
- Pastikan akses tautan Google Drive sudah diatur ke "Siapa saja yang memiliki link", kalau tidak pengunjung akan diminta login.

## Mengubah atau Menghapus Tautan

- **Mengubah tujuan:** edit URL pada baris slug yang bersangkutan.
- **Menghapus tautan:** hapus barisnya. Slug tersebut akan menampilkan halaman 404.

## Deploy

Repo ini memakai format `_redirects` dan `404.html` yang dikenali oleh hosting statis seperti **Cloudflare Pages** dan **Netlify**. Cukup hubungkan repo ke salah satunya dan arahkan custom domain `go.kurniawanhafiz.web.id` ke proyek tersebut.

Pengaturan build:

| Opsi | Nilai |
| --- | --- |
| Build command | *(kosongkan)* |
| Output directory | `/` (root repo) |
| Branch produksi | `main` |

## Menjalankan Lokal

Tidak ada dependensi yang perlu dipasang. Untuk mencoba halaman 404 secara lokal:

```bash
npx serve .
```

Catatan: aturan di `_redirects` tidak diproses oleh server lokal biasa. Untuk mengujinya, pakai emulator bawaan platform, misalnya `npx netlify dev` atau `npx wrangler pages dev .`.

## Catatan

- Repo ini hanya berisi konfigurasi redirect. File yang dituju tetap tersimpan di Google Drive.
- Siapa pun yang mengetahui sebuah slug dapat membukanya, jadi jangan arahkan ke berkas yang bersifat rahasia.

## Pemilik

Dikelola oleh [@kurniawanhafiz](https://github.com/kurniawanhafiz).
