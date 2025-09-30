# Frontend Practice: Building User Interfaces

## Practice Project 1: Personal Portfolio Website

### Project Overview
Build a responsive personal portfolio website with HTML, CSS, and JavaScript.

### Step 1: Project Setup

**Create project structure:**
```bash
mkdir portfolio-website
cd portfolio-website

# Create files
touch index.html styles.css script.js
mkdir assets
mkdir assets/images
```

### Step 2: HTML Structure

**index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Portfolio</title>
    <link rel="stylesheet" href="styles.css">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
</head>
<body>
    <nav class="navbar">
        <div class="nav-container">
            <h1 class="logo">Portfolio</h1>
            <ul class="nav-menu">
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#projects">Projects</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
            <div class="hamburger">
                <span class="bar"></span>
                <span class="bar"></span>
                <span class="bar"></span>
            </div>
        </div>
    </nav>

    <section id="home" class="hero">
        <div class="hero-container">
            <div class="hero-content">
                <h2>Hi, I'm <span class="highlight">Your Name</span></h2>
                <h3>Full Stack Developer</h3>
                <p>I create beautiful and functional web applications</p>
                <div class="hero-buttons">
                    <a href="#projects" class="btn btn-primary">View My Work</a>
                    <a href="#contact" class="btn btn-secondary">Get In Touch</a>
                </div>
            </div>
            <div class="hero-image">
                <img src="assets/images/profile.jpg" alt="Profile" class="profile-img">
            </div>
        </div>
    </section>

    <section id="about" class="about">
        <div class="container">
            <h2>About Me</h2>
            <div class="about-content">
                <div class="about-text">
                    <p>I'm passionate about creating digital experiences that make a difference. With expertise in modern web technologies, I bring ideas to life through clean, efficient code.</p>
                    <div class="skills">
                        <span class="skill-tag">HTML</span>
                        <span class="skill-tag">CSS</span>
                        <span class="skill-tag">JavaScript</span>
                        <span class="skill-tag">React</span>
                        <span class="skill-tag">Node.js</span>
                    </div>
                </div>
                <div class="about-stats">
                    <div class="stat">
                        <h3>50+</h3>
                        <p>Projects Completed</p>
                    </div>
                    <div class="stat">
                        <h3>2+</h3>
                        <p>Years Experience</p>
                    </div>
                    <div class="stat">
                        <h3>100%</h3>
                        <p>Client Satisfaction</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section id="projects" class="projects">
        <div class="container">
            <h2>My Projects</h2>
            <div class="projects-grid" id="projects-grid">
                <!-- Projects will be loaded here -->
            </div>
        </div>
    </section>

    <section id="contact" class="contact">
        <div class="container">
            <h2>Get In Touch</h2>
            <div class="contact-content">
                <div class="contact-info">
                    <div class="contact-item">
                        <h3>Email</h3>
                        <p>your.email@example.com</p>
                    </div>
                    <div class="contact-item">
                        <h3>Location</h3>
                        <p>Your City, Country</p>
                    </div>
                    <div class="social-links">
                        <a href="#" class="social-link">LinkedIn</a>
                        <a href="#" class="social-link">GitHub</a>
                        <a href="#" class="social-link">Twitter</a>
                    </div>
                </div>
                <form class="contact-form" id="contact-form">
                    <div class="form-group">
                        <label for="name">Name</label>
                        <input type="text" id="name" name="name" required>
                    </div>
                    <div class="form-group">
                        <label for="email">Email</label>
                        <input type="email" id="email" name="email" required>
                    </div>
                    <div class="form-group">
                        <label for="message">Message</label>
                        <textarea id="message" name="message" rows="5" required></textarea>
                    </div>
                    <button type="submit" class="btn btn-primary">Send Message</button>
                </form>
            </div>
        </div>
    </section>

    <footer class="footer">
        <div class="container">
            <p>&copy; 2024 Your Name. All rights reserved.</p>
        </div>
    </footer>

    <script src="script.js"></script>
</body>
</html>
```

### Step 3: CSS Styling

**styles.css:**
```css
/* Reset and base styles */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Inter', sans-serif;
    line-height: 1.6;
    color: #333;
    overflow-x: hidden;
}

/* Container */
.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
}

/* Navigation */
.navbar {
    position: fixed;
    top: 0;
    width: 100%;
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    z-index: 1000;
    transition: all 0.3s ease;
}

.nav-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 80px;
}

.logo {
    font-size: 1.8rem;
    font-weight: 700;
    color: #2c3e50;
}

.nav-menu {
    display: flex;
    list-style: none;
    gap: 2rem;
}

.nav-menu a {
    text-decoration: none;
    color: #2c3e50;
    font-weight: 500;
    transition: color 0.3s ease;
}

.nav-menu a:hover {
    color: #3498db;
}

/* Hamburger menu for mobile */
.hamburger {
    display: none;
    cursor: pointer;
}

.bar {
    display: block;
    width: 25px;
    height: 3px;
    margin: 5px auto;
    background-color: #2c3e50;
    transition: all 0.3s ease;
}

/* Hero Section */
.hero {
    min-height: 100vh;
    display: flex;
    align-items: center;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding-top: 80px;
}

.hero-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: center;
}

.hero-content h2 {
    font-size: 3rem;
    margin-bottom: 1rem;
}

.highlight {
    color: #ffd700;
}

.hero-content h3 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
    opacity: 0.9;
}

.hero-content p {
    font-size: 1.2rem;
    margin-bottom: 2rem;
    opacity: 0.8;
}

