-- ================================================
-- BUSINESS DATABASE - ПОВНИЙ СКРИПТ
-- ================================================

DROP DATABASE IF EXISTS business;
CREATE DATABASE business CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE business;

-- ================================================
-- ПОБУДОВА СТРУКТУРИ БД
-- ================================================

-- 4. Таблиця: Categories
CREATE TABLE Categories (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    parent_id INT DEFAULT NULL,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (parent_id) REFERENCES Categories(id)
);

-- 5. Таблиця: Suppliers
CREATE TABLE Suppliers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    company_name VARCHAR(255) NOT NULL,
    contact_person VARCHAR(255),
    email VARCHAR(255),
    phone VARCHAR(50),
    address TEXT,
    country VARCHAR(100),
    payment_terms VARCHAR(100),
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- 3. Таблиця: Products
CREATE TABLE Products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    sku VARCHAR(100) NOT NULL UNIQUE,
    category_id INT,
    supplier_id INT,
    description TEXT,
    unit_price DECIMAL(10,2) NOT NULL DEFAULT 0.00,
    cost_price DECIMAL(10,2) NOT NULL DEFAULT 0.00,
    unit VARCHAR(50) NOT NULL DEFAULT 'шт',
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES Categories(id),
    FOREIGN KEY (supplier_id) REFERENCES Suppliers(id)
);

-- 1. Таблиця: Users
CREATE TABLE Users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    role VARCHAR(50) NOT NULL DEFAULT 'staff',
    phone VARCHAR(50),
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    last_login DATETIME,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- 2. Таблиця: Customers
CREATE TABLE Customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    email VARCHAR(255),
    phone VARCHAR(50),
    address TEXT,
    city VARCHAR(100),
    country VARCHAR(100) DEFAULT 'Україна',
    customer_type VARCHAR(50) NOT NULL DEFAULT 'Фізична особа',
    discount_percent DECIMAL(5,2) DEFAULT 0.00,
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    registered_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- 6. Таблиця: Orders
CREATE TABLE Orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,
    user_id INT,
    order_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    status VARCHAR(50) NOT NULL DEFAULT 'Новий',
    total_amount DECIMAL(15,2) NOT NULL DEFAULT 0.00,
    discount_amount DECIMAL(15,2) DEFAULT 0.00,
    final_amount DECIMAL(15,2) NOT NULL DEFAULT 0.00,
    notes TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES Customers(id),
    FOREIGN KEY (user_id) REFERENCES Users(id)
);

-- 7. Таблиця: Order_Items
CREATE TABLE Order_Items (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL DEFAULT 1,
    unit_price DECIMAL(10,2) NOT NULL,
    discount_percent DECIMAL(5,2) DEFAULT 0.00,
    total_price DECIMAL(10,2) NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES Orders(id),
    FOREIGN KEY (product_id) REFERENCES Products(id)
);

-- 8. Таблиця: Sales
CREATE TABLE Sales (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    sale_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    amount DECIMAL(15,2) NOT NULL,
    payment_method VARCHAR(50) NOT NULL DEFAULT 'Готівка',
    sales_channel VARCHAR(100) DEFAULT 'Офлайн',
    user_id INT,
    notes TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES Orders(id),
    FOREIGN KEY (user_id) REFERENCES Users(id)
);

-- 9. Таблиця: Payments
CREATE TABLE Payments (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    payment_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    amount DECIMAL(15,2) NOT NULL,
    payment_method VARCHAR(50) NOT NULL DEFAULT 'Готівка',
    status VARCHAR(50) NOT NULL DEFAULT 'Виконано',
    transaction_reference VARCHAR(255),
    currency VARCHAR(10) NOT NULL DEFAULT 'UAH',
    notes TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES Orders(id)
);

-- 10. Таблиця: Delivery
CREATE TABLE Delivery (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    delivery_method VARCHAR(100) NOT NULL DEFAULT 'Нова Пошта',
    tracking_number VARCHAR(255),
    status VARCHAR(50) NOT NULL DEFAULT 'Очікує відправлення',
    recipient_name VARCHAR(255),
    recipient_phone VARCHAR(50),
    delivery_address TEXT,
    city VARCHAR(100),
    estimated_date DATE,
    actual_date DATE,
    delivery_cost DECIMAL(10,2) DEFAULT 0.00,
    notes TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES Orders(id)
);

