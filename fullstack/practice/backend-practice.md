# Backend Practice: Building Server-Side Applications

## Practice Project 1: REST API with Express.js

### Project Overview
Build a complete REST API for a task management system using Node.js and Express.js.

### Step 1: Project Setup

**Create project structure:**
```bash
mkdir task-api
cd task-api
npm init -y
npm install express cors helmet morgan dotenv
npm install -D nodemon
```

**Project structure:**
```
task-api/
├── src/
│   ├── controllers/
│   │   └── taskController.js
│   ├── models/
│   │   └── taskModel.js
│   ├── routes/
│   │   └── taskRoutes.js
│   ├── middleware/
│   │   └── validation.js
│   └── utils/
│       └── response.js
├── server.js
├── package.json
└── .env
```

### Step 2: Basic Server Setup

**server.js:**
```javascript
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');
require('dotenv').config();

const taskRoutes = require('./src/routes/taskRoutes');

const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(helmet()); // Security headers
app.use(cors()); // Cross-origin requests
app.use(morgan('combined')); // Logging
app.use(express.json()); // Parse JSON bodies
app.use(express.urlencoded({ extended: true })); // Parse URL-encoded bodies

// Routes
app.use('/api/tasks', taskRoutes);

// Health check endpoint
app.get('/health', (req, res) => {
  res.status(200).json({
    status: 'OK',
    timestamp: new Date().toISOString(),
    uptime: process.uptime()
  });
});

// 404 handler
app.use('*', (req, res) => {
  res.status(404).json({
    error: 'Route not found',
    message: `Cannot ${req.method} ${req.originalUrl}`
  });
});

// Global error handler
app.use((error, req, res, next) => {
  console.error('Error:', error);

  res.status(error.status || 500).json({
    error: error.message || 'Internal server error',
    ...(process.env.NODE_ENV === 'development' && { stack: error.stack })
  });
});

// Start server
app.listen(PORT, () => {
  console.log(`🚀 Server running on port ${PORT}`);
  console.log(`📊 Health check: http://localhost:${PORT}/health`);
});

module.exports = app;
```

**package.json scripts:**
```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "test": "jest"
  }
}
```

### Step 3: Data Model

**src/models/taskModel.js:**
```javascript
// In-memory data store (replace with database in production)
let tasks = [];
let nextId = 1;

class TaskModel {
  // Get all tasks
  static getAllTasks() {
    return [...tasks];
  }

  // Get task by ID
  static getTaskById(id) {
    return tasks.find(task => task.id === id);
  }

  // Create new task
  static createTask(taskData) {
    const newTask = {
      id: nextId++,
      title: taskData.title,
      description: taskData.description || '',
      completed: false,
      priority: taskData.priority || 'medium',
      createdAt: new Date().toISOString(),
      updatedAt: new Date().toISOString()
    };

    tasks.push(newTask);
    return newTask;
  }

  // Update task
  static updateTask(id, updateData) {
    const taskIndex = tasks.findIndex(task => task.id === id);

    if (taskIndex === -1) {
      return null;
    }

    const updatedTask = {
      ...tasks[taskIndex],
      ...updateData,
      updatedAt: new Date().toISOString()
    };

    tasks[taskIndex] = updatedTask;
    return updatedTask;
  }

  // Delete task
  static deleteTask(id) {
    const taskIndex = tasks.findIndex(task => task.id === id);

    if (taskIndex === -1) {
      return false;
    }

    tasks.splice(taskIndex, 1);
    return true;
  }

  // Get tasks by status
  static getTasksByStatus(completed) {
    return tasks.filter(task => task.completed === completed);
  }

  // Get tasks by priority
  static getTasksByPriority(priority) {
    return tasks.filter(task => task.priority === priority);
  }

  // Search tasks
  static searchTasks(query) {
    const lowercaseQuery = query.toLowerCase();
    return tasks.filter(task =>
      task.title.toLowerCase().includes(lowercaseQuery) ||
      task.description.toLowerCase().includes(lowercaseQuery)
    );
  }

  // Get task statistics
  static getTaskStats() {
    const total = tasks.length;
    const completed = tasks.filter(task => task.completed).length;
    const pending = total - completed;

    const priorityStats = {
      high: tasks.filter(task => task.priority === 'high').length,
      medium: tasks.filter(task => task.priority === 'medium').length,
      low: tasks.filter(task => task.priority === 'low').length
    };

    return {
      total,
      completed,
      pending,
      priorityStats
    };
  }

  // Clear all tasks (for testing)
  static clearAllTasks() {
    tasks = [];
    nextId = 1;
    return true;
  }
}

