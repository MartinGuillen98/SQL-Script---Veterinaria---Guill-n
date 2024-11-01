# SQL-Script---Veterinaria---Guillen
Este repositorio fue creado para publicar el script de SQL implementado en la creación de la base de datos "veterinaria". Esta última es creada como parte del proyecto final del curso de SQL Flex de CODERHOUSE.

Código SQL:

-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
-- -----------------------------------------------------
-- Schema veterinaria
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema veterinaria
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `veterinaria` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci ;
USE `veterinaria` ;

-- -----------------------------------------------------
-- Table `veterinaria`.`empleados`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`empleados` (
  `id_empleado` INT NOT NULL AUTO_INCREMENT,
  `nombre_empleado` VARCHAR(45) NULL DEFAULT NULL,
  `especialidad` VARCHAR(45) NULL DEFAULT NULL,
  `fecha_contratacion` DATE NULL DEFAULT NULL,
  PRIMARY KEY (`id_empleado`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`historial_medico`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`historial_medico` (
  `id_historial` INT NOT NULL AUTO_INCREMENT,
  `diagnostico` VARCHAR(45) NULL DEFAULT NULL,
  `tratamiento` VARCHAR(45) NULL DEFAULT NULL,
  `fecha_diagnostico` DATE NULL DEFAULT NULL,
  PRIMARY KEY (`id_historial`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`mascotas`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`mascotas` (
  `id_mascota` INT NOT NULL AUTO_INCREMENT,
  `nombre_mascota` VARCHAR(45) NULL DEFAULT NULL,
  `tipo_mascota` VARCHAR(45) NULL DEFAULT NULL,
  `fecha_nacimiento` DATE NULL DEFAULT NULL,
  `peso_mascota` DECIMAL(20,0) NULL DEFAULT NULL,
  `id_historial` INT NOT NULL,
  PRIMARY KEY (`id_mascota`),
  INDEX `fk_mascotas_historial_idx` (`id_historial` ASC) VISIBLE,
  CONSTRAINT `fk_mascotas_historial`
    FOREIGN KEY (`id_historial`)
    REFERENCES `veterinaria`.`historial_medico` (`id_historial`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`citas`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`citas` (
  `id_cita` INT NOT NULL AUTO_INCREMENT,
  `id_mascota` INT NOT NULL,
  `fecha_cita` DATETIME NULL DEFAULT NULL,
  `id_empleado` INT NOT NULL,
  PRIMARY KEY (`id_cita`),
  INDEX `fk_cita_mascota_idx` (`id_mascota` ASC) VISIBLE,
  INDEX `fk_cita_empleado_idx` (`id_empleado` ASC) VISIBLE,
  CONSTRAINT `fk_cita_empleado`
    FOREIGN KEY (`id_empleado`)
    REFERENCES `veterinaria`.`empleados` (`id_empleado`),
  CONSTRAINT `fk_cita_mascota`
    FOREIGN KEY (`id_mascota`)
    REFERENCES `veterinaria`.`mascotas` (`id_mascota`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`clientes`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`clientes` (
  `id_cliente` INT NOT NULL AUTO_INCREMENT,
  `nombre_cliente` VARCHAR(45) NULL DEFAULT NULL,
  `direccion` VARCHAR(45) NULL DEFAULT NULL,
  `telefono` VARCHAR(15) NULL DEFAULT NULL,
  PRIMARY KEY (`id_cliente`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`clientes_mascotas`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`clientes_mascotas` (
  `id_cliente_mascota` INT NOT NULL AUTO_INCREMENT,
  `id_cliente` INT NULL DEFAULT NULL,
  `id_mascota` INT NULL DEFAULT NULL,
  PRIMARY KEY (`id_cliente_mascota`),
  INDEX `fk_clientesmascotas_clientes_idx` (`id_cliente` ASC) VISIBLE,
  INDEX `fk_clientesmascotas_mascotas_idx` (`id_mascota` ASC) VISIBLE,
  CONSTRAINT `fk_clientesmascotas_clientes`
    FOREIGN KEY (`id_cliente`)
    REFERENCES `veterinaria`.`clientes` (`id_cliente`),
  CONSTRAINT `fk_clientesmascotas_mascotas`
    FOREIGN KEY (`id_mascota`)
    REFERENCES `veterinaria`.`mascotas` (`id_mascota`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`facturas`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`facturas` (
  `id_factura` INT NOT NULL AUTO_INCREMENT,
  `monto_total` DECIMAL(10,2) NULL DEFAULT NULL,
  `fecha_factura` DATE NULL DEFAULT NULL,
  PRIMARY KEY (`id_factura`))
ENGINE = InnoDB
AUTO_INCREMENT = 2
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`items`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`items` (
  `id_item` INT NOT NULL AUTO_INCREMENT,
  `tipo_item` VARCHAR(45) NULL DEFAULT NULL,
  `descripcion_item` VARCHAR(45) NULL DEFAULT NULL,
  PRIMARY KEY (`id_item`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`metodos_de_pago`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`metodos_de_pago` (
  `id_metodo_pago` INT NOT NULL AUTO_INCREMENT,
  `descripcion_metodo` VARCHAR(45) NULL DEFAULT NULL,
  PRIMARY KEY (`id_metodo_pago`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`proveedores`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`proveedores` (
  `id_proveedor` INT NOT NULL AUTO_INCREMENT,
  `nombre_proveedor` VARCHAR(45) NULL DEFAULT NULL,
  PRIMARY KEY (`id_proveedor`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`proveedores_items`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`proveedores_items` (
  `id_proveedor_item` INT NOT NULL AUTO_INCREMENT,
  `id_proveedor` INT NULL DEFAULT NULL,
  `id_item` INT NULL DEFAULT NULL,
  `costo_unitario` DECIMAL(10,2) NULL DEFAULT NULL,
  `cantidad_disponible` DECIMAL(10,2) NULL DEFAULT NULL,
  `fecha_suministro` DATE NULL DEFAULT NULL,
  PRIMARY KEY (`id_proveedor_item`),
  INDEX `fk_proveedoresitem_proveedores_idx` (`id_proveedor` ASC) VISIBLE,
  INDEX `fk_proveedoresitem_items_idx` (`id_item` ASC) VISIBLE,
  CONSTRAINT `fk_proveedoresitems_items`
    FOREIGN KEY (`id_item`)
    REFERENCES `veterinaria`.`items` (`id_item`),
  CONSTRAINT `fk_proveedoresitems_proveedores`
    FOREIGN KEY (`id_proveedor`)
    REFERENCES `veterinaria`.`proveedores` (`id_proveedor`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`ventas`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`ventas` (
  `id_venta` INT NOT NULL AUTO_INCREMENT,
  `fecha_venta` DATE NULL DEFAULT NULL,
  `total_venta` DECIMAL(10,2) NULL DEFAULT NULL,
  `id_factura` INT NULL DEFAULT NULL,
  `id_empleado` INT NULL DEFAULT NULL,
  `id_cliente` INT NULL DEFAULT NULL,
  `id_metodo_pago` INT NULL DEFAULT NULL,
  PRIMARY KEY (`id_venta`),
  INDEX `ventas_facturas_idx` (`id_factura` ASC) VISIBLE,
  INDEX `fk_ventas_empleados_idx` (`id_empleado` ASC) VISIBLE,
  INDEX `fl_ventas_clientes_idx` (`id_cliente` ASC) VISIBLE,
  CONSTRAINT `fk_ventas_clientes`
    FOREIGN KEY (`id_cliente`)
    REFERENCES `veterinaria`.`clientes` (`id_cliente`),
  CONSTRAINT `fk_ventas_empleados`
    FOREIGN KEY (`id_empleado`)
    REFERENCES `veterinaria`.`empleados` (`id_empleado`),
  CONSTRAINT `fk_ventas_facturas`
    FOREIGN KEY (`id_factura`)
    REFERENCES `veterinaria`.`facturas` (`id_factura`))
ENGINE = InnoDB
AUTO_INCREMENT = 2
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`ventas_items`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`ventas_items` (
  `id_venta_item` INT NOT NULL AUTO_INCREMENT,
  `id_venta` INT NULL DEFAULT NULL,
  `id_item` INT NULL DEFAULT NULL,
  `cantidad` INT NULL DEFAULT NULL,
  `precio_unitario` DECIMAL(10,2) NULL DEFAULT NULL,
  PRIMARY KEY (`id_venta_item`),
  INDEX `fk_ventasitem_ventas_idx` (`id_venta` ASC) VISIBLE,
  INDEX `fk_ventasitem_item_idx` (`id_item` ASC) VISIBLE,
  CONSTRAINT `fk_ventasitems_items`
    FOREIGN KEY (`id_item`)
    REFERENCES `veterinaria`.`items` (`id_item`),
  CONSTRAINT `fk_ventasitems_ventas`
    FOREIGN KEY (`id_venta`)
    REFERENCES `veterinaria`.`ventas` (`id_venta`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


-- -----------------------------------------------------
-- Table `veterinaria`.`ventas_metodosdepago`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `veterinaria`.`ventas_metodosdepago` (
  `id_venta_metododepago` INT NOT NULL AUTO_INCREMENT,
  `id_venta` INT NULL DEFAULT NULL,
  `id_metodo_pago` INT NULL DEFAULT NULL,
  PRIMARY KEY (`id_venta_metododepago`),
  INDEX `fk_ventasmetodopago_ventas_idx` (`id_venta` ASC) VISIBLE,
  INDEX `fk_ventasmetodopago_metodopago_idx` (`id_metodo_pago` ASC) VISIBLE,
  CONSTRAINT `fk_ventasmetodopago_metodopago`
    FOREIGN KEY (`id_metodo_pago`)
    REFERENCES `veterinaria`.`metodos_de_pago` (`id_metodo_pago`),
  CONSTRAINT `fk_ventasmetodopago_ventas`
    FOREIGN KEY (`id_venta`)
    REFERENCES `veterinaria`.`ventas` (`id_venta`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