-- 11. Таблиця: Inventory
CREATE TABLE Inventory (
    id INT PRIMARY KEY AUTO_INCREMENT,
    product_id INT NOT NULL,
    quantity_in_stock INT NOT NULL DEFAULT 0,
    quantity_reserved INT NOT NULL DEFAULT 0,
    quantity_available INT GENERATED ALWAYS AS (quantity_in_stock - quantity_reserved) STORED,
    warehouse_location VARCHAR(100),
    min_stock_level INT DEFAULT 0,
    max_stock_level INT,
    last_restock_date DATE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES Products(id),
    UNIQUE (product_id)
);

-- 12. Таблиця: Transactions (Accounting)
CREATE TABLE Transactions (
    id INT PRIMARY KEY AUTO_INCREMENT,
    transaction_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    transaction_type VARCHAR(50) NOT NULL,
    category VARCHAR(100) NOT NULL,
    amount DECIMAL(15,2) NOT NULL,
    currency VARCHAR(10) NOT NULL DEFAULT 'UAH',
    description TEXT,
    reference_type VARCHAR(50),
    reference_id INT,
    balance_after DECIMAL(15,2),
    user_id INT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(id)
);

-- ================================================
-- ІНДЕКСИ
-- ================================================

CREATE INDEX IX_Products_Category ON Products(category_id);
CREATE INDEX IX_Products_Supplier ON Products(supplier_id);
CREATE INDEX IX_Orders_Customer ON Orders(customer_id);
CREATE INDEX IX_Orders_Status ON Orders(status);
CREATE INDEX IX_Orders_Date ON Orders(order_date);
CREATE INDEX IX_OrderItems_Order ON Order_Items(order_id);
CREATE INDEX IX_OrderItems_Product ON Order_Items(product_id);
CREATE INDEX IX_Sales_Order ON Sales(order_id);
CREATE INDEX IX_Sales_Date ON Sales(sale_date);
CREATE INDEX IX_Payments_Order ON Payments(order_id);
CREATE INDEX IX_Payments_Status ON Payments(status);
CREATE INDEX IX_Delivery_Order ON Delivery(order_id);
CREATE INDEX IX_Delivery_Status ON Delivery(status);
CREATE INDEX IX_Transactions_Date ON Transactions(transaction_date);
CREATE INDEX IX_Transactions_Type ON Transactions(transaction_type);
CREATE INDEX IX_Users_Email ON Users(email);

-- ================================================
-- ПЕРЕВІРКА ПЕРВИННИХ КЛЮЧІВ
-- ================================================

SELECT 
    TABLE_NAME AS 'Таблиця',
    COLUMN_NAME AS 'Колонка',
    CONSTRAINT_NAME AS 'Обмеження'
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE CONSTRAINT_SCHEMA = DATABASE()
  AND CONSTRAINT_NAME = 'PRIMARY'
ORDER BY TABLE_NAME;

-- ================================================
-- ПЕРЕВІРКА ЗОВНІШНІХ КЛЮЧІВ
-- ================================================

SELECT 
    TABLE_NAME AS 'Таблиця',
    COLUMN_NAME AS 'Колонка FK',
    CONSTRAINT_NAME AS 'Назва FK',
    REFERENCED_TABLE_NAME AS 'Батьківська таблиця',
    REFERENCED_COLUMN_NAME AS 'Батьківська колонка'
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE CONSTRAINT_SCHEMA = DATABASE()
  AND REFERENCED_TABLE_NAME IS NOT NULL
ORDER BY TABLE_NAME;

-- ================================================
-- ЗАПОВНЕННЯ ТАБЛИЦЬ
-- ================================================

-- Categories
INSERT INTO Categories (name, description, parent_id) VALUES
('Електроніка', 'Електронні пристрої та гаджети', NULL),
('Одяг', 'Чоловічий та жіночий одяг', NULL),
('Продукти харчування', 'Їжа та напої', NULL),
('Смартфони', 'Мобільні телефони', 1),
('Ноутбуки', 'Портативні комп''ютери', 1),
('Чоловічий одяг', 'Одяг для чоловіків', 2),
('Жіночий одяг', 'Одяг для жінок', 2);

