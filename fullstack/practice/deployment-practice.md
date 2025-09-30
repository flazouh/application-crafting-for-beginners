# Deployment Practice: Shipping Applications to Production

## Practice Project 1: Deploy to Vercel

### Project Overview
Deploy a React application to Vercel with continuous deployment.

### Step 1: Create a React Application

**Initialize the project:**
```bash
npx create-react-app my-vercel-app
cd my-vercel-app
```

**Create a simple portfolio website:**
```jsx
// src/App.js
import React from 'react';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <h1>My Portfolio</h1>
        <p>Deployed on Vercel 🚀</p>
        <div className="social-links">
          <a href="https://github.com" target="_blank" rel="noopener noreferrer">
            GitHub
          </a>
          <a href="https://linkedin.com" target="_blank" rel="noopener noreferrer">
            LinkedIn
          </a>
        </div>
      </header>
      <main>
        <section className="about">
          <h2>About Me</h2>
          <p>I'm a full-stack developer passionate about creating amazing web applications.</p>
        </section>
        <section className="projects">
          <h2>My Projects</h2>
          <div className="project-grid">
            <div className="project-card">
              <h3>Project 1</h3>
              <p>A React application with modern UI</p>
            </div>
            <div className="project-card">
              <h3>Project 2</h3>
              <p>A Node.js API with Express</p>
            </div>
            <div className="project-card">
              <h3>Project 3</h3>
              <p>A mobile app using React Native</p>
            </div>
          </div>
        </section>
      </main>
    </div>
  );
}

export default App;
```

**Add some styling:**
```css
/* src/App.css */
.App {
  text-align: center;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.App-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 60px 20px;
  border-radius: 12px;
  margin-bottom: 40px;
}

.social-links {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 20px;
}

.social-links a {
  color: white;
  text-decoration: none;
  padding: 10px 20px;
  border: 2px solid white;
  border-radius: 25px;
  transition: all 0.3s ease;
}

.social-links a:hover {
  background: white;
  color: #667eea;
}

section {
  margin-bottom: 40px;
  padding: 20px;
}

.about {
  background: #f8f9fa;
  border-radius: 8px;
}

.projects h2 {
  color: #2c3e50;
  margin-bottom: 20px;
}

.project-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

.project-card {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.project-card:hover {
  transform: translateY(-5px);
}

.project-card h3 {
  color: #3498db;
  margin-bottom: 10px;
}
```

### Step 2: Initialize Git Repository

**Set up version control:**
```bash
git init
git add .
git commit -m "Initial commit: Create React portfolio website"
```

### Step 3: Create Vercel Account and Deploy

1. **Go to Vercel.com and sign up/sign in**
2. **Connect your GitHub account**
3. **Click "New Project"**
4. **Import your Git repository**
5. **Configure build settings:**
   - Framework Preset: Create React App
   - Build Command: `npm run build`
   - Output Directory: `build`
   - Install Command: `npm install`

### Step 4: Deploy and Test

**The deployment will happen automatically:**
- Vercel will build your application
- Deploy it to their CDN
- Provide you with a live URL (e.g., `my-vercel-app.vercel.app`)

**Test your deployment:**
```bash
# Check if the site is live
curl -I https://my-vercel-app.vercel.app
```

### Step 5: Set Up Custom Domain (Optional)

1. **Go to your project settings in Vercel**
2. **Navigate to "Domains"**
3. **Add your custom domain**
4. **Configure DNS records as instructed**

### Step 6: Environment Variables

**Add environment variables:**
1. **In Vercel dashboard, go to Project Settings → Environment Variables**
2. **Add variables like:**
   - `REACT_APP_API_URL=https://api.example.com`
   - `REACT_APP_ANALYTICS_ID=your-analytics-id`

**Use in your React app:**
```javascript
// src/config.js
const config = {
  apiUrl: process.env.REACT_APP_API_URL,
  analyticsId: process.env.REACT_APP_ANALYTICS_ID
};

export default config;
```

---

## Practice Project 2: Deploy Node.js API to Railway

### Project Overview
Deploy a Node.js REST API to Railway with database integration.

### Step 1: Create the API

**Initialize the project:**
```bash
mkdir railway-api
cd railway-api
npm init -y
npm install express cors helmet morgan dotenv pg
npm install -D nodemon
```

