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

Proyek ini melibatkan deployment aplikasi SaaS yang dirancang untuk English AI Assistant. Tujuan aplikasi ini adalah untuk meningkatkan pembelajaran bahasa Inggris melalui dialog interaktif dengan AI. Fitur utama aplikasi: Mengidentifikasi kesalahan berbahasa. Memberikan saran kata yang lebih tepat. Menyediakan tips penggunaan kata dalam percakapan. Aplikasi ini dibangun menggunakan: Next.js 14 (untuk frontend). Open-source LLM model (untuk AI backend). Tugas Anda: Deploy frontend (client application) menggunakan ***AWS Amplify***. Deploy backend (API Gateway, Lambda, Database, LLM, dll.) sesuai arendpoints Aplikasi klien harus memiliki fungsionalitas penuh dan akses yang mulus ke backend endpoints. Selain itu, Anda perlu melakukan deployment infrastruktur LLM yang meliputi: LLM Workers, Scoring Workers, Chat Workers Semua komponen ini harus dijalankan di region **N. Virginia (us-east-1)** dan Oregon **(us-west-2)**. Worker tersebut akan menjadi mesin inti untuk pemrosesan LLM. Selanjutnya, Anda harus menyiapkan API endpoints untuk LLM serta membuat endpoint untuk menyimpan data percakapan. Endpoint ini harus dapat diakses publik oleh aplikasi frontend (client app).

Diagram arsitektur tersedia di bagian Architecture. Source code dapat diakses di repositori berikut: ***https://github.com/betuah/lks-llm***

## Detail Layanan

### Client App

Aplikasi klien untuk LLM menggunakan Next.js versi 14 dan akan terkoneksi dengan AWS Cognito untuk: autentikasi, dan memperoleh ID token untuk otorisasi dengan backend API. Anda harus men-setup Cognito User Pool dengan spesifikasi berikut:

- Gunakan email sebagai atribut untuk sign-in.
- Jangan gunakan temporary password dalam kebijakan password.
- Terapkan single authentication factor (faktor tunggal).
- Atribut yang wajib saat sign-up: name dan email.
- Izinkan aplikasi menggunakan refresh token dan user password untuk autentikasi.

Aplikasi klien harus di-deploy ke ***AWS Amplify*** sebagai platform hosting. Detail instalasi dan setup environment sudah ada di README.md dalam repositori client.

> **Catatan**: Jika ada user yang sudah sign-up, Anda mungkin perlu mengonfirmasi registrasinya secara manual di Cognito.

## Arsitektur Jaringan

Dalam proyek ini, Anda diwajibkan membuat multi-region network dengan dua zona: lks-zone-a (172.32.0.0/23), lks-zone-b (10.10.0.0/23) Setiap zone direpresentasikan oleh sebuah VPC. Detail:

- Zone A berada di us-east-1, Zone B berada di us-west-2.
- Pastikan VPC A dapat terhubung dengan VPC B dan sebaliknya.
- Setiap Zone harus memiliki 3 subnet: 1 Public Subnet, 2 Private Subnet
- Alokasi subnet: Untuk Public Subnet gunakan network pertama dalam range (hingga 200 host).
- Gunakan network kedua dan ketiga dalam range sebagai Private Subnet.
- Public Subnet harus ditempatkan di Availability Zone 1a.
- Private Subnet ditempatkan di Availability Zone 1a dan 1b.
- Gunakan hanya 2 Route Table per VPC: lks-rtb-private untuk private subnet. lks-rtb-public untuk public subnet.
- Pastikan setiap Private Subnet dapat mengakses internet. Untuk menghemat biaya, gunakan NAT Instance (bukan NAT Gateway). Panduan pembuatan NAT Instance ada di bagian NAT Instance Details.
