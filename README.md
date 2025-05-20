# Architectural-Pattern-Monolithic
# 🏗️ Monolithic Architecture

## Pengantar
**Monolithic Architecture** adalah pendekatan tradisional dalam membangun aplikasi di mana semua komponen aplikasi — seperti User Interface, Business Logic, dan Data Access — digabungkan menjadi satu unit kesatuan dalam satu source code dan satu proses deployment.

---

## 🧱 Karakteristik Utama

- Semua komponen (frontend, backend, database access) menyatu dalam satu aplikasi.
- Komunikasi antar komponen terjadi langsung dalam satu proses.
- Satu source code dan satu proses deployment.
- Jika satu bagian mengalami error, bisa memengaruhi keseluruhan sistem.

---

## 🌟 Kelebihan

- ✅ Mudah dikembangkan untuk aplikasi kecil-menengah.
- ✅ Deployment sederhana (cukup satu file atau container).
- ✅ Komunikasi antar modul cepat dan efisien.
- ✅ Cocok untuk MVP dan pengembangan cepat.

---

## ⚠️ Kekurangan

- ❌ Sulit diskalakan seiring pertumbuhan aplikasi.
- ❌ Perubahan kecil memerlukan deploy ulang seluruh aplikasi.
- ❌ Sulit dikelola jika source code membengkak.
- ❌ Jika satu modul error, seluruh sistem bisa ikut gagal.

---

## 🏠 Analogi: Rumah Konvensional

Bayangkan sebuah rumah satu lantai besar yang berisi:
- Dapur
- Kamar mandi
- Kamar tidur
- Ruang tamu

Semua berada dalam satu struktur. Jika ada kerusakan di satu ruangan (misalnya pipa bocor di kamar mandi), maka bisa berdampak ke keseluruhan rumah.

> Sama halnya dengan Monolithic Architecture — satu kesalahan dalam modul bisa membuat seluruh aplikasi bermasalah.

---

## 💼 Real Case: Aplikasi E-Commerce Monolitik

### 🔹 Studi Kasus: Amazon (awal pengembangan)
- Seluruh fitur seperti katalog, cart, pembayaran, dan pengiriman dibangun sebagai satu aplikasi monolitik.
- Semua tim bekerja di satu codebase besar.
- Update fitur kecil tetap butuh redeploy seluruh sistem.

### 🔄 Solusi:
Amazon akhirnya memigrasikan sistem ke **microservices** untuk mengatasi tantangan skalabilitas dan pengembangan paralel.

---

## 🧪 Contoh Implementasi: Spring Boot Monolith
Aplikasi sederhana `monolithic-app` dengan dua service:
- `UserService` dan `ProductService`
- Menggunakan: Spring Boot, Spring Data JPA, MySQL

Struktur dasar:


## Prasyarat

- Java Development Kit (JDK) 21 atau yang lebih baru
- Eclipse IDE (atau IDE lain yang mendukung Spring Boot)
- MySQL Database
- Postman (untuk pengujian API)

## Struktur Project

```
monolithic-app/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── monolithic/
│   │   │               ├── MonolithicAppApplication.java
│   │   │               ├── model/
│   │   │               │   ├── User.java
│   │   │               │   └── Product.java
│   │   │               ├── repository/
│   │   │               │   ├── UserRepository.java
│   │   │               │   └── ProductRepository.java
│   │   │               ├── service/
│   │   │               │   ├── UserService.java
│   │   │               │   └── ProductService.java
│   │   │               └── controller/
│   │   │                   ├── UserController.java
│   │   │                   └── ProductController.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/
│           └── com/
│               └── example/
│                   └── monolithic/
│                       └── MonolithicAppApplicationTests.java
└── pom.xml
```

## Langkah 1: Membuat Project Spring Boot

1. Buka Eclipse IDE
2. Klik **File > New > Spring Starter Project**
3. Isi informasi project:
   - **Name**: monolithic-app
   - **Type**: Maven
   - **Packaging**: Jar
   - **Java Version**: 17
   - **Group**: com.example
   - **Artifact**: monolithic
   - **Package**: com.example.monolithic
4. Klik **Next**
5. Pilih dependencies:
   - Spring Web
   - Spring Data JPA
   - MySQL Driver
   - Spring Boot DevTools
6. Klik **Finish**

## Langkah 2: Persiapan Database

1. Buka MySQL Command Line atau MySQL Workbench
2. Jalankan perintah SQL:
   ```sql
   CREATE DATABASE monolithic_db;
   USE monolithic_db;
   ```

## Langkah 3: Konfigurasi Database

Buka file `src/main/resources/application.properties` dan tambahkan kode berikut:

```properties
# DataSource Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/monolithic_db
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.properties.hibernate.format_sql=true

# Server Configuration
server.port=8080
```

> **Catatan**: Ganti `username` dan `password` dengan kredensial MySQL Anda.

## Langkah 4: Membuat Package Structure

Klik kanan pada folder `src/main/java/com/example/monolithic` dan buat package berikut:
- `com.example.monolithic.model`
- `com.example.monolithic.repository`
- `com.example.monolithic.service`
- `com.example.monolithic.controller`

## Langkah 5: Membuat Model

### User.java

Buat file `User.java` di package `com.example.monolithic.model`:

```java
package com.example.monolithic.model;

import jakarta.persistence.*;

@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(nullable = false, unique = true)
    private String email;
    
    public User() {}
    
    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }
    
    public Long getId() {
        return id;
    }
    
    public void setId(Long id) {
        this.id = id;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getEmail() {
        return email;
    }
    
    public void setEmail(String email) {
        this.email = email;
    }
}
```

### Product.java

Buat file `Product.java` di package `com.example.monolithic.model`:

