# Permalist

A dynamic Permalist web application built using **Node.js**, **Express**, **PostgreSQL**, and **EJS**.  
It allows users to add, edit, and delete tasks with data stored persistently in a database.  
The app features a clean UI and server-side rendering for smooth task management.

---

### Implementation Details

#### 1. Dependencies

First, the project initializes and installs necessary dependencies:

```bash
npm init
npm i express body-parser ejs pg
```


#### 2. Server Setup (index.js)

```javascript
import express from "express";
import bodyParser from "body-parser";
import pg from "pg";

const app = express();
const port = 3000;

const db = new pg.Client({
  user: "postgres",
  host: "localhost",
  database: "permalist",
  password: "password",
  port: 5432,
});
db.connect();
```

#### 3. Database Configuration

```sql
CREATE TABLE items (
  id SERIAL PRIMARY KEY,
  title VARCHAR(100) NOT NULL
);
```

#### 4. Routes (index.js)

```javascript
app.get("/", async (req, res) => {
  try {
    const result = await db.query("SELECT * FROM items ORDER BY id ASC");
    items = result.rows;

    res.render("index.ejs", {
      listTitle: "Today",
      listItems: items,
    });
  } catch (err) {
    console.log(err);
  }
});

app.post("/add", async (req, res) => {
  const item = req.body.newItem;
  // items.push({title: item});
  try {
    await db.query("INSERT INTO items (title) VALUES ($1)", [item]);
    res.redirect("/");
  } catch (err) {
    console.log(err);
  }
});

app.post("/edit", async (req, res) => {
  const item = req.body.updatedItemTitle;
  const id = req.body.updatedItemId;

  try {
    await db.query("UPDATE items SET title = ($1) WHERE id = $2", [item, id]);
    res.redirect("/");
  } catch (err) {
    console.log(err);
  }
});

app.post("/delete", async (req, res) => {
  const id = req.body.deleteItemId;
  try {
    await db.query("DELETE FROM items WHERE id = $1", [id]);
    res.redirect("/");
  } catch (err) {
    console.log(err);
  }
});
```

#### Features

- Add new tasks  
- Edit existing tasks  
- Delete tasks  
- Data persistence using PostgreSQL  
- Server-side rendering with EJS  

---

#### Tech Stack

- Node.js  
- Express  
- PostgreSQL  
- EJS  
- Body-Parser  

---

####  Installation

 **Clone the repository**
   ```bash
   https://github.com/imconcurrente/Permalist.git
   cd permalist
   
```
  ### Conclusion
This project showcases a simple yet functional implementation of a full-stack Todo List application.  
It demonstrates core concepts such as CRUD operations, database integration, and server-side rendering.  
Perfect for beginners looking to build practical web development skills using modern technologies.