-- Suppliers
INSERT INTO Suppliers (company_name, contact_person, email, phone, country, payment_terms) VALUES
('TechDistrib LLC', 'Олег Захаренко', 'o.zakharenko@techdistrib.ua', '+380671234501', 'Україна', 'Net 30'),
('FashionWholesale', 'Катерина Лисенко', 'k.lysenko@fashionws.com', '+380672345602', 'Україна', 'Net 15'),
('GlobalImport Ltd', 'David Chen', 'd.chen@globalimport.com', '+86-10-12345678', 'Китай', 'Net 60'),
('EuroFoods GmbH', 'Hans Bauer', 'h.bauer@eurofoods.de', '+491711234501', 'Німеччина', 'Net 30');

-- Products
INSERT INTO Products (name, sku, category_id, supplier_id, unit_price, cost_price, unit) VALUES
('Samsung Galaxy A55', 'SMGA55-BLK', 4, 1, 14999.00, 11500.00, 'шт'),
('iPhone 15', 'APL-IPH15-128', 4, 3, 39999.00, 30000.00, 'шт'),
('Ноутбук Lenovo IdeaPad 3', 'LNV-IP3-15', 5, 1, 24999.00, 19000.00, 'шт'),
('Чоловіча куртка зимова', 'MJK-WIN-L', 6, 2, 2499.00, 1200.00, 'шт'),
('Жіноче пальто', 'WCT-BLK-M', 7, 2, 3199.00, 1500.00, 'шт'),
('Кава мелена Lavazza 250г', 'COF-LAV-250', 3, 4, 189.00, 110.00, 'уп');

-- Users
INSERT INTO Users (email, password_hash, first_name, last_name, role, phone) VALUES
('admin@business.ua', 'hashed_pwd_001', 'Адміністратор', 'Системний', 'admin', '+380671000001'),
('sales1@business.ua', 'hashed_pwd_002', 'Ірина', 'Мороз', 'sales', '+380672000002'),
('sales2@business.ua', 'hashed_pwd_003', 'Максим', 'Ткач', 'sales', '+380673000003'),
('warehouse@business.ua', 'hashed_pwd_004', 'Василь', 'Гончар', 'warehouse', '+380674000004');

-- Customers
INSERT INTO Customers (first_name, last_name, email, phone, city, customer_type) VALUES
('Олена', 'Ковальчук', 'o.kovalchuk@gmail.com', '+380671111111', 'Київ', 'Фізична особа'),
('Андрій', 'Степаненко', 'a.stepanenko@gmail.com', '+380672222222', 'Харків', 'Фізична особа'),
('ТОВ Бізнес Плюс', 'Корпоративний', 'biz@bizplus.ua', '+380673333333', 'Дніпро', 'Юридична особа'),
('Наталія', 'Кравець', 'n.kravets@ukr.net', '+380674444444', 'Львів', 'Фізична особа'),
('ФОП Іваненко', 'Сервіс', 'ivanenko@fop.ua', '+380675555555', 'Одеса', 'ФОП');

-- Orders
INSERT INTO Orders (customer_id, user_id, status, total_amount, discount_amount, final_amount) VALUES
(1, 2, 'Завершено', 14999.00, 0.00, 14999.00),
(2, 2, 'Завершено', 39999.00, 2000.00, 37999.00),
(3, 3, 'В обробці', 49998.00, 5000.00, 44998.00),
(4, 3, 'Доставляється', 5698.00, 0.00, 5698.00),
(5, 2, 'Завершено', 189.00, 0.00, 189.00);

-- Order_Items
INSERT INTO Order_Items (order_id, product_id, quantity, unit_price, discount_percent, total_price) VALUES
(1, 1, 1, 14999.00, 0.00, 14999.00),
(2, 2, 1, 39999.00, 5.00, 37999.05),
(3, 3, 2, 24999.00, 10.00, 44998.20),
(4, 4, 1, 2499.00, 0.00, 2499.00),
(4, 5, 1, 3199.00, 0.00, 3199.00),
(5, 6, 1, 189.00, 0.00, 189.00);

-- Sales
INSERT INTO Sales (order_id, amount, payment_method, sales_channel, user_id) VALUES
(1, 14999.00, 'Картка', 'Онлайн', 2),
(2, 37999.00, 'Готівка', 'Офлайн', 2),
(3, 44998.00, 'Безготівковий', 'Корпоративний', 3),
(4, 5698.00, 'Картка', 'Онлайн', 3),
(5, 189.00, 'Готівка', 'Офлайн', 2);

