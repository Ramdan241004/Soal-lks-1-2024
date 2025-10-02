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

## NAT Instance

Anda dapat menggunakan instance Ubuntu OS dengan tipe **t2.micro** sebagai NAT Instance, dengan konfigurasi berikut:
1. Aktifkan IP forwarding dengan mengedit file **sysctl.conf** dan tambahkan atau uncomment baris berikut:
```sh
net.ipv4.ip_forward=1
```
2. Terapkan perubahan:
```sh
sudo sysctl -p
```
3. Install **iptables-persistent** agar aturan NAT tetap ada setelah reboot:
```sh
sudo apt install iptables-persistent -y
```
4. Atur aturan NAT menggunakan **iptables**:
```sh
iptables -t nat -A POSTROUTING -o [interface] -j MASQUERADE
iptables -F FORWARD
```

> **Catatan**: Semua EC2 instance dan network interface memiliki source dan destination check aktif secara default. Itu artinya, instance hanya bisa mengirim/menerima traffic untuk IP address miliknya sendiri dan tidak mendukung transitive routing.


## Storage

Anda akan menggunakan Elastic File System (EFS) sebagai shared storage untuk menyimpan LLM models. Persyaratan: Buat dan deploy EFS dalam VPC di region **us-east-1**. Atur performance ke mode bursting. Aktifkan automatic backup. Mount EFS pada setiap LLM Worker, dengan mount point: **/share** Dilarang membuat EFS di region lain selain yang ditentukan.

## Security

Keamanan adalah bagian krusial dalam proyek ini. Jangan pernah mengekspos service yang memerlukan Security Group ke anywhere. Detail keamanan:

- Database hanya boleh diakses dari Lambda functions (lks-sg-db).
- LLM Workers hanya boleh diakses melalui port 11434 dari Load Balancer (lks-sg-llm).
- Load Balancer hanya boleh diakses melalui port 80 (lks-sg-lb).
- NAT Instance hanya boleh digunakan untuk traffic forwarding → jangan izinkan all traffic dari internet (lks-sg-nat).
- EFS hanya boleh diakses dari private subnets atau LLM Security Group di setiap VPC zone (lks-sg-efs).


Memberikan akses berlebihan akan menciptakan celah keamanan pada arsitektur Anda. Batas jumlah Security Group: Maksimal 5 Security Group di us-east-1 Maksimal 2 Security Group di us-west-2

> **Catatan**: jangan lupa untuk menghapus security group yang tidak terpakai di setiap region.
Baik, saya terjemahkan bagian ini ke Bahasa Indonesia:

## LLM (Large Language Model)

Akan ada tiga jenis worker:

- **LLM Workers** : bertanggung jawab untuk embedding, menarik model, mengambil daftar model, dan tugas LLM lainnya.
- **Scoring Workers** : khusus untuk menghasilkan feedback penilaian (scoring) pada percakapan di aplikasi.
- **Chat Workers** : khusus menangani percakapan chat streaming.

Semua worker ini harus di-deploy di setiap VPC Zone pada private subnet, sehingga komputasi untuk LLM, scoring, dan chat terpisah di masing-masing zone. Gunakan instance type t2.large untuk setiap worker.

Konfigurasi Worker

- Setiap worker menggunakan model yang sama, disimpan di **EFS**.
- Update variabel EFS di playbook Ansible dengan alamat EFS Anda.
- Jalankan playbook konfigurasi dengan State Manager (AWS Systems Manager - SSM).
- Pastikan target worker dipilih berdasarkan tags di State Manager.
- Mungkin perlu install Ansible agent di setiap worker.

Verifikasi Worker Setelah konfigurasi berhasil, lakukan uji coba dengan akses endpoint worker pada port 11434. Cek koneksi:
```curl
curl -i http://WORKER_IP:11434/api/tags
```
Jika respon HTTP 200, berarti worker sudah berjalan dengan benar. Pull model (contoh orca-mini):
```curl
curl http://WORKER_IP:11434/api/pull -d '{"name": "orca-mini"}'
```
Cek model yang sudah diunduh:
```curl
http://WORKER_IP:11434/api/tags
```

