# Dokumen Pengadaan: Unit Bangunan Daop 2 Bandung

Aplikasi satu halaman untuk dokumen pengadaan. Versi ini memakai Firebase (login email dan Firestore), jadi data tersimpan permanen dan dipakai bersama dua admin.

## Pasang Firebase (sekali saja)
1. Buka https://console.firebase.google.com, klik **Add project**, beri nama bebas, lalu buat (Google Analytics boleh dimatikan).
2. Di **Build > Authentication > Get started**, aktifkan **Email/Password**. Buka tab **Users > Add user**, tambahkan **dua** akun admin (email dan kata sandi).
3. Di **Build > Firestore Database > Create database**, pilih lokasi (misalnya `asia-southeast2` Jakarta), pilih **Production mode**.
4. Buka tab **Rules** di Firestore, ganti isinya dengan ini (ganti kedua email dengan email admin), lalu **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /store/{doc} {
      allow read, write: if request.auth != null
        && request.auth.token.email in ['admin1@email.com', 'admin2@email.com'];
    }
  }
}
```
5. Di **Project settings (ikon roda gigi) > General > Your apps**, klik ikon **Web (</>)**, daftarkan aplikasi, lalu salin objek `firebaseConfig`.
6. Edit file `firebase-config.js` di repositori ini: tempel nilai konfigurasi dan isi `FIREBASE_ADMINS` dengan dua email yang sama dengan langkah 2 dan 4. Commit.
7. Buka halaman GitHub Pages. Masuk dengan salah satu email admin. Jika muncul galat domain, tambahkan `agilp.github.io` di Authentication > Settings > Authorized domains.

## Catatan
- Selama `firebase-config.js` belum diisi, aplikasi berjalan dalam mode lokal (data tidak tersimpan).
- Aturan Firestore di atas membuat hanya dua email itu yang bisa membaca dan menulis data.
- Ekspor cadangan rutin dari menu Admin > Cadangan data.
