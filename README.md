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
8. Saat menjalankan LLM, mungkin Anda akan mengalami respon yang lambat karena tipe instance maksimum yang tersedia hanya **Large**, sedangkan komputasi LLM akan lebih optimal jika menggunakan GPU atau vCPU yang lebih besar. Hal ini bukan masalah, yang penting LLM berjalan dengan benar dan tetap dapat diakses, meskipun agak lambat.
9. Pastikan Anda memberi label pada setiap AWS service yang Anda buat, kecuali service yang terbentuk otomatis. Memperhatikan detail ini akan membantu mendapatkan poin tambahan.
10. Ingat untuk selalu memberi deskripsi yang jelas agar hasil kerja Anda mudah dipahami. Hal ini bisa menambah poin.
11. Sebelum proyek berakhir, lakukan review terhadap pekerjaan Anda dan hapus semua service yang tidak diperlukan agar tidak membingungkan hasil akhir. Jika tidak, Anda bisa kehilangan banyak poin.
12. Bahasa pemrograman yang digunakan dalam proyek ini adalah JavaScript dengan Node.js versi 18 atau lebih baru.

## Arsitektur

![Infra Diagram](https://github.com/Ramdan241004/Soal-lks-1-2024/blob/main/Modul%201%20%E2%80%93%20Distributed%20LLM.png) 

Contoh arsitektur yang diberikan hanya merupakan salah satu kemungkinan desain untuk aplikasi English AI Assistant. Ini bukan arsitektur final yang wajib diikuti. Arsitektur tersebut menunjukkan bagaimana tim development membangun sistem aplikasi untuk mempermudah pemahaman cara kerjanya. Silakan baca bagian Application Details.

## Detail Aplikasi

Proyek ini melibatkan deployment aplikasi SaaS yang dirancang untuk English AI Assistant. Tujuan aplikasi ini adalah untuk meningkatkan pembelajaran bahasa Inggris melalui dialog interaktif dengan AI. Fitur utama aplikasi: Mengidentifikasi kesalahan berbahasa. Memberikan saran kata yang lebih tepat. Menyediakan tips penggunaan kata dalam percakapan. Aplikasi ini dibangun menggunakan: Next.js 14 (untuk frontend). Open-source LLM model (untuk AI backend).

Tugas Anda: Deploy frontend (client application) menggunakan ***AWS Amplify***. Deploy backend (API Gateway, Lambda, Database, LLM, dll.) sesuai arsitektur.
