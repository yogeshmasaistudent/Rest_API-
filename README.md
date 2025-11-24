# ⭐⭐⭐ Welcome to today's REST API session.⭐⭐⭐

---

## INTRODUCTION —

> "Hi everyone, welcome to this session.  
> Today we will learn REST APIs in the clearest and essay way . 
> We will build everything step-by-step, from complete basics to fully functional APIs using Node.js and MongoDB.  
> I will explain each method (GET, POST, PUT, PATCH, DELETE) one at a time.  
> We will write theory → then code → then test it → then move to the next part.  
> By the end of this session, you will be able to build this entire backend by yourself."

---

## 🧠 STEP 1 — Understanding Backend

> "Before writing any code, I want you to understand what backend actually means.  
> Backend is the brain of an application.  
> Front-end is what we see, backend is what works behind the scenes.
>
> **Examples:**
>
> - • Opening Instagram feed → backend sends data
> - • Creating a new account → backend stores data
> - • Editing profile → backend updates data
> - • Deleting a post → backend removes data
>
> So backend handles **Read, Create, Update, Delete**, which directly map to:
>
> - • **GET** (Read)
> - • **POST** (Create)
> - • **PUT/PATCH** (Update)
> - • **DELETE** (Remove)"

---

## 🧠 STEP 2 — What is a REST API?

> "REST API is simply a way for frontend and backend to talk to each other.  
> It uses specific methods to tell the server what we want to do.  
> Let me explain these 5 methods very simply:"

| Method | Meaning | Real Life Example |
|--------|---------|-------------------|
| GET | Read data | View Instagram feed |
| POST | Add new data | Create new account |
| PUT | Replace full data | Edit full profile |
| PATCH | Update partial data | Change only username |
| DELETE | Remove | Delete a post |

> "We will write all of these today,  
> but one-by-one, so everything becomes crystal clear."

---

## 🏗️ STEP 3 — Project Folder Structure

> "Before writing APIs, we must set up our backend project properly.  
> Professional developers never put everything in one file.  
> **We will follow this clean structure:**"

```
rest-demo/
│
├── config/      → MongoDB connection
├── models/      → Mongoose schema
├── routes/      → REST API routes
└── index.js     → Entry point
```

> "This structure keeps code clean and understandable for beginners."
>
> ---

#### 📝 Step-by-Step Project Setup

**1. Create Project Folder**

```bash
mkdir rest-demo
cd rest-demo
```

**2. Initialize Node Project**

```bash
npm init -y
```

**3. Install Required Dependencies**

```bash
npm install express mongoose
```

You can add more dependencies as needed (like dotenv or nodemon):

```bash
npm install dotenv nodemon
```

> "Now students can copy-paste these commands and quickly set up the project structure!"

---

## 🟦 STEP 4 — Creating the Entry Point (`index.js`)

> "Let's start from the most basic file — our entry point.  
> Just like index.html is the starting point for frontend,  
> index.js is the starting point for backend."
>
> **What is Express?**
> Express is a fast, lightweight framework for Node.js that makes building web servers and APIs super easy.
> Without Express, we would need to write hundreds of lines of code just to handle requests and responses.
> Express gives us simple functions like `app.get()`, `app.post()` to handle different routes.
> Think of Express as a helpful assistant that manages all the complex backend logic for us.

**Create `index.js`:**

```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Server is working...");
});

app.listen(8080, () => {
  console.log("Server started on port 8080");
});
```

> "Now let's run this command:"
>
> ```bash
> node index.js
> ```
>
> "Open browser → http://localhost:8080  
> If you see 'Server is working…', then our setup is correct."

---

## 🟦 STEP 5 — Connecting MongoDB (`config/db.js`)

> "Now we need to connect to MongoDB.  
> We will keep the connection logic in a separate file inside the config folder."  
**What is Mongoose?**
> > "Mongoose is a library that helps Node.js talk to MongoDB easily. It gives us schemas so our data stays structured, and it provides simple functions to save, update, delete, and read data. Instead of writing complex queries, Mongoose lets us work with JavaScript objects directly."

**Create: `config/db.js`**

```javascript
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect("mongodb+srv://kushawahyogesh93:Yogesh@cluster0.j9tkecq.mongodb.net/Codinggita?appName=Cluster0");
    console.log("MongoDB connected");
  } catch (error) {
    console.error("MongoDB connection failed:", error);
    process.exit(1);
  }
};

module.exports = connectDB;
```

> "That's it. Very simple."

---

## 🟦 STEP 6 — Creating Our Model (`models/student.model.js`)

> "Now we create a model.  
> A model is like a structure or a form—  
> it tells MongoDB what fields each student will have."

**Create: `models/student.model.js`**

```javascript
const mongoose = require("mongoose");

const studentSchema = new mongoose.Schema({
  name: String,
  age: Number,
  city: String
});

module.exports = mongoose.model("Student", studentSchema);
```

---

## 🟦 STEP 7 — Creating Route File (Starting Empty)

> "Now we create a route file where all REST API methods will go.  
> But remember—we will add them one by one."

**Create: `routes/student.routes.js`**

```javascript
const express = require("express");
const Student = require("../models/student.model");
const router = express.Router();

module.exports = router;
```

---

# ⭐⭐⭐ NOW OUR MAIN PART BEGINS ⭐⭐⭐

