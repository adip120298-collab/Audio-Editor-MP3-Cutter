# Audio Editor & MP3 Cutter

**Potong lagu, edit audio, ubah format, dan buat nada dering dalam satu aplikasi. Gratis, offline, tanpa watermark.**

Aplikasi Android native (bukan PWA) untuk memotong dan mengedit audio langsung di HP. Seluruh proses berjalan di perangkat, tanpa internet dan tanpa mengunggah file ke server mana pun.

## Fitur

| Fitur | Keterangan |
|---|---|
| Cut & Trim | Waveform dengan dua garis geser, plus kolom waktu dalam detik untuk presisi milidetik. Putar pilihan sebelum menyimpan. |
| Hapus pilihan | Buang bagian tengah lagu, sisanya tersambung otomatis. |
| Split | Bagi satu file panjang per durasi (misalnya tiap 60 detik) atau di titik tertentu. |
| Merge | Gabungkan 2-10 file dengan jeda yang bisa diatur. Format berbeda tetap bisa digabung. |
| Copy & Paste / Loop | Gandakan bagian terpilih (misalnya reff) 2-20 kali. |
| Fade In / Fade Out | Durasi 1-5 detik. |
| Volume & Normalize | Naikkan volume pilihan atau samakan puncak volume seluruh file. |
| Silence Remover | Pangkas bagian hening pada rekaman suara, voice note, dan podcast. Ambang hening bisa diatur. |
| Speed & Pitch | Kecepatan 0.5x-2x tanpa mengubah nada. Nada ±12 semiton tanpa mengubah kecepatan. |
| Video ke Audio | Ambil suara dari video MP4, MKV, MOV, dan WEBM yang didukung perangkat. |
| Konversi | Simpan ke WAV, MP3, M4A/AAC, M4R (nada dering iPhone), OGG Opus, dan FLAC. |
| Konversi Batch | Ubah hingga 10 file sekaligus ke format yang dipilih. |
| Compressor | Kecilkan ukuran file (mono 22 kHz) lalu simpan ke bitrate rendah. |
| Undo | Satu langkah kembali. |

Pilihan bitrate: MP3 128/192/256/320 kbps, M4A 64/128/192/256/320 kbps, OGG Opus 96/160 kbps.

## Persyaratan

- Android 10 (API 29) atau lebih baru.
- Tidak memerlukan izin internet maupun izin penyimpanan.

## Lokasi hasil simpan

- WAV, MP3, M4A, OGG, FLAC: `Music/AudioEditor`
- M4R (nada dering): `Download/AudioEditor`

## Cara membangun APK lewat GitHub (tanpa komputer)

1. Buat repo baru di GitHub.
2. Unggah file ZIP proyek (`AudioEditorMP3Cutter_V2_Source.zip`) ke root repo lewat **Add file → Upload files**.
3. Buat file `.github/workflows/build.yml` lewat **Add file → Create new file**, lalu isi dengan workflow yang disediakan.
4. Buka tab **Actions** dan tunggu build selesai.
5. Unduh artifact `AudioEditor-MP3Cutter-debug-apk`, ekstrak, lalu pasang `app-debug.apk` di HP. Izinkan pemasangan dari sumber ini jika diminta.

Workflow otomatis membuka ZIP di root repo jika proyek belum diekstrak.

## Cara membangun di komputer

Syarat: JDK 17, Android SDK (platform 34), Gradle 8.9.

```bash
gradle assembleDebug
```

APK ada di `app/build/outputs/apk/debug/`.

## Struktur proyek

```
app/src/main/java/com/adip/mp3cutter/
├── MainActivity.kt    # antarmuka dan alur kerja
├── WaveformView.kt    # tampilan waveform dan garis pilihan
└── Engine.kt          # decode, edit, dan encode audio
```

## Cara kerja singkat

- Decode: `MediaExtractor` + `MediaCodec` mengubah audio atau trek audio video menjadi PCM 16-bit.
- Edit: semua operasi dilakukan pada PCM di memori.
- Encode: WAV ditulis langsung, M4A/M4R/OGG lewat `MediaCodec` + `MediaMuxer`, FLAC lewat encoder bawaan Android, MP3 lewat LAME.

## Batasan yang perlu diketahui

- File sangat panjang (lebih dari sekitar 30 menit) dapat memakan banyak memori karena audio dimuat penuh ke RAM.
- Tag dan cover lagu belum dibawa saat konversi.
- Encoder OGG Opus dan FLAC memakai codec bawaan Android, sehingga dukungannya bergantung pada perangkat.
- Mengganti ekstensi M4A menjadi M4R dilakukan otomatis. Pada beberapa iPhone, nada dering tetap perlu dimasukkan lewat Finder atau iTunes.
- Kecepatan dan nada diubah dengan algoritma WSOLA sederhana, jadi hasil ekstrem (0.5x atau 2x) bisa terdengar sedikit bergetar pada musik.

## Kredit

- [TAndroidLame](https://github.com/naman14/TAndroidLame) (pembungkus LAME) untuk encoder MP3. LAME berlisensi LGPL.

## Lisensi

Tentukan lisensi proyek ini sebelum dipublikasikan (misalnya MIT). Jika memakai LAME, patuhi syarat LGPL.
