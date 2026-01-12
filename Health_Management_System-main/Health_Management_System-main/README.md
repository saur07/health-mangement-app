# 🏥 Health Management System (HMS)

A complete **Health Management System** built with **Node.js, Express, SQL Server, and MSSQL library**, providing role‑based access for **Patients, Doctors, and Admins**. The system supports user registration, doctor management, appointment booking, and real‑time chat for appointments.

---

## 🚀 Project Motive

The goal of this project is to create a **simple, scalable, and secure healthcare management platform** where:

* Patients can register, book appointments, and chat with doctors.
* Doctors can manage appointments & communicate with patients.
* Admins can manage users and doctors.
* Provides a clean modular backend structure.

This system demonstrates practical **full‑stack backend architecture** using Express + SQL Server.

---

## 🗄️ Database Schema (SQL Server)

Below are the SQL tables used in the project.

### **Users Table**

```
CREATE TABLE users (
    id INT IDENTITY(1,1) PRIMARY KEY,
    userId varchar(20) NOT NULL UNIQUE,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    role varchar(10) CHECK (role IN('PATIENT','DOCTOR','ADMIN')) DEFAULT 'PATIENT',
    password VARCHAR(255) NOT NULL,
    phoneNo VARCHAR(15),
    gender VARCHAR(10) CHECK (gender IN ('Male', 'Female')),
    age INT,
    createdAt DATETIME DEFAULT GETDATE()
);
```

### **Doctors Table**

```
create table doctors(
 id INT IDENTITY(1,1) PRIMARY KEY,
 name VARCHAR(100) NOT NULL,
 email VARCHAR(100) NOT NULL UNIQUE,
 phoneNo VARCHAR(15),
 specialization VARCHAR(100) NOT NULL,
 experience INT NOT NULL,
 qualification VARCHAR(255),
 doctorId VARCHAR(20) NOT NULL UNIQUE,
 createdAt DATETIME DEFAULT GETDATE(),
 CONSTRAINT FK_Doctors_User FOREIGN KEY (doctorId) REFERENCES users(userId)
);
```

### **Appointments Table**

```
CREATE TABLE appointments (
  id INT IDENTITY(1,1) PRIMARY KEY,
  appointmentId NVARCHAR(50) UNIQUE NOT NULL,
  date DATETIME NULL,
  time NVARCHAR(50) NULL,
  status NVARCHAR(20) CHECK (status IN ('PENDING', 'BOOKED', 'REJECTED')) DEFAULT 'PENDING',
  notes NVARCHAR(MAX) NULL,
  patientId varchar(20) NOT NULL,
  doctorId varchar(20) NOT NULL,
  createdAt DATETIME DEFAULT GETDATE(),
  CONSTRAINT FK_Patient FOREIGN KEY (patientId) REFERENCES users(userId),
  CONSTRAINT FK_Doctor FOREIGN KEY (doctorId) REFERENCES doctors(doctorId)
);
```

### **Chats Table**

```
CREATE TABLE chats (
  id INT IDENTITY(1,1) PRIMARY KEY,
  appointmentId NVARCHAR(50) NOT NULL,
  senderId varchar(20) NOT NULL,
  receiverId varchar(20) NOT NULL,
  message NVARCHAR(MAX) NOT NULL,
  createdAt DATETIME DEFAULT GETDATE(),
  CONSTRAINT FK_Chat_Appointment FOREIGN KEY (appointmentId) REFERENCES appointments(appointmentId),
  CONSTRAINT FK_Chat_Sender FOREIGN KEY (senderId) REFERENCES users(userId),
  CONSTRAINT FK_Chat_Receiver FOREIGN KEY (receiverId) REFERENCES users(userId)
);
```

---

## 🔌 Database Connection (`db.js`)

```
// db.js
import sql from "mssql";
import dotenv from "dotenv";
dotenv.config();

const dbConfig = {
  user: process.env.DB_USER,       
  password: process.env.DB_PASSWORD,
  server: process.env.DB_SERVER,
  database: process.env.DB_NAME,
  port: 1433,
  options: {
    encrypt: false,
    trustServerCertificate: true,
  },
};

let pool;

export async function getPool() {    
  try {
    if (pool && pool.connected) {
      return pool; // reuse existing connection
    }

    pool = await new sql.ConnectionPool(dbConfig).connect();
    console.log("✅ Connected to SQL Server");
    return pool;
  } catch (err) {
    console.error("❌ Database connection error:", err.message);
    throw err;
  }
}

export { sql };
```

---

## ✅ Features

* User Signup/Login (JWT Based)
* Doctor Management
* Appointment Booking + Status Control
* Chat Messaging System
* SQL Server Integration
* Role‑Based Access
* Clean Folder Structure

---

## 📌 Future Enhancements

* Patient medical history
* Prescription module
* Admin dashboard
* Real-time chat using WebSockets

---

## 🏁 Conclusion

This project is a complete backend for a **professional health management platform**.
Well-structured, scalable, and production‑ready.

---

**Made with ❤️ for learning and real-world use cases.**
