# Database Practice: Hands-On Data Management

## Practice Project 1: Task Management Database

### Project Overview
Build a complete database schema for a task management application with relationships, constraints, and queries.

### Step 1: Database Setup

**Create database and connect:**
```sql
-- Create database
CREATE DATABASE task_manager;
USE task_manager;

-- Create users table
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    full_name VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Create projects table
CREATE TABLE projects (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    owner_id INT NOT NULL,
    status ENUM('active', 'completed', 'archived') DEFAULT 'active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Create tasks table
CREATE TABLE tasks (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    status ENUM('todo', 'in_progress', 'review', 'done') DEFAULT 'todo',
    priority ENUM('low', 'medium', 'high', 'urgent') DEFAULT 'medium',
    project_id INT,
    assignee_id INT,
    creator_id INT NOT NULL,
    due_date DATE,
    estimated_hours DECIMAL(5,2),
    actual_hours DECIMAL(5,2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (project_id) REFERENCES projects(id) ON DELETE SET NULL,
    FOREIGN KEY (assignee_id) REFERENCES users(id) ON DELETE SET NULL,
    FOREIGN KEY (creator_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Create comments table
CREATE TABLE comments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    task_id INT NOT NULL,
    user_id INT NOT NULL,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (task_id) REFERENCES tasks(id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Create tags table
CREATE TABLE tags (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) UNIQUE NOT NULL,
    color VARCHAR(7) DEFAULT '#3498db',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create task_tags junction table (many-to-many relationship)
CREATE TABLE task_tags (
    task_id INT NOT NULL,
    tag_id INT NOT NULL,
    PRIMARY KEY (task_id, tag_id),
    FOREIGN KEY (task_id) REFERENCES tasks(id) ON DELETE CASCADE,
    FOREIGN KEY (tag_id) REFERENCES tags(id) ON DELETE CASCADE
);
```

### Step 2: Sample Data Insertion

**Insert sample data:**
```sql
-- Insert users
INSERT INTO users (username, email, password_hash, full_name) VALUES
('john_doe', 'john@example.com', '$2b$10$hashedpassword1', 'John Doe'),
('jane_smith', 'jane@example.com', '$2b$10$hashedpassword2', 'Jane Smith'),
('bob_wilson', 'bob@example.com', '$2b$10$hashedpassword3', 'Bob Wilson');

-- Insert projects
INSERT INTO projects (name, description, owner_id) VALUES
('Website Redesign', 'Complete overhaul of company website', 1),
('Mobile App Development', 'Native mobile app for iOS and Android', 2),
('API Integration', 'Integrate third-party APIs', 1);

-- Insert tasks
INSERT INTO tasks (title, description, status, priority, project_id, assignee_id, creator_id, due_date, estimated_hours) VALUES
('Design homepage mockups', 'Create wireframes and mockups for the new homepage', 'in_progress', 'high', 1, 2, 1, '2024-02-15', 16.00),
('Implement user authentication', 'Set up login and registration system', 'todo', 'high', 2, 3, 2, '2024-02-20', 24.00),
('Write API documentation', 'Document all API endpoints and usage', 'review', 'medium', 3, 1, 1, '2024-02-10', 8.00),
('Database optimization', 'Improve database query performance', 'todo', 'medium', 1, 3, 1, '2024-02-25', 12.00);

-- Insert tags
INSERT INTO tags (name, color) VALUES
('frontend', '#e74c3c'),
('backend', '#3498db'),
('design', '#9b59b6'),
('testing', '#2ecc71'),
('documentation', '#f39c12');

-- Assign tags to tasks
INSERT INTO task_tags (task_id, tag_id) VALUES
(1, 3), (1, 1),  -- Design homepage: design, frontend
(2, 2),           -- Authentication: backend
(3, 5),           -- Documentation: documentation
(4, 2);           -- Database optimization: backend

-- Insert comments
INSERT INTO comments (task_id, user_id, content) VALUES
(1, 2, 'Working on the mobile-first responsive design'),
(1, 1, 'Looks great! Can you add some animation to the hero section?'),
(2, 3, 'Planning to use JWT for token-based authentication'),
(3, 1, 'Documentation is almost complete, just need to add examples');
```

### Step 3: Query Practice

