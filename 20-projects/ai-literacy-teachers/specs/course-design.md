---
title: Spesifikasi Rancangan Kurikulum Workshop, Aplikasi Mobile, dan Literasi AI untuk Guru Indonesia
aliases: [course-design, mobile-app-spec]
type: spec
project: ai-literacy-teachers
status: active
created: 2026-07-14
updated: 2026-07-15
tags: [spec, kurikulum, ai-literacy, guru-indonesia, workshop, mobile-app, design-system]
source: C:\research-workspace\research\2026-07-14-ai-literacy-upskilling-indonesian-teachers
---

# Spesifikasi Rancangan Kurikulum Workshop, Aplikasi Mobile, dan Literasi AI untuk Guru Indonesia

> [!abstract] Goal
> Membangun produk **Aplikasi Mobile-First (Course + App)** yang dipadukan dengan **Sesi Praktik Langsung (Workshop)** untuk: (1) membangun rasa percaya diri dan kelancaran pribadi (*Personal Fluency First*) Guru SD/SMP/SMA/SMK di Indonesia terlebih dahulu, lalu (2) membekali mereka keterampilan mentransfer etika dan penggunaan AI yang tepat kepada siswa di kelas.

## Background

Spesifikasi ini merupakan turunan langsung dari riset pembandingan (*benchmarking*) terhadap 7 platform global dan lokal (*MagicSchool AI, Khanmigo, Elements of AI, Common Sense, Google AI Essentials, Ruangguru, dan Platform Merdeka Mengajar/PMM*). 

Akar permasalahan utama guru Indonesia dalam mengadopsi AI adalah rendahnya rasa percaya diri (*digital confidence*), keterbatasan waktu (*time-poor*), dan kecemasan menghadapi kotak percakapan kosong (*blank box syndrome*). Jika langsung diajarkan teori rumit atau instruksi (*prompting*) tingkat lanjut tanpa keberhasilan nyata terlebih dahulu, guru cenderung mundur atau melarang total penggunaan AI di kelas.

Oleh karena itu, rancangan produk dan kurikulum ini menerapkan prinsip **"Personal Fluency First"** (guru harus mahir dan merasakan manfaat langsung bagi tugas admin/mengajar mereka sebelum bisa membimbing siswa), didukung oleh aplikasi mobile-first yang dapat diakses dalam kondisi kuota/bandwidth terbatas (*low-bandwidth & offline-capable*).

Tautan ke catatan konteks proyek: [ai-literacy-teachers](../ai-literacy-teachers.md) | [modul-workshop-perangkat-pembelajaran](../context/modul-workshop-perangkat-pembelajaran.md) | [modul-etika-dan-kesepakatan-kelas](../context/modul-etika-dan-kesepakatan-kelas.md) | [glosarium-dan-panduan-fasilitator](../context/glosarium-dan-panduan-fasilitator.md)

---

## 1. Kompetensi Keluaran (*Exit Competencies - C1 s.d. C6*)

Spesifikasi dirancang mundur dari 6 kompetensi inti yang terbagi dalam dua alur (*strand*). Alur A mendahului Alur B:

### Alur A — Personal Fluency (*Do It Myself / Kemahiran Pribadi*)
- **C1 (Prompt & Refine):** Guru mampu menyusun dan menyempurnakan instruksi (*prompt*) untuk menghasilkan perangkat pembelajaran yang akurat sesuai konteks kelasnya.
- **C2 (Verify & Evaluate):** Guru memiliki kebiasaan mengevaluasi kritis (*critical evaluation*) terhadap keluaran AI untuk mendeteksi halusinasi, bias, atau ketidaksesuaian kurikulum.
- **C3 (Workflow Integration):** Guru mengintegrasikan AI ke dalam $\ge 3$ alur kerja nyata (perencanaan modul ajar/RPP, pembuatan soal evaluasi, dan komunikasi/administrasi).

