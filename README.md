# Результати нормалізації

## Початкова таблиця (ненормалізована)

| Номер_замовлення | Назва_товару і кількість | Адреса_клієнта | Дата_замовлення | Клієнт |
|------------------|--------------------------|----------------|-----------------|--------|
| 101 | Лептоп: 3, Мишка: 2 | Хрещатик 1 | 2023-03-15 | Мельник |
| 102 | Принтер: 1 | Басейна 2 | 2023-03-16 | Шевченко |
| 103 | Мишка: 4 | Комп'ютерна 3 | 2023-03-17 | Коваленко |

---

## Крок 1: Перша нормальна форма (1 НФ)
> Кожен атрибут у таблиці повинен бути атомарним.

### Замовлення

| Номер_замовлення | Назва_товару | Кількість | Адреса_клієнта | Дата_замовлення | Клієнт |
|------------------|--------------|-----------|----------------|-----------------|--------|
| 101 | Лептоп | 3 | Хрещатик 1 | 2023-03-15 | Мельник |
| 101 | Мишка | 2 | Хрещатик 1 | 2023-03-15 | Мельник |
| 102 | Принтер | 1 | Бастіон 2 | 2023-03-16 | Шевченко |
| 103 | Мишка | 4 | Комп’ютерна 3 | 2023-03-17 | Коваленко |

---

## Крок 2: Друга нормальна форма (2 НФ)
> Друга нормальна форма вимагає, щоб кожен неключовий атрибут повністю залежав від всіх ключових атрибутів, а не лише від їх підмножини

### Замовлення

| Номер_замовлення | Дата_замовлення | Клієнт | Адреса_клієнта |
|------------------|-----------------|--------|----------------|
| 101 | 2023-03-15 | Мельник | Хрещатик 1 |
| 102 | 2023-03-16 | Шевченко | Бастіон 2 |
| 103 | 2023-03-17 | Коваленко | Комп’ютерна 3 |

### Деталі_замовлення

| Номер_замовлення | Назва_товару | Кількість |
|------------------|--------------|-----------|
| 101 | Лептоп | 3 |
| 101 | Мишка | 2 |
| 102 | Принтер | 1 |
| 103 | Мишка | 4 |

---

## Крок 3: Третя нормальна форма (3 НФ)
> Третя нормальна форма вимагає, щоб всі неключові атрибути повністю залежали від ключа таблиці й не залежали від інших неключових атрибутів

### Клієнти

| id | Клієнт | Адреса_клієнта |
|----|--------|----------------|
| 1 | Мельник | Хрещатик 1 |
| 2 | Шевченко | Бастіон 2 |
| 3 | Коваленко | Комп’ютерна 3 |

### Замовлення

| id | id_клієнта | Дата_замовлення |
|----|------------|-----------------|
| 101 | 1 | 2023-03-15 |
| 102 | 2 | 2023-03-16 |
| 103 | 3 | 2023-03-17 |

### Товар

| id | Назва |
|----|-------|
| 1  | Лептоп |
| 2  | Мишка |
| 3  | Принтер |

### Деталі_замовлення

| id | id_товару | Кількість |
|----|-----------|-----------|
| 101 | 1 | 3 |
| 101 | 2 | 2 |
| 102 | 3 | 1 |
| 103 | 2 | 4 |


# ER діаграма

<img width="1122" alt="EER Diagram" src="https://github.com/user-attachments/assets/10f7ddcf-bacc-40e6-99dd-e03930921469" />

# Автоматично, використовуючи MySQL Workbench Forward Engineering створено таблиці й колонки з урахуванням зв'язків

![schema2](https://github.com/user-attachments/assets/7f40773a-5703-419f-ab80-7b9a84243dd2)
![schema1](https://github.com/user-attachments/assets/097e8606-09c6-4b1b-989f-222cdf9ffb17)


-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
DROP SCHEMA IF EXISTS `mydb` ;

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `mydb` DEFAULT CHARACTER SET utf8 ;
USE `mydb` ;

-- -----------------------------------------------------
-- Table `mydb`.`customers`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `mydb`.`customers` ;

CREATE TABLE IF NOT EXISTS `mydb`.`customers` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NOT NULL,
  `address` VARCHAR(45) NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`orders`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `mydb`.`orders` ;

CREATE TABLE IF NOT EXISTS `mydb`.`orders` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `customer_id` INT NULL,
  `order_date` DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  INDEX `customer_id_idx` (`customer_id` ASC) VISIBLE,
  CONSTRAINT `customer_id`
    FOREIGN KEY (`customer_id`)
    REFERENCES `mydb`.`customers` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`products`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `mydb`.`products` ;

CREATE TABLE IF NOT EXISTS `mydb`.`products` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(45) NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`order_details`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `mydb`.`order_details` ;
CREATE TABLE IF NOT EXISTS `mydb`.`order_details` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `order_id` INT NOT NULL,
  `product_id` INT NOT NULL,
  `quantity` INT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `unique_order_product` (`order_id` ASC, `product_id` ASC) VISIBLE,
  INDEX `product_id_idx` (`product_id` ASC) VISIBLE,
  CONSTRAINT `order_id`
    FOREIGN KEY (`order_id`)
    REFERENCES `mydb`.`orders` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `product_id`
    FOREIGN KEY (`product_id`)
    REFERENCES `mydb`.`products` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