.hero-buttons {
    display: flex;
    gap: 1rem;
}

.btn {
    padding: 12px 24px;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    text-decoration: none;
    display: inline-block;
}

.btn-primary {
    background: #3498db;
    color: white;
}

.btn-primary:hover {
    background: #2980b9;
    transform: translateY(-2px);
}

.btn-secondary {
    background: transparent;
    color: white;
    border: 2px solid white;
}

.btn-secondary:hover {
    background: white;
    color: #3498db;
}

.profile-img {
    width: 300px;
    height: 300px;
    border-radius: 50%;
    object-fit: cover;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

/* About Section */
.about {
    padding: 100px 0;
    background: #f8f9fa;
}

.about h2 {
    text-align: center;
    margin-bottom: 3rem;
    font-size: 2.5rem;
    color: #2c3e50;
}

.about-content {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: 3rem;
    align-items: center;
}

.about-text p {
    font-size: 1.2rem;
    margin-bottom: 2rem;
    line-height: 1.8;
}

.skills {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
}

.skill-tag {
    background: #3498db;
    color: white;
    padding: 8px 16px;
    border-radius: 20px;
    font-size: 0.9rem;
    font-weight: 500;
}

.about-stats {
    display: grid;
    grid-template-columns: 1fr;
    gap: 2rem;
}

.stat {
    text-align: center;
    padding: 2rem;
    background: white;
    border-radius: 12px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

.stat h3 {
    font-size: 2.5rem;
    color: #3498db;
    margin-bottom: 0.5rem;
}

.stat p {
    color: #7f8c8d;
    font-weight: 500;
}

/* Projects Section */
.projects {
    padding: 100px 0;
    background: white;
}

.projects h2 {
    text-align: center;
    margin-bottom: 3rem;
    font-size: 2.5rem;
    color: #2c3e50;
}

.projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
    gap: 2rem;
}

.project-card {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
}

.project-card:hover {
    transform: translateY(-5px);
}

.project-image {
    height: 200px;
    background: linear-gradient(45deg, #3498db, #2980b9);
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-size: 3rem;
}

.project-content {
    padding: 1.5rem;
}

.project-content h3 {
    margin-bottom: 0.5rem;
    color: #2c3e50;
}

.project-content p {
    color: #7f8c8d;
    margin-bottom: 1rem;
    line-height: 1.6;
}

.project-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
    margin-bottom: 1rem;
}

.project-tag {
    background: #ecf0f1;
    color: #2c3e50;
    padding: 4px 8px;
    border-radius: 12px;
    font-size: 0.8rem;
}

.project-links {
    display: flex;
    gap: 1rem;
}

.project-links a {
    color: #3498db;
    text-decoration: none;
    font-weight: 500;
}

.project-links a:hover {
    text-decoration: underline;
}

/* Contact Section */
.contact {
    padding: 100px 0;
    background: #f8f9fa;
}

.contact h2 {
    text-align: center;
    margin-bottom: 3rem;
    font-size: 2.5rem;
    color: #2c3e50;
}

.contact-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
}

.contact-info {
    display: flex;
    flex-direction: column;
    gap: 2rem;
}

.contact-item h3 {
    color: #2c3e50;
    margin-bottom: 0.5rem;
}

.contact-item p {
    color: #7f8c8d;
    font-size: 1.1rem;
}

.social-links {
    display: flex;
    gap: 1rem;
    margin-top: 1rem;
}

.social-link {
    color: #3498db;
    text-decoration: none;
    font-weight: 500;
    padding: 8px 16px;
    border: 2px solid #3498db;
    border-radius: 25px;
    transition: all 0.3s ease;
}

.social-link:hover {
    background: #3498db;
    color: white;
}

/* Contact Form */
.contact-form {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
}

.form-group {
    display: flex;
    flex-direction: column;
}

.form-group label {
    margin-bottom: 0.5rem;
    font-weight: 500;
    color: #2c3e50;
}

.form-group input,
.form-group textarea {
    padding: 12px;
    border: 2px solid #ecf0f1;
    border-radius: 8px;
    font-size: 1rem;
    font-family: inherit;
    transition: border-color 0.3s ease;
}

.form-group input:focus,
.form-group textarea:focus {
    outline: none;
    border-color: #3498db;
}

/* Footer */
.footer {
    background: #2c3e50;
    color: white;
    padding: 2rem 0;
    text-align: center;
}

.footer p {
    opacity: 0.8;
}

/* Responsive Design */
@media (max-width: 768px) {
    .nav-menu {
        position: fixed;
        left: -100%;
        top: 70px;
        flex-direction: column;
        background-color: white;
        width: 100%;
        text-align: center;
        transition: 0.3s;
        box-shadow: 0 10px 27px rgba(0, 0, 0, 0.05);
        padding: 2rem 0;
    }

    .nav-menu.active {
        left: 0;
    }

    .hamburger {
        display: block;
    }

    .hero-container {
        grid-template-columns: 1fr;
        text-align: center;
        gap: 2rem;
    }

    .hero-content h2 {
        font-size: 2rem;
    }

    .hero-buttons {
        justify-content: center;
    }

    .about-content,
    .contact-content {
        grid-template-columns: 1fr;
        gap: 2rem;
    }

    .projects-grid {
        grid-template-columns: 1fr;
    }

    .about-stats {
        grid-template-columns: repeat(3, 1fr);
    }
}