module.exports = TaskModel;
```

### Step 4: Validation Middleware

**src/middleware/validation.js:**
```javascript
const validateTaskData = (req, res, next) => {
  const { title, description, priority, completed } = req.body;
  const errors = [];

  // Validate title
  if (!title || typeof title !== 'string') {
    errors.push('Title is required and must be a string');
  } else if (title.trim().length === 0) {
    errors.push('Title cannot be empty');
  } else if (title.length > 100) {
    errors.push('Title cannot exceed 100 characters');
  }

  // Validate description (optional)
  if (description !== undefined && typeof description !== 'string') {
    errors.push('Description must be a string');
  } else if (description && description.length > 500) {
    errors.push('Description cannot exceed 500 characters');
  }

  // Validate priority
  const validPriorities = ['low', 'medium', 'high'];
  if (priority !== undefined && !validPriorities.includes(priority)) {
    errors.push('Priority must be one of: low, medium, high');
  }

  // Validate completed status
  if (completed !== undefined && typeof completed !== 'boolean') {
    errors.push('Completed must be a boolean value');
  }

  if (errors.length > 0) {
    return res.status(400).json({
      error: 'Validation failed',
      details: errors
    });
  }

  // Sanitize data
  req.body.title = title.trim();
  if (description) {
    req.body.description = description.trim();
  }

  next();
};

const validateIdParam = (req, res, next) => {
  const id = parseInt(req.params.id);

  if (isNaN(id) || id <= 0) {
    return res.status(400).json({
      error: 'Invalid ID',
      message: 'ID must be a positive integer'
    });
  }

  req.params.id = id;
  next();
};

const validateQueryParams = (req, res, next) => {
  const { completed, priority, search, limit, offset } = req.query;

  // Validate completed filter
  if (completed !== undefined) {
    if (completed !== 'true' && completed !== 'false') {
      return res.status(400).json({
        error: 'Invalid query parameter',
        message: 'completed must be true or false'
      });
    }
    req.query.completed = completed === 'true';
  }

  // Validate priority filter
  const validPriorities = ['low', 'medium', 'high'];
  if (priority !== undefined && !validPriorities.includes(priority)) {
    return res.status(400).json({
      error: 'Invalid query parameter',
      message: 'priority must be one of: low, medium, high'
    });
  }

  // Validate limit
  if (limit !== undefined) {
    const limitNum = parseInt(limit);
    if (isNaN(limitNum) || limitNum < 1 || limitNum > 100) {
      return res.status(400).json({
        error: 'Invalid query parameter',
        message: 'limit must be a number between 1 and 100'
      });
    }
    req.query.limit = limitNum;
  }

  // Validate offset
  if (offset !== undefined) {
    const offsetNum = parseInt(offset);
    if (isNaN(offsetNum) || offsetNum < 0) {
      return res.status(400).json({
        error: 'Invalid query parameter',
        message: 'offset must be a non-negative number'
      });
    }
    req.query.offset = offsetNum;
  }

  next();
};

module.exports = {
  validateTaskData,
  validateIdParam,
  validateQueryParams
};
```

### Step 5: Response Utilities

**src/utils/response.js:**
```javascript
const sendSuccess = (res, data, message = 'Success', statusCode = 200) => {
  const response = {
    success: true,
    message,
    data,
    timestamp: new Date().toISOString()
  };

  return res.status(statusCode).json(response);
};

const sendError = (res, message, statusCode = 500, details = null) => {
  const response = {
    success: false,
    message,
    timestamp: new Date().toISOString()
  };

  if (details && process.env.NODE_ENV === 'development') {
    response.details = details;
  }

  return res.status(statusCode).json(response);
};

const sendPaginated = (res, data, pagination, message = 'Success') => {
  const response = {
    success: true,
    message,
    data,
    pagination,
    timestamp: new Date().toISOString()
  };

  return res.status(200).json(response);
};

module.exports = {
  sendSuccess,
  sendError,
  sendPaginated
};
```

### Step 6: Controller

**src/controllers/taskController.js:**
```javascript
const TaskModel = require('../models/taskModel');
const { sendSuccess, sendError, sendPaginated } = require('../utils/response');

const getAllTasks = async (req, res) => {
  try {
    let tasks = TaskModel.getAllTasks();

    // Apply filters
    const { completed, priority, search, limit, offset } = req.query;

    if (completed !== undefined) {
      tasks = tasks.filter(task => task.completed === completed);
    }

    if (priority) {
      tasks = tasks.filter(task => task.priority === priority);
    }

    if (search) {
      tasks = TaskModel.searchTasks(search);
    }

    // Apply pagination
    const total = tasks.length;
    const startIndex = offset || 0;
    const endIndex = limit ? startIndex + limit : tasks.length;
    const paginatedTasks = tasks.slice(startIndex, endIndex);

    // Prepare pagination info
    const pagination = {
      total,
      count: paginatedTasks.length,
      limit: limit || null,
      offset: startIndex,
      hasNext: endIndex < total,
      hasPrev: startIndex > 0
    };

    sendPaginated(res, paginatedTasks, pagination, 'Tasks retrieved successfully');
  } catch (error) {
    sendError(res, 'Failed to retrieve tasks', 500, error.message);
  }
};

