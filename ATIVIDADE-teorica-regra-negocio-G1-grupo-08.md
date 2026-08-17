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
Validação de entradas, camadas de serviço, frameworks — vantagens e limitações.

### 1.4 Comparativo BD x Aplicação
Tabela comparativa: consistência, segurança, performance, manutenção,
portabilidade, controle central da regra.

### 1.5 Análise crítica: qual a melhor opção?
Posição fundamentada do grupo e condições em que cada abordagem se aplica.

## 2. Exemplos e Casos

Regra no SQL:

- Regra de Negócio: O estoque de um produto nunca pode ser negativo (>=0) e o preço deve ser maior que zero (>0).
  
  CREATE TABLE produtos (
      id SERIAL PRIMARY KEY,
      nome VARCHAR(100) NOT NULL,
      preco NUMERIC(10, 2) NOT NULL,
      estoque INT NOT NULL,
      CONSTRAINT ver_preco_positivo CHECK (preco > 0),
      CONSTRAINT ver_estoque_nao_negativo CHECK (estoque >= 0)
      );
      
Exemplo da Validação em Código (Python):

  def registrar_venda(produto, quantidade):
    if quantidade <= 0:
        return "Quantidade inválida."
    if produto.estoque < quantidade:
        return "Estoque insuficiente."
    produto.estoque -= quantidade
    salvar_no_banco(produto)
    return "Venda concluída."

## 3. Referências

Fontes consultadas (livros, artigos, documentação oficial do PostgreSQL, materiais do curso).

## 4. Conclusões

Aprendizados, reflexões e principais pontos observados pelo grupo.

## Link do Repositório Git
https://github.com/dmagno6/atividade-bd-G1-08