**Basic SELECT queries:**
```sql
-- Get all users
SELECT * FROM users;

-- Get active projects with owner names
SELECT p.name, p.description, u.full_name as owner
FROM projects p
JOIN users u ON p.owner_id = u.id
WHERE p.status = 'active';

-- Get tasks with assignee and project info
SELECT t.title, t.status, t.priority, t.due_date,
       u.full_name as assignee, p.name as project
FROM tasks t
LEFT JOIN users u ON t.assignee_id = u.id
LEFT JOIN projects p ON t.project_id = p.id;

-- Count tasks by status
SELECT status, COUNT(*) as count
FROM tasks
GROUP BY status;

-- Get overdue tasks
SELECT title, due_date, DATEDIFF(CURDATE(), due_date) as days_overdue
FROM tasks
WHERE due_date < CURDATE() AND status != 'done';
```

**Advanced queries:**
```sql
-- Get project progress
SELECT p.name as project,
       COUNT(t.id) as total_tasks,
       SUM(CASE WHEN t.status = 'done' THEN 1 ELSE 0 END) as completed_tasks,
       ROUND((SUM(CASE WHEN t.status = 'done' THEN 1 ELSE 0 END) / COUNT(t.id)) * 100, 1) as progress_percentage
FROM projects p
LEFT JOIN tasks t ON p.id = t.project_id
GROUP BY p.id, p.name;

-- Get user workload
SELECT u.full_name,
       COUNT(t.id) as assigned_tasks,
       SUM(CASE WHEN t.status = 'done' THEN 1 ELSE 0 END) as completed_tasks,
       AVG(t.estimated_hours) as avg_estimated_hours
FROM users u
LEFT JOIN tasks t ON u.id = t.assignee_id
GROUP BY u.id, u.full_name;

-- Get tasks with their tags
SELECT t.title, GROUP_CONCAT(tag.name SEPARATOR ', ') as tags
FROM tasks t
LEFT JOIN task_tags tt ON t.id = tt.task_id
LEFT JOIN tags tag ON tt.tag_id = tag.id
GROUP BY t.id, t.title;

-- Get recent activity (comments in last 7 days)
SELECT c.content, c.created_at,
       u.full_name as commenter,
       t.title as task_title
FROM comments c
JOIN users u ON c.user_id = u.id
JOIN tasks t ON c.task_id = t.id
WHERE c.created_at >= DATE_SUB(NOW(), INTERVAL 7 DAY)
ORDER BY c.created_at DESC;
```

### Step 4: Database Operations

**Update operations:**
```sql
-- Update task status
UPDATE tasks SET status = 'done', actual_hours = 14.50
WHERE id = 1;

-- Update project status when all tasks are done
UPDATE projects SET status = 'completed'
WHERE id = 1 AND NOT EXISTS (
    SELECT 1 FROM tasks
    WHERE project_id = 1 AND status != 'done'
);

-- Bulk update task priorities
UPDATE tasks SET priority = 'high'
WHERE due_date <= DATE_ADD(CURDATE(), INTERVAL 3 DAY)
AND status != 'done';
```

**Delete operations:**
```sql
-- Delete completed tasks older than 30 days
DELETE FROM tasks
WHERE status = 'done'
AND updated_at < DATE_SUB(NOW(), INTERVAL 30 DAY);

-- Remove user from all tasks (set assignee to NULL)
UPDATE tasks SET assignee_id = NULL
WHERE assignee_id = 3;
```

### Step 5: Indexes and Performance

**Create indexes for better performance:**
```sql
-- Index for user email lookups
CREATE INDEX idx_users_email ON users(email);

-- Index for task status queries
CREATE INDEX idx_tasks_status ON tasks(status);

-- Index for task due dates
CREATE INDEX idx_tasks_due_date ON tasks(due_date);

-- Composite index for project and status
CREATE INDEX idx_tasks_project_status ON tasks(project_id, status);

-- Index for comments by task
CREATE INDEX idx_comments_task_date ON comments(task_id, created_at);
```

**Analyze query performance:**
```sql
-- Show execution plan
EXPLAIN SELECT * FROM tasks WHERE status = 'in_progress';

-- Show table indexes
SHOW INDEX FROM tasks;

-- Analyze table
ANALYZE TABLE tasks;
```

---

## Practice Project 2: E-commerce Database

### Project Overview
Design and implement a database for an e-commerce platform with products, orders, customers, and inventory.

### Step 1: Database Schema