const getTaskById = async (req, res) => {
  try {
    const task = TaskModel.getTaskById(req.params.id);

    if (!task) {
      return sendError(res, 'Task not found', 404);
    }

    sendSuccess(res, task, 'Task retrieved successfully');
  } catch (error) {
    sendError(res, 'Failed to retrieve task', 500, error.message);
  }
};

const createTask = async (req, res) => {
  try {
    const taskData = req.body;
    const newTask = TaskModel.createTask(taskData);

    sendSuccess(res, newTask, 'Task created successfully', 201);
  } catch (error) {
    sendError(res, 'Failed to create task', 500, error.message);
  }
};

const updateTask = async (req, res) => {
  try {
    const taskId = req.params.id;
    const updateData = req.body;

    const updatedTask = TaskModel.updateTask(taskId, updateData);

    if (!updatedTask) {
      return sendError(res, 'Task not found', 404);
    }

    sendSuccess(res, updatedTask, 'Task updated successfully');
  } catch (error) {
    sendError(res, 'Failed to update task', 500, error.message);
  }
};

const deleteTask = async (req, res) => {
  try {
    const taskId = req.params.id;
    const deleted = TaskModel.deleteTask(taskId);

    if (!deleted) {
      return sendError(res, 'Task not found', 404);
    }

    sendSuccess(res, null, 'Task deleted successfully');
  } catch (error) {
    sendError(res, 'Failed to delete task', 500, error.message);
  }
};

const getTaskStats = async (req, res) => {
  try {
    const stats = TaskModel.getTaskStats();
    sendSuccess(res, stats, 'Task statistics retrieved successfully');
  } catch (error) {
    sendError(res, 'Failed to retrieve task statistics', 500, error.message);
  }
};

const toggleTaskStatus = async (req, res) => {
  try {
    const taskId = req.params.id;
    const task = TaskModel.getTaskById(taskId);

    if (!task) {
      return sendError(res, 'Task not found', 404);
    }

    const updateData = { completed: !task.completed };
    const updatedTask = TaskModel.updateTask(taskId, updateData);

    sendSuccess(res, updatedTask, 'Task status updated successfully');
  } catch (error) {
    sendError(res, 'Failed to update task status', 500, error.message);
  }
};

module.exports = {
  getAllTasks,
  getTaskById,
  createTask,
  updateTask,
  deleteTask,
  getTaskStats,
  toggleTaskStatus
};
```

### Step 7: Routes

**src/routes/taskRoutes.js:**
```javascript
const express = require('express');
const router = express.Router();
const taskController = require('../controllers/taskController');
const { validateTaskData, validateIdParam, validateQueryParams } = require('../middleware/validation');

// GET /api/tasks - Get all tasks with optional filtering and pagination
router.get('/', validateQueryParams, taskController.getAllTasks);

// GET /api/tasks/stats - Get task statistics
router.get('/stats', taskController.getTaskStats);

// GET /api/tasks/:id - Get a specific task
router.get('/:id', validateIdParam, taskController.getTaskById);

// POST /api/tasks - Create a new task
router.post('/', validateTaskData, taskController.createTask);

// PUT /api/tasks/:id - Update a task completely
router.put('/:id', validateIdParam, validateTaskData, taskController.updateTask);

// PATCH /api/tasks/:id/toggle - Toggle task completion status
router.patch('/:id/toggle', validateIdParam, taskController.toggleTaskStatus);

// DELETE /api/tasks/:id - Delete a task
router.delete('/:id', validateIdParam, taskController.deleteTask);

module.exports = router;
```

### Step 8: Environment Configuration

**.env:**
```
NODE_ENV=development
PORT=3000
```

### Step 9: Testing the API

**Start the server:**
```bash
npm run dev
```

**Test endpoints with curl:**

```bash
# Health check
curl http://localhost:3000/health

# Create a task
curl -X POST http://localhost:3000/api/tasks \
  -H "Content-Type: application/json" \
  -d '{"title": "Learn Node.js", "description": "Study Express.js fundamentals", "priority": "high"}'

# Get all tasks
curl http://localhost:3000/api/tasks

# Get task statistics
curl http://localhost:3000/api/tasks/stats

# Update a task (replace 1 with actual ID)
curl -X PUT http://localhost:3000/api/tasks/1 \
  -H "Content-Type: application/json" \
  -d '{"title": "Learn Node.js", "completed": true}'