### Alur B — Classroom Transfer (*Teach It / Transfer ke Kelas*)
- **C4 (Explain AI in Bahasa):** Guru mampu menjelaskan apa itu AI dan mengapa "penggunaannya yang tepat" penting dengan bahasa Indonesia yang santun dan mudah dipahami siswa.
- **C5 (Facilitate & Discuss):** Guru mampu memfasilitasi pelajaran literasi AI dan memimpin diskusi dilema etika/integritas akademis (*Dilemma Discussion*).
- **C6 (Policy & Honest Assignment):** Guru menyusun kebijakan/Kesepakatan Kelas penggunaan AI dan merancang penugasan yang memanfaatkan AI secara jujur (*AI-assisted, not AI-replaced*).

---

## 2. Arsitektur Produk & Pedagogi Inti (Hasil Riset Benchmarking F1–F8)

### A. Form-First Generation ("No Blank Box") [F1]
- **Masalah:** Kotak percakapan terbuka (*blank chat box*) menuntut keterampilan *prompt engineering* yang justru menjadi penghalang utama bagi pemula.
- **Solusi Produk:** Mengganti *blank box* pada tahap awal dengan formulir terstruktur (*structured form generator*). Guru memilih alat berniat kerja (misal: *Generator Modul Ajar Kurikulum Merdeka, Generator Kuis 10 Soal HOTS*), memilih dropdown *jenjang/mapel*, mengisi topik pokok, dan AI langsung menghasilkan draf pertama yang siap pakai dalam hitungan menit (*first quick win*).

### B. Tangga Kelancaran Instruksi (*The Prompting Fluency Ladder*) [F2]
Kurikulum tidak langsung mengajarkan teknik *prompting*, melainkan menaiki anak tangga secara bertahap:
1. **Shield (Melindungi / Modul 2):** Menyembunyikan kerumitan *prompting* sepenuhnya di balik formulir terstruktur (*Form-First*) agar guru merasakan keberhasilan (*enactive mastery*).
2. **Minimize (Meminimalkan / Modul 3):** Mempertahankan bantuan antarmuka sambil mengajari cara memeriksa kesalahan (*verify*) dan memberikan instruksi revisi sederhana.
3. **Teach (Mengajarkan / Modul 4):** Membuka kotak percakapan dan mengajarkan *prompting* sebagai keterampilan yang dapat ditransfer menggunakan **Rumus P.T.K.F. (Peran, Tugas, Konteks, Format)**.

### C. Sekuensi Etika Setelah "Quick Win" Pertama [F4]
- **Aturan PII Depan (Modul 1):** Aturan mutlak perlindungan data diposisikan di awal: **"Jangan pernah memasukkan data pribadi siswa (PII/rapor/identitas) ke dalam AI."**
- **Etika & Dilema (Modul 5):** Pembahasan mendalam mengenai bias, halusinasi, dan etika integritas akademis (*Teman Belajar vs. Joki Tugas*) diposisikan di pertengahan/akhir setelah guru merasakan langsung manfaat AI (*after the first win*), bukan di awal yang dapat memicu ketakutan/penolakan.

### D. Mesin Siklus Belajar (*Unit Learning Loop*) [F7]
Setiap unit pembelajaran mikro (*micro-learning unit* berdurasi 8–12 menit) di aplikasi mobile berputar dalam 4 ketukan tetap untuk menurunkan beban kognitif:
1. **Hook (~30 detik):** Mengangkat masalah nyata guru dalam Bahasa Indonesia (misal: *"Menghabiskan Minggu malam hanya untuk membuat rubrik penilaian?"*).
2. **Model (2–3 menit):** Contoh yang dikerjakan (*worked example*) dengan narasi langkah demi langkah.
3. **Do-It-With-Your-Own-Material (3–5 menit):** Guru langsung mengisi formulir/instruksi dengan materi kelas *mereka sendiri* dan keluar membawa artefak nyata (Modul Ajar/Soal).
4. **Reflect + Micro-check (~1 menit):** Refleksi transfer kelas dan satu pertanyaan evaluasi kritis (*spot-the-flaw*).