@media (max-width: 480px) {
    .container {
        padding: 0 15px;
    }

    .hero-content h2 {
        font-size: 1.8rem;
    }

    .about-stats {
        grid-template-columns: 1fr;
    }

    .hero-buttons {
        flex-direction: column;
        align-items: center;
    }

    .btn {
        width: 200px;
    }
}
```

### Step 4: JavaScript Functionality

**script.js:**
```javascript
// Sample projects data
const projects = [
    {
        title: "E-commerce Website",
        description: "A full-featured online store with shopping cart, user authentication, and payment integration.",
        tags: ["React", "Node.js", "MongoDB", "Stripe"],
        demoLink: "#",
        codeLink: "#"
    },
    {
        title: "Task Management App",
        description: "A collaborative project management tool with real-time updates and team collaboration features.",
        tags: ["Vue.js", "Express", "PostgreSQL", "Socket.io"],
        demoLink: "#",
        codeLink: "#"
    },
    {
        title: "Weather Dashboard",
        description: "A responsive weather application with location-based forecasts and interactive maps.",
        tags: ["JavaScript", "OpenWeather API", "Chart.js"],
        demoLink: "#",
        codeLink: "#"
    },
    {
        title: "Social Media Platform",
        description: "A microblogging platform with real-time feeds, likes, comments, and user profiles.",
        tags: ["React", "Firebase", "Tailwind CSS"],
        demoLink: "#",
        codeLink: "#"
    },
    {
        title: "Learning Management System",
        description: "An educational platform with course management, progress tracking, and interactive quizzes.",
        tags: ["Next.js", "Prisma", "PostgreSQL", "Stripe"],
        demoLink: "#",
        codeLink: "#"
    },
    {
        title: "Portfolio Website",
        description: "A responsive personal portfolio with project showcase, blog, and contact functionality.",
        tags: ["HTML", "CSS", "JavaScript", "Responsive Design"],
        demoLink: "#",
        codeLink: "#"
    }
];

// Load projects dynamically
function loadProjects() {
    const projectsGrid = document.getElementById('projects-grid');

    projects.forEach(project => {
        const projectCard = document.createElement('div');
        projectCard.className = 'project-card';

        projectCard.innerHTML = `
            <div class="project-image">
                <span>${project.title.charAt(0)}</span>
            </div>
            <div class="project-content">
                <h3>${project.title}</h3>
                <p>${project.description}</p>
                <div class="project-tags">
                    ${project.tags.map(tag => `<span class="project-tag">${tag}</span>`).join('')}
                </div>
                <div class="project-links">
                    <a href="${project.demoLink}" target="_blank">Live Demo</a>
                    <a href="${project.codeLink}" target="_blank">View Code</a>
                </div>
            </div>
        `;

        projectsGrid.appendChild(projectCard);
    });
}

// Contact form handling
function handleContactForm() {
    const form = document.getElementById('contact-form');

    form.addEventListener('submit', function(e) {
        e.preventDefault();

        const formData = new FormData(form);
        const data = Object.fromEntries(formData);

        // Simulate form submission
        console.log('Form submitted:', data);

        // Show success message
        showMessage('Thank you for your message! I\'ll get back to you soon.', 'success');

        // Reset form
        form.reset();
    });
}

// Show notification messages
function showMessage(message, type) {
    // Remove existing messages
    const existingMessage = document.querySelector('.message');
    if (existingMessage) {
        existingMessage.remove();
    }

    // Create new message
    const messageDiv = document.createElement('div');
    messageDiv.className = `message ${type}`;
    messageDiv.textContent = message;

    // Style the message
    messageDiv.style.cssText = `
        position: fixed;
        top: 100px;
        right: 20px;
        padding: 15px 20px;
        border-radius: 8px;
        color: white;
        font-weight: 500;
        z-index: 1000;
        animation: slideIn 0.3s ease;
    `;

    if (type === 'success') {
        messageDiv.style.backgroundColor = '#27ae60';
    } else {
        messageDiv.style.backgroundColor = '#e74c3c';
    }

    document.body.appendChild(messageDiv);

    // Remove message after 5 seconds
    setTimeout(() => {
        messageDiv.remove();
    }, 5000);
}

// Mobile navigation toggle
function setupMobileNav() {
    const hamburger = document.querySelector('.hamburger');
    const navMenu = document.querySelector('.nav-menu');

    hamburger.addEventListener('click', function() {
        navMenu.classList.toggle('active');
    });

    // Close mobile menu when clicking on a link
    document.querySelectorAll('.nav-menu a').forEach(link => {
        link.addEventListener('click', () => {
            navMenu.classList.remove('active');
        });
    });
}

// Smooth scrolling for navigation links
function setupSmoothScrolling() {
    document.querySelectorAll('a[href^="#"]').forEach(anchor => {
        anchor.addEventListener('click', function (e) {
            e.preventDefault();

            const target = document.querySelector(this.getAttribute('href'));
            if (target) {
                target.scrollIntoView({
                    behavior: 'smooth',
                    block: 'start'
                });
            }
        });
    });
}

// Add scroll effects
function setupScrollEffects() {
    const navbar = document.querySelector('.navbar');

    window.addEventListener('scroll', function() {
        if (window.scrollY > 100) {
            navbar.style.background = 'rgba(255, 255, 255, 0.98)';
            navbar.style.boxShadow = '0 2px 10px rgba(0, 0, 0, 0.1)';
        } else {
            navbar.style.background = 'rgba(255, 255, 255, 0.95)';
            navbar.style.boxShadow = 'none';
        }
    });
}