**Create the database structure:**
```sql
CREATE DATABASE ecommerce;
USE ecommerce;

-- Customers table
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    phone VARCHAR(20),
    date_of_birth DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Addresses table
CREATE TABLE addresses (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    type ENUM('billing', 'shipping') NOT NULL,
    street_address VARCHAR(255) NOT NULL,
    city VARCHAR(100) NOT NULL,
    state VARCHAR(50),
    postal_code VARCHAR(20) NOT NULL,
    country VARCHAR(50) DEFAULT 'USA',
    is_default BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES customers(id) ON DELETE CASCADE
);

-- Categories table
CREATE TABLE categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) UNIQUE NOT NULL,
    description TEXT,
    parent_id INT,
    image_url VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (parent_id) REFERENCES categories(id) ON DELETE SET NULL
);

-- Products table
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    sku VARCHAR(50) UNIQUE NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    compare_price DECIMAL(10,2),
    cost_price DECIMAL(10,2),
    stock_quantity INT DEFAULT 0,
    min_stock_level INT DEFAULT 0,
    weight DECIMAL(8,2),
    dimensions VARCHAR(50),
    category_id INT,
    brand VARCHAR(100),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE SET NULL
);

-- Product images table
CREATE TABLE product_images (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT NOT NULL,
    image_url VARCHAR(255) NOT NULL,
    alt_text VARCHAR(255),
    sort_order INT DEFAULT 0,
    is_primary BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);

-- Orders table
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_number VARCHAR(20) UNIQUE NOT NULL,
    customer_id INT NOT NULL,
    status ENUM('pending', 'confirmed', 'processing', 'shipped', 'delivered', 'cancelled') DEFAULT 'pending',
    subtotal DECIMAL(10,2) NOT NULL,
    tax_amount DECIMAL(10,2) DEFAULT 0,
    shipping_amount DECIMAL(10,2) DEFAULT 0,
    discount_amount DECIMAL(10,2) DEFAULT 0,
    total_amount DECIMAL(10,2) NOT NULL,
    billing_address_id INT,
    shipping_address_id INT,
    payment_method VARCHAR(50),
    notes TEXT,
    ordered_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    shipped_at TIMESTAMP NULL,
    delivered_at TIMESTAMP NULL,
    FOREIGN KEY (customer_id) REFERENCES customers(id) ON DELETE CASCADE,
    FOREIGN KEY (billing_address_id) REFERENCES addresses(id),
    FOREIGN KEY (shipping_address_id) REFERENCES addresses(id)
);

-- Order items table
CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(10,2) NOT NULL,
    total_price DECIMAL(10,2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(id)
);

-- Shopping cart table
CREATE TABLE cart_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    added_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES customers(id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE,
    UNIQUE KEY unique_cart_item (customer_id, product_id)
);

-- Reviews table
CREATE TABLE reviews (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT NOT NULL,
    customer_id INT NOT NULL,
    order_id INT,
    rating INT NOT NULL CHECK (rating >= 1 AND rating <= 5),
    title VARCHAR(255),
    comment TEXT,
    is_verified BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE,
    FOREIGN KEY (customer_id) REFERENCES customers(id) ON DELETE CASCADE,
    FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE SET NULL
);

-- Inventory transactions table
CREATE TABLE inventory_transactions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT NOT NULL,
    transaction_type ENUM('purchase', 'sale', 'adjustment', 'return') NOT NULL,
    quantity_change INT NOT NULL,
    reference_id INT,
    reference_type VARCHAR(50),
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);
```

### Step 2: Sample Data

