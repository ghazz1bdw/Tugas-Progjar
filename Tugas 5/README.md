# Tugas 5 Pemrograman Jaringan
- Nama: Kurnia Cahya Febryanto
- NRP: 5025201073
- Kelas: Pemrograman Jaringan A

## Daftar Isi
- [Environment](#environment)
- [Soal](#soal)
    - [1. Implementasikan arsitektur load balancer dengan 2 mode](#1-implementasikan-arsitektur-load-balancer-dengan-2-mode)
        - [1.1. Mode Asynchronous](#11-mode-asynchronous)
        - [1.2. Mode Server Pool](#12-mode-server-pool)
    - [2. Perbandingan Kinerja Web Server](#2-perbandingan-kinerja-web-server)


## Environment
Mesin Progjar dijalankan dengan melalui WSL pada url sebagai berikut: </br>
•	Mesin 1: http://localhost:8889/lab/ </br>
•	Mesin 2: http://localhost:8890/lab/ </br>
•	Mesin 3: http://localhost:8891/lab/ </br>
•	Mesin 4: http://localhost:8892/lab/ </br>
<img src="https://user-images.githubusercontent.com/70510279/235426744-525e77ac-0d0d-411d-90a0-fa9914c8167f.png" width="500"/>
</br>
Salah satu tampilan mesin yang dijalankan: </br>
<img src="https://user-images.githubusercontent.com/70510279/235427940-7f986841-98f7-4e27-abee-48351893d753.png" width="500"/>

## Soal
Langkah pertama adalah dengan melakukan clone pada https://github.com/rm77/progjar/blob/master/progjar6. Dengan memanfaatkan program pada progjar6, maka lakukan hal berikut untuk menyelesaikan tugas 5:
### 1. Implementasikan arsitektur load balancer dengan 2 mode
#### 1.1. Mode Asynchronous
- Langkah pertama sebelum menjalankan adalah melakukan modifikasi pada file `lb_async.py` yaitu mengubah port sesuai dengan enviroment. Adapun yang digunakan adalah port 8001, 8002, 8003, dan 8004. </br>
<img src="https://user-images.githubusercontent.com/70510279/235428142-eac074e8-75ab-4ed2-9a6a-1b4bf76d08b4.png" width="300"/>
- Modifikasi beberapa fungsi supaya tidak terjadi error saat membaca socket dengan port yang banyak. Pada program awal, jika dijalankan maka terdapat nilai socket error yang besar. Adapun bagian yang dimodifikasi yaitu fungsi handle_read(), handle_close(), dan handle_accept(). </br>
<img src="https://user-images.githubusercontent.com/70510279/235428267-bfa76742-3e14-40ad-9d47-3e7b70802bbe.png" width="300"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235428357-490039cc-6591-4f63-bc09-44f405d2ebdc.png" width="300"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235428378-eb45ed69-d78f-4526-a1cb-0656655c5c72.png" width="300"/> </br>
- Ubahlah file `runserver.sh` sesuai dengan port yang digunakan yaitu </br>
<img src="https://user-images.githubusercontent.com/70510279/235428932-9251a7ed-6841-4c24-aa8c-28281a8925fa.png" width="300"/> </br>
- Jalankan `lb_async.py` menggunakan `python3`. </br>
<img src="https://user-images.githubusercontent.com/70510279/235429215-97cd9535-7306-4621-b3fc-0c99702196f0.png" width="1000"/> </br>

#### 1.2. Mode Server Pool
- Langkah pertama sebelum menjalankan adalah melakukan modifikasi pada file `lb_process.py` yaitu mengubah port sesuai dengan enviroment. Adapun yang digunakan adalah port 8889, 8890, 8891, dan 8892. </br>
<img src="https://user-images.githubusercontent.com/70510279/235430574-f8b7c679-6952-43db-9414-79fa046ef668.png" width="300"/> </br>
- Jalankan `lb_process.py` menggunakan `python3`. </br>

### 2. Perbandingan Kinerja Web Server 
- Install terlebih dahulu `wrk` pada environment Linux. </br>
<i>`sudo apt-get install wrk`</i>
- Setelah di-instal, jalankan masing-masing program menggunakan python3. Kemudian testing menggunakan wrk dengan command berikut: </br>
<i>` wrk -c 1000 -t {n} http://url`</i> </br>
#### 2.1. Testing Mode Asynchronous tanpa Load Balancer
- Jalankan web server tanpa load balancer yaitu `async_server.py`. </br>
`python3 async_server.py` </br>
<img src="https://user-images.githubusercontent.com/70510279/235431133-5423296e-6af9-4e22-b165-79a95eb5cc4b.png" width="1000"/> </br>
- Di terminal yang lain, jalankan testing wrk yaitu </br>
`wrk -c 1000 -t {n} http://localhost:8887/`
- Jalankan dengan jumlah concurrency yang berbeda yaitu 10, 50, 100, 150, dan 200. </br>
<img src="https://user-images.githubusercontent.com/70510279/235431227-6f2e9873-04bb-4203-b300-dc7d4c31f181.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235432085-3a1299d0-d878-4073-8236-09515ffc89cb.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235432096-35d864fb-cffa-4d42-8a4d-0fb4962f84b7.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235432104-c5755871-6c86-40ba-80ba-90ac1a36575e.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235432120-55339f0f-ad94-4cb1-b57e-a255ae284b6e.png" width="500"/> </br>

#### 2.2. Testing Mode Asynchronous dengan Load Balancer
- Jalankan server dengan melakukan running pada terminal 1 pada `runserver.sh` yaitu `./runserver.sh`. </br>
<img src="https://user-images.githubusercontent.com/70510279/235432366-89bc53c8-8ce1-43bf-9f09-95794b6c5abd.png" width="1000"/> </br>
- Jalankan web server dengan load balancer yaitu `lb_async.py` pada terminal 2. </br>
`python3 lb_async.py` </br>
<img src="https://user-images.githubusercontent.com/70510279/235432455-4df0df67-0077-4276-a427-f1e730696dbb.png" width="1000"/> </br>
- Di terminal yang lain, jalankan testing wrk yaitu </br>
` wrk -c 1000 -t {n} http://localhost:55555/ `
- Jalankan dengan jumlah concurrency yang berbeda yaitu 10, 50, 100, 150, dan 200
<img src="https://user-images.githubusercontent.com/70510279/235432529-d9d95b61-4f21-49bd-a43c-609aa64eb1fd.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235432535-db427fd3-1cd1-42fd-bfde-87c609829875.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235432539-8e00bb9c-3ce9-439f-aa24-79371dcdad0f.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235432545-c6ffa8d9-b733-48d8-824a-81e25c1a21d8.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235432551-a43d3c97-2dbd-4096-a57c-87f91e576408.png" width="500"/> </br>

#### 2.3. Testing Mode Server Pool tanpa Load Balancer
- Jalankan Jalankan web server tanpa load balancer yaitu `server_process_pool_http.py`. </br>
`python3 server_process_pool_http.py` </br>
<img src="https://user-images.githubusercontent.com/70510279/235437560-7d6693b8-374f-44ea-90c3-e19040dd5e46.png" width="1000"/> </br>
- Di terminal yang lain, jalankan testing wrk yaitu </br>
` wrk -c 1000 -t {n} http://localhost:8000/ `
- Jalankan dengan jumlah concurrency yang berbeda yaitu 10, 50, 100, 150, dan 200 </br>
<img src="https://user-images.githubusercontent.com/70510279/235437664-0ee15e98-dcb5-41a3-a10e-3e56e81a8df7.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235437676-8997784e-47b4-47d5-9cf4-fefc8108d554.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235437683-1bf049fb-3044-492e-b430-a0c2ad6ff945.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235437690-34be6460-d70a-4f62-b69b-3ccaca70bcf3.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235437697-53443a05-047a-4233-998c-366b1502372c.png" width="500"/> </br>

#### 2.4. Testing Mode Server Pool dengan Load Balancer
- Buat file baru berupa script untuk menjalankan pool server pada port berbeda yaitu file `runserverprocess.sh` </br>
<img src="https://user-images.githubusercontent.com/70510279/235438511-98f55b43-2482-46c5-b670-b16776cdcb2b.png" width="300"/> </br>
- Jalankan server dengan melakukan running pada terminal 1 pada `runserverprocess.sh` yaitu `./runserverprocess.sh` </br>
<img src="https://user-images.githubusercontent.com/70510279/235438612-8f29f43d-56c0-413f-b65a-181cf01ed7dd.png" width="1000"/> </br>
- Jalankan web server dengan load balancer yaitu `lb_process.py` pada terminal 2. </br>
`python3 lb_process.py` </br>
<img src="https://user-images.githubusercontent.com/70510279/235438684-86719336-9031-4488-a81d-dd946631564b.png" width="1000"/> </br>
- Di terminal yang lain, jalankan testing wrk yaitu
` wrk -c 1000 -t {n} http://localhost:44444/ ` </br>
- Jalankan dengan jumlah concurrency yang berbeda yaitu 10, 50, 100, 150, dan 200 </br>
<img src="https://user-images.githubusercontent.com/70510279/235442621-d22ad8c2-91a3-425e-8ffd-95441568e11c.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235442624-9acb08b9-8b95-42e1-a24e-0fd10cba359b.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235442629-a44bee60-4260-4c40-9ce0-2c8edaa4ea36.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235442634-0bfb3110-ab89-462d-85b3-988ac4e7a5df.png" width="500"/> </br>
<img src="https://user-images.githubusercontent.com/70510279/235442637-5d33fcbe-3674-4116-89a0-9b85ee7b162c.png" width="500"/> </br>