# Delete a task
curl -X DELETE http://localhost:3000/api/tasks/1
```

---

## Practice Project 2: User Authentication System

### Project Overview
Build a user authentication system with registration, login, and JWT tokens.

### Step 1: Authentication Setup

**Install additional dependencies:**
```bash
npm install bcryptjs jsonwebtoken
```

**Create auth structure:**
```
src/
├── controllers/
│   └── authController.js
├── models/
│   └── userModel.js
├── routes/
│   └── authRoutes.js
├── middleware/
│   ├── auth.js
│   └── validation.js (add auth validation)
└── utils/
    └── jwt.js
```

### Step 2: User Model

**src/models/userModel.js:**
```javascript
const bcrypt = require('bcryptjs');

let users = [];
let nextId = 1;

class UserModel {
  static async createUser(userData) {
    // Check if user already exists
    const existingUser = users.find(user => user.email === userData.email);
    if (existingUser) {
      throw new Error('User already exists');
    }

    // Hash password
    const saltRounds = 10;
    const hashedPassword = await bcrypt.hash(userData.password, saltRounds);

    const newUser = {
      id: nextId++,
      name: userData.name,
      email: userData.email,
      password: hashedPassword,
      role: userData.role || 'user',
      createdAt: new Date().toISOString(),
      updatedAt: new Date().toISOString()
    };

    users.push(newUser);

    // Return user without password
    const { password, ...userWithoutPassword } = newUser;
    return userWithoutPassword;
  }

  static async authenticateUser(email, password) {
    const user = users.find(user => user.email === email);
    if (!user) {
      return null;
    }

    const isValidPassword = await bcrypt.compare(password, user.password);
    if (!isValidPassword) {
      return null;
    }

    // Return user without password
    const { password: _, ...userWithoutPassword } = user;
    return userWithoutPassword;
  }

  static getUserById(id) {
    const user = users.find(user => user.id === id);
    if (!user) return null;

    const { password, ...userWithoutPassword } = user;
    return userWithoutPassword;
  }

  static getUserByEmail(email) {
    const user = users.find(user => user.email === email);
    if (!user) return null;

    const { password, ...userWithoutPassword } = user;
    return userWithoutPassword;
  }

  static updateUser(id, updateData) {
    const userIndex = users.findIndex(user => user.id === id);
    if (userIndex === -1) return null;

    const updatedUser = {
      ...users[userIndex],
      ...updateData,
      updatedAt: new Date().toISOString()
    };

    users[userIndex] = updatedUser;

    const { password, ...userWithoutPassword } = updatedUser;
    return userWithoutPassword;
  }

  static deleteUser(id) {
    const userIndex = users.findIndex(user => user.id === id);
    if (userIndex === -1) return false;

    users.splice(userIndex, 1);
    return true;
  }

  static getAllUsers() {
    return users.map(user => {
      const { password, ...userWithoutPassword } = user;
      return userWithoutPassword;
    });
  }

  // Clear all users (for testing)
  static clearAllUsers() {
    users = [];
    nextId = 1;
    return true;
  }
}

module.exports = UserModel;
```

### Step 3: JWT Utilities

**src/utils/jwt.js:**
```javascript
const jwt = require('jsonwebtoken');

const JWT_SECRET = process.env.JWT_SECRET || 'your-secret-key-change-in-production';
const JWT_EXPIRE = process.env.JWT_EXPIRE || '7d';

const generateToken = (payload) => {
  return jwt.sign(payload, JWT_SECRET, { expiresIn: JWT_EXPIRE });
};

const verifyToken = (token) => {
  try {
    return jwt.verify(token, JWT_SECRET);
  } catch (error) {
    return null;
  }
};

const extractTokenFromHeader = (authHeader) => {
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return null;
  }
  return authHeader.substring(7);
};

module.exports = {
  generateToken,
  verifyToken,
  extractTokenFromHeader
};
```

### Step 4: Authentication Middleware

**src/middleware/auth.js:**
```javascript
const UserModel = require('../models/userModel');
const { verifyToken, extractTokenFromHeader } = require('../utils/jwt');
const { sendError } = require('../utils/response');

const authenticate = async (req, res, next) => {
  try {
    const token = extractTokenFromHeader(req.headers.authorization);

    if (!token) {
      return sendError(res, 'Access denied. No token provided.', 401);
    }

    const decoded = verifyToken(token);
    if (!decoded) {
      return sendError(res, 'Invalid token.', 401);
    }

    const user = UserModel.getUserById(decoded.id);
    if (!user) {
      return sendError(res, 'User not found.', 401);
    }

    req.user = user;
    next();
  } catch (error) {
    sendError(res, 'Authentication failed.', 500, error.message);
  }
};

const authorize = (...roles) => {
  return (req, res, next) => {
    if (!req.user) {
      return sendError(res, 'Authentication required.', 401);
    }

    if (!roles.includes(req.user.role)) {
      return sendError(res, 'Insufficient permissions.', 403);
    }

    next();
  };
};

