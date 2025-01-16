# Desafio Dio - DB Ordem de Serviço Oficina
![Static Badge](https://img.shields.io/badge/Banco-MySQL-blue?style=for-the-badge&logo=mysql&logoColor=white&color=blue)
![Static Badge](https://img.shields.io/badge/Desafio-DIO-blue?style=for-the-badge)

### Criação de Diagrama usando MySQL Wokbench

## 🥇 Desafio

### Objetivo 

<p> Criar o esquema conceitual para o contexto de oficina com base na narrativa fornecida </p>

## 📣 Narrativa:

- Sistema de controle e gerenciamento de execução de ordens de serviço em uma oficina mecânica
- Clientes levam veículos à oficina mecânica para serem consertados ou para passarem por revisões  periódicas
- Cada veículo é designado a uma equipe de mecânicos que identifica os serviços a serem executados e preenche uma OS com data de entrega.
- A partir da OS, calcula-se o valor de cada serviço, consultando-se uma tabela de referência de mão-de-obra
- O valor de cada peça também irá compor a OSO cliente autoriza a execução dos serviços
- A mesma equipe avalia e executa os serviços
- Os mecânicos possuem código, nome, endereço e especialidade
- Cada OS possui: n°, data de emissão, um valor, status e uma data para conclusão dos trabalhos.

## 🗒️ Script MySQL

~~~ mysql

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `mydb` DEFAULT CHARACTER SET utf8 ;
USE `mydb` ;

-- -----------------------------------------------------
-- Table `mydb`.`Clientes`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Clientes` (
  `idCadastroGeral` INT NOT NULL,
  `Nome` VARCHAR(90) NULL,
  `Endereço` VARCHAR(45) NULL,
  `Documento` VARCHAR(45) NULL,
  `Telefone` VARCHAR(45) NULL,
  PRIMARY KEY (`idCadastroGeral`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Equipe`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Equipe` (
  `idEquipe` INT NOT NULL,
  `Setor` VARCHAR(45) NULL,
  PRIMARY KEY (`idEquipe`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Funcionarios`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Funcionarios` (
  `idFuncionarios` INT NOT NULL,
  `Nome` VARCHAR(45) NULL,
  `Endereço` VARCHAR(45) NULL,
  `Matricula` VARCHAR(45) NULL,
  `Especialidade` VARCHAR(45) NULL,
  `Equipe_idEquipe` INT NOT NULL,
  PRIMARY KEY (`idFuncionarios`, `Equipe_idEquipe`),
  INDEX `fk_Funcionarios_Equipe_idx` (`Equipe_idEquipe` ASC) VISIBLE,
  CONSTRAINT `fk_Funcionarios_Equipe`
    FOREIGN KEY (`Equipe_idEquipe`)
    REFERENCES `mydb`.`Equipe` (`idEquipe`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`VeiculoCliente`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`VeiculoCliente` (
  `idVeiculoCliente` INT NOT NULL,
  `Modelo` VARCHAR(45) NULL,
  `Placa` VARCHAR(45) NULL,
  `Clientes_idCadastroGeral` INT NOT NULL,
  PRIMARY KEY (`idVeiculoCliente`, `Clientes_idCadastroGeral`),
  INDEX `fk_VeiculoCliente_Clientes1_idx` (`Clientes_idCadastroGeral` ASC) VISIBLE,
  CONSTRAINT `fk_VeiculoCliente_Clientes1`
    FOREIGN KEY (`Clientes_idCadastroGeral`)
    REFERENCES `mydb`.`Clientes` (`idCadastroGeral`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`TabelaPreços`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`TabelaPreços` (
  `idTabelaPreços` INT NOT NULL,
  `MaoObra` VARCHAR(45) NULL,
  `Peça` VARCHAR(45) NULL,
  `Valor` FLOAT NULL,
  PRIMARY KEY (`idTabelaPreços`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`OS_Geral`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`OS_Geral` (
  `VeiculoCliente_idVeiculoCliente` INT NOT NULL,
  `VeiculoCliente_Clientes_idCadastroGeral` INT NOT NULL,
  `Equipe_idEquipe` INT NOT NULL,
  `TabelaPreços_idTabelaPreços` INT NOT NULL,
  `Status` VARCHAR(45) NULL,
  `ValorTotal` FLOAT NULL,
  PRIMARY KEY (`VeiculoCliente_idVeiculoCliente`, `VeiculoCliente_Clientes_idCadastroGeral`, `Equipe_idEquipe`, `TabelaPreços_idTabelaPreços`),
  INDEX `fk_VeiculoCliente_has_Equipe_Equipe1_idx` (`Equipe_idEquipe` ASC) VISIBLE,
  INDEX `fk_VeiculoCliente_has_Equipe_VeiculoCliente1_idx` (`VeiculoCliente_idVeiculoCliente` ASC, `VeiculoCliente_Clientes_idCadastroGeral` ASC) VISIBLE,
  INDEX `fk_OS_Geral_TabelaPreços1_idx` (`TabelaPreços_idTabelaPreços` ASC) VISIBLE,
  CONSTRAINT `fk_VeiculoCliente_has_Equipe_VeiculoCliente1`
    FOREIGN KEY (`VeiculoCliente_idVeiculoCliente` , `VeiculoCliente_Clientes_idCadastroGeral`)
    REFERENCES `mydb`.`VeiculoCliente` (`idVeiculoCliente` , `Clientes_idCadastroGeral`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_VeiculoCliente_has_Equipe_Equipe1`
    FOREIGN KEY (`Equipe_idEquipe`)
    REFERENCES `mydb`.`Equipe` (`idEquipe`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_OS_Geral_TabelaPreços1`
    FOREIGN KEY (`TabelaPreços_idTabelaPreços`)
    REFERENCES `mydb`.`TabelaPreços` (`idTabelaPreços`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Autorização`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Autorização` (
  `OS_Geral_VeiculoCliente_idVeiculoCliente` INT NOT NULL,
  `OS_Geral_VeiculoCliente_Clientes_idCadastroGeral` INT NOT NULL,
  `OS_Geral_Equipe_idEquipe` INT NOT NULL,
  `OS_Geral_TabelaPreços_idTabelaPreços` INT NOT NULL,
  `ServiçoAutorizado` TINYINT NOT NULL,
  PRIMARY KEY (`OS_Geral_VeiculoCliente_idVeiculoCliente`, `OS_Geral_VeiculoCliente_Clientes_idCadastroGeral`, `OS_Geral_Equipe_idEquipe`, `OS_Geral_TabelaPreços_idTabelaPreços`),
  CONSTRAINT `fk_Autorização_OS_Geral1`
    FOREIGN KEY (`OS_Geral_VeiculoCliente_idVeiculoCliente` , `OS_Geral_VeiculoCliente_Clientes_idCadastroGeral` , `OS_Geral_Equipe_idEquipe` , `OS_Geral_TabelaPreços_idTabelaPreços`)
    REFERENCES `mydb`.`OS_Geral` (`VeiculoCliente_idVeiculoCliente` , `VeiculoCliente_Clientes_idCadastroGeral` , `Equipe_idEquipe` , `TabelaPreços_idTabelaPreços`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`Execução_Serviço`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`Execução_Serviço` (
  `Autorização_OS_Geral_VeiculoCliente_idVeiculoCliente` INT NOT NULL,
  `Autorização_OS_Geral_VeiculoCliente_Clientes_idCadastroGeral` INT NOT NULL,
  `Autorização_OS_Geral_Equipe_idEquipe` INT NOT NULL,
  `Autorização_OS_Geral_TabelaPreços_idTabelaPreços` INT NOT NULL,
  `Equipe_idEquipe` INT NOT NULL,
  `Data_Entrega` VARCHAR(45) NULL,
  PRIMARY KEY (`Autorização_OS_Geral_VeiculoCliente_idVeiculoCliente`, `Autorização_OS_Geral_VeiculoCliente_Clientes_idCadastroGeral`, `Autorização_OS_Geral_Equipe_idEquipe`, `Autorização_OS_Geral_TabelaPreços_idTabelaPreços`, `Equipe_idEquipe`),
  INDEX `fk_Autorização_has_Equipe_Equipe1_idx` (`Equipe_idEquipe` ASC) VISIBLE,
  INDEX `fk_Autorização_has_Equipe_Autorização1_idx` (`Autorização_OS_Geral_VeiculoCliente_idVeiculoCliente` ASC, `Autorização_OS_Geral_VeiculoCliente_Clientes_idCadastroGeral` ASC, `Autorização_OS_Geral_Equipe_idEquipe` ASC, `Autorização_OS_Geral_TabelaPreços_idTabelaPreços` ASC) VISIBLE,
  CONSTRAINT `fk_Autorização_has_Equipe_Autorização1`
    FOREIGN KEY (`Autorização_OS_Geral_VeiculoCliente_idVeiculoCliente` , `Autorização_OS_Geral_VeiculoCliente_Clientes_idCadastroGeral` , `Autorização_OS_Geral_Equipe_idEquipe` , `Autorização_OS_Geral_TabelaPreços_idTabelaPreços`)
    REFERENCES `mydb`.`Autorização` (`OS_Geral_VeiculoCliente_idVeiculoCliente` , `OS_Geral_VeiculoCliente_Clientes_idCadastroGeral` , `OS_Geral_Equipe_idEquipe` , `OS_Geral_TabelaPreços_idTabelaPreços`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Autorização_has_Equipe_Equipe1`
    FOREIGN KEY (`Equipe_idEquipe`)
    REFERENCES `mydb`.`Equipe` (`idEquipe`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

~~~
<br>

## 🖼️ Diagrama do Projeto 

<br>
<BR>

<div align="center">
  
![Diagrama](https://github.com/alecsmelo/DesafioMySQLPowerBiSuzano_2/blob/main/Diagrama_Projeto_DB_Oficina_DIO.png)

</div>

## Download Projeto

### 🔗 [Downlaod PDF](https://github.com/alecsmelo/DesafioMySQLPowerBiSuzano_2/releases/tag/PDF)



