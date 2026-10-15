# alfi.ai.id profile

Situs profil satu halaman, dimigrasikan dari Jekyll ke Hugo. Konten, layout, warna navy `#0b1220`, dan pola blueprint SVG 24 px dipertahankan. Tidak ada fitur blog baru.

## Stack dan pengembangan lokal

- Hugo standar **0.167.0**, binari lokal `.tools/hugo/hugo.exe`, sama dengan fi-lab.
- npm hanya untuk shortcut perintah lokal Windows; tidak ada dependency npm. CI memasang Hugo langsung, tanpa Node.js atau Wrangler.
- HTML template Hugo dan CSS biasa. Pico.css sengaja tidak digunakan agar aturan bawaan tidak mengubah tampilan profil.
- Inter disimpan lokal, menggunakan berkas dari Google Fonts yang sama dengan halaman lama, bukan mengganti versi font. Lisensi OFL di `static/assets/fonts/INTER-LICENSE.txt`.
- Tidak ada JavaScript aplikasi. Skrip legacy `scale.fix.js` yang membatasi zoom iPhone dilepas; meta viewport standar dipertahankan.

```powershell
npm run dev
```

Buka http://localhost:1314/. Port dibedakan dari fi-lab (1313). `npm run build` menghasilkan `public/`; folder ini tidak dilacak git. Konten profil berada di `content/_index.md`, template di `layouts/`, aset di `static/`.