**Insert comprehensive sample data:**
```sql
-- Insert categories
INSERT INTO categories (name, description) VALUES
('Electronics', 'Electronic devices and accessories'),
('Clothing', 'Fashion and apparel'),
('Home & Garden', 'Home improvement and garden supplies'),
('Sports', 'Sports equipment and apparel'),
('Books', 'Books and publications');

-- Insert subcategories
INSERT INTO categories (name, description, parent_id) VALUES
('Smartphones', 'Mobile phones and accessories', 1),
('Laptops', 'Portable computers', 1),
('Men\'s Clothing', 'Clothing for men', 2),
('Women\'s Clothing', 'Clothing for women', 2),
('Fiction', 'Fiction books', 5),
('Non-Fiction', 'Non-fiction books', 5);

-- Insert customers
INSERT INTO customers (email, password_hash, first_name, last_name, phone) VALUES
('john.doe@email.com', '$2b$10$hashedpassword1', 'John', 'Doe', '555-0101'),
('jane.smith@email.com', '$2b$10$hashedpassword2', 'Jane', 'Smith', '555-0102'),
('bob.wilson@email.com', '$2b$10$hashedpassword3', 'Bob', 'Wilson', '555-0103');

-- Insert addresses
INSERT INTO addresses (customer_id, type, street_address, city, state, postal_code, is_default) VALUES
(1, 'billing', '123 Main St', 'Anytown', 'CA', '12345', TRUE),
(1, 'shipping', '123 Main St', 'Anytown', 'CA', '12345', TRUE),
(2, 'billing', '456 Oak Ave', 'Somewhere', 'NY', '67890', TRUE),
(2, 'shipping', '456 Oak Ave', 'Somewhere', 'NY', '67890', TRUE);

-- Insert products
INSERT INTO products (name, description, sku, price, stock_quantity, category_id, brand) VALUES
('iPhone 15 Pro', 'Latest iPhone with advanced camera system', 'IPH15P-128', 999.99, 50, 6, 'Apple'),
('MacBook Air M2', 'Powerful laptop with M2 chip', 'MBA-M2-256', 1199.99, 30, 7, 'Apple'),
('Wireless Headphones', 'Premium noise-cancelling headphones', 'WH-XB950N', 299.99, 100, 1, 'Sony'),
('Running Shoes', 'Comfortable running shoes for athletes', 'RUN-PRO-10', 129.99, 75, 4, 'Nike'),
('Coffee Maker', 'Programmable coffee maker with thermal carafe', 'CM-500', 79.99, 40, 3, 'Mr. Coffee');

-- Insert product images
INSERT INTO product_images (product_id, image_url, alt_text, is_primary) VALUES
(1, '/images/iphone15-pro.jpg', 'iPhone 15 Pro front view', TRUE),
(1, '/images/iphone15-pro-side.jpg', 'iPhone 15 Pro side view', FALSE),
(2, '/images/macbook-air-m2.jpg', 'MacBook Air M2', TRUE),
(3, '/images/sony-headphones.jpg', 'Sony WH-XB950N headphones', TRUE);

-- Insert cart items
INSERT INTO cart_items (customer_id, product_id, quantity) VALUES
(1, 1, 1),
(1, 3, 2),
(2, 2, 1),
(3, 4, 1);

-- Insert orders
INSERT INTO orders (order_number, customer_id, status, subtotal, tax_amount, shipping_amount, total_amount, billing_address_id, shipping_address_id, payment_method) VALUES
('ORD-2024-001', 1, 'delivered', 1299.98, 103.20, 9.99, 1413.17, 1, 2, 'credit_card'),
('ORD-2024-002', 2, 'processing', 1199.99, 96.00, 0.00, 1295.99, 3, 4, 'paypal');

-- Insert order items
INSERT INTO order_items (order_id, product_id, quantity, unit_price, total_price) VALUES
(1, 1, 1, 999.99, 999.99),
(1, 3, 1, 299.99, 299.99),
(2, 2, 1, 1199.99, 1199.99);

-- Insert reviews
INSERT INTO reviews (product_id, customer_id, order_id, rating, title, comment, is_verified) VALUES
(1, 1, 1, 5, 'Amazing phone!', 'Best iPhone I\'ve ever owned. Camera is incredible.', TRUE),
(3, 1, 1, 4, 'Great sound quality', 'Excellent noise cancellation and sound quality.', TRUE),
(2, 2, 2, 5, 'Perfect for development', 'Fast, reliable, and great battery life for coding.', TRUE);

-- Insert inventory transactions
INSERT INTO inventory_transactions (product_id, transaction_type, quantity_change, notes) VALUES
(1, 'purchase', 50, 'Initial stock'),
(2, 'purchase', 30, 'Initial stock'),
(3, 'purchase', 100, 'Initial stock'),
(1, 'sale', -1, 'Order ORD-2024-001'),
(3, 'sale', -1, 'Order ORD-2024-001'),
(2, 'sale', -1, 'Order ORD-2024-002');
```

### Step 3: Complex Queries