const optionalAuth = async (req, res, next) => {
  try {
    const token = extractTokenFromHeader(req.headers.authorization);

    if (token) {
      const decoded = verifyToken(token);
      if (decoded) {
        const user = UserModel.getUserById(decoded.id);
        if (user) {
          req.user = user;
        }
      }
    }

    next();
  } catch (error) {
    // Ignore auth errors for optional auth
    next();
  }
};

module.exports = {
  authenticate,
  authorize,
  optionalAuth
};
```

### Step 5: Auth Controller

**src/controllers/authController.js:**
```javascript
const UserModel = require('../models/userModel');
const { generateToken } = require('../utils/jwt');
const { sendSuccess, sendError } = require('../utils/response');

const register = async (req, res) => {
  try {
    const userData = req.body;
    const newUser = await UserModel.createUser(userData);

    const token = generateToken({ id: newUser.id, email: newUser.email });

    sendSuccess(res, {
      user: newUser,
      token
    }, 'User registered successfully', 201);
  } catch (error) {
    if (error.message === 'User already exists') {
      return sendError(res, 'User already exists', 409);
    }
    sendError(res, 'Registration failed', 500, error.message);
  }
};

const login = async (req, res) => {
  try {
    const { email, password } = req.body;

    const user = await UserModel.authenticateUser(email, password);
    if (!user) {
      return sendError(res, 'Invalid email or password', 401);
    }

    const token = generateToken({ id: user.id, email: user.email });

    sendSuccess(res, {
      user,
      token
    }, 'Login successful');
  } catch (error) {
    sendError(res, 'Login failed', 500, error.message);
  }
};

const getProfile = async (req, res) => {
  try {
    sendSuccess(res, req.user, 'Profile retrieved successfully');
  } catch (error) {
    sendError(res, 'Failed to retrieve profile', 500, error.message);
  }
};

const updateProfile = async (req, res) => {
  try {
    const userId = req.user.id;
    const updateData = req.body;

    // Don't allow password updates through this endpoint
    delete updateData.password;
    delete updateData.email; // Email changes might need verification

    const updatedUser = UserModel.updateUser(userId, updateData);
    if (!updatedUser) {
      return sendError(res, 'User not found', 404);
    }

    sendSuccess(res, updatedUser, 'Profile updated successfully');
  } catch (error) {
    sendError(res, 'Failed to update profile', 500, error.message);
  }
};

const logout = async (req, res) => {
  // For stateless JWT, logout is handled client-side
  // In a production app with refresh tokens, you might blacklist the token
  sendSuccess(res, null, 'Logout successful');
};

module.exports = {
  register,
  login,
  getProfile,
  updateProfile,
  logout
};
```

### Step 6: Auth Routes

**src/routes/authRoutes.js:**
```javascript
const express = require('express');
const router = express.Router();
const authController = require('../controllers/authController');
const { authenticate } = require('../middleware/auth');
const { validateRegistration, validateLogin } = require('../middleware/validation');

// POST /api/auth/register - Register a new user
router.post('/register', validateRegistration, authController.register);

// POST /api/auth/login - Login user
router.post('/login', validateLogin, authController.login);

// GET /api/auth/profile - Get user profile (protected)
router.get('/profile', authenticate, authController.getProfile);

// PUT /api/auth/profile - Update user profile (protected)
router.put('/profile', authenticate, authController.updateProfile);

// POST /api/auth/logout - Logout user
router.post('/logout', authController.logout);

module.exports = router;
```

### Step 7: Update Server and Validation

**Update server.js:**
```javascript
// Add auth routes
const authRoutes = require('./src/routes/authRoutes');
app.use('/api/auth', authRoutes);
```

**Add auth validation to validation.js:**
```javascript
const validateRegistration = (req, res, next) => {
  const { name, email, password } = req.body;
  const errors = [];

  // Validate name
  if (!name || typeof name !== 'string' || name.trim().length === 0) {
    errors.push('Name is required and must be a non-empty string');
  } else if (name.length > 50) {
    errors.push('Name cannot exceed 50 characters');
  }

  // Validate email
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!email || typeof email !== 'string') {
    errors.push('Email is required and must be a string');
  } else if (!emailRegex.test(email)) {
    errors.push('Invalid email format');
  }

  // Validate password
  if (!password || typeof password !== 'string') {
    errors.push('Password is required and must be a string');
  } else if (password.length < 6) {
    errors.push('Password must be at least 6 characters long');
  }

  if (errors.length > 0) {
    return res.status(400).json({
      error: 'Validation failed',
      details: errors
    });
  }

  // Sanitize data
  req.body.name = name.trim();
  req.body.email = email.trim().toLowerCase();

  next();
};

