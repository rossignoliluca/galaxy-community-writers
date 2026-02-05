# Handling File Uploads in Node.js using Multer
## Introduction
File uploads are a common feature in web applications, allowing users to upload images, documents, and other files. In this article, we will explore how to handle file uploads in Node.js using the Multer middleware.
## 1. Setting Up Multer
First, you need to install Multer by running:
```
npm install multer
```Then, set up a basic Multer configuration in your Node.js application:
```javascript
const express = require('express');
const multer = require('multer');
const app = express();

// Set up storage engine
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, 'uploads/')
  },
  filename: function (req, file, cb) {
    cb(null, file.fieldname + '-' + Date.now() + path.extname(file.originalname)) // 123-456789.jpg
  }
});

// Initialize upload variable
const upload = multer({ storage: storage });

app.post('/upload', upload.single('myFile'), (req, res) => {
  console.log(req.file);
  res.send('File uploaded');
});
```## 2. Configuring Multer Options
Multer offers several options to customize the file uploading process. Here are some common configurations:
- **limits**: Set limits on the size of uploads.
- **fileFilter**: Filter out unwanted files.
- **storage**: Define where and how files should be stored.
## 3. Handling Multiple File Uploads
You can handle multiple file uploads by using the `multer.array()` method:
```javascript
app.post('/uploadMultiple', upload.array('myFiles', 10), (req, res) => {
  console.log(req.files);
  res.send('Multiple files uploaded');
});
```## 4. Processing Uploaded Files
After uploading files, you may need to process them further. Here are some common tasks:
- **Resizing images**: Use libraries like Sharp or Jimp.
- **Converting documents**: Use libraries like PDFKit or docxtemplater.
- **Storing metadata**: Save file details in a database.
## 5. Error Handling
Handling errors is crucial to ensure a smooth user experience. Multer provides built-in error handling capabilities, and you can also add custom error handlers:
```javascript
app.post('/upload', upload.single('myFile'), (req, res) => {
  if (!req.file) {
    return res.status(400).send('No file uploaded.');
  }
  console.log(req.file);
  res.send('File uploaded');
});
```## Conclusion
Handling file uploads in Node.js using Multer is straightforward and efficient. By following this article, you should be able to set up a robust file upload system for your web application.