**Business intelligence queries:**
```sql
-- Top-selling products
SELECT p.name, p.sku,
       SUM(oi.quantity) as total_sold,
       SUM(oi.total_price) as total_revenue
FROM products p
JOIN order_items oi ON p.id = oi.product_id
JOIN orders o ON oi.order_id = o.id
WHERE o.status = 'delivered'
GROUP BY p.id, p.name, p.sku
ORDER BY total_sold DESC;

-- Customer lifetime value
SELECT c.first_name, c.last_name, c.email,
       COUNT(DISTINCT o.id) as total_orders,
       SUM(o.total_amount) as lifetime_value,
       AVG(o.total_amount) as avg_order_value,
       MAX(o.ordered_at) as last_order_date
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
GROUP BY c.id, c.first_name, c.last_name, c.email
ORDER BY lifetime_value DESC;

-- Product performance by category
SELECT cat.name as category,
       COUNT(DISTINCT p.id) as total_products,
       SUM(p.stock_quantity) as total_stock,
       AVG(p.price) as avg_price,
       SUM(CASE WHEN p.stock_quantity <= p.min_stock_level THEN 1 ELSE 0 END) as low_stock_products
FROM categories cat
LEFT JOIN products p ON cat.id = p.category_id
GROUP BY cat.id, cat.name;

-- Monthly sales trends
SELECT DATE_FORMAT(ordered_at, '%Y-%m') as month,
       COUNT(*) as orders_count,
       SUM(total_amount) as monthly_revenue,
       AVG(total_amount) as avg_order_value
FROM orders
WHERE status = 'delivered'
GROUP BY DATE_FORMAT(ordered_at, '%Y-%m')
ORDER BY month DESC;

-- Customer retention analysis
SELECT
    COUNT(DISTINCT CASE WHEN order_count = 1 THEN customer_id END) as one_time_customers,
    COUNT(DISTINCT CASE WHEN order_count > 1 THEN customer_id END) as repeat_customers,
    ROUND((COUNT(DISTINCT CASE WHEN order_count > 1 THEN customer_id END) /
           COUNT(DISTINCT customer_id)) * 100, 2) as retention_rate
FROM (
    SELECT customer_id, COUNT(*) as order_count
    FROM orders
    GROUP BY customer_id
) customer_orders;
```

### Step 4: Triggers and Stored Procedures

**Create triggers for automatic updates:**
```sql
-- Update product stock when orders are placed
DELIMITER //
CREATE TRIGGER update_stock_after_order
AFTER INSERT ON order_items
FOR EACH ROW
BEGIN
    UPDATE products
    SET stock_quantity = stock_quantity - NEW.quantity,
        updated_at = NOW()
    WHERE id = NEW.product_id;
END//
DELIMITER ;

-- Log inventory changes
DELIMITER //
CREATE TRIGGER log_inventory_changes
AFTER UPDATE ON products
FOR EACH ROW
BEGIN
    IF OLD.stock_quantity != NEW.stock_quantity THEN
        INSERT INTO inventory_transactions
        (product_id, transaction_type, quantity_change, notes)
        VALUES
        (NEW.id, 'adjustment', NEW.stock_quantity - OLD.stock_quantity, 'Stock update');
    END IF;
END//
DELIMITER ;

-- Calculate order totals automatically
DELIMITER //
CREATE TRIGGER calculate_order_totals
BEFORE UPDATE ON orders
FOR EACH ROW
BEGIN
    IF NEW.subtotal != OLD.subtotal OR NEW.discount_amount != OLD.discount_amount THEN
        SET NEW.total_amount = NEW.subtotal + NEW.tax_amount + NEW.shipping_amount - NEW.discount_amount;
    END IF;
END//
DELIMITER ;
```

**Create stored procedures:**
```sql
-- Procedure to place an order
DELIMITER //
CREATE PROCEDURE place_order(
    IN customer_id INT,
    IN billing_address_id INT,
    IN shipping_address_id INT,
    IN payment_method VARCHAR(50)
)
BEGIN
    DECLARE order_id INT;
    DECLARE order_total DECIMAL(10,2);

    -- Calculate cart total for customer
    SELECT SUM(ci.quantity * p.price) INTO order_total
    FROM cart_items ci
    JOIN products p ON ci.product_id = p.id
    WHERE ci.customer_id = customer_id;

    -- Create order
    INSERT INTO orders (
        order_number, customer_id, subtotal, total_amount,
        billing_address_id, shipping_address_id, payment_method
    ) VALUES (
        CONCAT('ORD-', DATE_FORMAT(NOW(), '%Y-%m-%d-'), LPAD(FLOOR(RAND() * 1000), 3, '0')),
        customer_id, order_total, order_total,  -- Assuming no tax/shipping for simplicity
        billing_address_id, shipping_address_id, payment_method
    );

    SET order_id = LAST_INSERT_ID();

    -- Move cart items to order items
    INSERT INTO order_items (order_id, product_id, quantity, unit_price, total_price)
    SELECT order_id, ci.product_id, ci.quantity, p.price, (ci.quantity * p.price)
    FROM cart_items ci
    JOIN products p ON ci.product_id = p.id
    WHERE ci.customer_id = customer_id;

    -- Clear cart
    DELETE FROM cart_items WHERE customer_id = customer_id;

    -- Return order ID
    SELECT order_id as order_id;
END//
DELIMITER ;

-- Procedure to get low stock products
DELIMITER //
CREATE PROCEDURE get_low_stock_products()
BEGIN
    SELECT id, name, sku, stock_quantity, min_stock_level,
           (stock_quantity - min_stock_level) as shortage
    FROM products
    WHERE stock_quantity <= min_stock_level
    ORDER BY (stock_quantity - min_stock_level) ASC;
END//
DELIMITER ;
```

