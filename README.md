# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   ✅ Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   ✅ Commit: `Create Notification model struct.`
    -   ✅ Commit: `Create SubscriberRequest model struct.`
    -   ✅ Commit: `Create Notification database and Notification repository struct skeleton.`
    -   ✅ Commit: `Implement add function in Notification repository.`
    -   ✅ Commit: `Implement list_all_as_string function in Notification repository.`
    -   ✅ Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   ✅ Commit: `Create Notification service struct skeleton.`
    -   ✅ Commit: `Implement subscribe function in Notification service.`
    -   ✅ Commit: `Implement subscribe function in Notification controller.`
    -   ✅ Commit: `Implement unsubscribe function in Notification service.`
    -   ✅ Commit: `Implement unsubscribe function in Notification controller.`
    -   ✅ Commit: `Implement receive_notification function in Notification service.`
    -   ✅ Commit: `Implement receive function in Notification controller.`
    -   ✅ Commit: `Implement list_messages function in Notification service.`
    -   ✅ Commit: `Implement list function in Notification controller.`
    -   ✅ Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
1. RwLock<> digunakan karena memungkinkan banyak thread membaca Vec<Notifications> secara bersamaan, sementara hanya satu thread yang dapat menulis pada satu waktu. Mutex<> kurang optimal karena hanya mengizinkan satu thread mengakses data, baik untuk membaca maupun menulis, yang dapat menyebabkan bottleneck jika banyak thread hanya ingin membaca notifikasi. Oleh karena itu, RwLock<> lebih cocok dalam kasus ini karena frekuensi baca lebih tinggi daripada tulis.

2. Di Rust, static variables bersifat immutable secara default untuk menjaga thread safety. Berbeda dengan Java yang mengizinkan mutasi langsung, Rust memerlukan mekanisme seperti Mutex<> atau RwLock<> agar static variables dapat diubah dengan aman. lazy_static memungkinkan inisialisasi secara lazy di runtime, tetapi mutasi tetap harus dilakukan melalui tipe yang mendukung synchronization, seperti Mutex<> atau DashMap.

#### Reflection Subscriber-2
1. Ya, saya telah mengeksplorasi src/lib.rs, yang berisi konfigurasi penting seperti Error Response, root URL, dan Singleton untuk AppConfig. Salah satu yang saya pelajari adalah implementasi Default untuk AppConfig, di mana fungsi default() menyediakan nilai bawaan seperti instance_root_url, publisher_root_url, dan instance_name. Hal ini mempermudah pembuatan instance AppConfig tanpa harus menentukan nilai setiap kali, tetapi tetap bisa diubah jika diperlukan.

2. Observer Pattern mempermudah penambahan subscriber baru karena setiap subscriber cukup didaftarkan tanpa mengubah kode inti aplikasi (Open/Closed Principle). Jika kita menambah lebih dari satu instance Main App, sistem masih bisa berjalan dengan baik, selama subscriber didaftarkan melalui API ke instance yang sesuai. Namun, perlu dipastikan bahwa sinkronisasi antar instance Main App dikelola dengan baik untuk menghindari inkonsistensi data.

3. Ya, saya telah menggunakan Postman untuk testing dan collection. Postman sangat berguna untuk memverifikasi response API dan memastikan aplikasi bekerja sesuai ekspektasi. Dengan Postman, saya dapat menguji endpoint program sendiri maupun anggota tim, serta memastikan request dan response sesuai dengan data asli. Hal ini sangat membantu dalam pengembangan kolaboratif karena setiap anggota dapat mengecek dan memastikan API bekerja dengan benar.