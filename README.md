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
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
*1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?*

Dalam Observer Pattern, Subscriber biasanya didefinisikan sebagai sebuah antar muka agar tidak bergantung pada implementasi dari Subscriber. Ini menimbulkan kemungkinan fleksibilitas dalam menambahkan berbagai jenis Subscriber di masa depan tanpa harus mengubah kode.

Dalam kasus BambangShop, hanya ada satu jenis Subscriber yang mana menerima notifikasi contohnya dengan URL tertentu, maka ketika menggunakan satu struct model saja sudah lebih dari cukup. Namun jika ada kebutuhan yang diperlukan untuk mendukung berbagai jenis Subscriber dengan metode notifikasi yang berbeda - beda seperti melalui email atau sistem pesan lain, maka perlu menggunakan trait yang mana akan lebih baik. trait akan memberikan fleksibilitas untuk menambahkan berbagai implementasi baru tanpa mengubah kode utama.

*2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?*

Dalam BambangShop, id pada program dan juga url pada subscriber harus dibuat unik. Pertanyaan disini adalah mana yang lebih baik antara penggunaan Vec atau DashMap jika ingin menyimpan id dan url yang unik. Menurut saya DashMap lebih baik. Karena DashMap adalah sebuah implementasi thread-safe dari HashMap yang mungkin untuk melakukan pencarian cepat berbasis `key`. Dengan penggunaan DashMap, program dapat dengan mudah memastikan bahwa id pada program dan url pada subscriber tetap unik tanpa melakukan iterasi keseluruhan koleksi secara manual.

Sedangkan Vec adalah struktur data berbasis daftar yang bekerja baik untuk data dalam jumlah kecil tetapi kurang efisien dalam pencarian elemen unik karena harus melakukan iterasi secara linear untuk memastikan tidak ada duplikasi yang mana tidak efektif.

*3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?*

Menurut saya DashMap disini masih diperlukan karena memberikan keuntungan dalam akses thread-safe yang mana lebih baik jika hanya dibandingkan dengan menggunakan Singleton Pattern. Rust juga mempunyai aturan thread-safety yang ketat, jadi menggunakan DashMap lebih sesuai untuk kebutuhan BambangShop dibanding Singleton Pattern.

Meskipun DashMap lebih baik, tidak menutup kemungkinan bahwa Singleton Pattern juga dapat digunakan dalam BambangShop ini. Karena Singleton Pattern bisa digunakan untuk memastikan hanya ada satu instance dari daftar subscriber dalam aplikasi.

#### Reflection Publisher-2
*1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?*

Dalam Model-View-Controller (MVC) klasik, Model bertanggung jawab atas data dan logika bisnis. Namun terkadang dengan bertambahnya kompleksitas aplikasi dikarenakan improvisasi, akan lebih baik jika service dan juga repository dipisah agar menyajikan kode yang lebih terorganisir.

Pemisahan ini juga bukan tanpa alasan. Pemisahan ini juga mempunyai manfaat tersendiri yang akan berefek ke kode kedepannya. Beberapa manfaatnya yaitu lebih mudah diuji ketika ingin mengetes logika secara terpisah dari akses database. Kemudian lebih fleksibel jika ada perubahan pada database jadi cukup mengubah repository tanpa mempengaruhi service. Lalu juga kode akan lebih bersih dan modular.

*2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?*

Jika hanya menggunakan model, maka interaksi antar Model akan menjadi sulit dirawat dan kompleks. Model akan melakukan semua aspek mulai dari database, logika dan juga komunikasi antar entitas sekaligus. Ini akan berakibat Model terlalu besar dan sulit diuji maupun di-maintain. Jadi peran seperti Service, Repository dan juga Controller penting adanya dan memiliki masing - masing tugas yang jelas sehingga kode mudah dikelola.

*3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.*

Menurut saya postman cukup berguna ketika ingin menguji suatu API tanpa harus membuat sebuah testcase atau frontend output.

Ada beberapa manfaat postman yang bisa digunakan pada tutorial kali ini:
    - Mengirim request HTTP untuk mengetes API bekerja atau tidak.
    - Menyimpan request sehingga bisa mengulangi pengujian tanpa menulis ulang data.
    - Menjalankan tes otomatis dengan Postman Tests untuk mengecek respons dari API.
    - Menggunakan Environment Variables (env) untuk menyimpan base URL ataupun token autentikasi sehingga tidak perlu diketik ulang.
    - Simulasi berbagai skenario untuk menguji beberapa testcase atau kasus.

Untuk proyek ke depan, postman juga memiliki fitur lain yang dapat digunakan seperti Mock Servers yang bisa digunakan untuk mensimulasikan respons API. Lalu juga ada Newman CLI yang bisa digunakan untuk menjalankan postman secara otomatis dalam CI/CD pipeline untuk mengetes API berfungsi atau tidak setelah perubahan kode.

#### Reflection Publisher-3
*1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?*

Pada tutorial ini, variasi observer pattern adalah *Push Model*. Pendekatan ini, Publisher yang mana NotificationService akan langsung mengirimkan data ke semua subscriber saat terjadi perubahan tanpa harus diminta oleh subscriber. Untuk data yang dikirim berupa informasi seperti jenis produk, status dan URL.

*2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)*

Keuntungan ketika menggunakan *Pull Mode*:
    - Subscriber memiliki kontrol lebih besar karena bisa memilih kapan saja dan seberapa sering mengambil data sehingga bisa menghindari notifikasi yang tidak diperlukan.
    - Mengurangi transfer data yang tidak diperlukan. Jika tidak membutuhkan update maka Pull Mode akan menundanya dan juga akan menghemat penggunaan bandwidth.

Kerugian ketika menggunakan *Pull Mode*:
    - Akan lebih kompleks karena subscriber harus mengimplementasikan mekanism permintaan secara berkala untuk mengecek update yang mana akan menambah beban pengembangan dan menambah kompleksitas kode.
    - Update bisa tertunda karena subscriber hanya mendapatkan data ketika hanya diminta saja. Itu bisa mengakibatkan keterlambatan dalam menerima notifikasi dibandingkan jika kita menggunakan Push Model yang bersifat real-time.
    - Publisher juga akan menanggung beban yang lebih besar jika subscriber sering meminta data dan bisa mengakibatkan kewalahan saat menangani permintaan terus menerus.

*3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.*

Jika tidak menggunakan multi-threading pada proses motifikasi akan muncul beberapa masalah seperti:
    - Proses akan menjadi lambat (blocking main thread)
    - Kinerja menurun (performance bottleneck)
    - Kurang skalabilitas (poor scalability)
    - Respons sistem menurun