---

## 3. Peta Kurikulum: Integrasi 6 Modul Aplikasi & 3 Sesi Workshop

Pelatihan menggabungkan **6 Modul Pembelajaran Mandiri di Aplikasi Mobile** dengan **3 Sesi Workshop Praktik Langsung (Jadwal 2.5 Jam)**:

| Modul Aplikasi Mobile | Sesi Workshop Terkait | Fokus Kompetensi & Mekanisme Inti | Keluaran Nyata (*Artifact*) |
|---|---|---|---|
| **M1: "AI Tidak Akan Menggantikan Anda"** (Orientasi & Aturan PII) | Prasyarat Mandiri / Pembuka W1 | Penurunan rasa takut, pemahaman dasar AI, dan aturan mutlak **[Danger Green/Red]: Tanpa PII Siswa**. | Kuis Mikro 3 Soal Mitos vs. Fakta |
| **M2: "10 Menit Pertama Anda Kembali"** (First Win — SHIELD) | **Workshop Modul 1 (W1)** <br/> *(Penyusunan Perangkat)* | **Shielded Prompting [F1, F2]:** Menggunakan *Form-First Generator* di aplikasi untuk membuat Modul Ajar Kurikulum Merdeka / RPP. | 1 Draf Modul Ajar / RPP Lengkap |
| **M3: "Jangan Percaya Begitu Saja"** (Critical Evaluation — MINIMIZE) | **Workshop Modul 1 (W1)** <br/> *(Evaluasi & Kuis)* | **Minimize Prompting [F2]:** Membangun kebiasaan **[Verify Blue]** memeriksa fakta, kurikulum, dan merancang 10 soal kuis HOTS beserta rubrik. | 1 Paket Kuis 10 Soal + Rubrik Penilaian |
| **M4: "Rutinitas Mingguan"** (Prompting Mastery — TEACH) | Mandiri di Aplikasi / *Asynchronous Hub* | **Teach Prompting [F2]:** Menguasai **Rumus P.T.K.F.** di *blank box* untuk integrasi ke tugas administrasi dan komunikasi orang tua siswa. | 3 Templat Prompt Kustom Pribadi |
| **M5: "Penggunaan yang Bertanggung Jawab"** (Etika & Integritas) | **Workshop Modul 2 (W2)** <br/> *(Bimbingan & Etika)* | **Ethics After Win [F4, F3]:** Menelaah batasan *Teman Belajar vs. Joki Tugas* (Dilemma Discussion) dan cara AI menahan jawaban langsung. | Analisis Kasus Plagiarisme vs Tutor AI |
| **M6: "Dari Learner ke Teacher"** (Classroom Transfer Kit) | **Workshop Modul 2 (W2)** <br/> *(Kesepakatan Kelas)* | **Transfer Assets [F5, C6]:** Mengunduh paket transfer kelas siap pakai dari platform dan menyesuaikan untuk sekolah masing-masing. | Poster Kesepakatan Kelas Penggunaan AI |
| **Capstone: Praktik Nyata di Kelas** | **Workshop Modul 3 (W3)** <br/> *(Showcase & Handoff)* | **PD Recognition [F8]:** Unggah bukti penerapan/draf proyek akhir ke platform dalam 7 hari untuk validasi otomatis sertifikat resmi 32 JP. | **Sertifikat Kelulusan Resmi (32 JP / IHT)** |

---

## 4. Sistem Desain & Spesifikasi Antarmuka Aplikasi Mobile (*Design Foundations*)

Aplikasi diproyeksikan 100% **Mobile-First** (mengacu pada standar PMM dan Ruangguru [F8]) dengan parameter desain dasar (*Design Foundations*) berikut:

### A. Parameter Tata Letak (*Layout & Viewport Constraints*)
- **Kanvas Utama:** Portrait **360–390px** lebar (target pengujian utama: HP Android spesifikasi menengah/rendah saat layar terkena cahaya matahari siang).
- **Struktur Layar:** Satu kolom tunggal (*single column vertical scroll*), **satu aksi utama per layar (*One Primary Action*)**, dan tanpa navigasi horizontal untuk konten utama.
- **Navigasi Jempol (*Thumb-Zone Nav*):** Bilah tab bawah (*bottom tab bar*) untuk menu utama dan tombol pil menempel di bawah (*sticky bottom pill button*) untuk aksi utama. Aksi destruktif dijauhkan dari jangkauan ketuk cepat jempol.
- **Input Formulir:** Pemilih *jenjang/mapel* menggunakan **Bottom-Sheet Pickers** (bukan dropdown atas yang rentan tertutup *keyboard* virtual).
- **Ketahanan Kuota (*Performance & Offline Resiliency*):** Aset gambar minim, ikon SVG *outline* ringan (Lucide 24px grid), tanpa gradien berat, *subset font*, dan modul dapat diunduh untuk diakses secara luring (*offline-capable*).

### B. Token Warna & Aksesibilitas (WCAG 2.2 AA Compliance)
Desain mengadopsi identitas **"Calm & Credible"** yang santun dan tepercaya, memperbaiki kegagalan kontras teks pada platform pembanding (*a11y-audit fixes*):

| Token Semantik | Light Mode | Dark Mode | Fungsi & Target Kontras (*Contrast Target*) |
|---|---|---|---|
| `surface` | `#FBF9F5` | `#16140F` | Latar belakang halaman (hangat & nyaman dimata) |
| `card` | `#FFFFFF` | `#262019` | Komponen kartu (*raised surface*) |
| `ink` | `#1C1917` | `#F5F1EA` | Teks utama & judul ($\approx 15:1$ pada kedua mode) |
| `ink-muted` | `#57534E` | `#B8B0A4` | Teks sekunder ($\approx 7:1$, memperbaiki teks abu-abu pudar yang gagal audit) |
| `primary` | `#0F766E` | `#2DD4BF` | **Deep Teal** — Identitas utama, tombol aksi primer |
| `on-primary` | `#FFFFFF` | `#0B1F1C` | Teks/ikon di atas tombol primary ($\ge 4.8:1$) |

#### Warna Makna Semantik (*Semantic Signal Colours - Tidak untuk Dekorasi!*)
- **`win` (`#F59E0B` Amber):** HANYA muncul saat momen keberhasilan (*First win badge, streak, modul selesai*).
- **`verify` (`#DBEAFE` / Text `#1D4ED8` Blue):** HANYA muncul pada petunjuk pemeriksaan kritis (*M3 critical evaluation cue*).
- **`safe` (`#047857` Green):** Konfirmasi keamanan privasi data atau jawaban benar.
- **`danger` (`#B91C1C` Red):** Aturan larangan keras (*Never-do: Jangan salin PII siswa* / error sistem).

### C. Tipografi & Gaya Bahasa (*Register*)
- **Typeface:** **Plus Jakarta Sans** (Weights: 400, 500, 600, 700), disubset (*Latin + Latin-ext*) agar ringan diunduh.
- **Gaya Bahasa (*Register*):** **"Anda"** (Hangat, santun, dan menghormati profesi guru).
- **Istilah AI (*Scaffolded Jargon*):** Menggunakan bahasa penjelasan sederhana di awal, memperkenalkan nama teknis resmi, lalu menggunakannya konsisten (misal: *Instruksi Tugas $\rightarrow$ Prompt $\rightarrow$ Prompting*).
- **Bentuk Komponen (*Pill-First UI*):** Tombol, *chips*, *tabs*, dan *badges* berbentuk pil (*pill-shaped*); kartu dan *bottom-sheets* bersudut halus (*rounded-xl*). Tanpa emoji di antarmuka chrome (hanya ikon SVG).