-- Payments
INSERT INTO Payments (order_id, amount, payment_method, status, currency) VALUES
(1, 14999.00, 'Картка', 'Виконано', 'UAH'),
(2, 37999.00, 'Готівка', 'Виконано', 'UAH'),
(3, 20000.00, 'Безготівковий', 'Виконано', 'UAH'),
(3, 24998.00, 'Безготівковий', 'В очікуванні', 'UAH'),
(4, 5698.00, 'Картка', 'Виконано', 'UAH'),
(5, 189.00, 'Готівка', 'Виконано', 'UAH');

-- Delivery
INSERT INTO Delivery (order_id, delivery_method, tracking_number, status, recipient_name, recipient_phone, city, estimated_date, delivery_cost) VALUES
(1, 'Нова Пошта', 'NP20240001', 'Доставлено', 'Ковальчук Олена', '+380671111111', 'Київ', '2024-01-15', 85.00),
(2, 'Укрпошта', 'UP20240002', 'Доставлено', 'Степаненко Андрій', '+380672222222', 'Харків', '2024-01-20', 65.00),
(3, 'Кур''єр', NULL, 'В дорозі', 'ТОВ Бізнес Плюс', '+380673333333', 'Дніпро', '2024-02-01', 150.00),
(4, 'Нова Пошта', 'NP20240004', 'В дорозі', 'Кравець Наталія', '+380674444444', 'Львів', '2024-01-25', 85.00),
(5, 'Самовивіз', NULL, 'Доставлено', 'Іваненко ФОП', '+380675555555', 'Одеса', '2024-01-10', 0.00);

-- Inventory
INSERT INTO Inventory (product_id, quantity_in_stock, quantity_reserved, warehouse_location, min_stock_level) VALUES
(1, 50, 3, 'А-1-01', 5),
(2, 30, 2, 'А-1-02', 5),
(3, 20, 4, 'А-2-01', 3),
(4, 100, 5, 'Б-1-01', 10),
(5, 80, 3, 'Б-1-02', 10),
(6, 200, 10, 'В-1-01', 20);

-- Transactions
INSERT INTO Transactions (transaction_date, transaction_type, category, amount, currency, description, reference_type, reference_id, user_id) VALUES
('2024-01-10', 'Дохід', 'Продаж товарів', 14999.00, 'UAH', 'Оплата замовлення #1', 'Order', 1, 2),
('2024-01-15', 'Дохід', 'Продаж товарів', 37999.00, 'UAH', 'Оплата замовлення #2', 'Order', 2, 2),
('2024-01-18', 'Дохід', 'Продаж товарів', 20000.00, 'UAH', 'Часткова оплата замовлення #3', 'Order', 3, 3),
('2024-01-20', 'Витрата', 'Оренда', 25000.00, 'UAH', 'Оренда складу за січень', NULL, NULL, 1),
('2024-01-22', 'Дохід', 'Продаж товарів', 5698.00, 'UAH', 'Оплата замовлення #4', 'Order', 4, 3),
('2024-01-25', 'Витрата', 'Закупівля товарів', 55000.00, 'UAH', 'Поповнення складу (смартфони)', NULL, NULL, 4),
('2024-01-30', 'Витрата', 'Зарплата', 80000.00, 'UAH', 'Виплата зарплати за січень', NULL, NULL, 1);

-- ================================================
-- ЗАПИТИ ДО БД
-- ================================================

-- ЗАПИТ 1: Кількість замовлень та сума продажів по клієнтах
SELECT 
    CONCAT(c.first_name, ' ', c.last_name) AS 'Клієнт',
    c.city AS 'Місто',
    c.customer_type AS 'Тип',
    COUNT(o.id) AS 'Кількість замовлень',
    SUM(o.final_amount) AS 'Загальна сума',
    ROUND(AVG(o.final_amount), 2) AS 'Середній чек'
FROM Customers c
LEFT JOIN Orders o ON c.id = o.customer_id
GROUP BY c.id, c.first_name, c.last_name, c.city, c.customer_type
ORDER BY SUM(o.final_amount) DESC;

-- ЗАПИТ 2: Залишки товарів на складі з попередженням про мінімум
SELECT 
    p.name AS 'Товар',
    p.sku AS 'Артикул',
    cat.name AS 'Категорія',
    i.quantity_in_stock AS 'На складі',
    i.quantity_reserved AS 'Зарезервовано',
    i.quantity_available AS 'Доступно',
    i.min_stock_level AS 'Мінімум',
    i.warehouse_location AS 'Місце',
    CASE 
        WHEN i.quantity_available <= i.min_stock_level THEN '⚠ Потрібне поповнення'
        ELSE 'OK'
    END AS 'Статус'
