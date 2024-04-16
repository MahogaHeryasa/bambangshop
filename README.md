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
    -   [X] Commit: `Implement subscribe function in Notification service.`
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

1. Berdasarkan pemahaman saya terhadap *observer pattern*, *Subscriber* biasanya didefinisikan sebagai *interface* untuk mendukung adanya kemungkinan beberapa implementasi konkret dari *Subcriber* dengan tipe dan cara *update* dari *Publisher* yang berbeda-beda. Hal ini menjadikan *Publisher* tidak terikat dengan *Subscriber*. Dalam kasus BambangShop, jika semua *Subscriber* memiliki perilaku dan struktur yang sama, sebuah struktur model tunggal mungkin sudah cukup, namun, jika terdapat berbagai jenis pelanggan dengan perilaku yang berbeda-beda, akan bermanfaat untuk mendefinisikan sebuah interface yang mewakili perilaku umum yang dimiliki oleh semua pelanggan. 

2. Untuk menyimpan id dan url yang unik, `DashMap` lebih baik digunakan dibandingkan menggunakan `vec` karena dapat mencegah terjadinya duplikasi. Selain itu, operasi pencarian, penambahan, dan penghapusan lebih efisien pada `DashMap` yaitu sebesar O(1), sementara operasi-operasi tersbut pada `vec` memiliki tingkat efesiensi sebesar O(n). `DashMap` juga dirancang untuk keamanan konkurensi *multi-threading*, sehingga jika mengimplementasikan *multi-threading*, operasi-operasi pada DashMap secara alami thread-safe dan tidak memerlukan sinkronisasi tambahan.

3. *Singleton pattern* memastikan bahwa sebuah kelas memiliki hanya satu instansi dan menyediakan titik akses global ke instansi tersebut, sehingga jika diterapkan, varibel `SUBSCRIBERS` hanya ada satu untuk kesuluruhan aplikasi. Tanpa menggunakan `DashMap` yang merupakan *thread-safety* `HashMap`, jika mengimplementaskan *multi-threading*, akan muncul isu-isu concurency dalam penerapannya. Sehingga lebih baik menggunakan `DashMap` karena `SUBSCRIBERS` akan diakses banyak *thread*. 


#### Reflection Publisher-2

1. Pemisahan antara `Service` dan `Repository` dari sebuah `Model` akan memisahkan tanggung jawab antara logika bisnis (`Service`) dan akses data (`Repository`). Hal ini memenuhi kaidah *clean code* karena code menjadi lebih rapih dan mudah dipamahi, serta memenuhi *Single Responsibility Principle* karena adanya pemisahan tanggung jawab yang tunggal. Dengan penerapan kaidah tersebut, sistem menjadi lebih *maintainable* dan fleksibel.

2. Jika kita hanya menggunakan `Model`, maka setiap model `Program`, `Subscriber`, serta `Notification` akan memiliki banyak tanggung jawab dan menjadi lebih kompleks. Hal ini melanggar *Single Responsibility Principle* karena setiap model akan mengatur logika bisnis, akses data, dan interaksi dengan model lain secara mandiri sehingga model menjadi sangat terikat satu sama lain. Jika kita hanya menggunakan `Model`, maka secara keseluruhan aplikasi kita akan memiliki *maintainability* yang buruk.  

3. Saya sudah memiliki sedikit pengalaman menggunakan aplikasi *Postman* pada mata kuliah PBP semester lalu dan sudah melakukan sedikit eksplorasi fitur-fitur yang ada. Berdasarkan pengalaman saya, aplikasi *Postman* berguna untuk melakukan pengujian pada API sehingga fungsionalitas aplikasi kita dapat dipastikan berjalan dengan lancar. Pengujian API dilakukan dengan mengirimkan *HTTP request* pada API yang dituju, dan menerima *response* yang sesuai dengan harapan kita. Menurut saya fitur yang akan membantu pada proyek kelompok maupun proyek-proyek kedepannya adalah fitur pengujian API yang mudah dan fitur *collection* yang memudahkan kita untuk mengelompokkan, menyimpan, dan berbagi *HTTP request* dalam satu tempat yang terorganisir

#### Reflection Publisher-3

1. Tutorial ini menggunakan variasi *Obeserver pattern* berupa *push model*. Hal ini karena *Publisher* akan mengirimkan notifikasi kepada `Subscribers` melalui `NotificationService` yang memanggil *method* `notify` setiap kali suatu produk dibuat, dihapus, atau dipublish.

2. Kelebihan menggunakan *pull model* adalah *Subscriber* dapat menentukan kapan menerima data yang dikirim *Publisher*. Hal ini memberikan fleksibilitas dan kontrol penuh akan data bagi *Subscriber*. Di sisi lain, Kekurangan menggunakan *pull model* adalah perlunya *logic* tambahan di sisi *receiver* untuk mengatur masuknya notifikasi yang dilakukan oleh *Subscriber*. Selain itu, *Subscriber* yang tidak megambil data secara aktif, akan mengalami penundaan pembaharuan data.

3. Jika kita tidak menggunakan *multi-threading* pada proses notifikasi, ketika terdapat banyak notifikasi yang perlu dikirimkan ke *Subscribers* secara bersamaan, maka aplikasi dapat berjalan dengan lambat karena pengiriman notifikasi dilakukan secara bergilir atau berurutan satu-persatu. Sehingga, ketika pengiriman notifikasi ke beberapa subscriber dilakukan secara bersamaan, *Subscriber* selanjutnya harus menunggu pengiriman notifikasi *Subscriber* sebelumnya. 