const validateLogin = (req, res, next) => {
  const { email, password } = req.body;
  const errors = [];

  // Validate email
  if (!email || typeof email !== 'string') {
    errors.push('Email is required');
  }

  // Validate password
  if (!password || typeof password !== 'string') {
    errors.push('Password is required');
  }

  if (errors.length > 0) {
    return res.status(400).json({
      error: 'Validation failed',
      details: errors
    });
  }

  req.body.email = email.trim().toLowerCase();

  next();
};
```

### Step 8: Testing Authentication

**Test the auth endpoints:**

```bash
# Register a new user
curl -X POST http://localhost:3000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123"
  }'

# Login
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "password123"
  }'

# Get profile (use token from login response)
curl -X GET http://localhost:3000/api/auth/profile \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"

# Update profile
curl -X PUT http://localhost:3000/api/auth/profile \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{"name": "John Smith"}'
```

---

## Practice Project 3: File Upload System

### Project Overview
Build a file upload system with image processing and cloud storage simulation.

### Step 1: File Upload Setup

**Install dependencies:**
```bash
npm install multer sharp
```

**Create upload structure:**
```
src/
├── controllers/
│   └── uploadController.js
├── routes/
│   └── uploadRoutes.js
├── middleware/
│   └── upload.js
└── utils/
    └── file.js
```

### Step 2: Upload Middleware

**src/middleware/upload.js:**
```javascript
const multer = require('multer');
const path = require('path');
const fs = require('fs');

// Ensure upload directory exists
const uploadDir = path.join(__dirname, '../../uploads');
if (!fs.existsSync(uploadDir)) {
  fs.mkdirSync(uploadDir, { recursive: true });
}

// Storage configuration
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, uploadDir);
  },
  filename: (req, file, cb) => {
    // Generate unique filename
    const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1E9);
    cb(null, file.fieldname + '-' + uniqueSuffix + path.extname(file.originalname));
  }
});

// File filter
const fileFilter = (req, file, cb) => {
  // Allowed file types
  const allowedTypes = /jpeg|jpg|png|gif|pdf|doc|docx|txt/;
  const extname = allowedTypes.test(path.extname(file.originalname).toLowerCase());
  const mimetype = allowedTypes.test(file.mimetype);

  if (mimetype && extname) {
    return cb(null, true);
  } else {
    cb(new Error('Invalid file type. Only images, PDFs, and documents are allowed.'));
  }
};

// Upload configuration
const upload = multer({
  storage: storage,
  limits: {
    fileSize: 5 * 1024 * 1024, // 5MB limit
    files: 10 // Maximum 10 files
  },
  fileFilter: fileFilter
});

// Export different upload configurations
const uploadSingle = upload.single('file');
const uploadMultiple = upload.array('files', 10);
const uploadFields = upload.fields([
  { name: 'avatar', maxCount: 1 },
  { name: 'documents', maxCount: 5 }
]);

module.exports = {
  uploadSingle,
  uploadMultiple,
  uploadFields,
  uploadDir
};
```

### Step 3: File Utilities

**src/utils/file.js:**
```javascript
const fs = require('fs');
const path = require('path');
const sharp = require('sharp');

const deleteFile = (filePath) => {
  return new Promise((resolve, reject) => {
    fs.unlink(filePath, (error) => {
      if (error) {
        reject(error);
      } else {
        resolve();
      }
    });
  });
};

const getFileInfo = (filePath) => {
  try {
    const stats = fs.statSync(filePath);
    const ext = path.extname(filePath).toLowerCase();

    return {
      size: stats.size,
      created: stats.birthtime,
      modified: stats.mtime,
      extension: ext,
      isImage: ['.jpg', '.jpeg', '.png', '.gif', '.webp'].includes(ext)
    };
  } catch (error) {
    return null;
  }
};

const processImage = async (inputPath, outputPath, options = {}) => {
  try {
    let sharpInstance = sharp(inputPath);

    // Apply transformations
    if (options.resize) {
      sharpInstance = sharpInstance.resize(options.resize.width, options.resize.height, {
        fit: options.resize.fit || 'cover',
        position: options.resize.position || 'center'
      });
    }

    if (options.rotate) {
      sharpInstance = sharpInstance.rotate(options.rotate);
    }

    if (options.flip) {
      sharpInstance = sharpInstance.flip();
    }

    if (options.flop) {
      sharpInstance = sharpInstance.flop();
    }

    if (options.blur) {
      sharpInstance = sharpInstance.blur(options.blur);
    }

    if (options.sharpen) {
      sharpInstance = sharpInstance.sharpen(options.sharpen);
    }

    // Set format and quality
    const format = options.format || 'jpeg';
    const quality = options.quality || 80;

    await sharpInstance
      [format]({ quality })
      .toFile(outputPath);

    return true;
  } catch (error) {
    console.error('Image processing error:', error);
    return false;
  }
};

