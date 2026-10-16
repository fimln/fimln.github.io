# alfi.ai.id

Situs profil pribadi satu halaman untuk **https://alfi.ai.id/** yang dibangun menggunakan **Hugo 0.167.0**, HTML template, dan CSS kustom bertema gelap (`#0b1220`) dengan latar grid blueprint interaktif. Situs artikel blog dikelola secara terpisah di repositori `fimln/blog` (`https://blog.alfi.ai.id/`).

Konten utama berada di `content/_index.md`, template di `layouts/`, serta seluruh aset statis di `static/`. Font (`Inter`) dan ikon teknologi disimpan secara lokal beserta lisensinya di `static/licenses/`, sementara kartu repositori dan statistik GitHub dimuat dari endpoint publik `github-stats-extended.vercel.app`.

Untuk pengembangan lokal di Windows, letakkan binari `hugo.exe` (v0.167.0) di `.tools/hugo/hugo.exe`, lalu jalankan `npm run dev` untuk membuka server di `http://localhost:1314/` atau `npm run build` untuk membangun output statis ke folder `public/`. File `package.json` hanya menyediakan pintasan skrip lokal tanpa dependency npm.

Publikasi dilakukan secara otomatis melalui GitHub Actions (`.github/workflows/hugo.yaml`) setiap kali ada push ke branch `master`. Workflow memasang Hugo 0.167.0 di runner Ubuntu, menjalankan `hugo --gc --minify --cleanDestinationDir`, dan menerbitkan artefak `public/` ke GitHub Pages dengan domain kustom `alfi.ai.id`.
