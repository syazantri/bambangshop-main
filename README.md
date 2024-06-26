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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
> Pada kasus BambangShop ini, menurut saya masih menguntungkan jika kita menggunakan interface atau trait untuk pengimplementasian kelas Subscriber dalam Observer pattern. Implementasi ini memudahkan kita untuk memaintain kode, lebih flexible, scalable, dan mudah juga dalam membuat testing.
2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
> Menurut saya walaupun possible menggunakan Vec, lebih baik kita pakai DashMap karena kalau pakai Vec, untuk mencari dan memastikan keunikan suatu id itu harus menggunakan iterasi yang tidak efisien ke seluruh list (kompleksitas O(N)). Berbeda dengan DashMap yang bisa langsung dapat karena terjamin id nya unik (kompleksitas(O(1))).
3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
> Menurut saya lebih baik pakai DashMap dalam memanage list of subscribers dalam thread-safe di Rust. Hal ini karena kalau pakai Singleton pattern, bakal memanage tiap instance dalam list sehingga tidak cocok dengan Rust yang punya aturan ownership dan borrowing sehingga perlu tambahan sistem sinkronisasi seperti Mutex atau RwLock. DashMap mampu menghandle concurrency dengan baik jadi kita tidak perlu repot-repot menambah sinkronisasi tambahan.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
> Menurut saya, kita harus memisahkan service dan repository dari model agar lebih mudah mengatur business logicnya karena keperluan/task masing-masing dari mereka berbeda (sekaligus mengimplementasikan SRP). Pemisahan ini akan membuat kode kita lebiih terstruktur, maintainable, scalable, dan testable.
2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?
> Kalau cuma pakai model, bakal membuat kodenya sangat kompleks karena tiap model harus meliputi sangat banyak task/responsible (business logic dan data access operationnya) jadi akan sulit di maintain ataupun extend kodenya, atau yang biasa disebut bloated.
3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
> Sudah, saya sudah mengeksplorasi lebih lanjut terkait Postman. Yang saya pahami sejauh ini, Postman bisa membantu kita dalam hit endpoint dan bisa tahu responsenya apa. Fitur yang saya sukai dari postman itu kita bisa mengubah format responsenya sesuka kita jadi lebih mudah ketika ingin melihat format tertentu. Lalu kita bisa set environment juga untuk bisa berkolaborasi dengan orang lain dalam hal api.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
> Di sini kita pakai push model, bisa terlihat di notify function itu publishernya iterasi ke seluruh list of subscribers yang didapat dari SubscriberRepository secara aktif memanggil method update. 
2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
> Mengimplementasikan Pull variation of Observer Pattern dapat menguntungkan dalam mengatasi overhead karena subscribernya cuma bakal request update ketika dibutuhkan. Tapi jadinya tiidak real-time pengiriman notificationnya, yang merupakan kekurangan dari Pull variation ini.
3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.
> Kalau ga pakai multi-threading, program bakal sangat tidak efisien karena pengiriman notifikasi harus menunggu yang lain selesai dulu. Delay ini nanti bakal pengaruh ke performancenya