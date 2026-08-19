# Atividade Teórica: Regra de Negócio no BD versus na Aplicação

**Aluno(s):** Danielly Magno Barbosa, Gabriel Lima Rosa, Kaio de Lima Campana, Wellington Pereira do Nascimento.
**Turma:** Banco de Dados 2026
**Data:** ../08/2026
**Repositório Git:** https://github.com/dmagno6/atividade-bd-G1-08

## Resumo Executivo

Breve descrição do tema e da posição adotada pelo grupo.

## 1. Desenvolvimento Teórico

### 1.1 O que é regra de negócio?
Regra de negócio é uma condição, restrição ou orientação que define como uma organização deve realizar suas operações e tomar decisões. No sistema, essas regras determinam quais operações são permitidas e quais condições devem ser respeitadas.

As regras podem ser classificadas conforme sua natureza. Regras relacionadas diretamente à integridade dos dados são adequadas ao banco de dados, como a unicidade do CPF (`UNIQUE`), a obrigatoriedade de um campo (`NOT NULL`) e a integridade referencial (`FOREIGN KEY`). Já regras relacionadas ao comportamento do negócio são tipicamente tratadas na aplicação, como não permitir uma venda sem estoque, exigir idade mínima de 18 anos ou impedir que um cliente tenha dois pedidos em aberto. Essas regras podem envolver fluxos, validações e condições que podem mudar conforme as necessidades do negócio.

Assim, nem toda regra de negócio precisa estar exclusivamente no banco ou na aplicação: a escolha depende da natureza da regra e de onde sua garantia é mais adequada.

### 1.2 Regras no banco de dados
O banco de dados possui mecanismos próprios para garantir regras relacionadas à integridade e consistência dos dados. No PostgreSQL, os principais são:

`CHECK`: define uma condição que os valores devem atender. Por exemplo, impedir que a idade de um cliente seja menor que zero.
`NOT NULL`: impede que uma coluna obrigatória receba valor nulo.
`UNIQUE`: garante que determinado valor, ou combinação de valores, não se repita. Pode ser utilizado para garantir que o CPF de cada cliente seja único.
`FOREIGN KEY`: garante a integridade referencial entre tabelas, impedindo referências a registros inexistentes.
Triggers: executam automaticamente uma função quando determinados eventos ocorrem, como uma inserção, alteração ou exclusão. São úteis para regras que não podem ser expressas adequadamente por constraints simples.
Stored procedures/functions: permitem colocar operações e lógica diretamente no banco, podendo centralizar determinados comportamentos relacionados aos dados.
Transações e propriedades ACID: permitem que operações relacionadas sejam realizadas de forma confiável. ACID representa Atomicidade, Consistência, Isolamento e Durabilidade, garantindo, por exemplo, que uma operação não seja parcialmente concluída e deixe os dados em um estado inválido.

A principal vantagem de colocar regras de integridade no banco é que elas são aplicadas independentemente da aplicação que está realizando a operação. Assim, se um sistema possui diferentes aplicações acessando o mesmo banco, uma restrição `UNIQUE`, por exemplo, continuará sendo aplicada mesmo que uma das aplicações não faça essa validação corretamente. Outra vantagem é a garantia da consistência dos dados, pois o próprio banco impede que informações que violem as restrições sejam armazenadas, reduzindo o risco de dados inválidos ou inconsistentes.

Por outro lado, utilizar excessivamente lógica complexa no banco pode dificultar a manutenção e aumentar a dependência de um determinado sistema gerenciador de banco de dados. Regras de negócio que mudam frequentemente ou que possuem muitos comportamentos e condições também podem ser mais difíceis de compreender e testar quando concentradas em triggers ou procedimentos armazenados.

### 1.3 Regras na aplicação
Validação de entradas, camadas de serviço, frameworks — vantagens e limitações.

### 1.4 Comparativo BD x Aplicação
Tabela comparativa: consistência, segurança, performance, manutenção,
portabilidade, controle central da regra.

### 1.5 Análise crítica: qual a melhor opção?
Posição fundamentada do grupo e condições em que cada abordagem se aplica.

## 2. Exemplos e Casos

Exemplo em PostgreSQL (regra no BD) e exemplo de validação na aplicação
(pseudocódigo ou código). Um caso real: sistema de vendas, clínica ou biblioteca.

## 3. Referências

Fontes consultadas (livros, artigos, documentação oficial do PostgreSQL, materiais do curso).

## 4. Conclusões

Aprendizados, reflexões e principais pontos observados pelo grupo.

## Link do Repositório Git
https://github.com/dmagno6/atividade-bd-G1-08
