-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema Notas
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema Notas
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `Notas` DEFAULT CHARACTER SET utf8 ;
USE `Notas` ;

-- -----------------------------------------------------
-- Table `Notas`.`usuario`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Notas`.`usuario` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(255) NOT NULL,
  `surname` VARCHAR(255) NOT NULL,
  `email` VARCHAR(255) NOT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Notas`.`notas`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Notas`.`notas` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `title` VARCHAR(100) NOT NULL,
  `creation` DATE NOT NULL,
  `modification` DATE NOT NULL,
  `delete` INT NOT NULL,
  `description` TEXT NOT NULL,
  `idNotas` INT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_notas_usuarios_idx` (`idNotas` ASC) VISIBLE,
  CONSTRAINT `fk_notas_usuarios`
    FOREIGN KEY (`idNotas`)
    REFERENCES `Notas`.`usuario` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Notas`.`mecanismo`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Notas`.`mecanismo` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `notRemoved` INT NULL,
  `deleted` INT NULL,
  `decisionId` INT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_decision_notas_idx` (`decisionId` ASC) VISIBLE,
  CONSTRAINT `fk_decision_notas`
    FOREIGN KEY (`decisionId`)
    REFERENCES `Notas`.`notas` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Notas`.`nota vacio`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Notas`.`nota vacio` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `notasNuevas` TEXT NOT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Notas`.`nota informacion`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Notas`.`nota informacion` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `music` INT NOT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Notas`.`categorias`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Notas`.`categorias` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `musicId` INT NULL,
  `vacioId` INT NULL,
  `mecanismoId` INT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_categorias_notaVacias_idx` (`vacioId` ASC) VISIBLE,
  INDEX `fk_categorias_notaInfo_idx` (`musicId` ASC) VISIBLE,
  INDEX `fk_categorias_mecanismo_idx` (`mecanismoId` ASC) VISIBLE,
  CONSTRAINT `fk_categorias_mecanismo`
    FOREIGN KEY (`mecanismoId`)
    REFERENCES `Notas`.`mecanismo` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_categorias_notaVacias`
    FOREIGN KEY (`vacioId`)
    REFERENCES `Notas`.`nota vacio` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_categorias_notaInfo`
    FOREIGN KEY (`musicId`)
    REFERENCES `Notas`.`nota informacion` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;