const generateThumbnail = async (inputPath, outputPath, size = 200) => {
  return processImage(inputPath, outputPath, {
    resize: { width: size, height: size, fit: 'cover' },
    format: 'jpeg',
    quality: 70
  });
};

const getMimeType = (filename) => {
  const ext = path.extname(filename).toLowerCase();
  const mimeTypes = {
    '.txt': 'text/plain',
    '.html': 'text/html',
    '.css': 'text/css',
    '.js': 'application/javascript',
    '.json': 'application/json',
    '.xml': 'application/xml',
    '.jpg': 'image/jpeg',
    '.jpeg': 'image/jpeg',
    '.png': 'image/png',
    '.gif': 'image/gif',
    '.webp': 'image/webp',
    '.pdf': 'application/pdf',
    '.doc': 'application/msword',
    '.docx': 'application/vnd.openxmlformats-officedocument.wordprocessingml.document'
  };

  return mimeTypes[ext] || 'application/octet-stream';
};

module.exports = {
  deleteFile,
  getFileInfo,
  processImage,
  generateThumbnail,
  getMimeType
};
```

### Step 4: Upload Controller

**src/controllers/uploadController.js:**
```javascript
const path = require('path');
const { uploadDir } = require('../middleware/upload');
const { deleteFile, getFileInfo, processImage, generateThumbnail, getMimeType } = require('../utils/file');
const { sendSuccess, sendError } = require('../utils/response');

const uploadSingleFile = async (req, res) => {
  try {
    if (!req.file) {
      return sendError(res, 'No file uploaded', 400);
    }

    const file = req.file;
    const fileInfo = getFileInfo(file.path);

    const fileData = {
      id: Date.now().toString(),
      originalName: file.originalname,
      filename: file.filename,
      path: file.path,
      size: file.size,
      mimetype: file.mimetype,
      info: fileInfo,
      uploadedAt: new Date().toISOString(),
      uploadedBy: req.user ? req.user.id : null
    };

    // Generate thumbnail for images
    if (fileInfo && fileInfo.isImage) {
      const thumbnailPath = path.join(uploadDir, 'thumbnails', `thumb-${file.filename}`);
      const thumbnailDir = path.dirname(thumbnailPath);

      // Ensure thumbnail directory exists
      const fs = require('fs');
      if (!fs.existsSync(thumbnailDir)) {
        fs.mkdirSync(thumbnailDir, { recursive: true });
      }

      await generateThumbnail(file.path, thumbnailPath);
      fileData.thumbnail = thumbnailPath;
    }

    sendSuccess(res, fileData, 'File uploaded successfully', 201);
  } catch (error) {
    sendError(res, 'File upload failed', 500, error.message);
  }
};

const uploadMultipleFiles = async (req, res) => {
  try {
    if (!req.files || req.files.length === 0) {
      return sendError(res, 'No files uploaded', 400);
    }

    const uploadedFiles = await Promise.all(
      req.files.map(async (file) => {
        const fileInfo = getFileInfo(file.path);

        const fileData = {
          id: Date.now().toString() + Math.random().toString(36).substr(2, 9),
          originalName: file.originalname,
          filename: file.filename,
          path: file.path,
          size: file.size,
          mimetype: file.mimetype,
          info: fileInfo,
          uploadedAt: new Date().toISOString(),
          uploadedBy: req.user ? req.user.id : null
        };

        // Generate thumbnail for images
        if (fileInfo && fileInfo.isImage) {
          const thumbnailPath = path.join(uploadDir, 'thumbnails', `thumb-${file.filename}`);
          const thumbnailDir = path.dirname(thumbnailPath);

          const fs = require('fs');
          if (!fs.existsSync(thumbnailDir)) {
            fs.mkdirSync(thumbnailDir, { recursive: true });
          }

          await generateThumbnail(file.path, thumbnailPath);
          fileData.thumbnail = thumbnailPath;
        }

        return fileData;
      })
    );

    sendSuccess(res, uploadedFiles, `${uploadedFiles.length} files uploaded successfully`, 201);
  } catch (error) {
    sendError(res, 'File upload failed', 500, error.message);
  }
};

const getFile = async (req, res) => {
  try {
    const filename = req.params.filename;
    const filePath = path.join(uploadDir, filename);

    // Check if file exists
    const fs = require('fs');
    if (!fs.existsSync(filePath)) {
      return sendError(res, 'File not found', 404);
    }

    const fileInfo = getFileInfo(filePath);
    if (!fileInfo) {
      return sendError(res, 'File information unavailable', 500);
    }

    // Set appropriate headers
    res.setHeader('Content-Type', getMimeType(filename));
    res.setHeader('Content-Length', fileInfo.size);

    // Stream the file
    const fileStream = fs.createReadStream(filePath);
    fileStream.pipe(res);

  } catch (error) {
    sendError(res, 'Failed to retrieve file', 500, error.message);
  }
};