**Create the server:**
```javascript
// server.js
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');
require('dotenv').config();

const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(helmet());
app.use(cors());
app.use(morgan('combined'));
app.use(express.json());

// Routes
app.get('/', (req, res) => {
  res.json({
    message: 'Railway API is running! 🚂',
    timestamp: new Date().toISOString(),
    environment: process.env.NODE_ENV || 'development'
  });
});

app.get('/api/health', (req, res) => {
  res.json({
    status: 'healthy',
    uptime: process.uptime(),
    timestamp: new Date().toISOString()
  });
});

app.get('/api/users', (req, res) => {
  // Mock user data (replace with database queries)
  const users = [
    { id: 1, name: 'John Doe', email: 'john@example.com' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com' },
    { id: 3, name: 'Bob Wilson', email: 'bob@example.com' }
  ];

  res.json({
    success: true,
    data: users,
    count: users.length
  });
});

app.post('/api/users', (req, res) => {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({
      success: false,
      error: 'Name and email are required'
    });
  }

  // Mock user creation
  const newUser = {
    id: Date.now(),
    name,
    email,
    createdAt: new Date().toISOString()
  };

  res.status(201).json({
    success: true,
    data: newUser,
    message: 'User created successfully'
  });
});

// Error handling middleware
app.use((error, req, res, next) => {
  console.error('Error:', error);
  res.status(500).json({
    success: false,
    error: 'Internal server error'
  });
});

// 404 handler
app.use('*', (req, res) => {
  res.status(404).json({
    success: false,
    error: 'Route not found'
  });
});

app.listen(PORT, () => {
  console.log(`🚀 Server running on port ${PORT}`);
  console.log(`🌍 Environment: ${process.env.NODE_ENV || 'development'}`);
});

module.exports = app;
```

**Create package.json scripts:**
```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  }
}
```

**Create .env file:**
```
NODE_ENV=development
PORT=3000
```

### Step 2: Set Up Git and Push to GitHub

**Initialize git and push:**
```bash
git init
git add .
git commit -m "Initial commit: Create Node.js API for Railway deployment"

# Create GitHub repository and push
gh repo create railway-api --public --source=. --push
```

### Step 3: Deploy to Railway

1. **Go to Railway.app and sign up/sign in**
2. **Click "New Project"**
3. **Choose "Deploy from GitHub repo"**
4. **Connect your GitHub account**
5. **Select your `railway-api` repository**
6. **Configure deployment:**
   - Build Command: `npm install`
   - Start Command: `npm start`
   - Add environment variables in Railway dashboard

### Step 4: Add Database (PostgreSQL)

**In Railway dashboard:**
1. **Add a PostgreSQL database to your project**
2. **Copy the database URL from Railway**
3. **Add it as an environment variable:** `DATABASE_URL`

**Update your code to use PostgreSQL:**
```javascript
// Add to package.json dependencies
// "pg": "^8.11.0"

// db.js
const { Pool } = require('pg');

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  ssl: process.env.NODE_ENV === 'production' ? { rejectUnauthorized: false } : false
});

// Test database connection
pool.on('connect', () => {
  console.log('🗄️ Connected to PostgreSQL database');
});

pool.on('error', (err) => {
  console.error('❌ Database connection error:', err);
});

module.exports = pool;
```

**Create users table and operations:**
```javascript
// models/userModel.js
const pool = require('../db');

class UserModel {
  static async getAllUsers() {
    const query = 'SELECT id, name, email, created_at FROM users ORDER BY created_at DESC';
    const result = await pool.query(query);
    return result.rows;
  }

  static async createUser(userData) {
    const { name, email } = userData;
    const query = 'INSERT INTO users (name, email) VALUES ($1, $2) RETURNING id, name, email, created_at';
    const values = [name, email];
    const result = await pool.query(query, values);
    return result.rows[0];
  }

  static async getUserById(id) {
    const query = 'SELECT id, name, email, created_at FROM users WHERE id = $1';
    const result = await pool.query(query, [id]);
    return result.rows[0];
  }
}

module.exports = UserModel;
```

### Step 5: Update API Routes

