# Atividade Teórica: Regra de Negócio no BD versus na Aplicação

**Aluno(s):** Danielly Magno Barbosa, Gabriel Lima Rosa, Kaio de Lima Campana, Wellington Pereira do Nascimento.
**Turma:** Banco de Dados 2026
**Data:** ../08/2026
**Repositório Git:** https://github.com/dmagno6/atividade-bd-G1-08

## Resumo Executivo

  Este trabalho analisa as estratégias de implementação de regras de negócio em sistemas computacionais, comparando a validação aplicada na camada de Banco de Dados com a executada na camada da Aplicação. O estudo avalia os impactos de cada abordagem em termos de consistência, segurança, desempenho, manutenção e experiência do usuário.

  Por meio de um estudo de caso aplicado a um sistema de vendas, com foco específico no controle de estoque e preços, são demonstrados exemplos práticos utilizando restrições no PostgreSQL e lógica condicional em Python. Conclui-se que a abordagem híbrida é a mais eficiente, pois a aplicação atua como primeira barreira para prover respostas amigáveis e fluxos dinâmicos ao usuário, enquanto o banco de dados serve como a camada final e mandatória para assegurar a integridade estrutural e a consistência dos dados.

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
Geralmente na aplicação, as regras de negócio ficam nas camadas responsáveis pela lógica do sistema, principalmente a camada de serviço. Antes dos dados irem para o banco, a aplicação pode fazer validações para verificar se o usuário passou informações válidas.

Por exemplo, para cadastrar um cliente, pode verificar o nome, CPF e a idade:
```
def cadastrar_cliente(cliente):
    if cliente["nome"] == "":
        return "Nome é obrigatório"

    if not cpf_valido(cliente["cpf"]):
        return "CPF inválido"

    if cliente["idade"] < 18:
        return "Cliente deve possuir pelo menos 18 anos"

    salvar_cliente(cliente)
    return "Cliente cadastrado com sucesso"
```
Em uma camada de serviço pode-se controlar as regras como cálculos de descontos, condições de frete, aprovação de pedidos e comportamentos diferentes de acordo com o tipo de cliente.

Frameworks como Spring Boot, Django, Laravel e ASP.NET Core, podem ajudar na validação dos dados, execução da lógica de negócio e na organização das camadas, facilitando o desenvolvimento e manutenção das regras na aplicação.

Vantagens:
- Flexibilidade: Facilita a implementação de regras complexas,  condições, cálculos e diferentes fluxos.
- Melhor experiência para o usuário: A aplicação valida os dados antes de mandá-los para o banco e apresenta claras mensagens de erro.
- Facilidade para alterações: A volatilidade de regras, como campanhas comerciais e descontos, podem ser alterados no código sem precisar alterar a estrutura do banco diretamente.

Desvantagens:
- Risco de inconsistência: Outra aplicação que acessar o mesmo banco pode não fazer a mesma validação.
- Problemas de concorrência: Diferentes aplicações podem mudar os mesmos dados ao mesmo tempo.
- Dependência da aplicação: Alterações feitas diretamente no banco podem ignorar as regras que já existem somente no código.

Desse modo, a aplicação é mais apropriada para regras atreladas aos comportamentos, fluxos, cálculos e validações, porém, ela não deve ser a única proteção para regras fundamentais de integridade dos dados.


### 1.4 Comparativo BD x Aplicação
Tabela comparativa: consistência, segurança, performance, manutenção,
portabilidade, controle central da regra.

### 1.5 Análise crítica: qual a melhor opção?
Posição fundamentada do grupo e condições em que cada abordagem se aplica.

## 2. Exemplos e Casos

Regra no SQL:

- Regra de Negócio: O estoque de um produto nunca pode ser negativo (>=0) e o preço deve ser maior que zero (>0).
  
 ```sql
CREATE TABLE produtos (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    preco NUMERIC(10, 2) NOT NULL,
    estoque INT NOT NULL,
    CONSTRAINT chk_preco_positivo CHECK (preco > 0),
    CONSTRAINT chk_estoque_nao_negativo CHECK (estoque >= 0)
);
```
      
Exemplo da Validação em Código (Python):

  ```python
def registrar_venda(produto, quantidade):
    if quantidade <= 0:
        return "Quantidade inválida"
    
    if produto.estoque < quantidade:
        return "Estoque insuficiente"
    
    produto.estoque -= quantidade
    salvar_no_banco(produto)
    return "Venda concluída com sucesso"
```

## 3. Referências

Fontes consultadas (livros, artigos, documentação oficial do PostgreSQL, materiais do curso).
IBM. O que são regras de negócios? Disponível em: https://www.ibm.com/br-pt/think/topics/business-rules. Acesso em: 15 ago. 2026.

POSTGRESQL GLOBAL DEVELOPMENT GROUP. 5.5. Constraints. Disponível em: https://www.postgresql.org/docs/current/ddl-constraints.html. Acesso em: 15 ago. 2026.

POSTGRESQL GLOBAL DEVELOPMENT GROUP. Chapter 5. Data Definition. Disponível em: https://www.postgresql.org/docs/current/ddl.html. Acesso em: 15 ago. 2026.

## 4. Conclusões

Aprendizados, reflexões e principais pontos observados pelo grupo.

## Link do Repositório Git
https://github.com/dmagno6/atividade-bd-G1-08
