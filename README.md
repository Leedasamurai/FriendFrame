# FriendFrame

## Overview

**FriendFrame** is a fully functional social media application designed and developed by Lesego Phuku. This project was inspired by a curiosity to understand and replicate the workings of popular social media platforms. Built using the MERN stack, FriendFrame allows users to register, create posts, upload images, like and dislike posts, manage friend lists, and view profiles, all within a responsive and modern user interface.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Architecture](#architecture)
- [Installation](#installation)
- [Usage](#usage)
- [Core Algorithms](#core-algorithms)
- [Challenges Overcome](#challenges-overcome)
- [Learnings](#learnings)
- [Contributing](#contributing)
- [License](#license)

## Features

- User Registration and Login with complete validation
- Profile picture upload
- Post creation with image upload
- Like and dislike posts
- Comment on posts
- View user feed with all posts
- Manage friends (add/remove)
- View other user profiles
- Responsive design for smaller screens

## Technologies Used

### Frontend

- **React**: JavaScript library for building user interfaces
- **Material-UI**: React component library for faster and easier web development
- **React Router**: For navigation
- **Formik & Yup**: For form handling and validation
- **Redux Toolkit**: For state management
- **Redux Persist**: To persist Redux state in local storage
- **React Dropzone**: For file uploads

### Backend

- **Node.js**: JavaScript runtime
- **Express.js**: Web framework for Node.js
- **Mongoose**: MongoDB object modeling tool
- **JWT (JSON Web Token)**: For authentication
- **Multer**: For file uploading

### Database

- **MongoDB**: NoSQL database

## Architecture

FriendFrame uses a typical MERN stack architecture:

1. **Frontend**: Built with React, communicating with the backend via RESTful APIs.
2. **Backend**: Built with Node.js and Express.js, handling requests and performing CRUD operations.
3. **Database**: MongoDB for storing user data, posts, and friend relationships.

### System Architecture Diagram

![System Architecture Diagram](path/to/architecture-diagram.png)

## Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/Leedasamurai/friendframe.git
   cd friendframe
   ```

2. **Install dependencies:**

   ```bash
   npm install
   cd client
   npm install
   cd ..
   ```

3. **Set up environment variables:**
   Create a `.env` file in the root directory and add the following:

   ```env
   MONGO_URL=your_mongodb_connection_string
   JWT_SECRET=your_jwt_secret
   ```

4. **Run the application:**
   ```bash
   npm run dev
   ```

## Usage

1. **Register a new user**: Access the registration page and create a new account.
2. **Login**: Use your credentials to log in.
3. **Create a post**: Write a post and optionally upload an image.
4. **Interact with posts**: Like, dislike, and comment on posts.
5. **Manage friends**: Add or remove friends from the friend list.
6. **View profiles**: Check out other users' profiles.

## Core Algorithms

### User Authentication

User authentication is handled using JWT. Passwords are hashed using bcrypt before being stored in the database.

````javascript
import bcrypt from 'bcrypt';
import jwt from 'jsonwebtoken';

export const register = async (req, res) => {
  const { firstName, lastName, email, password } = req.body;
  const salt = await bcrypt.genSalt();
  const passwordHash = await bcrypt.hash(password, salt);

  const newUser = new User({
    firstName,
    lastName,
    email,
    password: passwordHash,
  });

  const savedUser = await newUser.save();
  res.status(201).json(savedUser);
};

export const login = async (req, res) => {
  const { email, password } = req.body;
  const user = await User.findOne({ email });
  if (!user) return res.status(400).json({ msg: "User does not exist." });

  const isMatch = await bcrypt.compare(password, user.password);
  if (!isMatch) return res.status(400).json({ msg: "Invalid credentials." });

  const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET);
  res.status(200).json({ token, user });
};

## Post Creation

Posts can be created with text and optional image uploads, and are stored in the database.

```javascript
import Post from "../models/Post.js";

export const createPost = async (req, res) => {
  const { userId, description, picturePath } = req.body;
  const user = await User.findById(userId);

  const newPost = new Post({
    userId,
    firstName: user.firstName,
    lastName: user.lastName,
    description,
    picturePath,
    likes: {},
    comments: [],
  });

  await newPost.save();
  res.status(201).json(newPost);
};

## Challenges Overcome

- **Image Routing**: Ensuring images were correctly routed and accessible both from the backend and frontend.
- **Frontend-Backend Communication**: Handling asynchronous data fetching and state management efficiently.
- **Error Handling**: Implementing robust error handling for various operations, particularly in user authentication and post creation.

## Learnings

- Gained a deeper understanding of database management with MongoDB.
- Enhanced skills in frontend development with React and state management with Redux.
- Improved problem-solving and debugging techniques.

## Contributing

Contributions are welcome! Please fork this repository and submit a pull request for any enhancements or bug fixes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

Thank you for exploring FriendFrame! If you have any questions or feedback, feel free to reach out.
````