// Initialize everything when DOM is loaded
document.addEventListener('DOMContentLoaded', function() {
    loadProjects();
    handleContactForm();
    setupMobileNav();
    setupSmoothScrolling();
    setupScrollEffects();
});

// Add CSS animations
const style = document.createElement('style');
style.textContent = `
    @keyframes slideIn {
        from {
            transform: translateX(100%);
            opacity: 0;
        }
        to {
            transform: translateX(0);
            opacity: 1;
        }
    }

    .project-card {
        animation: fadeInUp 0.6s ease forwards;
        opacity: 0;
        transform: translateY(30px);
    }

    .project-card:nth-child(1) { animation-delay: 0.1s; }
    .project-card:nth-child(2) { animation-delay: 0.2s; }
    .project-card:nth-child(3) { animation-delay: 0.3s; }
    .project-card:nth-child(4) { animation-delay: 0.4s; }
    .project-card:nth-child(5) { animation-delay: 0.5s; }
    .project-card:nth-child(6) { animation-delay: 0.6s; }

    @keyframes fadeInUp {
        to {
            opacity: 1;
            transform: translateY(0);
        }
    }
`;
document.head.appendChild(style);
```

### Step 5: Testing the Website

1. **Open index.html in your browser**
2. **Test responsiveness**: Resize your browser window
3. **Test navigation**: Click navigation links
4. **Test contact form**: Fill out and submit the form
5. **Test mobile view**: Use browser dev tools

---

## Practice Project 2: Interactive Todo Application

### Project Overview
Build a feature-rich todo application with local storage persistence.

### Step 1: HTML Structure

**todo.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todo App</title>
    <link rel="stylesheet" href="todo.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>📝 My Todo List</h1>
            <p>Stay organized and productive</p>
        </header>

        <form id="todo-form" class="todo-form">
            <div class="input-group">
                <input
                    type="text"
                    id="todo-input"
                    placeholder="What needs to be done?"
                    maxlength="100"
                    required
                >
                <select id="priority-select">
                    <option value="low">Low Priority</option>
                    <option value="medium" selected>Medium Priority</option>
                    <option value="high">High Priority</option>
                </select>
                <button type="submit" class="add-btn">Add Task</button>
            </div>
        </form>

        <div class="filters">
            <button class="filter-btn active" data-filter="all">All</button>
            <button class="filter-btn" data-filter="active">Active</button>
            <button class="filter-btn" data-filter="completed">Completed</button>
        </div>

        <div class="stats">
            <span id="total-tasks">0 total</span>
            <span id="active-tasks">0 active</span>
            <span id="completed-tasks">0 completed</span>
        </div>

        <ul id="todo-list" class="todo-list">
            <!-- Todos will be inserted here -->
        </ul>

        <div id="empty-state" class="empty-state">
            <p>🎯 No tasks yet. Add one above to get started!</p>
        </div>
    </div>

    <script src="todo.js"></script>
</body>
</html>
```

### Step 2: CSS Styling

**todo.css:**
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    color: #333;
}

.container {
    max-width: 600px;
    margin: 0 auto;
    padding: 2rem 1rem;
}

/* Header */
header {
    text-align: center;
    margin-bottom: 2rem;
    color: white;
}

header h1 {
    font-size: 2.5rem;
    margin-bottom: 0.5rem;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

header p {
    font-size: 1.1rem;
    opacity: 0.9;
}

/* Form */
.todo-form {
    background: white;
    padding: 2rem;
    border-radius: 12px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
    margin-bottom: 2rem;
}

.input-group {
    display: flex;
    gap: 1rem;
    align-items: center;
}

#todo-input {
    flex: 1;
    padding: 12px 16px;
    border: 2px solid #e1e5e9;
    border-radius: 8px;
    font-size: 1rem;
    transition: border-color 0.3s ease;
}

#todo-input:focus {
    outline: none;
    border-color: #667eea;
}

#priority-select {
    padding: 12px;
    border: 2px solid #e1e5e9;
    border-radius: 8px;
    background: white;
    font-size: 0.9rem;
}

.add-btn {
    padding: 12px 24px;
    background: #667eea;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
}

.add-btn:hover {
    background: #5a6fd8;
    transform: translateY(-2px);
}

/* Filters */
.filters {
    display: flex;
    justify-content: center;
    gap: 1rem;
    margin-bottom: 1.5rem;
}

.filter-btn {
    padding: 8px 16px;
    border: 2px solid #e1e5e9;
    background: white;
    border-radius: 20px;
    cursor: pointer;
    font-size: 0.9rem;
    transition: all 0.3s ease;
}

.filter-btn.active {
    background: #667eea;
    color: white;
    border-color: #667eea;
}

.filter-btn:hover {
    border-color: #667eea;
}

/* Stats */
.stats {
    display: flex;
    justify-content: space-between;
    margin-bottom: 1.5rem;
    font-size: 0.9rem;
    color: rgba(255, 255, 255, 0.8);
}

/* Todo List */
.todo-list {
    list-style: none;
}

.todo-item {
    display: flex;
    align-items: center;
    background: white;
    padding: 1rem;
    margin-bottom: 0.5rem;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
    animation: slideIn 0.3s ease;
}

.todo-item.completed {
    opacity: 0.6;
    text-decoration: line-through;
}

.todo-item.completed .todo-text {
    text-decoration: line-through;
}

.checkbox {
    width: 20px;
    height: 20px;
    margin-right: 1rem;
    cursor: pointer;
}