```java
package com.example.monolithic.model;

import jakarta.persistence.*;

@Entity
@Table(name = "products")
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(nullable = false)
    private double price;
    
    public Product() {}
    
    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
    
    public Long getId() {
        return id;
    }
    
    public void setId(Long id) {
        this.id = id;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public double getPrice() {
        return price;
    }
    
    public void setPrice(double price) {
        this.price = price;
    }
}
```

## Langkah 6: Membuat Repository

### UserRepository.java

Buat file `UserRepository.java` di package `com.example.monolithic.repository`:

```java
package com.example.monolithic.repository;

import com.example.monolithic.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```

### ProductRepository.java

Buat file `ProductRepository.java` di package `com.example.monolithic.repository`:

```java
package com.example.monolithic.repository;

import com.example.monolithic.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
}
```

## Langkah 7: Membuat Service

### UserService.java

Buat file `UserService.java` di package `com.example.monolithic.service`:

```java
package com.example.monolithic.service;

import com.example.monolithic.model.User;
import com.example.monolithic.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
    
    public Optional<User> getUserById(Long id) {
        return userRepository.findById(id);
    }
    
    public User createUser(User user) {
        return userRepository.save(user);
    }
    
    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

### ProductService.java

Buat file `ProductService.java` di package `com.example.monolithic.service`:

```java
package com.example.monolithic.service;

import com.example.monolithic.model.Product;
import com.example.monolithic.repository.ProductRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class ProductService {
    @Autowired
    private ProductRepository productRepository;
    
    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }
    
    public Optional<Product> getProductById(Long id) {
        return productRepository.findById(id);
    }
    
    public Product createProduct(Product product) {
        return productRepository.save(product);
    }
    
    public void deleteProduct(Long id) {
        productRepository.deleteById(id);
    }
}
```

## Langkah 8: Membuat Controller

### UserController.java

Buat file `UserController.java` di package `com.example.monolithic.controller`:

```java
package com.example.monolithic.controller;

import com.example.monolithic.model.User;
import com.example.monolithic.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/users")
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
    
    @GetMapping("/{id}")
    public Optional<User> getUserById(@PathVariable Long id) {
        return userService.getUserById(id);
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }
    
    @DeleteMapping("/{id}")
    public String deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
        return "User deleted successfully";
    }
}
```

### ProductController.java

Buat file `ProductController.java` di package `com.example.monolithic.controller`:

```java
package com.example.monolithic.controller;

import com.example.monolithic.model.Product;
import com.example.monolithic.service.ProductService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/products")
public class ProductController {
    @Autowired
    private ProductService productService;
    
    @GetMapping
    public List<Product> getAllProducts() {
        return productService.getAllProducts();
    }
    
    @GetMapping("/{id}")
    public Optional<Product> getProductById(@PathVariable Long id) {
        return productService.getProductById(id);
    }
    
    @PostMapping
    public Product createProduct(@RequestBody Product product) {
        return productService.createProduct(product);
    }
    
    @DeleteMapping("/{id}")
    public String deleteProduct(@PathVariable Long id) {
        productService.deleteProduct(id);
        return "Product deleted successfully";
    }
}
```

## Langkah 9: Jalankan Aplikasi

1. Klik kanan pada file `MonolithicAppApplication.java`
2. Pilih **Run As > Java Application**
3. Tunggu hingga muncul log "Started MonolithicAppApplication" di konsol

## Langkah 10: Pengujian API dengan Postman

### API User

1. **Membuat User Baru**:
   - Method: POST
   - URL: http://localhost:8080/users
   - Body (JSON):
     ```json
     {
         "name": "John Doe",
         "email": "johndoe@example.com"
     }
     ```

2. **Mendapatkan Semua User**:
   - Method: GET
   - URL: http://localhost:8080/users

3. **Mendapatkan User berdasarkan ID**:
   - Method: GET
   - URL: http://localhost:8080/users/1

4. **Menghapus User**:
   - Method: DELETE
   - URL: http://localhost:8080/users/1

### API Product

1. **Membuat Product Baru**:
   - Method: POST
   - URL: http://localhost:8080/products
   - Body (JSON):
     ```json
     {
         "name": "Laptop",
         "price": 15000000
     }
     ```

2. **Mendapatkan Semua Product**:
   - Method: GET
   - URL: http://localhost:8080/products

3. **Mendapatkan Product berdasarkan ID**:
   - Method: GET
   - URL: http://localhost:8080/products/1

4. **Menghapus Product**:
   - Method: DELETE
   - URL: http://localhost:8080/products/1

## Kesimpulan

Selamat! Anda telah berhasil membuat aplikasi monolitik menggunakan Spring Boot. Aplikasi ini memiliki dua layanan utama (User dan Product) yang terintegrasi dalam satu codebase, menunjukkan karakteristik arsitektur monolitik.

Arsitektur monolitik ini memiliki beberapa keunggulan:
- Sederhana untuk dikembangkan
- Proses deployment mudah
- Komunikasi antar komponen langsung dan cepat

Namun, perlu diperhatikan bahwa aplikasi monolitik memiliki keterbatasan dalam skalabilitas ketika aplikasi semakin besar.

## Troubleshooting

1. **Database Connection Error**:
   - Pastikan server MySQL berjalan
   - Verifikasi username dan password di application.properties
   - Pastikan database monolithic_db sudah dibuat

2. **Port Already in Use**:
   - Ubah port di application.properties jika port 8080 sudah digunakan
     ```properties
     server.port=8081
     ```

3. **Java Version Error**:
   - Pastikan JDK yang digunakan sesuai dengan versi di pom.xml
   - Jika menggunakan JDK yang berbeda, sesuaikan java.version di pom.xml