### Step 5: Database Maintenance

**Create indexes for performance:**
```sql
-- Product search indexes
CREATE INDEX idx_products_category ON products(category_id);
CREATE INDEX idx_products_price ON products(price);
CREATE INDEX idx_products_name ON products(name);

-- Order query indexes
CREATE INDEX idx_orders_customer_status ON orders(customer_id, status);
CREATE INDEX idx_orders_date ON orders(ordered_at);

-- Customer search indexes
CREATE INDEX idx_customers_email ON customers(email);
CREATE INDEX idx_customers_name ON customers(first_name, last_name);

-- Review query indexes
CREATE INDEX idx_reviews_product_rating ON reviews(product_id, rating);
CREATE INDEX idx_reviews_customer ON reviews(customer_id);
```

**Create views for common queries:**
```sql
-- Product catalog view
CREATE VIEW product_catalog AS
SELECT p.id, p.name, p.description, p.price, p.stock_quantity,
       c.name as category_name, pi.image_url as primary_image,
       AVG(r.rating) as avg_rating, COUNT(r.id) as review_count
FROM products p
LEFT JOIN categories c ON p.category_id = c.id
LEFT JOIN product_images pi ON p.id = pi.product_id AND pi.is_primary = TRUE
LEFT JOIN reviews r ON p.id = r.product_id
WHERE p.is_active = TRUE
GROUP BY p.id, p.name, p.description, p.price, p.stock_quantity, c.name, pi.image_url;

-- Order summary view
CREATE VIEW order_summary AS
SELECT o.id, o.order_number, o.status, o.total_amount, o.ordered_at,
       CONCAT(c.first_name, ' ', c.last_name) as customer_name,
       c.email as customer_email,
       COUNT(oi.id) as item_count
FROM orders o
JOIN customers c ON o.customer_id = c.id
LEFT JOIN order_items oi ON o.id = oi.order_id
GROUP BY o.id, o.order_number, o.status, o.total_amount, o.ordered_at, c.first_name, c.last_name, c.email;
```

---

## Practice Project 3: Social Media Database

### Project Overview
Design a database for a social media platform with users, posts, comments, likes, and follows.

### Step 1: Schema Design

**Create the social media database:**
```sql
CREATE DATABASE social_media;
USE social_media;

-- Users table
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    full_name VARCHAR(100),
    bio TEXT,
    profile_picture VARCHAR(255),
    website VARCHAR(255),
    is_private BOOLEAN DEFAULT FALSE,
    is_verified BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Posts table
CREATE TABLE posts (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    content TEXT,
    image_url VARCHAR(255),
    video_url VARCHAR(255),
    location VARCHAR(255),
    is_deleted BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    CHECK (content IS NOT NULL OR image_url IS NOT NULL OR video_url IS NOT NULL)
);

-- Comments table
CREATE TABLE comments (
    id INT AUTO_INCREMENT PRIMARY KEY,
    post_id INT NOT NULL,
    user_id INT NOT NULL,
    parent_comment_id INT,
    content TEXT NOT NULL,
    is_deleted BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (parent_comment_id) REFERENCES comments(id) ON DELETE CASCADE
);

-- Likes table
CREATE TABLE likes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    post_id INT,
    comment_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE,
    FOREIGN KEY (comment_id) REFERENCES comments(id) ON DELETE CASCADE,
    CHECK (
        (post_id IS NOT NULL AND comment_id IS NULL) OR
        (post_id IS NULL AND comment_id IS NOT NULL)
    )
);

-- Follows table (many-to-many relationship)
CREATE TABLE follows (
    id INT AUTO_INCREMENT PRIMARY KEY,
    follower_id INT NOT NULL,
    following_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (follower_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (following_id) REFERENCES users(id) ON DELETE CASCADE,
    UNIQUE KEY unique_follow (follower_id, following_id),
    CHECK (follower_id != following_id)
);

-- Hashtags table
CREATE TABLE hashtags (
    id INT AUTO_INCREMENT PRIMARY KEY,
    tag VARCHAR(100) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Post hashtags junction table
CREATE TABLE post_hashtags (
    post_id INT NOT NULL,
    hashtag_id INT NOT NULL,
    PRIMARY KEY (post_id, hashtag_id),
    FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE,
    FOREIGN KEY (hashtag_id) REFERENCES hashtags(id) ON DELETE CASCADE
);

-- Notifications table
CREATE TABLE notifications (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    type ENUM('like', 'comment', 'follow', 'mention') NOT NULL,
    actor_id INT NOT NULL,
    post_id INT,
    comment_id INT,
    is_read BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (actor_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE,
    FOREIGN KEY (comment_id) REFERENCES comments(id) ON DELETE CASCADE
);
```

