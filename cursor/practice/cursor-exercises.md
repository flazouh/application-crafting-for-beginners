# Cursor Practice: Hands-On AI Development Exercises

## Exercise 1: Getting Started with Cursor

### Step 1: Download and Install Cursor

1. **Go to the official website:**
   - Visit https://cursor.sh/
   - Click "Download for [your operating system]"

2. **Install the application:**
   - Run the installer
   - Follow the setup wizard
   - Launch Cursor when installation completes

### Step 2: First Look at the Interface

**Open Cursor and explore:**

1. **Menu Bar**: File, Edit, Selection, View, Go, Run
2. **Side Panel**: Explorer (files), Search, Source Control, Extensions
3. **Main Area**: Editor tabs and AI chat panel
4. **Status Bar**: Shows current project, Git status, and notifications

**Activity**: Click around and get familiar with the layout.

---

## Exercise 2: Your First AI Conversation

### Open the AI Chat

1. **Find the chat icon** in the sidebar (💬 speech bubble)
2. **Click it** to open the AI chat panel
3. **Type your first message**

### Try Basic AI Interactions

**Prompt 1: Simple Code Generation**
```
Create a simple HTML page with a heading that says "Hello World"
```

**What the AI should generate:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello World</title>
</head>
<body>
    <h1>Hello World</h1>
</body>
</html>
```

**Prompt 2: Code with Styling**
```
Create a beautiful button with hover effects using HTML and CSS
```

---

## Exercise 3: Inline AI Code Completion

### Practice AI Code Suggestions

1. **Create a new HTML file:**
   - File → New File
   - Name it `index.html`

2. **Start typing HTML structure:**
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My First Page</title>
   ```

3. **Press `Ctrl+K` (or `Cmd+K` on Mac)** and ask:
   ```
   Complete this HTML page with a navigation bar and main content area
   ```

4. **The AI will generate:**
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>My First Page</title>
       <style>
           nav {
               background-color: #333;
               padding: 10px;
           }
           nav a {
               color: white;
               margin-right: 20px;
               text-decoration: none;
           }
           main {
               padding: 20px;
           }
       </style>
   </head>
   <body>
       <nav>
           <a href="#home">Home</a>
           <a href="#about">About</a>
           <a href="#contact">Contact</a>
       </nav>
       <main>
           <h1>Welcome to My Website</h1>
           <p>This is the main content area.</p>
       </main>
   </body>
   </html>
   ```

---

## Exercise 4: AI Code Explanation

### Understanding Existing Code

1. **Copy this JavaScript code into a new file:**
   ```javascript
   function calculateTotal(items) {
       let total = 0;
       for (let i = 0; i < items.length; i++) {
           total += items[i].price * items[i].quantity;
       }
       return total;
   }
   ```

2. **Highlight the entire function**
3. **Press `Ctrl+Shift+L`** and ask:
   ```
   Explain what this function does step by step
   ```

**Expected AI Response:**
- Function name and purpose
- Parameters explanation
- Loop logic breakdown
- Return value description

---

## Exercise 5: AI Bug Fixing

### Practice Error Correction

1. **Create a JavaScript file with intentional errors:**
   ```javascript
   function greetUser(name) {
       console.log("Hello " + name)
       // Missing semicolon and quote
   }

   // Call the function
   greetUser("Alice")
   ```

2. **Run the code** (you might see errors in console)
3. **Ask AI to fix errors:**
   ```
   Fix the JavaScript errors in this code
   ```

**AI should suggest:**
```javascript
function greetUser(name) {
   console.log("Hello " + name);
}

