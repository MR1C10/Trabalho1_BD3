-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema escola
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema escola
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `escola` DEFAULT CHARACTER SET utf8 ;
USE `escola` ;

-- -----------------------------------------------------
-- Table `escola`.`aluno`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `escola`.`aluno` (
  `id_aluno` INT NOT NULL AUTO_INCREMENT,
  `nome` VARCHAR(100) NOT NULL,
  `data_nascimento` DATE NOT NULL,
  `email` VARCHAR(100) NULL,
  `telefone` VARCHAR(20) NULL,
  PRIMARY KEY (`id_aluno`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `escola`.`professor`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `escola`.`professor` (
  `id_professor` INT NOT NULL AUTO_INCREMENT,
  `nome` VARCHAR(100) NOT NULL,
  `disciplina` VARCHAR(50) NOT NULL,
  `email` VARCHAR(100) NULL,
  `telefone` VARCHAR(20) NULL,
  PRIMARY KEY (`id_professor`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `escola`.`turma`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `escola`.`turma` (
  `id_turma` INT NOT NULL AUTO_INCREMENT,
  `curso` VARCHAR(50) NOT NULL,
  `id_professor` INT NOT NULL,
  `horario` VARCHAR(20) NOT NULL DEFAULT 'manha',
  `capacidade` INT NULL DEFAULT 30,
  PRIMARY KEY (`id_turma`, `id_professor`),
  INDEX `fk_turma_professor1_idx` (`id_professor` ASC) VISIBLE,
  CONSTRAINT `fk_turma_professor1`
    FOREIGN KEY (`id_professor`)
    REFERENCES `escola`.`professor` (`id_professor`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `escola`.`matricula`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `escola`.`matricula` (
  `id_matricula` INT NOT NULL AUTO_INCREMENT,
  `id_aluno` INT NOT NULL,
  `id_turma` INT NOT NULL,
  `data_matricula` DATE NOT NULL,
  `ativo` TINYINT(1) NULL DEFAULT 1,
  `status_final` ENUM('cursando', 'aprovado', 'reprovado', 'trancado') NULL DEFAULT 'cursando',
  PRIMARY KEY (`id_matricula`, `id_aluno`, `id_turma`),
  INDEX `fk_aluno_has_turma_turma1_idx` (`id_turma` ASC) VISIBLE,
  INDEX `fk_aluno_has_turma_aluno_idx` (`id_aluno` ASC) VISIBLE,
  CONSTRAINT `fk_aluno_has_turma_aluno`
    FOREIGN KEY (`id_aluno`)
    REFERENCES `escola`.`aluno` (`id_aluno`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_aluno_has_turma_turma1`
    FOREIGN KEY (`id_turma`)
    REFERENCES `escola`.`turma` (`id_turma`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `escola`.`nota`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `escola`.`nota` (
  `id_nota` INT NOT NULL AUTO_INCREMENT,
  `valor` DECIMAL(5,2) NOT NULL,
  `data_nota` DATE NOT NULL,
  `tipo_avaliacao` VARCHAR(50) NOT NULL,
  `id_matricula` INT NOT NULL,
  PRIMARY KEY (`id_nota`, `id_matricula`),
  INDEX `fk_nota_matricula1_idx` (`id_matricula` ASC) VISIBLE,
  CONSTRAINT `fk_nota_matricula1`
    FOREIGN KEY (`id_matricula`)
    REFERENCES `escola`.`matricula` (`id_matricula`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
