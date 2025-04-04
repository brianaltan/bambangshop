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
1. > In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

In my opinion, using a trait could allow the subject to operate independently of its observers. Typically, traits are used when we want to add more types of notifications or make changes, as they can be easily implemented. In the case of BambangShop, it is true that there is only a single subscriber, so using a simple struct may seem sufficient. However, I would still advocate for using a trait because it opens up a more dynamic approach in the future, in case I want to add additional subscriber types or behaviors without needing to rewrite the existing code.
 
2. > id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

A Vec has an O(n) performance complexity when finding a unique id within the dataset. However, a DashMap, due to its dictionary-like key-value setup, allows us to detect unique keys with O(1) complexity. In our case, DashMap is more appropriate because it provides thread-safe operations and enables much more efficient lookups.

3. > When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

I think DashMap provides thread safety and is optimized for multithreaded environments, effectively protecting against data race conditions. Given this, DashMap offers much better performance and safety compared to a self-implemented solution. While the Singleton pattern can be used as an alternative for learning purposes in our case, in terms of performance and stability, DashMap is still superior.

#### Reflection Publisher-2
1. > In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

The reasons why Service and Repository are separated are rooted in the implementation of design principles such as Separation of Concerns and the Single Responsibility Principle. This separation enhances scalability, improves readability for other developers, and provides clearer boundaries between different types of logic. By doing so, the codebase becomes easier to understand facilitating better collaboration among developers.

2. > What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

If we only use Models without separating the concerns into Service and Repository layers, it will definitely increase code complexity and raise concerns such as difficulty in reusability. The worst issue is that it becomes extremely difficult for a developer or other developers to test and debug. This happens because the Models know too much about other parts of the codebase, making the application fragile and difficult to maintain for other developers.

3. > Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Yes, Postman has been incredibly helpful for testing and debugging APIs, which are essential for validating my current work. It’s especially useful in group projects, where collaboration is key. With Postman, I can not only test my own APIs but also work with APIs developed by other team members by using features like collections and environment variables. These features greatly enhance productivity and streamline teamwork among the group.

#### Reflection Publisher-3
1. > Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

We used the Push model of the Observer Pattern. All relevant data is sent through a notification object using the update method. This ensures that subscribers receive the necessary information without needing to make additional requests.

2. > What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

If we used the Pull model instead of the Push model, one advantage is that subscribers would have more control and flexibility in configuring the specific type of data they want to retrieve. However, a disadvantage is that this approach would require subscribers to actively make requests to the publisher each time they need data, which could lead to increased overhead compared to Push model.

3. > Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Chaos is what’s going to happen! Without multi-threading, notifications will be processed one at a time, meaning the program will have to wait until one notification is completely handled before moving on to the next. This could lead to slow performance delays and potential bottlenecks, especially if there are many notifications to process simultaneously.