greetUser("Alice");
```

---

## Exercise 6: Building a Complete Website

### AI-Powered Website Creation

**Open AI chat and give this detailed prompt:**
```
Create a complete personal portfolio website with:
- Navigation bar with Home, About, Projects, Contact
- Hero section with my name and title
- About section with photo placeholder and description
- Projects section with 3 project cards
- Contact form with name, email, message fields
- Responsive design that works on mobile
- Modern, clean styling
```

**AI will generate multiple files:**
- `index.html` - Main HTML structure
- `styles.css` - Complete styling
- `script.js` - Interactive JavaScript

**Test the website:**
- Right-click `index.html`
- Select "Open in Default Browser"

---

## Exercise 7: Interactive JavaScript with AI

### Adding Dynamic Functionality

1. **Create a Todo List Application**

**Ask AI:**
```
Create a todo list app with HTML, CSS, and JavaScript that can:
- Add new tasks
- Mark tasks as complete
- Delete tasks
- Save tasks to local storage
```

2. **Test the functionality:**
   - Add several tasks
   - Mark some as complete
   - Delete a task
   - Refresh page (should remember tasks)

---

## Exercise 8: CSS Styling Challenges

### AI-Assisted Design

**Exercise 1: Card Design**
1. Create a simple card HTML structure
2. Ask AI: *"Make this look like a modern card design with shadow and hover effects"*

**Exercise 2: Navigation Styling**
1. Create a basic nav bar
2. Ask AI: *"Style this navigation bar with a professional look"*

**Exercise 3: Responsive Layout**
1. Create a 3-column layout
2. Ask AI: *"Make this layout responsive for mobile devices"*

---

## Exercise 9: Debugging Complex Issues

### Advanced Problem Solving

**Scenario 1: Form Validation**
```javascript
// This code has bugs
function validateForm() {
   const email = document.getElementById('email').value;
   const password = document.getElementById('password').value;

   if (email == "" || password == "") {
       alert("Please fill all fields");
       return false;
   }

   if (email.includes("@")) {
       return true;
   } else {
       alert("Invalid email");
       return false;
   }
}
```

**Ask AI:** *"Debug this form validation function and fix any issues"*

**Scenario 2: Array Manipulation**
```javascript
// Buggy array processing
function processNumbers(numbers) {
   const result = [];
   for (let i = 0; i < numbers.length; i++) {
       if (numbers[i] > 10) {
           result.push(numbers[i] * 2);
       }
   }
   return result;
}
```

**Ask AI:** *"Find and fix the bug in this array processing function"*

---

## Exercise 10: Code Refactoring

### Improving Code Quality

**Original Code:**
```javascript
function calculateGrade(score) {
   if (score >= 90) {
       return "A";
   } else if (score >= 80) {
       return "B";
   } else if (score >= 70) {
       return "C";
   } else if (score >= 60) {
       return "D";
   } else {
       return "F";
   }
}
```

**Ask AI:** *"Refactor this function to be more readable and maintainable"*

**AI might suggest:**
```javascript
function calculateGrade(score) {
   const gradeThresholds = [
       { min: 90, grade: 'A' },
       { min: 80, grade: 'B' },
       { min: 70, grade: 'C' },
       { min: 60, grade: 'D' }
   ];

   for (const threshold of gradeThresholds) {
       if (score >= threshold.min) {
           return threshold.grade;
       }
   }

   return 'F';
}
```

---

## Exercise 11: Learning New Concepts

### AI as Your Tutor

**Ask AI these learning questions:**

1. **CSS Grid Layout:**
   ```
   Explain CSS Grid with a practical example. Create a 3x3 grid layout.
   ```

2. **JavaScript Promises:**
   ```
   Explain how promises work in JavaScript with a real-world example.
   ```

3. **Event Handling:**
   ```
   Show me 3 different ways to handle click events in JavaScript.
   ```

4. **Array Methods:**
   ```
   Explain the difference between map(), filter(), and reduce() with examples.
   ```

---

## Exercise 12: Complete Project Build

### Build a Weather Dashboard

**Comprehensive Project Prompt:**
```
Create a weather dashboard application that:
1. Has a search input for city names
2. Displays current weather with temperature, humidity, wind speed
3. Shows a 5-day forecast
4. Has different backgrounds based on weather conditions
5. Is fully responsive for mobile and desktop
6. Includes error handling for invalid cities
7. Uses local storage to remember last searched city

