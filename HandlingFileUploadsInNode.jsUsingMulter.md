# Handling File Uploads in Node.js using Multer

## Introduction
Handling file uploads is a common requirement in web applications, especially for user-generated content like images, videos, and documents. This article will guide you through the process of handling file uploads in Node.js using Multer, a popular middleware for handling `multipart/form-data`, which is primarily used for uploading files.

## Prerequisites
- **Node.js**: Ensure Node.js is installed on your machine.
- **npm (Node Package Manager)**: Used to install and manage packages.

## Setting Up the Project
1. **Initialize a new Node.js project**:
   ```bash
git clone https://github.com/Oluwoleopeyemi/your-nodejs-project.git
cd your-nodejs-project
npm init -y
```
2. **Install Multer and Express**:
   ```bash
npm install express multer
```\n## 1. Basic File Upload
### 1.1 Setting Up the Server
Create a file named `app.js` and set up a basic Express server.
```javascript
const express = require('express');
const multer = require('multer');
const path = require('path');

const app = express();
const port = 3000;

// Configure storage for uploaded files
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, 'uploads/')
  },
  filename: function (req, file, cb) {
    cb(null, Date.now() + path.extname(file.originalname)) // Appends the ext name
  }
});

const upload = multer({ storage: storage });

// Middleware to serve static files
app.use(express.static('public'));

// Route for handling file uploads
app.post('/upload', upload.single('file'), (req, res) => {
  if (!req.file) {
    return res.status(400).send('No file uploaded.');
  }
  res.send(`File ${req.file.filename} uploaded successfully.`);
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}/`);
});
```\n### 1.2 Creating the HTML Form
Create a file named `index.html` in the `public` directory and add the following form:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>File Upload</title>
</head>
<body>
  <h1>Upload a File</h1>
  <form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="file">
    <button type="submit">Upload</button>
  </form>
</body>
</html>
```\n## 2. Multiple File Uploads
### 2.1 Configuring Multer for Multiple Files
Modify the `upload` middleware to handle multiple file uploads.
```javascript
const upload = multer({ storage: storage, limits: { fileSize: 5 * 1024 * 1024 }, fileFilter: function (req, file, cb) {
  checkFileType(file, cb);
}});

function checkFileType(file, cb) {
  const filetypes = /jpeg|jpg|png|gif/;
  const extname = filetypes.test(path.extname(file.originalname).toLowerCase());
  const mimetype = filetypes.test(file.mimetype);

  if (extname && mimetype) {
    return cb(null, true);
  } else {
    cb('Error: Images only!');
  }
}
```\n### 2.2 Handling Multiple File Uploads in Express
Update the route to handle multiple file uploads.
```javascript
app.post('/upload', upload.array('files', 5), (req, res) => {
  if (!req.files) {
    return res.status(400).send('No files uploaded.');
  }
  res.send(`Files ${req.files.map(file => file.filename)}. Uploaded successfully.`);
});
```\n## 3. Error Handling
### 3.1 Handling Multer Errors
Multer provides an `on` method to handle errors.
```javascript
app.post('/upload', upload.single('file'), (req, res) => {
  if (!req.file) {
    return res.status(400).send('No file uploaded.');
  }
  res.send(`File ${req.file.filename} uploaded successfully.`);
}).on('error', function (err) {
  console.log(err);
  res.status(500).send('Server Error');
});
```\n## Conclusion
In this article, you learned how to handle file uploads in Node.js using Multer. You covered basic file uploads, handling multiple files, and error management. With these steps, you can easily integrate file upload functionality into your Node.js applications.

Happy coding!\n
---

**Oluwole Emmanuel**\
[GitHub](https://github.com/Oluwoleopeyemi)\
[Email](oluwoleopeyemi31@gmail.com)