FROM Inventory i
JOIN Products p ON i.product_id = p.id
LEFT JOIN Categories cat ON p.category_id = cat.id
ORDER BY i.quantity_available ASC;

-- ЗАПИТ 3: Аналіз продажів по каналах та способах оплати
SELECT 
    s.sales_channel AS 'Канал продажів',
    s.payment_method AS 'Спосіб оплати',
    COUNT(s.id) AS 'Кількість продажів',
    SUM(s.amount) AS 'Загальна сума',
    ROUND(AVG(s.amount), 2) AS 'Середня сума'
FROM Sales s
GROUP BY s.sales_channel, s.payment_method
ORDER BY SUM(s.amount) DESC;

-- ЗАПИТ 4: Статус доставки замовлень з деталями
SELECT 
    o.id AS 'Замовлення №',
    CONCAT(c.first_name, ' ', c.last_name) AS 'Клієнт',
    o.final_amount AS 'Сума',
    d.delivery_method AS 'Метод доставки',
    d.tracking_number AS 'Трекінг',
    d.status AS 'Статус доставки',
    d.city AS 'Місто',
    d.estimated_date AS 'Очікувана дата',
    d.delivery_cost AS 'Вартість доставки'
FROM Orders o
JOIN Customers c ON o.customer_id = c.id
LEFT JOIN Delivery d ON o.id = d.order_id
ORDER BY o.id;

-- ЗАПИТ 5: Фінансовий звіт (доходи і витрати)
SELECT 
    DATE_FORMAT(transaction_date, '%Y-%m') AS 'Місяць',
    transaction_type AS 'Тип операції',
    category AS 'Категорія',
    COUNT(*) AS 'Кількість операцій',
    SUM(amount) AS 'Загальна сума'
FROM Transactions
GROUP BY DATE_FORMAT(transaction_date, '%Y-%m'), transaction_type, category
ORDER BY `Місяць`, transaction_type;

-- ЗАПИТ 6: Топ товарів за кількістю продажів
SELECT 
    p.name AS 'Товар',
    p.sku AS 'Артикул',
    cat.name AS 'Категорія',
    SUM(oi.quantity) AS 'Продано (шт)',
    SUM(oi.total_price) AS 'Виручка',
    ROUND(SUM(oi.total_price) - SUM(oi.quantity * p.cost_price), 2) AS 'Валовий прибуток'
FROM Order_Items oi
JOIN Products p ON oi.product_id = p.id
LEFT JOIN Categories cat ON p.category_id = cat.id
GROUP BY p.id, p.name, p.sku, cat.name
ORDER BY SUM(oi.quantity) DESC;

-- ================================================
-- ПРЕДСТАВЛЕННЯ
-- ================================================

-- Представлення: Активні замовлення з деталями
CREATE VIEW v_ActiveOrders AS
SELECT 
    o.id AS order_id,
    CONCAT(c.first_name, ' ', c.last_name) AS customer_name,
    c.phone AS customer_phone,
    o.order_date,
    o.status,
    o.final_amount,
    d.delivery_method,
    d.status AS delivery_status
FROM Orders o
JOIN Customers c ON o.customer_id = c.id
LEFT JOIN Delivery d ON o.id = d.order_id
WHERE o.status NOT IN ('Завершено', 'Скасовано');

-- Представлення: Зведення по платежах замовлень
CREATE VIEW v_OrderPaymentSummary AS
SELECT 
    o.id AS order_id,
    o.final_amount AS total_due,
    IFNULL(SUM(p.amount), 0) AS total_paid,
    ROUND(o.final_amount - IFNULL(SUM(p.amount), 0), 2) AS balance_due
FROM Orders o
LEFT JOIN Payments p ON o.id = p.order_id AND p.status = 'Виконано'
GROUP BY o.id, o.final_amount;

-- ================================================
-- СТАТИСТИКА БД
-- ================================================

SELECT 
    table_name AS 'Таблиця',
    table_rows AS 'Кількість записів'
FROM information_schema.tables
WHERE table_schema = DATABASE()
  AND table_type = 'BASE TABLE'
ORDER BY table_rows DESC;
