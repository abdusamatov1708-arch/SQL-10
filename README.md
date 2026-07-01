# SQL-10
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    username VARCHAR(30) NOT NULL,
    role VARCHAR(20) DEFAULT 'customer',
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT unique_first_last_name UNIQUE (first_name, last_name),
    CONSTRAINT check_user_role CHECK (role IN ('admin', 'customer', 'manager'))
);

CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(150) NOT NULL UNIQUE,
    price NUMERIC(10, 2) NOT NULL,
    stock_quantity INT DEFAULT 0,

    CONSTRAINT check_positive_price CHECK (price > 0),
    CONSTRAINT check_positive_stock CHECK (stock_quantity >= 0)
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    product_id INT,
    quantity INT NOT NULL CHECK (quantity > 0),
    total_amount NUMERIC(12, 2) NOT NULL,
    order_date TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_order_user
        FOREIGN KEY (user_id)
        REFERENCES users(user_id)
        ON DELETE CASCADE,

    CONSTRAINT fk_order_product
        FOREIGN KEY (product_id)
        REFERENCES products(product_id)
        ON DELETE RESTRICT
);
ALTER TABLE products
ADD COLUMN description TEXT;
INSERT INTO users (first_name, last_name, email, username)
VALUES ('Jas', 'Gulchapchao', 'JAs@example.com', 'jasurchapchap');

INSERT INTO products (product_name, price, stock_quantity)
VALUES ('iPhone 15', 999.99, 10);

INSERT INTO products (product_name, price, stock_quantity)
VALUES ('Samsung S24', -500.00, 5);

INSERT INTO users (first_name, last_name, email, username)
VALUES ('Jasur', 'Gulchapchap', 'jas2@example.com', 'jasurchapchap2');

INSERT INTO orders (user_id, product_id, quantity, total_amount)
VALUES (1, 1, 1, 999.99);

DELETE FROM products WHERE product_id = 1;

DELETE FROM users WHERE user_id = 1;