## ⭐ STEP 8 — Understanding and Coding Each REST Method One-by-One

> We will now learn each method in perfect order:  
> 1️⃣ **GET**  
> 2️⃣ **POST**  
> 3️⃣ **PUT**  
> 4️⃣ **PATCH**  
> 5️⃣ **DELETE**

---

### 🟦 METHOD 1 — GET (Read Data)

> "GET is used to read data from the server.  
> GET never changes the database.  
> It is the most commonly used method."  
> Real-life example:  
> "Viewing Instagram feed."

**🧑‍💻 NOW WRITE THE GET CODE**  
Update `routes/student.routes.js`:

```javascript
// GET - Read all students
router.get("/students", async (req, res) => {
  const data = await Student.find();
  res.send(data);
});
```

**🧪 TEST GET REQUEST**

```
GET http://localhost:8080/api/students
```

> "Great! GET method is working."

---

### 🟩 METHOD 2 — POST (Create Data)

> "POST is used to create new data.  
> If a new user signs up or a new student is added — that is POST."

**🧑‍💻 NOW WRITE POST CODE**

```javascript
// POST - Create a student
router.post("/students", async (req, res) => {
  const student = new Student(req.body);
  await student.save();
  res.send("Student created");
});
```

**🧪 TEST POST REQUEST**

```
POST http://localhost:8080/api/students
```

Send JSON body:

```json
{
  "name": "Yogesh",
  "age": 22,
  "city": "Indore"
}
```

> "Now open GET again —  
> you will see the new student added."

---

### 🟧 METHOD 3 — PUT (Full Replace)

> "PUT is used for full update.  
> It replaces the entire record with new data."  
> Real-life example:  
> "Updating your entire profile."

**🧑‍💻 NOW WRITE PUT CODE**

```javascript
// PUT - Full replace
router.put("/students/:id", async (req, res) => {
  await Student.findByIdAndUpdate(req.params.id, req.body, { overwrite: true });
  res.send("Student fully replaced");
});
```

**🧪 TEST PUT REQUEST**

```
PUT http://localhost:8080/api/students/<student_id>
```

> "PUT will replace everything."

---

### 🟨 METHOD 4 — PATCH (Partial Update)

> "PATCH updates only the fields you send.  
> PATCH does NOT replace the whole document."  
> Real-life example:  
> "Updating only your city or username."

**🧑‍💻 NOW WRITE PATCH CODE**

```javascript
// PATCH - Partial update
router.patch("/students/:id", async (req, res) => {
  await Student.findByIdAndUpdate(req.params.id, req.body);
  res.send("Student partially updated");
});
```

**🧪 TEST PATCH**

```
PATCH http://localhost:8080/api/students/<student_id>
```

---

### 🟥 METHOD 5 — DELETE (Remove Data)

> "DELETE is used to remove a record."  
> Example:  
> "Deleting an Instagram post."

**🧑‍💻 NOW WRITE DELETE CODE**

```javascript
// DELETE - Remove student
router.delete("/students/:id", async (req, res) => {
  await Student.findByIdAndDelete(req.params.id);
  res.send("Student deleted");
});
```

**🧪 TEST DELETE**

```
DELETE http://localhost:8080/api/students/<student_id>
```

---

## 🏆 FINAL ROUTES FILE (After Completing All Methods)

```javascript
const express = require("express");
const Student = require("../models/student.model");
const router = express.Router();

// GET
router.get("/students", async (req, res) => {
  const data = await Student.find();
  res.send(data);
});

// POST
router.post("/students", async (req, res) => {
  const student = new Student(req.body);
  await student.save();
  res.send("Student created");
});

// PUT
router.put("/students/:id", async (req, res) => {
  await Student.findByIdAndUpdate(req.params.id, req.body, { overwrite: true });
  res.send("Student fully replaced");
});

// PATCH
router.patch("/students/:id", async (req, res) => {
  await Student.findByIdAndUpdate(req.params.id, req.body);
  res.send("Student partially updated");
});

// DELETE
router.delete("/students/:id", async (req, res) => {
  await Student.findByIdAndDelete(req.params.id);
  res.send("Student deleted");
});

module.exports = router;
```

---

## 🟦 FINAL STEP — Update `index.js` to use DB + Routes

```javascript
const express = require("express");
const connectDB = require("./config/db");
const studentRoutes = require("./routes/student.routes");
const app = express();

app.use(express.json());

// Connect DB
connectDB().then(() => console.log("DB connected"));

// Use Routes
app.use("/api", studentRoutes);

// Root Route
app.get("/", (req, res) => {
  res.send("API is working...");
});

app.listen(8080, () => {
  console.log("Server started on 8080");
});
```

---

## 🎤 CLOSING STATEMENT (End Your Class With This)

> "Congratulations!  
> You now understand REST APIs completely — not just theory, but real coding too.  
> We learned:
>
> - • GET
> - • POST
> - • PUT
> - • PATCH
> - • DELETE
> - • Folder structure
> - • MongoDB connection
> - • Models
> - • Routes
> - • Entry point setup  
>
> This is the foundation of every backend system in the world.  
> If you master this, you can build any API."

---

**Copy and paste the above into your `README.md` to set a new standard for clarity, teaching, and readability!**