**Use the database model in routes:**
```javascript
// routes/userRoutes.js
const express = require('express');
const router = express.Router();
const UserModel = require('../models/userModel');

router.get('/', async (req, res) => {
  try {
    const users = await UserModel.getAllUsers();
    res.json({
      success: true,
      data: users,
      count: users.length
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: 'Failed to fetch users'
    });
  }
});

router.post('/', async (req, res) => {
  try {
    const { name, email } = req.body;

    if (!name || !email) {
      return res.status(400).json({
        success: false,
        error: 'Name and email are required'
      });
    }

    const newUser = await UserModel.createUser({ name, email });
    res.status(201).json({
      success: true,
      data: newUser,
      message: 'User created successfully'
    });
  } catch (error) {
    if (error.code === '23505') { // Unique constraint violation
      return res.status(409).json({
        success: false,
        error: 'Email already exists'
      });
    }

    res.status(500).json({
      success: false,
      error: 'Failed to create user'
    });
  }
});

router.get('/:id', async (req, res) => {
  try {
    const user = await UserModel.getUserById(req.params.id);

    if (!user) {
      return res.status(404).json({
        success: false,
        error: 'User not found'
      });
    }

    res.json({
      success: true,
      data: user
    });
  } catch (error) {
    res.status(500).json({
      success: false,
      error: 'Failed to fetch user'
    });
  }
});

module.exports = router;
```

**Update server.js:**
```javascript
const userRoutes = require('./routes/userRoutes');
// ... other middleware ...
app.use('/api/users', userRoutes);
```

### Step 6: Database Migration

**Create database initialization script:**
```sql
-- migrations/001_create_users_table.sql
CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Create index for better performance
CREATE INDEX IF NOT EXISTS idx_users_email ON users(email);
CREATE INDEX IF NOT EXISTS idx_users_created_at ON users(created_at);
```

**Run migration in Railway:**
1. **Go to your Railway project**
2. **Open the PostgreSQL database**
3. **Go to "Query" tab**
4. **Run the migration SQL**

### Step 7: Test Your Deployed API

**Test the endpoints:**
```bash
# Get health status
curl https://your-railway-app.railway.app/api/health

# Get all users
curl https://your-railway-app.railway.app/api/users

# Create a new user
curl -X POST https://your-railway-app.railway.app/api/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice Johnson", "email": "alice@example.com"}'
```

---

## Practice Project 3: Full Stack Deployment with Docker

### Project Overview
Containerize and deploy a full-stack application using Docker and Docker Compose.

### Step 1: Create Application Structure

**Set up the project:**
```bash
mkdir fullstack-docker
cd fullstack-docker

# Create directories
mkdir backend frontend database nginx
```

### Step 2: Backend API (Node.js)

**backend/package.json:**
```json
{
  "name": "backend",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "helmet": "^7.0.0",
    "morgan": "^1.10.0",
    "dotenv": "^16.3.1",
    "pg": "^8.11.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
```

**backend/server.js:**
```javascript
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');
require('dotenv').config();

const app = express();
const PORT = process.env.PORT || 5000;

// Middleware
app.use(helmet());
app.use(cors());
app.use(morgan('combined'));
app.use(express.json());

// Routes
app.get('/api', (req, res) => {
  res.json({
    message: 'Backend API is running! 🚀',
    environment: process.env.NODE_ENV,
    timestamp: new Date().toISOString()
  });
});

app.get('/api/posts', (req, res) => {
  // Mock data - replace with database queries
  const posts = [
    { id: 1, title: 'First Post', content: 'Hello Docker!' },
    { id: 2, title: 'Second Post', content: 'Containerized deployment' }
  ];

  res.json({ success: true, data: posts });
});

// Health check
app.get('/health', (req, res) => {
  res.json({ status: 'healthy', uptime: process.uptime() });
});

app.listen(PORT, () => {
  console.log(`🚀 Backend server running on port ${PORT}`);
});
```

**backend/Dockerfile:**
```dockerfile
FROM node:18-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy source code
COPY . .

# Create non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001

# Change ownership
RUN chown -R nextjs:nodejs /app
USER nextjs

EXPOSE 5000

CMD ["npm", "start"]
```

### Step 3: Frontend (React)

**frontend/package.json:**
```json
{
  "name": "frontend",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
}
```

**frontend/src/App.js:**
```jsx
import React, { useState, useEffect } from 'react';
import './App.css';

function App() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetchPosts();
  }, []);

  const fetchPosts = async () => {
    try {
      const response = await fetch('/api/posts');
      const data = await response.json();
      setPosts(data.data);
    } catch (err) {
      setError('Failed to fetch posts');
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="App">
      <header className="App-header">
        <h1>Docker Full Stack App 🐳</h1>
        <p>Frontend + Backend + Database</p>
      </header>

      <main className="App-main">
        <h2>Posts from API</h2>

        {loading && <p>Loading posts...</p>}
        {error && <p className="error">{error}</p>}

        <div className="posts-grid">
          {posts.map(post => (
            <div key={post.id} className="post-card">
              <h3>{post.title}</h3>
              <p>{post.content}</p>
            </div>
          ))}
        </div>
      </main>
    </div>
  );
}

export default App;
```

