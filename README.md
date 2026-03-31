# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Saya melihat bahwa pemakaian rust ada jalur alternatifnya tetapi untuk pattern observable tidak banyak yang berubah jadi untuk kasus
BambangShop kita tetap membutuhkan interface.
2. Saya cukup percaya penggunaan DashMap sangat dibutuhkan karena adanya key yang unique (yaitu id dan url). Secara umum bentuk id dan url
pasti bukan seperti angka yang incremental dan oleh karena itu penggunaan DashMap cocok karena value bisa diambil dengan custom key yang dibuat
oleh id dan url.
3. Kita tetap membutuhkan DashMap karena Singleton sendiri tidak cukup untuk memecahkan masalah concurrency. Ditambah tutorial ini menggunkaan rust dan untuk Singleton itu sangat tidak cocok karena tidak mungkin membuat multiple code mengubah data yang sama di waktu yang sama.

#### Reflection Publisher-2
1. Meski menurut compund pattern Model terdapat bagian repository dan service, pemisahan repository dan service dari Model ini karena Model seharusnya akan menjadi struktur untuk data. Lalu untuk service dan repository itu melakukan logika. Oleh karena itu menurut saya kode akan semakin berkembang akan semakin kompleks sehingga melakukan pemisahan jadi bisa melihat kode dengan perasaan ringan dan ini mengandung nilai Single Reponsibility Principle.

2. Kita bisa menganalogikan 3 tugas berbeda tetapi hanya dikerjakan oleh satu entinitas seperti misal sebuah pabrik bisa menghasilkan telur ayam, sayuran kentang, dan gir mobil. Pabrik tersebut pasti akan berantakan dan tidak berjalan secara efisien.

Misal jika hanya Model berarti selain mengurus data struktur, maka model juga harus mengatur logika pula.Sebagai contoh: `Subscriber` harus mengurus logika `Notification`, maka perubahan pada sistem notifikasi memiliki resiko pada kode di dalam `Subscriber`.

3. Saat mengerjakan part 2 bagian Main, saya belum terlalu explore jauh menggunakan Postman tetapi karena dibilang bahwa punya potensi untuk berguna untuk Group Project maka saya akan explore segera.

#### Reflection Publisher-3

1. Variasi Observer pattern yang digunakan adalah Push Model. Pada fungsi `notify` melakukan konstruksi pada `Notification` struct yang berisi data-data diperlukan yang kemudian di Push obyek-nya ke `subscriber.update(payload)` method.

2. Ketika memakai Pull model akan terjadi beberapa hal seperti secara efisiensi lebih baik tetapi akan terjadi performance lag disebabkan butuh "two-step" proses sehingga lebih lambat dibanding Push model.

3. Karena ini berhubungan berat dengan concurrency maka salah satu yang terjadi adalah Blocking Bottleneck dimana subscriber diproses satu per satu dan misal salah satu yaitu subscriber ke-1 itu memiliki internet yang lemot maka subscriber ke-2 tidak bisa berjalan karena harus menunggu ke-1 pekerjaan kelar.