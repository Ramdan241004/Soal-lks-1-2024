# Soal-lks-1-2024

## Deskripsi Proyek dan Tugas

Modul ini berdurasi delapan jam – **Hari ke-2 – Proyek 1: Distributed LLM.**
Tujuan dari proyek ini adalah untuk melakukan deployment SaaS-based AI assistant untuk pembelajaran bahasa Inggris, termasuk infrastruktur yang diperlukan untuk multi-region large language model (LLM).

Peran Anda adalah menyiapkan infrastruktur yang andal, aman, dan hemat biaya untuk AI assistant tersebut. Anda harus memastikan bahwa arsitektur yang dibangun tangguh dan efisien, sehingga pengguna dapat dengan mudah mengakses AI assistant dari region yang mereka pilih.

Target akhirnya adalah menciptakan lingkungan belajar yang stabil, aman, dapat diskalakan, dan ekonomis.


## Tugas

1. Baca dokumentasi dengan teliti (sudah dijabarkan di bawah).
2. Pahami arsitektur aplikasi pada bagian **Architecture.**
3. Baca dengan seksama bagian **Technical Details.**
4. Baca dengan seksama bagian **Application Details.**
5. Login ke AWS Console.
6. Siapkan VPC configuration. Detail konfigurasi VPC ada di **Network Architecture - Service Details.**
7. Siapkan Storage dengan EFS. Baca detailnya di **Storage – Service Details.**
8. Siapkan Security Group. Detail ada di **Security – Service Details.**
9. Siapkan LLM dan Load Balancer Worker untuk instance. Baca di **LLM – Service Details.**
10. Siapkan Relational Database. Detail ada di **Database – Service Details.**
11. Siapkan API Gateway dan Lambda untuk backend. Baca di API Gateway and **Lambda – Service Details.**
12. Siapkan Client Application. Baca di **Client App – Service Details.**
13. Lakukan pengujian menyeluruh pada seluruh infrastruktur dan pastikan semuanya berjalan sesuai harapan.
14. Konfigurasikan monitoring dan metrics yang diperlukan di CloudWatch.

## Detail Teknis

1. Coverage region untuk proyek ini adalah: **us-east-1 (N. Virginia)** dan **us-west-2 (Oregon)**
2. Gunakan **LabRole** untuk semua kebutuhan IAM Role di setiap service, termasuk pada EC2 instance (gunakan **LabInstanceProfile**).
3. Semua source code resource tersedia di GitHub Repository: **https://github.com/betuah/lks-llm**
4. Jangan pernah mengekspos service yang membutuhkan security ke **'anywhere'**. Terapkan security sesuai kebutuhan agar nilai lebih tinggi.
5. Sistem operasi yang diperbolehkan: Ubuntu minimal versi 20.04.
6. Semua konfigurasi yang menggunakan playbooks tidak boleh dikonfigurasi manual via **SSH**.
7. Setiap service yang dibuat harus menggunakan format nama dengan prefix **lks-**, contoh: lks-vpc-zone-a, lks-api-gateway dst.