.todo-content {
    flex: 1;
    display: flex;
    align-items: center;
    gap: 1rem;
}

.todo-text {
    flex: 1;
    font-size: 1rem;
}

.priority-badge {
    padding: 4px 8px;
    border-radius: 12px;
    font-size: 0.8rem;
    font-weight: 600;
    text-transform: uppercase;
}

.priority-low {
    background: #d4edda;
    color: #155724;
}

.priority-medium {
    background: #fff3cd;
    color: #856404;
}

.priority-high {
    background: #f8d7da;
    color: #721c24;
}

.timestamp {
    font-size: 0.8rem;
    color: #6c757d;
    margin-left: auto;
}

.delete-btn {
    background: #dc3545;
    color: white;
    border: none;
    padding: 6px 12px;
    border-radius: 4px;
    cursor: pointer;
    font-size: 0.8rem;
    transition: background 0.3s ease;
}

.delete-btn:hover {
    background: #c82333;
}

/* Empty State */
.empty-state {
    text-align: center;
    color: white;
    margin-top: 3rem;
}

.empty-state p {
    font-size: 1.2rem;
    opacity: 0.8;
}

/* Animations */
@keyframes slideIn {
    from {
        opacity: 0;
        transform: translateY(-10px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

/* Responsive Design */
@media (max-width: 768px) {
    .container {
        padding: 1rem;
    }

    .input-group {
        flex-direction: column;
        gap: 0.5rem;
    }

    #todo-input,
    #priority-select,
    .add-btn {
        width: 100%;
    }

    .filters {
        flex-wrap: wrap;
    }

    .stats {
        flex-direction: column;
        gap: 0.5rem;
        text-align: center;
    }

    .todo-item {
        flex-direction: column;
        align-items: flex-start;
        gap: 0.5rem;
    }

    .todo-content {
        width: 100%;
    }

    .timestamp {
        margin-left: 0;
        font-size: 0.7rem;
    }
}
```

### Step 3: JavaScript Functionality

**todo.js:**
```javascript
// Todo Application Class
class TodoApp {
    constructor() {
        this.todos = this.loadTodos();
        this.currentFilter = 'all';
        this.init();
    }

    init() {
        this.bindEvents();
        this.render();
        this.updateStats();
    }

    bindEvents() {
        // Form submission
        document.getElementById('todo-form').addEventListener('submit', (e) => {
            e.preventDefault();
            this.addTodo();
        });

        // Filter buttons
        document.querySelectorAll('.filter-btn').forEach(btn => {
            btn.addEventListener('click', (e) => {
                this.setFilter(e.target.dataset.filter);
            });
        });

        // Dynamic event delegation for todo items
        document.getElementById('todo-list').addEventListener('click', (e) => {
            const todoItem = e.target.closest('.todo-item');
            if (!todoItem) return;

            const todoId = parseInt(todoItem.dataset.id);

            if (e.target.classList.contains('checkbox')) {
                this.toggleTodo(todoId);
            } else if (e.target.classList.contains('delete-btn')) {
                this.deleteTodo(todoId);
            }
        });
    }

    addTodo() {
        const input = document.getElementById('todo-input');
        const prioritySelect = document.getElementById('priority-select');

        const text = input.value.trim();
        if (!text) return;

        const todo = {
            id: Date.now(),
            text: text,
            completed: false,
            priority: prioritySelect.value,
            createdAt: new Date().toISOString()
        };

        this.todos.push(todo);
        this.saveTodos();

        input.value = '';
        this.render();
        this.updateStats();
    }

    toggleTodo(id) {
        const todo = this.todos.find(t => t.id === id);
        if (todo) {
            todo.completed = !todo.completed;
            this.saveTodos();
            this.render();
            this.updateStats();
        }
    }

    deleteTodo(id) {
        this.todos = this.todos.filter(t => t.id !== id);
        this.saveTodos();
        this.render();
        this.updateStats();
    }

    setFilter(filter) {
        this.currentFilter = filter;

        // Update active filter button
        document.querySelectorAll('.filter-btn').forEach(btn => {
            btn.classList.toggle('active', btn.dataset.filter === filter);
        });

        this.render();
    }

    getFilteredTodos() {
        switch (this.currentFilter) {
            case 'active':
                return this.todos.filter(todo => !todo.completed);
            case 'completed':
                return this.todos.filter(todo => todo.completed);
            default:
                return this.todos;
        }
    }

    render() {
        const todoList = document.getElementById('todo-list');
        const filteredTodos = this.getFilteredTodos();

        if (filteredTodos.length === 0) {
            todoList.innerHTML = '';
            document.getElementById('empty-state').style.display =
                this.todos.length === 0 ? 'block' : 'none';
            return;
        }

        document.getElementById('empty-state').style.display = 'none';

        todoList.innerHTML = filteredTodos
            .sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
            .map(todo => `
                <li class="todo-item ${todo.completed ? 'completed' : ''}" data-id="${todo.id}">
                    <input type="checkbox" class="checkbox" ${todo.completed ? 'checked' : ''}>
                    <div class="todo-content">
                        <span class="todo-text">${this.escapeHtml(todo.text)}</span>
                        <span class="priority-badge priority-${todo.priority}">
                            ${todo.priority}
                        </span>
                        <span class="timestamp">
                            ${this.formatDate(todo.createdAt)}
                        </span>
                    </div>
                    <button class="delete-btn">Delete</button>
                </li>
            `).join('');
    }

    updateStats() {
        const total = this.todos.length;
        const active = this.todos.filter(t => !t.completed).length;
        const completed = this.todos.filter(t => t.completed).length;

        document.getElementById('total-tasks').textContent = `${total} total`;
        document.getElementById('active-tasks').textContent = `${active} active`;
        document.getElementById('completed-tasks').textContent = `${completed} completed`;
    }

    saveTodos() {
        localStorage.setItem('todos', JSON.stringify(this.todos));
    }

    loadTodos() {
        const todos = localStorage.getItem('todos');
        return todos ? JSON.parse(todos) : [];
    }

    escapeHtml(text) {
        const div = document.createElement('div');
        div.textContent = text;
        return div.innerHTML;
    }

    formatDate(dateString) {
        const date = new Date(dateString);
        const now = new Date();
        const diffInHours = Math.floor((now - date) / (1000 * 60 * 60));

        if (diffInHours < 1) return 'Just now';
        if (diffInHours < 24) return `${diffInHours}h ago`;

        const diffInDays = Math.floor(diffInHours / 24);
        if (diffInDays < 7) return `${diffInDays}d ago`;

        return date.toLocaleDateString();
    }
}

// Initialize the application
document.addEventListener('DOMContentLoaded', () => {
    new TodoApp();
});
```

### Step 4: Testing the Todo App

1. **Open todo.html in your browser**
2. **Add several todos with different priorities**
3. **Mark some as completed**
4. **Test filtering (All, Active, Completed)**
5. **Delete some todos**
6. **Refresh the page** (todos should persist)
7. **Test on mobile** (responsive design)

---

## Practice Project 3: Weather Dashboard

### Project Overview
Build a weather dashboard that shows current weather and forecasts using a weather API.

### Step 1: Sign up for Weather API

1. **Go to** https://openweathermap.org/api
2. **Sign up for a free account**
3. **Get your API key**

### Step 2: HTML Structure

**weather.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather Dashboard</title>
    <link rel="stylesheet" href="weather.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>🌤️ Weather Dashboard</h1>
            <div class="search-container">
                <input
                    type="text"
                    id="city-input"
                    placeholder="Enter city name..."
                    autocomplete="off"
                >
                <button id="search-btn">Search</button>
                <button id="location-btn" title="Use my location">📍</button>
            </div>
        </header>

        <main>
            <div id="loading" class="loading hidden">
                <div class="spinner"></div>
                <p>Loading weather data...</p>
            </div>

            <div id="error" class="error hidden">
                <h3>❌ Error</h3>
                <p id="error-message"></p>
                <button id="retry-btn">Try Again</button>
            </div>

            <div id="weather-container" class="weather-container hidden">
                <!-- Current Weather -->
                <div class="current-weather">
                    <div class="location">
                        <h2 id="city-name"></h2>
                        <p id="country"></p>
                    </div>
                    <div class="temperature">
                        <span id="temp"></span>
                        <span class="unit">°C</span>
                    </div>
                    <div class="weather-info">
                        <img id="weather-icon" src="" alt="Weather icon">
                        <div class="weather-details">
                            <p id="description"></p>
                            <p>Feels like <span id="feels-like"></span>°C</p>
                        </div>
                    </div>
                </div>

                <!-- Weather Details -->
                <div class="weather-details-grid">
                    <div class="detail-card">
                        <h4>💨 Wind</h4>
                        <p id="wind-speed"></p>
                        <small id="wind-direction"></small>
                    </div>
                    <div class="detail-card">
                        <h4>💧 Humidity</h4>
                        <p id="humidity"></p>
                    </div>
                    <div class="detail-card">
                        <h4>👁️ Visibility</h4>
                        <p id="visibility"></p>
                    </div>
                    <div class="detail-card">
                        <h4>📊 Pressure</h4>
                        <p id="pressure"></p>
                    </div>
                </div>

                <!-- 5-Day Forecast -->
                <div class="forecast">
                    <h3>5-Day Forecast</h3>
                    <div id="forecast-container" class="forecast-container">
                        <!-- Forecast cards will be inserted here -->
                    </div>
                </div>
            </div>
        </main>
    </div>

    <script src="weather.js"></script>
</body>
</html>
```

### Step 3: CSS Styling

**weather.css:**
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    color: #333;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem 1rem;
}

/* Header */
header {
    text-align: center;
    margin-bottom: 2rem;
    color: white;
}

header h1 {
    font-size: 2.5rem;
    margin-bottom: 1.5rem;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.search-container {
    display: flex;
    justify-content: center;
    gap: 1rem;
    margin-bottom: 1rem;
}

#city-input {
    padding: 12px 16px;
    border: none;
    border-radius: 25px;
    font-size: 1rem;
    width: 300px;
    outline: none;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

#search-btn, #location-btn {
    padding: 12px 20px;
    border: none;
    border-radius: 25px;
    background: #fff;
    color: #667eea;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

#search-btn:hover, #location-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

/* Loading */
.loading, .error {
    text-align: center;
    color: white;
    padding: 3rem;
}

.spinner {
    width: 50px;
    height: 50px;
    border: 4px solid rgba(255, 255, 255, 0.3);
    border-top: 4px solid white;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin: 0 auto 1rem;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

.error {
    background: rgba(231, 76, 60, 0.9);
    border-radius: 12px;
    margin: 2rem 0;
}

.error h3 {
    margin-bottom: 1rem;
}

#retry-btn {
    margin-top: 1rem;
    padding: 10px 20px;
    background: white;
    color: #e74c3c;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 600;
}

/* Weather Container */
.weather-container {
    background: white;
    border-radius: 16px;
    padding: 2rem;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

/* Current Weather */
.current-weather {
    display: grid;
    grid-template-columns: 1fr auto 1fr;
    align-items: center;
    margin-bottom: 2rem;
    gap: 2rem;
}

.location h2 {
    font-size: 2rem;
    color: #2c3e50;
    margin-bottom: 0.5rem;
}

.location p {
    color: #7f8c8d;
    font-size: 1.1rem;
}

.temperature {
    text-align: center;
}

.temperature span {
    font-size: 4rem;
    font-weight: 300;
    color: #2c3e50;
}

.unit {
    font-size: 2rem;
    vertical-align: top;
}

.weather-info {
    display: flex;
    align-items: center;
    gap: 1rem;
}

#weather-icon {
    width: 80px;
    height: 80px;
}

.weather-details p:first-child {
    font-size: 1.5rem;
    font-weight: 600;
    color: #2c3e50;
    text-transform: capitalize;
}

.weather-details p:last-child {
    color: #7f8c8d;
}

/* Weather Details Grid */
.weather-details-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
    margin-bottom: 2rem;
}

.detail-card {
    background: #f8f9fa;
    padding: 1.5rem;
    border-radius: 12px;
    text-align: center;
    border-left: 4px solid #667eea;
}

.detail-card h4 {
    margin-bottom: 0.5rem;
    color: #2c3e50;
}

.detail-card p {
    font-size: 1.2rem;
    font-weight: 600;
    color: #667eea;
}

.detail-card small {
    color: #7f8c8d;
    font-size: 0.9rem;
}

/* Forecast */
.forecast h3 {
    margin-bottom: 1rem;
    color: #2c3e50;
    font-size: 1.5rem;
}

.forecast-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 1rem;
}

.forecast-card {
    background: #f8f9fa;
    padding: 1rem;
    border-radius: 12px;
    text-align: center;
    transition: transform 0.3s ease;
}

.forecast-card:hover {
    transform: translateY(-2px);
}

.forecast-date {
    font-weight: 600;
    color: #2c3e50;
    margin-bottom: 0.5rem;
}

.forecast-icon {
    width: 50px;
    height: 50px;
    margin: 0.5rem 0;
}

.forecast-temp {
    font-size: 1.2rem;
    font-weight: 600;
    color: #667eea;
    margin-bottom: 0.5rem;
}

.forecast-desc {
    font-size: 0.9rem;
    color: #7f8c8d;
    text-transform: capitalize;
}

/* Responsive Design */
@media (max-width: 768px) {
    .container {
        padding: 1rem;
    }

    header h1 {
        font-size: 2rem;
    }

    .search-container {
        flex-direction: column;
        align-items: center;
        gap: 0.5rem;
    }

    #city-input {
        width: 100%;
        max-width: 300px;
    }

    .current-weather {
        grid-template-columns: 1fr;
        text-align: center;
        gap: 1rem;
    }

    .weather-info {
        justify-content: center;
    }

    .weather-details-grid {
        grid-template-columns: 1fr;
    }

    .forecast-container {
        grid-template-columns: repeat(3, 1fr);
    }
}