Hugo bukan dependency npm. Binari 0.167.0 untuk Windows amd64 saat migrasi disalin dari instalasi proyek fi-lab yang sudah tersedia. Untuk checkout baru, ambil versi tersebut dari [rilis resmi Hugo](https://github.com/gohugoio/hugo/releases/tag/v0.167.0) dan tempatkan `hugo.exe` di `.tools/hugo/`. Scripts lokal ini memakai path Windows.

## Aset dan tampilan

- CSS profil asli di `static/assets/css/style.css`, dengan struktur yang berasal dari tema [orderedlist/minimal](https://github.com/orderedlist) melalui `godalming123/minimal`. Atribusi footer lama tetap dipertahankan.
- Font: [stylesheet Inter asli](https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap), subset Latin dan Latin extended diunduh pada 16 Oktober 2026. Browser tidak lagi menghubungi Google Fonts.
- Ikon dari [Skill Icons](https://github.com/tandpfun/skill-icons/tree/7f7e691e71aec64e8354bf697835e009d1ad80f8/icons) dan [Simple Icons](https://github.com/simple-icons/simple-icons). Azure Dark, AWS Dark, Ubuntu Dark, dan JavaScript diambil dari Skill Icons pada 16 Oktober 2026. Roboflow memakai bentuk Simple Icons dengan kotak gelap dan warna ungu terang, tanpa filter invert. Logo [Hermes Agent](https://github.com/NousResearch/hermes-agent/blob/60f56a6b13c8eb9a9e088c6fc1165257c92f7e11/website/static/img/favicon.svg) dari repo resmi Nous Research. Semua ikon lokal; lisensi ada di `static/licenses/`. PHP, C++, dan Arch Linux dilepas. Jupyter memakai [logo original Devicon](https://github.com/devicons/devicon/blob/6a37858047b0e8d9c9c0e68f4a071fc410652e89/icons/jupyter/jupyter-original.svg) berwarna oranye dan abu-abu, tanpa wordmark agar jelas pada ukuran 40 px; lisensi Devicon disimpan lokal.
- Empat kartu repo dan dua kartu statistik memakai [GitHub Stats Extended](https://github.com/stats-organization/github-stats-extended), penerus aktif github-readme-stats. Di antara kandidat yang diperiksa pada 16 Oktober 2026 (Stats Extended, Profile Summary Cards, dan Readme Streak Stats), Stats Extended memiliki pengembangan default-branch paling baru dan rapat; commit terbaru 15 Oktober 2026. Proyek lama menyatakan tidak lagi dipelihara dan merekomendasikan penerus ini.
- `content/_index.md` memuat empat repo yang dipin pada [profil fimln](https://github.com/fimln), dalam urutan saat diperiksa 16 Oktober 2026: RentHub, RouteFinder, SelfInstruct, GlobalHE. Pilihan repo disimpan dalam konten; perbarui daftar ini jika pin profil berubah. Isi kartu diperbarui oleh layanan, tanpa JavaScript aplikasi atau token GitHub di browser/repo.
- Kartu menggunakan endpoint `/api/pin/`, `/api`, dan `/api/top-langs/` di `github-stats-extended.vercel.app`, tema `react`, serta animasi dimatikan. [Dokumentasi statistik](https://github-stats-extended.vercel.app/frontend/docs/cards/stats/) menjelaskan bahwa private stats membutuhkan akses tambahan; parameter lama `count_private` dilepas. Tidak ada akses privat yang diberikan.
- Semua enam kartu bergantung pada ketersediaan dan cache layanan eksternal. Endpoint publik dapat terlambat memperbarui data atau terkena pembatasan. CSS, font, foto, dan ikon teknologi tetap lokal. Kartu ditumpuk satu kolom agar teks tetap terbaca di panel desktop yang sempit.
- HTML mentah di Markdown diaktifkan untuk mempertahankan markup ikon dan kartu statistik yang sudah ada. Gunakan hanya konten penulis tepercaya.

## Build dan GitHub Pages Actions

```powershell
npm run build
```

Workflow `.github/workflows/hugo.yaml` mengikuti [panduan resmi Hugo untuk GitHub Pages](https://gohugo.io/host-and-deploy/host-on-github-pages/), disederhanakan untuk situs tanpa SCSS, Hugo Modules, atau dependency npm. Workflow berjalan saat push ke `master` atau lewat `workflow_dispatch`; job deploy hanya berjalan untuk `master`.

CI memakai Ubuntu, memasang Hugo standar 0.167.0 dari rilis resmi, menjalankan `hugo --gc --minify --cleanDestinationDir`, lalu mengunggah `public/` dan deploy lewat actions Pages resmi. Base URL mengikuti output `configure-pages`; custom domain harus tetap `alfi.ai.id`. `static/CNAME` dipertahankan. Output `public/` tidak di-commit. Tidak ada Wrangler, konfigurasi Workers, atau perintah deploy npm dalam proyek ini.

Sebelum publikasi, pastikan **Settings → Pages → Build and deployment → Source = GitHub Actions**, custom domain `alfi.ai.id`, dan konfigurasi situs artikel `/blog/` tetap terpisah. Workflow membangun versi Hugo ini di GitHub dan menerbitkannya ke Pages setelah push ke master. Push ke `master` setelah Source diaktifkan dapat memublikasikan perubahan.

Rollback menggunakan revert commit sumber lalu push ke `master` untuk membangun ulang versi sebelumnya; ini juga memicu publikasi dan perlu instruksi pemilik. Tidak ada rollback Cloudflare di proyek profil ini.

`AGENTS.md` merupakan panduan lokal, diabaikan git melalui `/AGENTS.md`, dan tidak disalin ke output situs. Lab memakai panduan terpisah dengan alur Hugo + Wrangler; jangan menambahkan stack lab ke profil.

## Verifikasi migrasi awal

Diperiksa pada 16 Oktober 2026 dengan build dan browser langsung, tanpa menambahkan suite test: konten dan pola blueprint cocok dengan hasil Jekyll lama; posisi dan ukuran header, panel, serta footer cocok pada lebar 390 dan 1440 px. Gambar lokal termuat, fokus keyboard terlihat, dan tautan kembali dari 404 berfungsi. Pada pemeriksaan migrasi awal, halaman utama/aset lokal dan halaman 404 diperiksa. Pemeriksaan tersebut bukan bukti workflow GitHub Actions sudah berjalan; jumlah aset dapat berubah mengikuti pembaruan ikon. `_site/` lama tetap diabaikan git sebagai acuan lokal, bukan sumber Hugo.


## Penyempurnaan layout

Grid blueprint diperjelas (opacity 0,65), dan tautan Instagram `https://www.instagram.com/alfimlnbr/` ditambahkan dengan label dua baris. Sidebar/footer desktop tetap di tempat, sementara panel kanan mengikuti scroll halaman. Roda mouse berfungsi dari panel, sidebar, maupun area kosong. Foto dan jarak sidebar menyesuaikan pada tinggi desktop hingga 850 px. Padding serta teks footer diperkecil khusus desktop. Lebar hingga 1100 px memakai satu kolom yang dibatasi 760 px; desktop tetap dua kolom. Tombol sosial di sidebar desktop tersusun 2 × 2 dengan jarak 8 px dan tinggi 56 px; mobile/tablet tetap satu baris dengan tinggi 48 px. Teks isi mobile diperbesar.

Diperiksa langsung lewat browser pada 320, 390, 600, 768, 820, 1024, 1100, 1101, dan 1440 px, termasuk tinggi layar 600 px. Tidak ditemukan scroll horizontal, konten sidebar terpotong, panel bertumpuk, atau label tombol terpotong pada ukuran tersebut. Tidak menambahkan berkas test.


## Verifikasi kartu GitHub

Enam endpoint provider diperiksa sebagai HTTP 200 dengan MIME SVG serta konten repo/statistik yang benar. Keenam gambar juga termuat di browser pada lebar 320, 390, 768, 1024, dan 1440 px; semuanya berada di dalam panel tanpa scroll horizontal. Tautan kartu dapat difokuskan lewat keyboard. Lebar kartu bahasa disetel 467 px agar ukuran teksnya seimbang dengan kartu statistik, kemudian diskalakan responsif lewat CSS. Build dan `git diff --check` berhasil. Tidak ada commit, push, atau deploy dalam perubahan ini.

## Verifikasi GitHub Actions dan ikon

Pada 16 Oktober 2026: build lokal normal dan dengan flag CI `--gc --minify --cleanDestinationDir` berhasil. Struktur YAML workflow, branch master, permission job, artifact, CNAME, 404, dependency npm kosong, urutan 12 ikon, dan ketiadaan AGENTS.md pada output diperiksa secara lokal. Browser Chrome pada 390, 768, dan 1440 px memuat semua ikon tanpa scroll horizontal; Roboflow terlihat pada desktop dan mobile. AGENTS.md kedua proyek dikonfirmasi diabaikan git. Workflow belum diuji di runner GitHub, pengaturan Pages belum diubah, dan tidak ada commit/push/deploy.

Situs artikel dipublikasikan dari repo `fimln/blog` menggunakan Jekyll + Actions di `https://blog.alfi.ai.id/`. Profil ini tetap Hugo + Actions di `https://alfi.ai.id/`. Pengalihan prefix `/blog` lama dikelola melalui Cloudflare Single Redirects; Cloudflare bukan hosting profil atau artikel.

Pada mobile (<=600 px), gambar kartu GitHub memakai sudut bawaan SVG tanpa border-radius CSS tambahan agar garis tepinya tidak terpotong ketika diperkecil. Tablet dan desktop mempertahankan aturan sebelumnya. Ikon AWS ditambahkan tepat setelah Azure, tetap disimpan lokal dengan lisensi Skill Icons yang sama.

Perbaikan sudut dan AWS diperiksa di browser lokal pada lebar 320, 390, 600, 768, dan 1440 px; semua gambar termuat tanpa overflow horizontal. Sudut kartu repo/statistik mobile diperiksa secara visual. URL stylesheet memakai hash isi CSS agar perubahan tidak tertahan cache lama. Build dan git diff --check lolos.
