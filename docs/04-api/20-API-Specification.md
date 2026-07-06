API SPECIFICATION



Proyek: Platform SaaS Undangan Digital



Lokasi Berkas: docs/04-api/API-Specification.md



Status: Ready for Development



Base URL: https://api.undangan-digital.com/v1



1\. Global API Standards



Authentication: Bearer Token (JWT).



Format: JSON.



Headers: \* Accept: application/json



Authorization: Bearer {token}



Success Code: 200 OK, 201 Created.



Error Code: 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 422 Unprocessable Entity.



2\. Auth Module



POST /auth/register



Deskripsi: Registrasi pengguna baru.



Request Body: { "name", "email", "password", "password\_confirmation" }



Validation: Email unique, password min 8 char.



Response Success: 201 - { "message": "User registered successfully" }



POST /auth/login



Deskripsi: Autentikasi pengguna dan mendapatkan token.



Request Body: { "email", "password" }



Response Success: 200 - { "token": "...", "user": {...} }



3\. User Module



GET /user/profile



Auth: Required (Bearer Token)



Response Success: 200 - { "id", "name", "email", "role" }



PATCH /user/profile



Auth: Required (Bearer Token)



Request Body: { "name" }



4\. Builder Module



POST /builder/save-draft



Auth: Required (Bearer Token)



Request Body: { "invitation\_id", "draft\_payload": JSON }



Notes: Menggunakan lock\_version untuk Optimistic Locking.



GET /builder/draft/{invitation\_id}



Auth: Required (Bearer Token)



Response Success: 200 - { "draft\_payload" }



5\. Invitation Module



GET /invitations



Auth: Required



Response Success: 200 - { "data": \[...] }



POST /invitations



Auth: Required



Request Body: { "template\_id", "slug", "events": \[...] }



Permission: User harus memiliki lisensi template aktif.



6\. RSVP Module



POST /invitations/{slug}/rsvp



Auth: Public



Request Body: { "guest\_name", "attendance", "total\_persons" }



Response Success: 201 - { "message": "RSVP recorded" }



7\. Wishes Module



POST /invitations/{slug}/wishes



Auth: Public



Request Body: { "guest\_name", "message" }



Validation: Moderasi teks (tidak boleh mengandung kata kasar).



8\. Gallery Module



POST /invitations/{invitation\_id}/gallery



Auth: Required



Request Body: { "image\_url", "caption" }



Permission: Owner invitation.



9\. Music Module



PUT /invitations/{invitation\_id}/music



Auth: Required



Request Body: { "music\_url", "auto\_play": boolean }



10\. Orders Module



POST /orders



Auth: Required



Request Body: { "template\_id" }



Response Success: 201 - { "order\_id", "payment\_url" }



11\. Payment Module



POST /payment/webhook



Auth: Internal / Gateway (Signature Verified)



Request Body: Payload dari Midtrans (Webhook).



Notes: Update status order menjadi paid jika verifikasi sukses.



12\. Admin Module



GET /admin/dashboard



Auth: Required (Admin Role)



Response Success: 200 - { "total\_users", "total\_orders", "revenue" }



13\. Super Admin Module



POST /super-admin/templates



Auth: Required (Super Admin Role)



Request Body: { "name", "price", "category\_id" }



14\. Reseller Module



GET /reseller/wallets



Auth: Required (Reseller Role)



Response Success: 200 - { "balance", "withdrawal\_history" }



15\. Analytics Module



GET /analytics/{invitation\_id}/daily



Auth: Required (Owner)



Response Success: 200 - { "dates": \[...], "views": \[...] }



Kesimpulan



Spesifikasi API ini dirancang untuk mendukung skalabilitas tinggi dengan pemisahan akses antara publik (RSVP/Wishes) dan privat (Builder/Admin). Pengembang disarankan menggunakan validasi Laravel Request Objects untuk setiap endpoint guna menjamin integritas data sebelum menyentuh database.