### Step 2: Sample Data and Queries

**Insert sample social media data:**
```sql
-- Insert users
INSERT INTO users (username, email, password_hash, full_name, bio) VALUES
('johndoe', 'john@example.com', '$2b$10$hashed1', 'John Doe', 'Software developer and coffee enthusiast'),
('janesmith', 'jane@example.com', '$2b$10$hashed2', 'Jane Smith', 'UX Designer | Nature lover'),
('techguru', 'tech@example.com', '$2b$10$hashed3', 'Tech Guru', 'Tech blogger and entrepreneur'),
('foodie', 'foodie@example.com', '$2b$10$hashed4', 'Food Lover', 'Cooking and trying new recipes');

-- Insert follows
INSERT INTO follows (follower_id, following_id) VALUES
(1, 2), (1, 3), (1, 4),
(2, 1), (2, 3),
(3, 1), (3, 2), (3, 4),
(4, 1), (4, 2);

-- Insert posts
INSERT INTO posts (user_id, content, image_url) VALUES
(1, 'Just finished an amazing coding session! 💻 #programming #webdev', '/images/code-session.jpg'),
(2, 'Beautiful sunset today 🌅 Sometimes you need to disconnect to reconnect.', '/images/sunset.jpg'),
(3, 'Excited to announce my new course on React development! Link in bio 📚', NULL),
(1, 'Coffee and code - perfect combination ☕👨‍💻', '/images/coffee-code.jpg'),
(4, 'Made the most delicious pasta tonight 🍝 Recipe coming soon!', '/images/pasta.jpg');

-- Insert hashtags
INSERT INTO hashtags (tag) VALUES
('programming'), ('webdev'), ('react'), ('javascript'), ('coding'),
('sunset'), ('nature'), ('photography'), ('food'), ('cooking'),
('pasta'), ('coffee'), ('productivity'), ('tech'), ('learning');

-- Link posts to hashtags
INSERT INTO post_hashtags (post_id, hashtag_id) VALUES
(1, 1), (1, 2), (1, 5),  -- programming, webdev, coding
(3, 3), (3, 4), (3, 15),  -- react, javascript, learning
(4, 12), (4, 13), (4, 5); -- coffee, productivity, coding

-- Insert likes
INSERT INTO likes (user_id, post_id) VALUES
(2, 1), (3, 1), (4, 1),
(1, 2), (3, 2), (4, 2),
(1, 3), (2, 3), (4, 3),
(2, 4), (3, 4),
(1, 5), (2, 5), (3, 5);

-- Insert comments
INSERT INTO comments (post_id, user_id, content) VALUES
(1, 2, 'Looks like a productive session! What are you working on?'),
(1, 3, 'Love seeing fellow developers coding. Keep it up! 🚀'),
(2, 1, 'Stunning photo! Nature has the best views 🌟'),
(3, 4, 'Definitely interested! When does enrollment open?'),
(5, 1, 'That looks delicious! What sauce did you use?');

-- Insert notifications
INSERT INTO notifications (user_id, type, actor_id, post_id) VALUES
(1, 'like', 2, 1), (1, 'like', 3, 1), (1, 'like', 4, 1),
(1, 'comment', 2, 1), (1, 'comment', 3, 1),
(2, 'like', 1, 2), (2, 'like', 3, 2), (2, 'like', 4, 2),
(2, 'comment', 1, 2);
```

