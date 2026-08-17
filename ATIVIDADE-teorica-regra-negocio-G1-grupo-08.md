# Atividade Teórica: Regra de Negócio no BD versus na Aplicação

**Aluno(s):** Danielly Magno Barbosa, Gabriel Lima Rosa, Kaio de Lima Campana, Wellington Pereira do Nascimento.
**Turma:** Banco de Dados 2026
**Data:** ../08/2026
**Repositório Git:** https://github.com/dmagno6/atividade-bd-G1-08

## Resumo Executivo

Breve descrição do tema e da posição adotada pelo grupo.

## 1. Desenvolvimento Teórico

### 1.1 O que é regra de negócio?
Definição e tipos de regras.

### 1.2 Regras no banco de dados
Constraints (CHECK, FK, UNIQUE), triggers, stored procedures, transações ACID —

vantagens e limitações.

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
| Critério | Banco de Dados | Aplicação |
|---|---|---|
| Consistência | Assegura a integridade dos dados. | Vai depender das regras aplicadas no código. |
| Segurança | Pode impedir dados inválidos. | Pode validar dados antes mesmo de seguirem para o banco. |
| Performance | Eficiente para constraints simples. | Operações desnecessárias no banco podem ser evitadas. |
| Manutenção | Simples para constraints, por outro lado, pode ser complexa com triggers. | Costuma ser mais fácil para regras complexas. |
| Portabilidade | Pode variar conforme o SGBD utilizado. | Pode ser mais independente do banco. |
| Controle da regra | Deixa regras centralizadas para aplicações diferentes. | Entre aplicações podem haver diferentes regras. | 

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