Lakukan ini untuk semua worker. Jika semua worker memiliki model yang sama, artinya konfigurasi dan mounting EFS berhasil. Load Balancer Buat private application load balancer yang mendengarkan di port 80, dengan routing rule:

- Semua endpoint /api digunakan untuk LLM Worker (kecuali /api/generate dan /api/chat)
- Endpoint /api/generate/ diarahkan ke Scoring Worker
- Endpoint /api/chat diarahkan ke Chat Worker
- Root path (/) harus mengembalikan pesan: "its worked!" dengan HTTP status 200 (untuk health check).

Pastikan setiap worker memiliki backup instance di Availability Zone yang berbeda.

> **Catatan**:
> - Detail API endpoint LLM ada di GitHub Repository.
> - Pastikan model **orca-mini**, **llama3**, dan **nomic-embed-text** sudah diunduh (lihat panduan di repository).

## Relational Database

Dalam proyek ini, aplikasi client akan menyimpan seluruh riwayat percakapan di relational database, sekaligus menyimpannya dalam bentuk vector. Gunakan PostgreSQL dengan pgvector plugin untuk menyimpan data percakapan beserta versi vektornya. Lambda function mungkin akan menangani proses enable pgvector extension. Vector percakapan akan digunakan untuk evaluasi feedback real-time selama percakapan berlangsung. Vector ini akan diperbarui seiring bertambahnya percakapan. Pastikan database dikonfigurasi dengan keamanan maksimal.

## Lambda Function

Dalam proyek ini, sebuah Lambda function diperlukan untuk menangani permintaan dari API Gateway terkait endpoint /conversations. Anda dapat menggunakan satu Lambda function atau memisahkannya berdasarkan method. Meskipun Anda mungkin perlu melakukan refactor kode yang ada, keputusan Anda akan memengaruhi efisiensi biaya, performa, dan nilai akhir. Seluruh repository untuk Lambda function yang diperlukan dapat ditemukan di source code repository (lihat bagian Technical Description) dengan path: **/serverless/src/function** Lambda function akan memproses permintaan API untuk endpoint **/conversations**, melakukan operasi CRUD (Create, Read, Update, Delete) pada data percakapan yang disimpan di database. Lambda function perlu berinteraksi dengan database untuk menyimpan dan mengambil data percakapan. Untuk memfasilitasi hal ini, function akan menggunakan environment variables untuk konfigurasi database:

- **DB_USER**: Username yang digunakan untuk autentikasi dan akses ke database. Ini harus berupa user yang valid dengan izin yang diperlukan untuk berinteraksi dengan database.
- **DB_PASSWORD**: Password yang terkait dengan DB_USER. Digunakan bersama username untuk melakukan autentikasi aman ke database.
- **DB_HOST**: Hostname atau alamat IP dari server database. Ini menentukan lokasi database yang harus dapat dijangkau oleh Lambda function.
- **DB_PORT**: Nomor port tempat server database berjalan/listening. Ini memungkinkan Lambda function untuk terhubung ke port yang benar.
- **DB_NAME**: Nama database yang akan dihubungkan oleh Lambda function. Ini menentukan database mana di dalam server yang akan digunakan oleh function.

## API Gateway

API Gateway adalah komponen penting dalam proyek ini. Anda harus menggunakan public API Gateway yang dapat diakses oleh client apps dari region mana pun. Dalam proyek ini, disarankan untuk membuat REST API Gateway dan hanya mengizinkan pengguna yang terdaftar di Cognito untuk mengakses seluruh endpoint pada API Gateway. Aplikasi client akan menggunakan Authorization header sebagai kredensial. Persyaratan Endpoint API untuk Client LLM Application

| Endpoint                  |	Method |	Path Parameters |	Body Payload (Format JSON) |
|---------------------------|--------|------------------|----------------------------|
| /conversations/(uid)      | GET	   | uid	            | None                       |
| /conversations/(uid)/(id) |	GET    | uid, id	        | None                       |
| /conversations/(uid)	    | POST   | uid	            | id (required) <br> title (required) <br> conversation (required) <br>embedding (not required) |
| /conversations/(uid)/(id)	| PUT    | uid, id	        | title (not required) <br> conversation (not required) <br> embedding (not required)                |
| /conversations/(uid)	    | DELETE | uid	            | None                       |
| /conversations/(uid)/(id) |	DELETE | uid, id          |	None                       |



