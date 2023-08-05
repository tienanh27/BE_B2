SQL Script: 

USE db_node33;

CREATE TABLE food_type(
	type_id INT PRIMARY KEY AUTO_INCREMENT,
	type_name VARCHAR(200)
);

CREATE TABLE food(
	food_id INT PRIMARY KEY AUTO_INCREMENT,
	food_name VARCHAR(200),
	image VARCHAR(200),
	price FLOAT,
	description VARCHAR(200),
	type_id INT,
	
	FOREIGN KEY (type_id) REFERENCES food_type(type_id)
)

CREATE TABLE user (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    full_name VARCHAR(200),
    email VARCHAR(200),
    password VARCHAR(200)
);

CREATE TABLE restaurant (
    res_id INT PRIMARY KEY AUTO_INCREMENT,
    res_name VARCHAR(200),
    image VARCHAR(200),
    description VARCHAR(200)
);

CREATE TABLE rate_res (
    user_id INT,
    res_id INT,
    amount INT,
    date_rate DATETIME,
    
    PRIMARY KEY (user_id, res_id),
    FOREIGN KEY (user_id) REFERENCES user(user_id),
    FOREIGN KEY (res_id) REFERENCES restaurant(res_id)
);

CREATE TABLE like_res (
    user_id INT,
    res_id INT,
    date_like DATETIME,
    PRIMARY KEY (user_id, res_id),
    FOREIGN KEY (user_id) REFERENCES user(user_id),
    FOREIGN KEY (res_id) REFERENCES restaurant(res_id)
);

CREATE TABLE sub_food (
    sub_id INT PRIMARY KEY AUTO_INCREMENT,
    sub_name VARCHAR(200),
    sub_price FLOAT,
    food_id INT,
    FOREIGN KEY (food_id) REFERENCES food(food_id)
); 

CREATE TABLE `order` (
    user_id INT,
    food_id INT,
    amount INT,
    code VARCHAR(100),
    arr_sub_id TEXT,
    PRIMARY KEY (user_id, food_id),
    FOREIGN KEY (user_id) REFERENCES user(user_id),
    FOREIGN KEY (food_id) REFERENCES food(food_id)
);

DELETE FROM user WHERE user_id IN (4, 5, 6, 7);

INSERT INTO food_type (type_id, type_name) VALUES
(0, 'Món chính'),
(0, 'Món phụ'),
(0, 'Tráng miệng'),
(0, 'Đồ uống'),
(0, 'Salad');

INSERT INTO food (food_name, image, price, description, type_id) VALUES
('Phở bò', 'pho_bo.jpg', 7.5, 'Món phở bò truyền thống', 1),
('Gà nướng', 'ga_nuong.jpg', 8.2, 'Gà nướng hương thơm', 1),
('Cơm gà', 'com_ga.jpg', 6.8, 'Cơm gà kiểu Hàn Quốc', 1),
('Gỏi cuốn', 'goi_cuon.jpg', 5.0, 'Gỏi cuốn tươi ngon', 2),
('Salad trái cây', 'salad_trai_cay.jpg', 4.5, 'Salad trái cây tự nhiên', 5),
('Bánh flan', 'banh_flan.jpg', 3.2, 'Bánh flan mềm mịn', 3),
('Nước cam', 'nuoc_cam.jpg', 2.0, 'Nước cam tươi', 4);

INSERT INTO user (full_name, email, password) VALUES
('John Doe', 'john.doe@example.com', 'password123'),
('Jane Smith', 'jane.smith@example.com', 'securepass'),
('Mike Johnson', 'mike.johnson@example.com', 'mypass123'),
('Mike Tike', 'dectike@example.com', 'mypass123'),
('Johnson', 'abc@example.com', 'mypass123'),
('Micheal', 'micheal@example.com', 'mypass123'),
('Dre', '123@example.com', 'mypass123');

INSERT INTO restaurant (res_name, image, description) VALUES
('Nhà hàng ABC', 'nhahang_abc.jpg', 'Nhà hàng phục vụ các món ăn truyền thống'),
('Nhà hàng NGON', 'nhahang_ngon.jpg', 'Nhà hàng chuyên về hải sản và ẩm thực địa phương'),
('Nhà hàng 123', 'nhahang_123.jpg', 'Nhà hàng sang trọng phục vụ các món ăn đa dạng'),
('Nhà hàng VUI', 'nhahang_vui.jpg', 'Nhà hàng với không gian thoải mái và dịch vụ chất lượng');

INSERT INTO rate_res (user_id, res_id, amount, date_rate) VALUES
(1, 1, 4, '2023-08-06 10:30:00'),
(1, 2, 5, '2023-08-05 14:15:00'),
(2, 1, 3, '2023-08-06 12:45:00'),
(3, 3, 4, '2023-08-04 18:20:00'),
(3, 2, 4, '2023-08-05 09:00:00');


INSERT INTO like_res (user_id, res_id, date_like) VALUES
(1, 1, '2023-08-06 08:30:00'),
(1, 2, '2023-08-05 12:15:00'),
(2, 1, '2023-08-06 11:45:00'),
(3, 3, '2023-08-04 17:20:00'),
(3, 2, '2023-08-05 08:30:00');

INSERT INTO sub_food (sub_name, sub_price, food_id) VALUES
('Trà chanh', 2.0, 1),
('Khoai tây chiên', 3.5, 1),
('Salad trái cây', 4.0, 2),
('Soup hải sản', 6.2, 2),
('Bánh flan', 3.0, 3),
('Chè hạt sen', 2.5, 3);

INSERT INTO `order` (user_id, food_id, amount, code, arr_sub_id) VALUES
(1, 1, 2, 'ORDER123', '[{"sub_id": 1}, {"sub_id": 2}]'),
(1, 3, 1, 'ORDER456', '[{"sub_id": 5}]'),
(2, 2, 3, 'ORDER789', '[{"sub_id": 4}, {"sub_id": 6}]'),
(3, 1, 1, 'ORDERABC', '[{"sub_id": 1}]'),
(3, 3, 2, 'ORDERXYZ', '[{"sub_id": 3}, {"sub_id": 5}]');

#Question 1
SELECT user_id, COUNT(res_id) AS total_likes
FROM like_res
GROUP BY user_id
ORDER BY total_likes DESC
LIMIT 5;

#Question 2
SELECT res_id, COUNT(user_id) AS total_likes
FROM like_res
GROUP BY res_id
ORDER BY total_likes DESC
LIMIT 2;

#Question 3
SELECT user_id, COUNT(*) AS total_orders
FROM `order`
GROUP BY user_id
ORDER BY total_orders DESC
LIMIT 1;