const deleteUploadedFile = async (req, res) => {
  try {
    const filename = req.params.filename;
    const filePath = path.join(uploadDir, filename);

    // Check if file exists
    const fs = require('fs');
    if (!fs.existsSync(filePath)) {
      return sendError(res, 'File not found', 404);
    }

    // Delete the file
    await deleteFile(filePath);

    // Also delete thumbnail if it exists
    const thumbnailPath = path.join(uploadDir, 'thumbnails', `thumb-${filename}`);
    if (fs.existsSync(thumbnailPath)) {
      await deleteFile(thumbnailPath);
    }

    sendSuccess(res, null, 'File deleted successfully');
  } catch (error) {
    sendError(res, 'Failed to delete file', 500, error.message);
  }
};

const processImageFile = async (req, res) => {
  try {
    const { filename } = req.params;
    const { width, height, format, quality } = req.query;

    const inputPath = path.join(uploadDir, filename);
    const outputFilename = `processed-${Date.now()}-${filename}`;
    const outputPath = path.join(uploadDir, 'processed', outputFilename);

    // Ensure processed directory exists
    const fs = require('fs');
    const processedDir = path.dirname(outputPath);
    if (!fs.existsSync(processedDir)) {
      fs.mkdirSync(processedDir, { recursive: true });
    }

    // Check if input file exists
    if (!fs.existsSync(inputPath)) {
      return sendError(res, 'Source file not found', 404);
    }

    const options = {
      format: format || 'jpeg',
      quality: parseInt(quality) || 80
    };

    if (width || height) {
      options.resize = {
        width: width ? parseInt(width) : null,
        height: height ? parseInt(height) : null,
        fit: 'cover'
      };
    }

    const success = await processImage(inputPath, outputPath, options);

    if (!success) {
      return sendError(res, 'Image processing failed', 500);
    }

    const fileInfo = getFileInfo(outputPath);

    sendSuccess(res, {
      originalFile: filename,
      processedFile: outputFilename,
      path: outputPath,
      info: fileInfo
    }, 'Image processed successfully');
  } catch (error) {
    sendError(res, 'Image processing failed', 500, error.message);
  }
};

module.exports = {
  uploadSingleFile,
  uploadMultipleFiles,
  getFile,
  deleteUploadedFile,
  processImageFile
};
```

### Step 5: Upload Routes

**src/routes/uploadRoutes.js:**
```javascript
const express = require('express');
const router = express.Router();
const uploadController = require('../controllers/uploadController');
const { uploadSingle, uploadMultiple } = require('../middleware/upload');
const { authenticate } = require('../middleware/auth');

// POST /api/upload/single - Upload single file
router.post('/single', authenticate, uploadSingle, uploadController.uploadSingleFile);

// POST /api/upload/multiple - Upload multiple files
router.post('/multiple', authenticate, uploadMultiple, uploadController.uploadMultipleFiles);

// GET /api/upload/files/:filename - Get uploaded file
router.get('/files/:filename', uploadController.getFile);

// DELETE /api/upload/files/:filename - Delete uploaded file
router.delete('/files/:filename', authenticate, uploadController.deleteUploadedFile);

// POST /api/upload/process/:filename - Process image file
router.post('/process/:filename', authenticate, uploadController.processImageFile);

module.exports = router;
```

### Step 6: Testing File Upload

**Test the upload endpoints:**

```bash
# Upload a single file (replace with actual file path)
curl -X POST http://localhost:3000/api/upload/single \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -F "file=@/path/to/your/file.jpg"

# Get uploaded file info
curl http://localhost:3000/api/upload/files/uploaded-filename.jpg

# Process an image
curl -X POST "http://localhost:3000/api/upload/process/uploaded-filename.jpg?width=300&height=300" \
  -H "Authorization: Bearer YOUR_TOKEN"

# Delete a file
curl -X DELETE http://localhost:3000/api/upload/files/uploaded-filename.jpg \
  -H "Authorization: Bearer YOUR_TOKEN"
```

---

## Additional Practice Exercises

### Exercise 1: API Rate Limiting
Add rate limiting to prevent API abuse.

### Exercise 2: Request Logging
Implement comprehensive request logging with Morgan.

### Exercise 3: API Documentation
Add Swagger/OpenAPI documentation to your APIs.

### Exercise 4: Error Monitoring
Integrate error monitoring and alerting.

### Exercise 5: API Caching
Implement Redis caching for API responses.

### Exercise 6: Background Jobs
Add background job processing with Bull/Agenda.

### Exercise 7: API Versioning
Implement API versioning strategy.

### Exercise 8: WebSocket Integration
Add real-time features with Socket.io.

Remember: Backend development focuses on reliability, security, and performance. Always validate inputs, handle errors gracefully, and monitor your applications!
