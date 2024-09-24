<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0"> <title>Signup Page</title>
<style> body { fontfamily: Arial, sans-serif;
background-color: #f0f0f0;
display: flex; justify-content: center; align-items: center; height: 100vh; margin: 0;
}
.signup-container
{ background-color: #ffffff padding: 20px; border-radius:
8px; box-shadow: 0 0 15px rgba(0, 0, 0, 0.1); max-width: 400px;
width: 100%;
}
.signup-container h1 { text-align: center; color:
#333; margin-bottom:
20px;
}
.signup-container label { font-weight: bold; color:
#555; margin-bottom:
5px;
display: block;
}
.signup-container input[type="text"],
.signup-container input[type="email"], .signupcontainer input[type="password"] {
width: 100%; padding: 10px; marginbottom: 15px; border: 1px solid #ccc; borderradius: 4px; boxsizing: border-box;
font-size: 14px;
}
.signup-container input[type="submit"] { width: 100%; padding: 10px; background-color: #28a745; color: #fff; border: none; border-
radius: 4px; cursor: pointer; font-size: 16px;
transition: background-color 0.3s ease;
}
.signup-container input[type="submit"]:hover { background-color: #218838;
}
</style>
</head>
<body>
<div class="signup-container">
<h1>Sign Up</h1>
<form action="/signup" method="POST">
<label for="name">Name:</label>
<input type="text" id="name" name="name" required>
<label for="email">Email:</label>
<input type="email" id="email" name="email" required>
<label for="password">Password:</label>
<input type="password" id="password" name="password" required>
<input type="submit" value="Sign Up">
</form>
</div>
</body>
</html>
//server.js
const express = require('express'); const
bodyParser = require('body-parser');
const fs = require('fs');
const path = require('path');
const app = express();
const port = 3000;
// Middleware to parse the body of the request
app.use(bodyParser.urlencoded({ extended: true }));
// Serve the signup page app.get('/',
(req, res) => { res.sendFile(path.join(__dirname, 'index.html'));
});
// Handle the signup form submission app.post('/signup',
(req, res) => { const { name, email, password } = req.body;
// Create a string of the user data const userData = `Name: ${name}, Email: ${email}, Password: ${password}\n`; // Append the user data to a file fs.appendFile('users.txt', userData, (err) => {
if (err) {
console.error('Error writing to file', err);
res.status(500).send('Internal Server Error');
} else {
res.send('Signup successful! Your data has been saved.');
}
});
});
// Route to read and display the content of users.txt app.get('/users',
(req, res) => { fs.readFile('users.txt', 'utf8', (err, data) => {
if (err) {
console.error('Error reading file', err);
res.status(500).send('Internal Server Error');
} else {
res.send(`<pre>${data}</pre>`);
}
});
});
// Start the server app.listen(port,
() => { console.log(`Server running at http://localhost:${port}`); });