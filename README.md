# Desafio Dio - DB Ordem de Serviço Oficina
![Static Badge](https://img.shields.io/badge/Banco-MySQL-blue?style=for-the-badge&logo=mysql&logoColor=white&color=blue)
![Static Badge](https://img.shields.io/badge/Desafio-DIO-blue?style=for-the-badge)

### Criação de Diagrama usando MySQL Wokbench

## 🥇 Desafio

### Objetivo 

<p> Cria o esquema conceitual para o contexto de oficina com base na narrativa fornecida </p>

## 📣 Narrativa:

- Sistema de controle e gerenciamento de execução de ordens de serviço em uma oficina mecânica
- Clientes levam veículos à oficina mecânica para serem consertados ou para passarem por revisões  periódicas
- Cada veículo é designado a uma equipe de mecânicos que identifica os serviços a serem executados e preenche uma OS com data de entrega.
- A partir da OS, calcula-se o valor de cada serviço, consultando-se uma tabela de referência de mão-de-obra
- O valor de cada peça também irá compor a OSO cliente autoriza a execução dos serviços
- A mesma equipe avalia e executa os serviços
- Os mecânicos possuem código, nome, endereço e especialidade
- Cada OS possui: n°, data de emissão, um valor, status e uma data para conclusão dos trabalhos.
