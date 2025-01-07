npx create-react-app spotify-clone
cd spotify-clone
npm start
// src/App.js
import React from 'react';

function App() {
  return (
    <div>
      <header>
        <h1>Spotify Clone</h1>
      </header>
      <main>
        <p>Welcome to your music streaming service!</p>
      </main>
    </div>
  );
}

export default App;
mkdir backend
cd backend
npm init -y
npm install express
// backend/server.js
const express = require('express');
const app = express();
const port = 3001;

app.get('/', (req, res) => {
  res.send('Welcome to the Spotify Clone Backend!');
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
// backend/db.js
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/spotify-clone', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

const db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', () => {
  console.log('Connected to MongoDB');
});
// backend/models/User.js
const mongoose = require('mongoose');
const bcrypt = require('bcrypt');

const userSchema = new mongoose.Schema({
  username: { type: String, required: true, unique: true },
  password: { type: String, required: true },
});

userSchema.pre('save', async function(next) {
  const user = this;
  if (user.isModified('password')) {
    user.password = await bcrypt.hash(user.password, 8);
  }
  next();
});

const User = mongoose.model('User', userSchema);
module.exports = User;

// backend/routes/auth.js
const express = require('express');
const router = express.Router();
const User = require('../models/User');
const jwt = require('jsonwebtoken');

router.post('/register', async (req, res) => {
  try {
    const user = new User(req.body);
    await user.save();
    const token = jwt.sign({ _id: user._id }, 'secretkey');
    res.status(201).send({ user, token });
  } catch (error) {
    res.status(400).send(error);
  }
});

router.post('/login', async (req, res) => {
  try {
    const user = await User.findOne({ username: req.body.username });
    if (!user || !await bcrypt.compare(req.body.password, user.password)) {
      return res.status(400).send('Invalid credentials');
    }
    const token = jwt.sign({ _id: user._id }, 'secretkey');
    res.send({ user, token });
  } catch (error) {
    res.status(500).send(error);
  }
});

module.exports = router;