---

Endpoint untuk LLM Untuk LLM, Anda diwajibkan membuat dua endpoint: **/us-east-1** dan **/us-west-2** masing-masing mengarah ke LLM Load Balancer di region yang bersangkutan. Perhatikan bahwa aplikasi client hanya akan mengakses endpoint LLM tanpaix **/api** (lihat detail di bagian LLM Section mengenai akses endpoint). Contoh: untuk mengakses endpoint LLM di region **us-east-1**, endpoint akan menjadi: **https://api_gateway_endpoint_url/us-east-1/tags** bukan **/api/tags** Metode yang diizinkan adalah POST dan GET. Anda mungkin memerlukan VPC Link dengan NLB (Network Load Balancer) untuk menghubungkan API Gateway dan LLM Load Balancer.

| Endpoint	   | Method |	Target                    |
|--------------|--------|---------------------------|
| /us-east-1/* | GET	  | LLM LB Region N. Virginia |
| /us-east-1/* | POST	  | LLM LB Region N. Virginia |
| /us-west-1/* | GET	  | LLM LB Region Oregon      |
| /us-west-1/* | POST	  | LLM LB Region Oregon      |

# Aplikasi Chat Bahasa Inggris
Ini adalah aplikasi asisten AI yang dirancang khusus untuk pembelajaran bahasa Inggris. Tujuannya adalah untuk memberikan pengalaman belajar yang komprehensif dan interaktif bagi pengguna yang ingin meningkatkan kemampuan bahasa Inggris mereka. Aplikasi ini menggunakan teknologi AI canggih untuk memfasilitasi pembelajaran bahasa yang menarik dan efektif melalui berbagai fitur interaktif.

## TechStack

Sistem Aplikasi Chat Bahasa Inggris menggunakan sejumlah teknologi agar dapat berjalan dengan baik:
- [Node.js](https://nodejs.org/) - Runtime untuk fungsi lambda
- [NextJS](https://nextjs.org/) - Framework JavaScript progresif dan dapat diadopsi secara bertahap
- [API Gateway](https://aws.amazon.com/api-gateway/) - Layanan terkelola penuh untuk membuat, mempublikasikan, memelihara, memantau, dan mengamankan API dalam skala apa pun.
- [Amazon Cognito](https://aws.amazon.com/pm/cognito/) - Layanan autentikasi pengguna, otorisasi, dan manajemen pengguna untuk aplikasi web dan mobile
- [PostgresQL](https://www.postgresql.org) - Database relasional open-source yang kuat.

<hr>

## Pengaturan Front-End
Folder root untuk proyek Front-End adalah **client-apps**.
### Variabel Lingkungan Front-End
Variabel lingkungan ini harus dijalankan selama fase build, dan pastikan semua environment sudah diatur sebelum membangun atau mengompilasi aplikasi. Aplikasi hanya boleh membaca environment dari file `.env`, tetapi jangan pernah mengunggah file `.env` ke Git repository atau Anda akan kehilangan poin.
```sh
AUTH_SECRET="GENERATE_RANDOM_STRING_SECRET"
AUTH_URL="YOUR_AMPLIFY_URL" # http://AMPLIFY_URL
NEXT_PUBLIC_COGNITO_CLIENT_ID="YOUR_COGNITO_APPS_CLIENT_ID"
NEXT_PUBLUC_COGNITO_ID_TOKEN_EXPIRED="YOUR_COGNITO_ID_TOKEN_EXPIRED_IN_MINUTES" # 10
NEXT_PUBLIC_API_GATEWAY_URL="YOUR_API_GATEWAY_URL"
```

#### Generate Random String
Untuk menghasilkan string acak untuk auth secret Anda bisa menggunakan perintah ini:
```sh
openssl rand -base64 16
```

### Pengaturan Proyek

```sh
npm install
```

### Kompilasi dan Hot-Reload untuk Development

```sh
npm run dev
```

### Type-Check, Kompilasi, dan Minify untuk Production

```sh
npm run build
```

Anda dapat menggunakan halaman check (`http://FRONTEND_HOST/check`) untuk memeriksa koneksi ke backend endpoint, Anda perlu login untuk mengakses endpoint ini.
<hr>

## Pengaturan Backend

### Endpoint API
Anda dapat membaca spesifikasi API Endpoint untuk `/conversations` di API Gateway pada detail dokumen.

### Source Code Lambda
Anda dapat menggunakan semua kebutuhan Lambda di `/serverless/src`.

### Pengaturan LLM Worker
Semua konfigurasi untuk LLM Workers harus dikelola menggunakan Ansible playbook untuk konfigurasi otomatis. Anda dapat menggunakan Ansible playbook yang terletak di `/serverless/playbook.yml` untuk mengotomatisasi konfigurasi semua LLM Workers. Pastikan menyesuaikan variabel yang diperlukan dalam file playbook `efs_ip: "YOUR_EFS_IP"`.

Di lingkungan AWS Anda, Anda dapat menjalankan playbook ini menggunakan AWS Systems Manager (SSM) untuk mengotomatisasi proses konfigurasi.

<hr>
## **Endpoint API LLM**
> ### Daftar Model
```sh
GET /api/tags
```
Menampilkan daftar model yang tersedia di server LLM. Contoh Request :
```sh
curl http://SERVER_HOST:11434/api/tags
```

> ### Pull Model
```sh
POST /api/pull
```
Mengunduh sebuah model dari library. Pull yang dibatalkan akan dilanjutkan dari posisi terakhir, dan beberapa pemanggilan akan berbagi progres unduhan yang sama.

#### Parameter
- `name` : nama model yang akan diunduh
- `insecure`: (opsional) izinkan koneksi tidak aman ke library. Gunakan hanya jika Anda mengunduh dari library sendiri selama pengembangan.
- `stream` : (opsional) jika `false` maka respon akan dikembalikan sebagai satu objek respon, bukan stream objek.

#### Contoh Request
```sh
curl http://SERVER_HOST:11434/api/pull -d '{
  "name": "orca-mini"
}'
```

> ### Generate Embedding
```sh
POST /api/embed
```
Menghasilkan embedding dari sebuah model

#### Parameter
- `model` : nama model yang digunakan untuk menghasilkan embedding
- `input` : teks atau daftar teks yang akan dibuat embedding

#### Contoh Request
```sh
curl http://SERVER_HOST:11434/api/embed -d '{
  "model": "nomic-embed-text",
  "input": "Why is the sky blue?"
}'
```

> ### Chat Stream
```sh
POST /api/chat
```
Menghasilkan pesan berikutnya dalam sebuah chat dengan model yang disediakan. Ini adalah endpoint streaming, jadi akan ada serangkaian respon. Streaming dapat dinonaktifkan dengan menggunakan "stream": false. Objek respon terakhir akan menyertakan statistik dan data tambahan dari permintaan.

#### Parameter
- `model` : nama model
- `messages` : pesan-pesan dalam chat, ini bisa digunakan untuk menyimpan memori percakapan
- `stream` : jika `false` maka respon akan dikembalikan sebagai satu objek respon, bukan stream objek Objek `message` memiliki field berikut:
- `role` : peran dari pesan, bisa berupa `system`, `user`, `assistant`
- `content` : isi pesan

#### Contoh Request
```sh
curl http://SERVER_HOST:11434/api/chat -d '{
  "model": "orca-mini",
  "messages": [
    {
      "role": "user",
      "content": "why is the sky blue?"
    }
  ]
}'
```

> ### Generate a Completion
```sh
POST /api/generate
```
Menghasilkan respon untuk sebuah prompt dengan model yang disediakan. Ini adalah endpoint streaming, jadi akan ada serangkaian respon. Objek respon terakhir akan menyertakan statistik dan data tambahan dari permintaan.

#### Parameter
- `model` : nama model
- `prompt` : prompt untuk menghasilkan respon
- `stream` : jika false maka respon akan dikembalikan sebagai satu objek respon, bukan stream objek

#### Contoh Request
POST Request
```sh
curl http://localhost:11434/api/generate -d '{
  "model": "orca-mini",
  "prompt": "Why is the sky blue?"
}'
```
