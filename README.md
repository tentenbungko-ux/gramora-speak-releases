# gramora-speak-releases

Repo rilis **publik** Gramora Speak (aplikasi belajar bahasa Inggris, dibagikan
ke semua orang — keputusan pemilik aplikasi).

## Isi repo

| File | Guna |
|---|---|
| `version.json` | Dibaca aplikasi saat dibuka untuk cek pembaruan (lihat bawah). |
| `releases/gramora-speak-vX.Y.Z.apk` | APK tiap rilis (diunggah lewat Contents API). |

## Cara kerja cek pembaruan di dalam aplikasi

1. Saat layar utama dibuka, aplikasi mengambil `version.json` ini (timeout 5 detik).
2. Bila `versionCode` di sini lebih besar dari versi terpasang, muncul dialog
   "Pembaruan tersedia" dengan catatan perubahan.
3. Tombol "Unduh & Pasang" mengunduh APK lewat DownloadManager lalu memasangnya
   lewat intent pemasang sistem.
4. **Batas jujur Android:** aplikasi sideload tidak bisa update diam-diam —
   Android selalu meminta pengguna mengetuk "Instal" sekali. Yang dihilangkan
   fitur ini hanyalah bolak-balik ke chat dan unduh manual.

Bila offline atau `version.json` rusak, aplikasi diam total (tidak mengganggu).

## Cara menerbitkan rilis baru (untuk integrator)

Jalankan dari proyek `gramora-speak` (bukan dari repo ini):

```bash
./tools/upload-release.sh app/gramora-speak-v1.8.1.apk 11 1.8.1 qa/catatan-v181.txt
```

Skrip itu membuat GitHub Release (tag `v1.8.1`), mengunggah APK ke
`releases/`, lalu memperbarui `version.json` di repo ini.

## Catatan teknis

- APK di-host sebagai file repo (`releases/...`) dan dilayani lewat
  `raw.githubusercontent.com`, BUKAN sebagai aset Release di
  `releases/download/...` — konektor GitHub yang dipakai di sini hanya
  terdaftar untuk `api.github.com` (unggah aset ke `uploads.github.com`
  ditolak 401, sudah diverifikasi 22 Nov 2026). GitHub Release tetap dibuat
  untuk penanda/tag + catatan perubahan.
- Jangan unggah APK palsu / dummy sebagai rilis. Uji skrip hanya dengan tag
  uji (`v0.0.0-test`) lalu hapus rilis + tag + file ujinya.
