# Cursor Beginner Guide: Your AI Coding Companion

## What is Cursor? 🤖

Cursor is like having a super-smart coding assistant that lives in your code editor! It's based on VS Code but adds powerful AI features that can:

- Write code for you
- Explain confusing code
- Fix bugs automatically
- Help you learn programming
- Answer coding questions instantly

Think of it as having a patient, knowledgeable coding tutor available 24/7!

## Getting Started

### Step 1: Download and Install

1. Go to https://cursor.sh/
2. Click "Download for [your operating system]"
3. Install like any other application
4. Open Cursor

### Step 2: First Look Around

When you open Cursor, you'll see:

```
┌─────────────────────────────────────────┐
│ File  Edit  Selection  View  Go  Run    │  ← Menu Bar
├─────────────────────────────────────────┤
│ 🗂️ Explorer    📝 Open Editors          │  ← Side Panel
│ ├── 📁 my-project                      │
│ │   ├── 📄 index.html                  │
│ │   └── 📄 script.js                   │
├─────────────────────────────────────────┤
│ Welcome to Cursor!                     │  ← Main Area
│                                        │
│ 💡 Try asking me to create a website! │
└─────────────────────────────────────────┘
```

## Your First AI Conversation

### Chat with AI

1. Look for the chat icon in the sidebar (💬)
2. Click it to open the AI chat panel
3. Try asking: *"Create a simple HTML page with a button that says 'Hello World'"*

The AI will write the code for you!

### Inline AI Help

1. Open a code file (or create one)
2. Type some code, then press `Ctrl+K` (or `Cmd+K` on Mac)
3. Ask the AI to complete your code

Example:
```javascript
function calculateArea(width, height) {
    // Press Ctrl+K here and ask "multiply width by height"
    return width * height;
}
```

## Core Features Explained

### 1. Code Completion

Cursor suggests code as you type:

```javascript
// Type: "function greet" then press Tab
function greetUser(name) {
    return `Hello, ${name}!`;
}
```

### 2. Code Explanation

Highlight confusing code and press `Ctrl+Shift+L` to ask "What does this do?"

### 3. Bug Fixing

When you see an error, press `Ctrl+Shift+E` and ask "Fix this error"

### 4. Code Refactoring

Highlight code and press `Ctrl+Shift+R` to ask "Make this more readable"

## Project Setup

### Create Your First Project

1. Click "File" → "New Folder"
2. Name it `my-first-website`
3. Click "File" → "New File"
4. Name it `index.html`

### Ask AI to Build a Complete Website

In the AI chat, ask:
*"Create a complete HTML/CSS/JavaScript website with:
- A navigation bar
- A hero section
- Three feature cards
- A contact form
- Responsive design"*

The AI will generate all the files and code!

## File Management

### Open and Edit Files

```
Explorer Panel
├── 📁 my-website
│   ├── 📄 index.html      ← Click to open
│   ├── 📄 styles.css     ← Click to open
│   └── 📄 script.js      ← Click to open
```

### Create New Files

- Right-click in Explorer → "New File"
- Or press `Ctrl+N`

### Organize with Folders

```
📁 my-website
├── 📁 assets
│   ├── 📁 images
│   └── 📁 css
├── 📁 pages
│   ├── about.html
│   └── contact.html
└── index.html
```

## Essential Shortcuts

| Shortcut | What it does |
|----------|-------------|
| `Ctrl+K` | Ask AI to complete code |
| `Ctrl+L` | Ask AI to explain code |
| `Ctrl+E` | Ask AI to fix errors |
| `Ctrl+N` | New file |
| `Ctrl+S` | Save file |
| `Ctrl+P` | Quick open file |
| `Ctrl+Shift+P` | Command palette |

## Working with HTML/CSS/JavaScript

### HTML Structure

Ask AI: *"Create the basic HTML structure for a blog post page"*

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My Blog</title>
</head>
<body>
    <!-- Your content here -->
</body>
</html>
```

### CSS Styling

Highlight HTML elements and ask AI: *"Make this look like a modern card design"*

### JavaScript Interactions

Ask AI: *"Add click events to make the buttons interactive"*

## Debugging with AI

### When Code Doesn't Work

1. Run your code (right-click HTML file → "Open in Default Browser")
2. If there's an error, copy the error message
3. Paste it in AI chat and ask: *"Fix this JavaScript error: [error message]"*

### Test Your Code

Ask AI: *"Add console.log statements to debug this function"*

## Learning New Concepts

### Ask Questions Anytime

- *"How does CSS Grid work?"*
- *"What's the difference between let and const?"*
- *"How do I center a div?"*
- *"Explain promises in JavaScript"*

### Code Examples

Ask for specific examples:
*"Show me 3 ways to loop through an array in JavaScript"*

## Project Ideas to Try

### Beginner Projects

1. **Personal Portfolio**
   - Ask: *"Create a personal portfolio website with sections for about, skills, and projects"*

2. **Todo List App**
   - Ask: *"Build a todo list app with add, delete, and mark complete features"*

3. **Weather Dashboard**
   - Ask: *"Create a weather app that shows current weather for any city"*

4. **Calculator**
   - Ask: *"Build a calculator with basic math operations"*

### Advanced Projects

1. **Blog Website**
2. **E-commerce Store**
3. **Social Media Dashboard**
4. **Game (like Tic-tac-toe)**

## Best Practices

### Code Organization

```
📁 project-name
├── 📄 index.html          ← Main page
├── 📄 styles.css          ← All styling
├── 📄 script.js           ← All JavaScript
└── 📁 assets             ← Images, fonts, etc.
    ├── 📁 images
    └── 📁 fonts
```

### Ask AI for Help

Don't struggle alone! Always ask AI:
- *"How can I improve this code?"*
- *"Make this more efficient"*
- *"Add error handling"*
- *"Make it mobile-friendly"*

### Learn by Doing

1. Start with simple projects
2. Break big tasks into small steps
3. Ask AI for each step
4. Experiment and modify code
5. Learn from AI explanations

## Common AI Prompts

### For HTML
*"Create a responsive navigation bar"*
*"Design a contact form with validation"*
*"Make a photo gallery layout"*

### For CSS
*"Style this button to look modern"*
*"Create a beautiful color scheme"*
*"Make this layout responsive"*

### For JavaScript
*"Add smooth scrolling to navigation links"*
*"Create a slideshow/carousel"*
*"Validate this form"*

## Troubleshooting

### AI Not Responding
- Check your internet connection
- Try refreshing the chat
- Restart Cursor

### Code Not Working
- Ask AI: *"Debug this code"*
- Check browser console for errors
- Try running in different browser

### Lost Your Work
- Cursor auto-saves
- Check "File" → "Open Recent"
- Look in your project folder

## Next Steps

1. **Practice Daily**: Code for at least 30 minutes every day
2. **Build Projects**: Complete 1-2 projects per week
3. **Learn Concepts**: Ask AI to explain things you don't understand
4. **Join Communities**: Share your work and learn from others
5. **Keep Learning**: Try new technologies and frameworks

## Resources

- **Cursor Documentation**: https://cursor.sh/docs
- **MDN Web Docs**: https://developer.mozilla.org/
- **freeCodeCamp**: https://www.freecodecamp.org/
- **CSS-Tricks**: https://css-tricks.com/
- **JavaScript Info**: https://javascript.info/

Remember: Everyone starts somewhere! Cursor's AI makes learning to code accessible to everyone. Don't be afraid to experiment and ask questions. Happy coding! 🚀