@media (max-width: 480px) {
    .forecast-container {
        grid-template-columns: 1fr;
    }

    .temperature span {
        font-size: 3rem;
    }

    .unit {
        font-size: 1.5rem;
    }
}

/* Hidden class */
.hidden {
    display: none !important;
}
```

### Step 4: JavaScript with Weather API

**weather.js:**
```javascript
// Weather Dashboard Application
class WeatherDashboard {
    constructor() {
        this.apiKey = 'YOUR_API_KEY_HERE'; // Replace with your OpenWeatherMap API key
        this.currentWeather = null;
        this.init();
    }

    init() {
        this.bindEvents();
        // Try to get user's location on load
        this.getCurrentLocation();
    }

    bindEvents() {
        const searchBtn = document.getElementById('search-btn');
        const locationBtn = document.getElementById('location-btn');
        const cityInput = document.getElementById('city-input');
        const retryBtn = document.getElementById('retry-btn');

        searchBtn.addEventListener('click', () => this.searchWeather());
        locationBtn.addEventListener('click', () => this.getCurrentLocation());
        retryBtn.addEventListener('click', () => this.retryLastSearch());

        cityInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                this.searchWeather();
            }
        });
    }

    async searchWeather() {
        const cityInput = document.getElementById('city-input');
        const city = cityInput.value.trim();

        if (!city) {
            this.showError('Please enter a city name');
            return;
        }

        await this.fetchWeather(city);
    }

    async getCurrentLocation() {
        if (!navigator.geolocation) {
            this.showError('Geolocation is not supported by this browser');
            return;
        }

        this.showLoading();

        navigator.geolocation.getCurrentPosition(
            async (position) => {
                const { latitude, longitude } = position.coords;
                await this.fetchWeatherByCoords(latitude, longitude);
            },
            (error) => {
                this.hideLoading();
                this.showError('Unable to get your location. Please search for a city instead.');
            }
        );
    }

    async fetchWeather(query) {
        this.showLoading();

        try {
            const response = await fetch(
                `https://api.openweathermap.org/data/2.5/weather?q=${query}&appid=${this.apiKey}&units=metric`
            );

            if (!response.ok) {
                throw new Error(response.status === 404 ? 'City not found' : 'Failed to fetch weather data');
            }

            const data = await response.json();
            this.currentWeather = data;
            await this.fetchForecast(data.coord.lat, data.coord.lon);
            this.displayWeather(data);

        } catch (error) {
            this.showError(error.message);
        }
    }

    async fetchWeatherByCoords(lat, lon) {
        try {
            const response = await fetch(
                `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${this.apiKey}&units=metric`
            );

            if (!response.ok) {
                throw new Error('Failed to fetch weather data');
            }

            const data = await response.json();
            this.currentWeather = data;
            await this.fetchForecast(lat, lon);
            this.displayWeather(data);

        } catch (error) {
            this.showError(error.message);
        }
    }

    async fetchForecast(lat, lon) {
        try {
            const response = await fetch(
                `https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&appid=${this.apiKey}&units=metric`
            );

            if (response.ok) {
                const data = await response.json();
                this.displayForecast(data);
            }
        } catch (error) {
            console.warn('Failed to fetch forecast data');
        }
    }

    displayWeather(data) {
        // Update current weather
        document.getElementById('city-name').textContent = data.name;
        document.getElementById('country').textContent = data.sys.country;
        document.getElementById('temp').textContent = Math.round(data.main.temp);
        document.getElementById('feels-like').textContent = Math.round(data.main.feels_like);
        document.getElementById('description').textContent = data.weather[0].description;
        document.getElementById('humidity').textContent = `${data.main.humidity}%`;
        document.getElementById('pressure').textContent = `${data.main.pressure} hPa`;
        document.getElementById('visibility').textContent = `${(data.visibility / 1000).toFixed(1)} km`;

        // Wind information
        const windSpeed = (data.wind.speed * 3.6).toFixed(1); // Convert m/s to km/h
        document.getElementById('wind-speed').textContent = `${windSpeed} km/h`;

        const windDirection = this.getWindDirection(data.wind.deg);
        document.getElementById('wind-direction').textContent = windDirection;

        // Weather icon
        const iconCode = data.weather[0].icon;
        document.getElementById('weather-icon').src = `https://openweathermap.org/img/wn/${iconCode}@2x.png`;

        this.hideLoading();
        this.showWeather();
    }

    displayForecast(data) {
        const forecastContainer = document.getElementById('forecast-container');
        forecastContainer.innerHTML = '';

        // Group forecast by day (take one reading per day)
        const dailyForecasts = {};
        data.list.forEach(item => {
            const date = new Date(item.dt * 1000);
            const dayKey = date.toDateString();

            if (!dailyForecasts[dayKey]) {
                dailyForecasts[dayKey] = item;
            }
        });

        // Display next 5 days
        Object.values(dailyForecasts).slice(0, 5).forEach(item => {
            const date = new Date(item.dt * 1000);
            const dayName = date.toLocaleDateString('en-US', { weekday: 'short' });

            const forecastCard = document.createElement('div');
            forecastCard.className = 'forecast-card';

            forecastCard.innerHTML = `
                <div class="forecast-date">${dayName}</div>
                <img class="forecast-icon" src="https://openweathermap.org/img/wn/${item.weather[0].icon}.png" alt="${item.weather[0].description}">
                <div class="forecast-temp">${Math.round(item.main.temp)}°C</div>
                <div class="forecast-desc">${item.weather[0].description}</div>
            `;

            forecastContainer.appendChild(forecastCard);
        });
    }

    getWindDirection(degrees) {
        const directions = ['N', 'NNE', 'NE', 'ENE', 'E', 'ESE', 'SE', 'SSE', 'S', 'SSW', 'SW', 'WSW', 'W', 'WNW', 'NW', 'NNW'];
        const index = Math.round(degrees / 22.5) % 16;
        return directions[index];
    }

    showLoading() {
        document.getElementById('loading').classList.remove('hidden');
        document.getElementById('error').classList.add('hidden');
        document.getElementById('weather-container').classList.add('hidden');
    }

    hideLoading() {
        document.getElementById('loading').classList.add('hidden');
    }

    showWeather() {
        document.getElementById('weather-container').classList.remove('hidden');
    }

    showError(message) {
        document.getElementById('error-message').textContent = message;
        document.getElementById('error').classList.remove('hidden');
        document.getElementById('loading').classList.add('hidden');
        document.getElementById('weather-container').classList.add('hidden');
    }

    retryLastSearch() {
        const cityInput = document.getElementById('city-input');
        if (cityInput.value.trim()) {
            this.searchWeather();
        } else {
            this.getCurrentLocation();
        }
    }
}

// Initialize the weather dashboard
document.addEventListener('DOMContentLoaded', () => {
    new WeatherDashboard();
});
```

### Step 5: Setup Instructions

1. **Get API Key:**
   - Sign up at https://openweathermap.org/api
   - Get your free API key
   - Replace `YOUR_API_KEY_HERE` in weather.js

2. **Test the Application:**
   - Search for cities
   - Use location button
   - Check forecast display
   - Test error handling

---

## Additional Practice Exercises

### Exercise 1: Form Validation
Add comprehensive form validation to the contact form in the portfolio website.

### Exercise 2: Local Storage
Implement persistent storage for the todo application using localStorage.

### Exercise 3: API Integration
Add a blog section to the portfolio that fetches posts from a mock API.

### Exercise 4: Responsive Images
Implement lazy loading and responsive images in the portfolio website.

### Exercise 5: Progressive Web App
Convert the todo app into a PWA with service workers and offline functionality.

Remember: Practice is key! Build, break, fix, and improve. Each project teaches you something new!