Include HTML, CSS, and JavaScript files. Use modern design principles.
```

**Project Steps:**
1. AI generates the complete codebase
2. Test the application functionality
3. Ask AI to improve specific aspects
4. Add additional features

---

## Exercise 13: Code Review and Optimization

### Professional Development Practices

1. **Review Your Own Code:**
   ```
   Review this code and suggest improvements for performance, readability, and best practices
   ```

2. **Performance Optimization:**
   ```
   Optimize this code for better performance
   ```

3. **Security Review:**
   ```
   Check this code for potential security vulnerabilities
   ```

---

## Exercise 14: API Integration

### Working with External Data

**Build a GitHub Profile Viewer:**

**Prompt:**
```
Create a GitHub profile viewer that:
- Takes a GitHub username as input
- Fetches user data from GitHub API
- Displays profile picture, name, bio, followers count
- Shows recent repositories
- Handles loading states and errors
- Has a clean, modern design
```

**Key Learning Points:**
- API calls with fetch()
- Async/await syntax
- Error handling
- Loading states
- Dynamic content rendering

---

## Exercise 15: Advanced AI Collaboration

### Multi-Step Development

**Build an E-commerce Product Page:**

1. **Planning Phase:**
   ```
   Plan the structure for an e-commerce product page with image gallery, product details, reviews, and add to cart functionality
   ```

2. **HTML Structure:**
   ```
   Create the HTML structure for the product page based on our plan
   ```

3. **CSS Styling:**
   ```
   Style the product page to look professional and modern
   ```

4. **JavaScript Functionality:**
   ```
   Add interactive features like image gallery, quantity selector, and add to cart
   ```

5. **Testing and Debugging:**
   ```
   Test all functionality and fix any issues
   ```

6. **Optimization:**
   ```
   Optimize the code for performance and user experience
   ```

---

## Essential Keyboard Shortcuts Reference

| Shortcut | Action | Use Case |
|----------|--------|----------|
| `Ctrl+K` | AI Code Completion | Writing new code |
| `Ctrl+L` | Explain Code | Understanding code |
| `Ctrl+E` | Fix Errors | Debugging |
| `Ctrl+R` | Refactor Code | Improving code |
| `Ctrl+N` | New File | Creating files |
| `Ctrl+S` | Save File | Saving work |
| `Ctrl+P` | Quick Open | Finding files |
| `Ctrl+Shift+P` | Command Palette | Advanced commands |

---

## Common AI Prompts by Category

### HTML Generation
- "Create a responsive navigation bar"
- "Build a contact form with validation"
- "Design a card component for blog posts"
- "Create a photo gallery layout"

### CSS Styling
- "Style this button with modern design"
- "Create a beautiful color scheme"
- "Make this layout responsive"
- "Add smooth hover animations"

### JavaScript Functionality
- "Add click events to all buttons"
- "Create a slideshow/carousel component"
- "Implement form validation"
- "Add smooth scrolling to navigation"

### Debugging
- "Debug this JavaScript error"
- "Find the bug in this function"
- "Why isn't this CSS working?"
- "Fix this layout issue"

### Learning
- "Explain how CSS Grid works"
- "What's the difference between let and const?"
- "How do JavaScript promises work?"
- "Show me array methods examples"

---

## Troubleshooting Common Issues

### AI Not Responding
1. Check internet connection
2. Refresh the AI chat panel
3. Restart Cursor
4. Check if you have the latest version

### Code Not Working as Expected
1. Ask AI: "Debug this code"
2. Check browser developer console (F12)
3. Try running in different browsers
4. Ask AI for specific error solutions

### Files Not Saving
1. Check file permissions
2. Ensure you're in the correct directory
3. Try saving with different file names
4. Restart Cursor if needed

### AI Giving Wrong Answers
1. Provide more context in your prompt
2. Break complex requests into smaller steps
3. Specify the programming language
4. Include error messages if applicable

---

## Project Portfolio Ideas

### Beginner Projects (1-2 days each)
1. **Personal Portfolio Website**
2. **Recipe Blog**
3. **Photo Gallery**
4. **Simple Calculator**
5. **Todo List App**

### Intermediate Projects (3-5 days each)
1. **Weather Dashboard**
2. **Movie Database App**
3. **E-commerce Product Page**
4. **Blog Platform**
5. **Social Media Dashboard**

### Advanced Projects (1-2 weeks each)
1. **Full E-commerce Site**
2. **Task Management System**
3. **Learning Management Platform**
4. **Social Networking Site**
5. **Real-time Chat Application**

---

## Next Steps and Best Practices

1. **Daily Practice**: Code for at least 30 minutes every day
2. **Project Building**: Complete 1-2 projects per week
3. **Concept Learning**: Ask AI to explain new concepts regularly
4. **Code Review**: Have AI review your code weekly
5. **Community Engagement**: Share projects and learn from others

Remember: The goal is progress, not perfection. Every line of code you write with AI assistance is a step toward becoming a better developer!