**Social media analytics queries:**
```sql
-- User engagement metrics
SELECT u.username,
       COUNT(DISTINCT p.id) as posts_count,
       COUNT(DISTINCT l.id) as likes_received,
       COUNT(DISTINCT c.id) as comments_received,
       COUNT(DISTINCT f1.id) as followers,
       COUNT(DISTINCT f2.id) as following
FROM users u
LEFT JOIN posts p ON u.id = p.user_id AND p.is_deleted = FALSE
LEFT JOIN likes l ON p.id = l.post_id
LEFT JOIN comments c ON p.id = c.post_id
LEFT JOIN follows f1 ON u.id = f1.following_id
LEFT JOIN follows f2 ON u.id = f2.follower_id
GROUP BY u.id, u.username;

-- Trending hashtags
SELECT h.tag,
       COUNT(ph.post_id) as post_count,
       COUNT(l.id) as total_likes,
       COUNT(c.id) as total_comments
FROM hashtags h
JOIN post_hashtags ph ON h.id = ph.hashtag_id
JOIN posts p ON ph.post_id = p.id AND p.is_deleted = FALSE
LEFT JOIN likes l ON p.id = l.post_id
LEFT JOIN comments c ON p.id = c.post_id
GROUP BY h.id, h.tag
ORDER BY post_count DESC, total_likes DESC;

-- User feed (posts from followed users)
SELECT p.id, p.content, p.image_url, p.created_at,
       u.username, u.full_name, u.profile_picture,
       COUNT(DISTINCT l.id) as like_count,
       COUNT(DISTINCT c.id) as comment_count,
       EXISTS(SELECT 1 FROM likes WHERE post_id = p.id AND user_id = 1) as user_liked
FROM posts p
JOIN users u ON p.user_id = u.id
JOIN follows f ON p.user_id = f.following_id AND f.follower_id = 1
LEFT JOIN likes l ON p.id = l.post_id
LEFT JOIN comments c ON p.id = c.post_id
WHERE p.is_deleted = FALSE
GROUP BY p.id, p.content, p.image_url, p.created_at, u.username, u.full_name, u.profile_picture
ORDER BY p.created_at DESC;

-- Most engaging posts
SELECT p.id, p.content, LEFT(p.content, 50) as preview,
       u.username,
       COUNT(DISTINCT l.id) as likes,
       COUNT(DISTINCT c.id) as comments,
       (COUNT(DISTINCT l.id) + COUNT(DISTINCT c.id)) as engagement_score
FROM posts p
JOIN users u ON p.user_id = u.id
LEFT JOIN likes l ON p.id = l.post_id
LEFT JOIN comments c ON p.id = c.post_id
WHERE p.is_deleted = FALSE
GROUP BY p.id, p.content, u.username
ORDER BY engagement_score DESC;

-- User activity summary
SELECT u.username,
       COUNT(CASE WHEN l.post_id IS NOT NULL THEN 1 END) as likes_given,
       COUNT(CASE WHEN c.id IS NOT NULL THEN 1 END) as comments_made,
       COUNT(CASE WHEN p.id IS NOT NULL THEN 1 END) as posts_created,
       COUNT(CASE WHEN f.id IS NOT NULL THEN 1 END) as users_followed
FROM users u
LEFT JOIN likes l ON u.id = l.user_id
LEFT JOIN comments c ON u.id = c.user_id
LEFT JOIN posts p ON u.id = p.user_id AND p.is_deleted = FALSE
LEFT JOIN follows f ON u.id = f.follower_id
GROUP BY u.id, u.username;
```

---

## Additional Practice Exercises

### Exercise 1: Database Normalization
Take an unnormalized table and normalize it to 3NF.

### Exercise 2: Query Optimization
Analyze slow queries and add appropriate indexes.

### Exercise 3: Database Backup and Recovery
Create backup scripts and test recovery procedures.

### Exercise 4: Data Migration
Write scripts to migrate data between different database schemas.

### Exercise 5: Database Security
Implement row-level security and data encryption.

### Exercise 6: Performance Monitoring
Set up monitoring and alerting for database performance.

### Exercise 7: Database Replication
Configure master-slave replication for high availability.

### Exercise 8: NoSQL Migration
Convert a relational design to a NoSQL document structure.

Remember: Good database design is crucial for application performance and maintainability. Always consider scalability, data integrity, and query patterns when designing your schemas!
