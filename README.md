<div align="center">

# 🚀 REST API Mastery Workshop
### *Build Production-Ready APIs from Scratch*

![REST API](https://img.shields.io/badge/REST-API-blue?style=for-the-badge)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)

### ⭐ **Welcome to the most practical REST API session you'll ever take!** ⭐

*From Zero to Hero in just one session*

</div>

---

## 📖 Table of Contents
- [Introduction](#-introduction)
- [Why REST APIs?](#-why-rest-apis)
- [What You'll Build](#-what-youll-build)
- [Prerequisites](#-prerequisites)
- [Project Setup](#-project-setup)
- [Step-by-Step Implementation](#-step-by-step-implementation)
- [Testing Your APIs](#-testing-your-apis)
- [Next Steps](#-next-steps)

---

## 🎯 Introduction

> "Hi everyone! 👋 Welcome to this hands-on session where we'll build a complete REST API from scratch.
> 
> By the end of this session, you'll:
> - ✅ Understand what REST APIs really are (no more confusion!)
> - ✅ Build 5 different HTTP methods (GET, POST, PUT, PATCH, DELETE)
> - ✅ Connect Node.js with MongoDB
> - ✅ Follow professional folder structure
> - ✅ Test your APIs like a pro
> 
> **Teaching Style:** Theory → Code → Test → Repeat  
> **Duration:** ~90 minutes  
> **Difficulty:** Beginner-Friendly 🎓"

---

## 🤔 Why REST APIs?

<div align="center">

### Real-World Examples You Use Every Day:

| Action | Behind The Scenes | HTTP Method |
|--------|-------------------|-------------|
| 📱 Opening Instagram feed | Backend sends your posts | `GET` |
| 👤 Creating a new account | Backend saves your data | `POST` |
| ✏️ Editing your profile | Backend updates data | `PUT/PATCH` |
| 🗑️ Deleting a post | Backend removes data | `DELETE` |

</div>

### 🧠 Understanding Backend

```
┌─────────────┐         REST API          ┌──────────────┐
│             │  ←──────────────────────→  │              │
│  Frontend   │     (GET, POST, etc.)      │   Backend    │
│  (What we   │                            │  (The Brain) │
│   see)      │                            │              │
└─────────────┘                            └──────────────┘
                                                   ↓
                                            ┌──────────────┐
                                            │   Database   │
                                            │   (Storage)  │
                                            └──────────────┘
```

> **Backend = Brain of Application**  
> Frontend is what we see, backend is what works behind the scenes!

---

## 📝 What You'll Build

A complete **Student Management API** with:

```javascript
✔️ GET    /api/students      // Fetch all students
✔️ POST   /api/students      // Add new student
✔️ PUT    /api/students/:id  // Fully replace student data
✔️ PATCH  /api/students/:id  // Partially update student
✔️ DELETE /api/students/:id  // Remove student
```

---

## ⚙️ Prerequisites

```bash
✓ Node.js installed (v14+)
✓ MongoDB Atlas account (free tier)
✓ Postman or Thunder Client (for testing)
✓ VS Code (recommended)
```

---

## 🛠️ Project Setup

### 📂 Professional Folder Structure

```
rest-api-workshop/
│
├── 📁 config/
│   └── db.js              # MongoDB connection logic
│
├── 📁 models/
│   └── student.model.js   # Data schema/structure
│
├── 📁 routes/
│   └── student.routes.js  # All API endpoints
│
└── 📄 index.js            # Entry point (server)
```

> 💡 **Why this structure?**  
> - **Separation of Concerns**: Each file has ONE job
> - **Scalability**: Easy to add more models/routes
> - **Industry Standard**: Used by professionals worldwide

---

## 🚀 Step-by-Step Implementation

### 🟦 STEP 1: Initial Setup

**Create project and install dependencies:**

```bash
mkdir rest-api-workshop
cd rest-api-workshop
npm init -y
npm install express mongoose
```

---

### 🟦 STEP 2: Create Entry Point (`index.js`)

> 📚 **What we're doing:** Setting up the basic server

```javascript
const express = require("express");
const app = express();

// Test route
app.get("/", (req, res) => {
  res.send("✅ Server is running!");
});

// Start server
app.listen(8080, () => {
  console.log("🚀 Server started on http://localhost:8080");
});
```

**🧪 Test it:**
```bash
node index.js
# Visit: http://localhost:8080
```

---

### 🟦 STEP 3: Connect MongoDB (`config/db.js`)

> 📚 **What is Mongoose?**  
> Mongoose makes talking to MongoDB super easy. Instead of complex queries, you work with JavaScript objects!

**Create `config/db.js`:**

```javascript
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect("YOUR_MONGODB_URI_HERE");
    console.log("✅ MongoDB connected successfully");
  } catch (error) {
    console.error("❌ MongoDB connection failed:", error);
    process.exit(1);
  }
};

module.exports = connectDB;
```

> ⚠️ **Important:** Replace `YOUR_MONGODB_URI_HERE` with your actual MongoDB Atlas connection string

---

### 🟦 STEP 4: Create Data Model (`models/student.model.js`)

> 📚 **What is a Model?**  
> Think of it as a "form template" - it defines what fields each student record should have.

```javascript
const mongoose = require("mongoose");

const studentSchema = new mongoose.Schema({
  name: { type: String, required: true },
  age: { type: Number, required: true },
  city: { type: String, required: true }
}, {
  timestamps: true  // Adds createdAt & updatedAt automatically
});

module.exports = mongoose.model("Student", studentSchema);
```

---

## ⭐ The Main Part: Building All 5 REST Methods

### 🟩 METHOD 1: GET (Read Data)

**📚 Purpose:** Retrieve/View data (doesn't modify anything)

**🌍 Real Example:** Opening Instagram feed, viewing YouTube videos

**Create `routes/student.routes.js`:**

```javascript
const express = require("express");
const Student = require("../models/student.model");
const router = express.Router();

// GET - Fetch all students
router.get("/students", async (req, res) => {
  try {
    const students = await Student.find();
    res.status(200).json({
      success: true,
      count: students.length,
      data: students
    });
  } catch (error) {
    res.status(500).json({ success: false, error: error.message });
  }
});

module.exports = router;
```

**🧪 Test:** `GET http://localhost:8080/api/students`

---

### 🟨 METHOD 2: POST (Create Data)

**📚 Purpose:** Add new data to database

**🌍 Real Example:** Creating Instagram account, posting a tweet

**Add to `routes/student.routes.js`:**

```javascript
// POST - Add new student
router.post("/students", async (req, res) => {
  try {
    const student = new Student(req.body);
    await student.save();
    res.status(201).json({
      success: true,
      message: "✨ Student created successfully!",
      data: student
    });
  } catch (error) {
    res.status(400).json({ success: false, error: error.message });
  }
});
```

**🧪 Test:** `POST http://localhost:8080/api/students`
```json
{
  "name": "Yogesh Kumar",
  "age": 22,
  "city": "Indore"
}
```

---

### 🟧 METHOD 3: PUT (Full Replace)

**📚 Purpose:** Replace entire record with new data

**🌍 Real Example:** Completely rewriting your LinkedIn profile

**⚠️ Key Difference:**
- **PUT** = Replace EVERYTHING (missing fields become null)
- **PATCH** = Update only sent fields (next method)

```javascript
// PUT - Full replacement
router.put("/students/:id", async (req, res) => {
  try {
    const student = await Student.findByIdAndUpdate(
      req.params.id,
      req.body,
      { new: true, overwrite: true, runValidators: true }
    );
    
    if (!student) {
      return res.status(404).json({ 
        success: false, 
        message: "🚫 Student not found" 
      });
    }
    
    res.status(200).json({
      success: true,
      message: "🔄 Student fully replaced!",
      data: student
    });
  } catch (error) {
    res.status(400).json({ success: false, error: error.message });
  }
});
```

**🧪 Test:** `PUT http://localhost:8080/api/students/<student_id>`

---

### 🟪 METHOD 4: PATCH (Partial Update)

**📚 Purpose:** Update only specific fields

**🌍 Real Example:** Changing just your phone number on a form

```javascript
// PATCH - Partial update
router.patch("/students/:id", async (req, res) => {
  try {
    const student = await Student.findByIdAndUpdate(
      req.params.id,
      { $set: req.body },  // Only updates provided fields
      { new: true, runValidators: true }
    );
    
    if (!student) {
      return res.status(404).json({ 
        success: false, 
        message: "🚫 Student not found" 
      });
    }
    
    res.status(200).json({
      success: true,
      message: "✏️ Student updated partially!",
      data: student
    });
  } catch (error) {
    res.status(400).json({ success: false, error: error.message });
  }
});
```

**🧪 Test:** `PATCH http://localhost:8080/api/students/<student_id>`
```json
{
  "city": "Mumbai"  // Only updates city
}
```

---

### 🟥 METHOD 5: DELETE (Remove Data)

**📚 Purpose:** Permanently remove record

**🌍 Real Example:** Deleting Instagram post, removing tweet

```javascript
// DELETE - Remove student
router.delete("/students/:id", async (req, res) => {
  try {
    const student = await Student.findByIdAndDelete(req.params.id);
    
    if (!student) {
      return res.status(404).json({ 
        success: false, 
        message: "🚫 Student not found" 
      });
    }
    
    res.status(200).json({
      success: true,
      message: "🗑️ Student deleted successfully!"
    });
  } catch (error) {
    res.status(500).json({ success: false, error: error.message });
  }
});
```

**🧪 Test:** `DELETE http://localhost:8080/api/students/<student_id>`

---

## 🔌 Final Integration

**Update `index.js` to connect everything:**

```javascript
const express = require("express");
const connectDB = require("./config/db");
const studentRoutes = require("./routes/student.routes");

const app = express();

// Middleware
app.use(express.json());  // Parse JSON bodies

// Connect to MongoDB
connectDB();

// Routes
app.use("/api", studentRoutes);

// Root route
app.get("/", (req, res) => {
  res.json({
    message: "🚀 REST API is running!",
    endpoints: {
      getAll: "GET /api/students",
      create: "POST /api/students",
      update: "PUT /api/students/:id",
      partialUpdate: "PATCH /api/students/:id",
      delete: "DELETE /api/students/:id"
    }
  });
});

// Start server
const PORT = 8080;
app.listen(PORT, () => {
  console.log(`🚀 Server running on http://localhost:${PORT}`);
});
```

---

## 🧪 Testing Your APIs

### Using Postman/Thunder Client:

1. **GET Request:**
   - URL: `http://localhost:8080/api/students`
   - Method: GET
   - No body needed

2. **POST Request:**
   - URL: `http://localhost:8080/api/students`
   - Method: POST
   - Body (JSON):
   ```json
   {
     "name": "Rahul Sharma",
     "age": 21,
     "city": "Delhi"
   }
   ```

3. **PATCH Request:**
   - URL: `http://localhost:8080/api/students/<id>`
   - Method: PATCH
   - Body (JSON):
   ```json
   {
     "age": 23
   }
   ```

4. **DELETE Request:**
   - URL: `http://localhost:8080/api/students/<id>`
   - Method: DELETE
   - No body needed

---

## 📊 Quick Comparison Table

| Method | Purpose | Example | Modifies DB? |
|--------|---------|---------|---------------|
| **GET** | Read data | View profile | ❌ No |
| **POST** | Create new | Sign up | ✅ Yes |
| **PUT** | Full replace | Rewrite profile | ✅ Yes |
| **PATCH** | Partial update | Change city only | ✅ Yes |
| **DELETE** | Remove | Delete account | ✅ Yes |

---

## 🎓 What You've Learned

✅ Backend fundamentals  
✅ REST API architecture  
✅ All 5 HTTP methods (GET, POST, PUT, PATCH, DELETE)  
✅ MongoDB integration with Mongoose  
✅ Professional project structure  
✅ Error handling  
✅ API testing  

---

## 🚀 Next Steps

1. **Add Validation:** Use `express-validator` for input validation
2. **Add Authentication:** Implement JWT-based auth
3. **Add Pagination:** For GET requests with large data
4. **Deploy:** Use Heroku, Render, or Railway
5. **Add Documentation:** Use Swagger/OpenAPI

---

## 📚 Additional Resources

- [Express.js Documentation](https://expressjs.com/)
- [Mongoose Documentation](https://mongoosejs.com/)
- [MongoDB Atlas Setup Guide](https://www.mongodb.com/cloud/atlas)
- [Postman Learning](https://learning.postman.com/)

---

## 👏 Congratulations!

<div align="center">

### You just built a production-ready REST API! 🎉

This is the **foundation of EVERY backend system** in the world.  
Master this, and you can build ANY API!

**Share this repo with your friends and give it a ⭐ if you found it helpful!**

---

### 💬 Connect With Me

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](YOUR_LINKEDIN_URL)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](YOUR_GITHUB_URL)
[![Portfolio](https://img.shields.io/badge/Portfolio-FF5722?style=for-the-badge&logo=todoist&logoColor=white)](YOUR_PORTFOLIO_URL)

---

**Made with ❤️ for students who want to learn backend development**

</div>
