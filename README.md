# Desafio Técnico: Bootcamp AWS AI FDE Driven Quality Engineering

Este projeto consiste em uma suíte de testes automatizados desenvolvida em **Python**, utilizando o framework **Pytest**. O objetivo principal é validar as regras de negócio e os contratos dos endpoints de *Usuários*, *Produtos*, *Login* e *Carrinhos* da API pública **ServeRest**, garantindo a integridade das informações, a resiliência do sistema e a exatidão das respostas.

---

## Evolução da Arquitetura e Refatoração (Semana 04)

Durante a quarta semana do Bootcamp, a arquitetura do projeto foi refatorada seguindo as melhores práticas de Engenharia de Qualidade para eliminar gargalos de manutenção e melhorar a legibilidade do código:

* **Modularização por Domínio de Negócio:** Reorganização dos arquivos de teste em módulos claros por contexto (Login, Usuários, Produtos e Carrinhos), abandonando abordagens monolíticas.
* **Adoção do Padrão AAA (Arrange, Act, Assert):** Remoção de blocos redundantes de `try/except` que poluíam o código. A estrutura agora segue rigorosamente o fluxo de *Organizar, Agir e Validar*, tornando os testes focados e legíveis.
* **Centralização da Base URL:** Criação de *fixtures* dedicadas para gerenciar a URL base da API, eliminando a repetição de strings nos testes e facilitando a transição entre diferentes ambientes de teste.
* **Gestão de Estado (Setup/Teardown):** Implementação de rotinas automatizadas de limpeza da base de dados (`teardown`) executadas logo após o ciclo de vida do teste (`yield`), garantindo a independência entre os cenários.
* **Testes de Contrato (Contract Testing):** Evolução das validações simples de `status_code` e chaves isoladas para uma validação estrutural completa do payload de resposta, utilizando a biblioteca `jsonschema`.

---

## Plano de Testes

### 1. Objetivo e Estratégia
* **Objetivo:** Assegurar a qualidade funcional, estabilidade de contrato e confiabilidade dos principais fluxos de negócio da API ServeRest.
* **Estratégia:** Automação de testes funcionais e de contrato na camada de Back-end (API).
* **Stack Tecnológica:** Python 3.13.x, Pytest, Requests e JSONSchema.
* **Escopo Coberto:** Endpoints `/login`, `/usuarios`, `/produtos` e `/carrinhos`.
* **Fora de Escopo:** Testes de carga, estresse e performance.

### 2. Cenários de Teste Mapeados

#### Módulo: `test_login.py` (Autenticação)
* `test_login_successfully` (POST - 200)
* `test_login_invalid_password` (POST - 401)
* `test_login_nonexistent_user` (POST - 401)
* `test_login_without_name` (POST - 400)
* `test_login_without_email` (POST - 400)
* `test_login_without_password` (POST - 400)
* `test_login_without_id` (POST - 400)

#### Módulo: `test_usuarios.py` (CRUD de Usuários)
* `test_create_user_successfully` (POST - 201)
* `test_create_user_duplicated_email` (POST - 400)
* `test_list_all_users` (GET - 200)
* `test_delete_user_successfully_by_id` (DELETE - 200 / 204)

#### Módulo: `test_produtos.py` (CRUD de Produtos)
* `test_create_product_successfully_with_admin_token` (POST - 201)
* `test_create_product_without_admin_token` (POST - 403 / 401)
* `test_create_product_without_name` (POST - 400)
* `test_create_product_without_price` (POST - 400)
* `test_create_product_without_description` (POST - 400)
* `test_search_product_by_id` (GET - 200)
* `test_update_product_with_token` (PUT - 200)
* `test_update_product_without_token` (PUT - 401)
* `test_delete_product_successfully` (DELETE - 200 / 204)

#### Módulo: `test_carrinhos.py` (Gestão de Carrinhos)
* `test_create_cart_successfully` (POST - 201)
* `test_create_cart_insufficient_stock` (POST - 400)
* `test_conclude_purchase_success` (DELETE / POST - 200)
* `test_cancel_purchase_and_stock_return` (DELETE - 200)

---

## Definition of Done (DoD)

Para garantir o padrão de qualidade e aceitação de novos cenários na suíte, cada teste deve atender aos seguintes critérios:
- [x] Nomenclatura padronizada seguindo a convenção `test_<action>_<expected_result>`.
- [x] Utilização de *fixtures* centralizadas no arquivo `conftest.py` para setup e teardown.
- [x] Massa de dados dinâmica e isolada utilizando randomização (ex: biblioteca UUID).
- [x] Validação estrutural completa de contrato via `JSON Schema`.

---

## Estrutura do Projeto

```text
├── tests/                                      
│   ├── conftest.py         # Configurações globais e fixtures do Pytest
│   ├── schemas.py          # Definições dos JSON Schemas para testes de contrato
│   ├── test_login.py       # Cenários de teste de autenticação
│   ├── test_produtos.py    # Cenários de teste do módulo de produtos
│   ├── test_usuarios.py    # Cenários de teste do módulo de usuários
│   └── test_carrinhos.py   # Cenários de teste do módulo de carrinhos

```

---

## Métricas e Resultados

* **Cobertura de Testes:** *[Inserir porcentagem de cobertura ou status de execução]*
* **Bugs Encontrados:** *[Listar eventuais falhas identificadas na API pública durante as validações]*
* **Conclusão:** *[Breve resumo sobre a estabilidade observada nos endpoints avaliados]*

---

## Referências Utilizadas em Consultas

* [AAA Pattern in Test Automation - Semaphore CI](https://semaphore.io/blog/aaa-pattern-test-automation)
* [Pytest Documentation & Best Practices](https://docs.pytest.org/)

---

## Como Executar este Repositório

1. **Clone o repositório:**
```bash
git clone [https://github.com/codebyfernanda/bootcamp-QA-2026-desafios.git](https://github.com/codebyfernanda/bootcamp-QA-2026-desafios.git)
cd bootcamp-QA-2026-desafios

```

2. **Crie e ative um ambiente virtual:**

```bash
# Windows
python -m venv venv
venv\Scripts\activate

```

```
# Linux/macOS
python -m venv venv
source venv/bin/activate

```

3. **Instale as dependências:**

```bash
pip install -r requirements.txt

```

4. **Execute os testes:**

```bash
pytest -v

```
---


## Sobre a Autora

O presente projeto foi desenvolvido por **Fernanda Bastos dos Santos** [(@codebyfernanda)](https://github.com/codebyfernanda), estudante de **Análise e Desenvolvimento de Sistemas** no Mackenzie, durante o **BOOTCAMP | AWS AI FDE DRIVEN QUALITY ENGINEERING**, da Compass UOL com [AI/R Company](https://aircompany.ai/).

## Agradecimentos Especiais
Gostaria de expressar meus agradecimentos à Squad 2 pelo engajamento, por toda troca de conhecimento e, também, pelo suporte durante o processo do nosso Bootcamp. Deixo aqui um agradecimento especial aos meus colegas [Renan Pacheco](https://github.com/Renanpacheco) e [Vitor Kunicki](https://github.com/vitto2099). A paciência, a disponibilidade e o auxílio técnico de vocês foram fundamentais para a superação dos desafios dessa entrega. Muito obrigada! :)