---

## Requirements

- **R1:** Platform wajib menyediakan alur *onboarding* berbasis formulir terstruktur (*Form-First Tools*) untuk minimal 3 generator: Modul Ajar Kurikulum Merdeka, Soal Evaluasi HOTS + Rubrik, dan Kesepakatan Kelas AI.
- **R2:** Sistem aplikasi harus mendukung mode gelap (*Dark Mode v1 peer theme*) dan menjaga rasio kontras teks tubuh minimal $4.5:1$ (target $7:1$) dan komponen UI minimal $3:1$ sesuai hasil audit WCAG 2.2 AA.
- **R3:** Alur pembelajaran di aplikasi harus menyimpan progress lokal (*offline caching*) sehingga guru tetap dapat membaca modul dan menyusun draf saat koneksi internet terputus sementara.
- **R4:** Platform LMS harus terintegrasi dengan pengumpulan tugas akhir (*Capstone Submission*) yang memicu verifikasi/penerbitan otomatis sertifikat pelatihan resmi 32 JP / IHT setelah draf disetujui.

---

## Non-goals

- **NG1:** Membangun atau melatih model LLM (Large Language Model) dari nol (aplikasi memanfaatkan API model terdepan yang diamankan dengan *system prompt* pedagogis).
- **NG2:** Menyediakan alur berbayar/langganan individual dalam aplikasi (*Never pay guardrail* — pelatihan gratis atau disponsori institusi/dinas).
- **NG3:** Membangun panel administrasi sekolah (*back-office ERP*) yang kompleks di luar kebutuhan tracking kepesertaan workshop.

---

## Acceptance Criteria

- [ ] **AC1 (Time-to-Value):** 90% guru pemula berhasil menghasilkan 1 draf Modul Ajar / RPP yang valid dalam waktu $< 15$ menit sejak pertama kali masuk ke aplikasi (*First Win Metric*).
- [ ] **AC2 (Digital Confidence):** Evaluasi pasca-pelatihan (SEQ + survei 1–5) menunjukkan peningkatan skor rasa percaya diri (*digital confidence*) minimal $+75\%$.
- [ ] **AC3 (A11y & Mobile Verification):** Pengujian pada perangkat Android `360px` di bawah cahaya terang lulus 100% pada pengecekan kontras teks (`ink`, `ink-muted`, `on-primary`) tanpa ada *horizontal reflow/scroll*.
- [ ] **AC4 (Classroom Transfer):** 100% peserta workshop menghasilkan minimal 1 draf Poster Kesepakatan Kelas AI (*Teman Belajar vs. Joki Tugas*) yang siap diterapkan di sekolahnya.

---

## Affected Repos / Files

- `ai-literacy-teachers/mobile-app/` *(Rancangan frontend Flutter/React Native/Web-App)*
- `ai-literacy-teachers/backend-services/` *(API Gateway untuk Form-First Prompt Generator & Certificate Engine)*
- `context/course-design.md` & `context/modul-*.md` *(Panduan kurikulum dan templat prompt P.T.K.F.)*

---

## Open Questions

- **OQ1:** Apakah verifikasi masuk tunggal (*Single Sign-On / SSO*) menggunakan akun `belajar.id` Kementerian Pendidikan dapat diintegrasikan langsung pada fase v1 tanpa menimbulkan *error gateway login* bagi guru sekolah swasta/daerah terpencil?
- **OQ2:** Untuk fase transfer kelas (Modul 6), apakah guru lebih memilih poster format PDF statis siap cetak atau tautan templat Canva yang dapat disunting bersama siswa di kelas?

> [!note] Tracking
> Status, penugasan tiket pengembangan aplikasi, dan progres pelaksanaan workshop dilacak melalui **ClickUp**, BUKAN di dalam catatan ini.
