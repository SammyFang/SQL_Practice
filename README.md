# SQL_Practice
Basic_SQL_Practice

### 建置SQL資料庫
* 需求：
使用MySQL語法於Database"user37"中一次建立"employees"與"salary"兩個tabel。
"employees"須包含：username,email,password,gender,position,years_of_service,onboard_day,recruitment_source,education,is_manager,department,supervisor,birthday。
username,email自employee0001開始連續不重複編，password長度為12並使用HEX隨機編，gender為male,female,other隨機編、position從01到08隨機編。
salary_test表格，欄位包含employee_id (外鍵),salary,insurse，一次新增90筆。

```
-- 建立employees表格
CREATE TABLE employees (
  username VARCHAR(20) NOT NULL PRIMARY KEY,
  email VARCHAR(50) NOT NULL UNIQUE,
  password VARCHAR(12) NOT NULL,
  gender ENUM('male', 'female', 'other') NOT NULL,
  position VARCHAR(2) NOT NULL,
  years_of_service INT NOT NULL,
  onboard_day DATE NOT NULL,
  recruitment_source VARCHAR(20) NOT NULL,
  education VARCHAR(10) NOT NULL,
  is_manager BOOLEAN NOT NULL,
  department VARCHAR(20) NOT NULL,
  supervisor VARCHAR(20) NOT NULL,
  birthday DATE NOT NULL
);

-- 建立salary表格
CREATE TABLE salary (
  username VARCHAR(20) NOT NULL PRIMARY KEY,
  salary INT NOT NULL,
  insurse tinyint(1)   NOT NULL
);

-- 新增90筆資料
SET @user_num = 0; -- 設定起始的user number
INSERT INTO employees (username, email, password, gender, position, years_of_service, onboard_day, recruitment_source, education, is_manager, department, supervisor, birthday)
SELECT CONCAT('employee', LPAD(@user_num := @user_num + 1, 4, '0'), ''), -- 生成username
       CONCAT('user', LPAD(@user_num, 4, '0'), '@example.com'), -- 生成email
       SUBSTRING(MD5(RAND()), 1, 12), -- 生成password
       IF(ROUND(RAND()), 'male', IF(ROUND(RAND()), 'female', 'other')), -- 隨機生成gender
       LPAD(ROUND(RAND() * 7 + 1), 2, '0'), -- 隨機生成position
       ROUND(RAND() * 10), -- 隨機生成years_of_service
       DATE_ADD('2023-03-21', INTERVAL ROUND(RAND() * 365) DAY), -- 隨機生成onboard_day
       CONCAT('source', LPAD(ROUND(RAND() * 20), 2, '0')), -- 隨機生成recruitment_source
       IF(ROUND(RAND()), 'high school', IF(ROUND(RAND()), 'college', 'master')), -- 隨機生成education
       ROUND(RAND()), -- 隨機生成is_manager
       CONCAT('Department', LPAD(ROUND(RAND() * 5 + 1), 2, '0')), -- 隨機生成department
       CONCAT('employee', LPAD(ROUND(RAND() * (@user_num - 1) + 1), 4, '0'), ''), -- 隨機生成supervisor
       DATE_SUB('2003-03-21', INTERVAL ROUND(RAND() * 365 * 40) DAY) -- 隨機生成birthday
FROM information_schema.tables
WHERE @user_num < 90; -- 新增90筆資料
-- 新增90筆資料
INSERT INTO salary (employee_id, salary, insurance)
SELECT id, 
       ROUND(RAND() * 10000 + 30000, 2), 
       FLOOR(RAND() * 2)
FROM employees
LIMIT 90;
```
結果截圖：
![](https://i.imgur.com/DQbIohy.png)
