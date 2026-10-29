# whisper-model

[English](README.md) | **Bahasa Indonesia**

Tempat penyimpanan model **faster-whisper small** untuk skill Claude **video-audio-transcribe**, yaitu skill yang membuat Claude bisa "mendengar" audio dari video yang di-upload ke chat.

## Kenapa repo ini ada

Claude hanya bisa melihat gambar (frame) dari video, tidak bisa mendengar suaranya. Solusinya adalah mengubah audio menjadi teks dengan Whisper, lalu teks itu dibaca Claude bersama frame video.

Masalahnya, container tempat Claude menjalankan kode tidak bisa mengakses HuggingFace, tempat model Whisper biasanya diunduh. GitHub tidak diblokir, jadi model disimpan di sini sebagai **Release**.

## Isi

| Release | File | Ukuran |
|---|---|---|
| `model-v1` | `faster-whisper-small.tar.gz` | ± 426 MB |

Di dalam arsip:

```
faster-whisper-small/
├── model.bin
├── config.json
├── tokenizer.json
└── vocabulary.txt
```

Link unduh langsung:

```
https://github.com/ashfy/whisper-model/releases/download/model-v1/faster-whisper-small.tar.gz
```

## Cara kerja skill

1. Video di-upload ke chat Claude.
2. Skill mengunduh model dari Release repo ini.
3. Audio diekstrak dengan ffmpeg, lalu ditranskripsi oleh faster-whisper (default Bahasa Indonesia).
4. Frame video diambil dan disusun menjadi grid bertimestamp.
5. Claude menggabungkan transkrip dan frame menjadi satu analisis.

Hasil tes: video 42 detik selesai ditranskripsi dalam ± 21 detik.

## Membuat ulang Release

Release dibuat otomatis oleh GitHub Actions lewat file `.github/workflows/model.yml`. Kalau Release terhapus atau ingin dibuat ulang:

1. Hapus Release dan tag `model-v1` yang lama (jika masih ada).
2. Buka tab **Actions** → **Siapkan model Whisper** → **Run workflow** (branch `main`).
3. Tunggu sampai muncul centang hijau (± 2–5 menit).

Workflow tersebut mengunduh `Systran/faster-whisper-small` dari HuggingFace, mengemasnya menjadi `.tar.gz`, lalu mengunggahnya ke Release `model-v1`.

## Penting

- Repo ini harus tetap **Public**, karena container Claude tidak bisa login ke GitHub.
- Jangan ganti nama repo, tag, atau nama file tanpa memperbarui skill, karena skill mengunduh dari link di atas.

## Lisensi & sumber

- Model: [Systran/faster-whisper-small](https://huggingface.co/Systran/faster-whisper-small), konversi CTranslate2 dari [OpenAI Whisper](https://github.com/openai/whisper) (lisensi MIT).
- Engine: [faster-whisper](https://github.com/SYSTRAN/faster-whisper) (lisensi MIT).