**frontend/Dockerfile:**
```dockerfile
FROM node:18-alpine as build

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine

# Copy built assets from build stage
COPY --from=build /app/build /usr/share/nginx/html

# Copy nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Step 4: Database (PostgreSQL)

**database/init.sql:**
```sql
-- Create database and user
CREATE DATABASE fullstack_app;
\c fullstack_app;

-- Create posts table
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    content TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Insert sample data
INSERT INTO posts (title, content) VALUES
('Welcome to Docker', 'This is a containerized full-stack application'),
('Microservices Architecture', 'Breaking down applications into smaller services'),
('DevOps Best Practices', 'Continuous integration and deployment');

-- Create indexes
CREATE INDEX idx_posts_created_at ON posts(created_at);
```

### Step 5: Nginx Reverse Proxy

**nginx/nginx.conf:**
```nginx
server {
    listen 80;
    server_name localhost;

    # Frontend
    location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
        try_files $uri $uri/ /index.html;
    }

    # Backend API
    location /api/ {
        proxy_pass http://backend:5000/api/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }

    # Health check
    location /health {
        proxy_pass http://backend:5000/health;
        access_log off;
    }

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Referrer-Policy "no-referrer-when-downgrade" always;
    add_header Content-Security-Policy "default-src 'self' http: https: data: blob: 'unsafe-inline'" always;
}
```

### Step 6: Docker Compose

**docker-compose.yml:**
```yaml
version: '3.8'

services:
  # Database
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: fullstack_app
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./database/init.sql:/docker-entrypoint-initdb.d/init.sql
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Backend API
  backend:
    build: ./backend
    environment:
      NODE_ENV: production
      PORT: 5000
      DATABASE_URL: postgresql://postgres:password@db:5432/fullstack_app
    ports:
      - "5000:5000"
    depends_on:
      db:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:5000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  # Frontend
  frontend:
    build: ./frontend
    ports:
      - "3000:80"
    depends_on:
      - backend
    environment:
      REACT_APP_API_URL: http://localhost:3000/api

  # Reverse Proxy
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - frontend
      - backend

volumes:
  postgres_data:
```

### Step 7: Environment Files

**backend/.env:**
```
NODE_ENV=production
PORT=5000
DATABASE_URL=postgresql://postgres:password@db:5432/fullstack_app
```

**frontend/.env.production:**
```
REACT_APP_API_URL=/api
```

### Step 8: Build and Run

**Build and start the application:**
```bash
# Build all services
docker-compose build

# Start all services
docker-compose up -d

# Check running containers
docker-compose ps

# View logs
docker-compose logs -f
```

**Test the application:**
```bash
# Test the full application
curl http://localhost

# Test the API directly
curl http://localhost/api

# Test database connectivity
curl http://localhost/api/posts
```

### Step 9: Deploy to Production

**Deploy to a cloud provider:**

1. **AWS ECS:**
   - Use AWS Fargate for serverless containers
   - Set up Application Load Balancer
   - Configure RDS for database

2. **Google Cloud Run:**
   - Containerize your application
   - Deploy to Cloud Run
   - Use Cloud SQL for database

3. **DigitalOcean App Platform:**
   - Connect your GitHub repository
   - Automatic deployments
   - Managed databases

---

## Additional Practice Exercises

### Exercise 1: CI/CD Pipeline
Set up GitHub Actions for automated testing and deployment.

### Exercise 2: Environment Management
Configure different environments (dev, staging, production).

### Exercise 3: Monitoring and Logging
Add application monitoring with health checks and log aggregation.

### Exercise 4: SSL Certificates
Configure HTTPS with Let's Encrypt certificates.

### Exercise 5: Load Balancing
Set up load balancing for high availability.

### Exercise 6: Database Backups
Implement automated database backups and recovery.

### Exercise 7: Security Hardening
Configure security groups, firewalls, and access controls.

### Exercise 8: Performance Optimization
Optimize application performance with caching and CDN.

### Exercise 9: Blue-Green Deployment
Implement blue-green deployment strategy for zero-downtime updates.

### Exercise 10: Disaster Recovery
Set up backup and recovery procedures for production environments.

Remember: Deployment is an ongoing process. Monitor your applications, gather feedback, and continuously improve your deployment practices!
