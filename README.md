# Desafio Técnico: Bootcamp AWS AI FDE Driven Quality Engineering

Este projeto consiste em uma suíte de testes automatizados desenvolvida em **Python**, utilizando o framework **Pytest**. O objetivo principal é validar as regras de negócio e os contratos dos endpoints de *Usuários*, *Produtos*, *Login* e *Carrinhos* da API pública **ServeRest**, garantindo a integridade das informações, a resiliência do sistema e a exatidão das respostas.

---

## Evolução da Arquitetura e Refatoração (Semana 04)

Durante a quarta semana do Bootcamp, a arquitetura do projeto foi refatorada seguindo as melhores práticas de Engenharia de Qualidade para eliminar gargalos de manutenção e melhorar a legibilidade do código:

* **Modularização por Domínio de Negócio:** Reorganização dos arquivos de teste em módulos claros por contexto (Login, Usuários, Produtos e Carrinhos), abandonando abordagens monolíticas.
* **Adoção do Padrão AAA (Arrange, Act, Assert):** Remoção de blocos redundantes de `try/except` que poluíam o código. A estrutura agora segue rigorosamente o fluxo de *Organizar, Agir e Validar*, tornando os testes focados e legíveis.
* **Centralização da Base URL:** Criação de *fixtures* dedicadas para gerenciar a URL base da API, eliminando a repetição de strings nos testes e facilitando a transição entre diferentes ambientes de teste.
* **Gestão de Estado (Setup/Teardown):** Implementação de rotinas automatizadas de limpeza da base de dados (`teardown`), garantindo a independência entre os cenários.
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
* `test_login_nonexistent_user` (POST - 404)
* `test_login_without_name` (POST - 400)
* `test_login_without_email` (POST - 400)
* `test_login_without_password` (POST - 400)
* `test_login_without_administrador` (POST - 400)

#### Módulo: `test_usuarios.py` (CRUD de Usuários)
* `test_create_user_successfully` (POST - 201)
* `test_create_user_duplicated_email` (POST - 400)
* `test_list_all_users` (GET - 200)
* `test_delete_user_successfully_by_id` (DELETE - 200 / 204)

#### Módulo: `test_produtos.py` (CRUD de Produtos)
* `test_create_product_successfully_with_admin_token` (POST - 200 / 201)
* `test_create_product_without_admin_token` (POST - 401 / 403)
* `test_create_product_without_name` (POST - 400)
* `test_create_product_without_price` (POST - 400)
* `test_create_product_without_description` (POST - 400)
* `test_search_product_by_id` (GET - 200  / 201)
* `test_update_product_with_token` (PUT - 200 / 201)
* `test_update_product_without_token` (PUT - 401)
* `test_delete_product_successfully` (DELETE - 200 / 204)

#### Módulo: `test_carrinhos.py` (Gestão de Carrinhos)
* `test_create_cart_successfully` (POST - 201)
* `test_create_cart_insufficient_stock` (POST - 400 / 422)
* `test_conclude_purchase_success` (DELETE 200 / 204)
* `test_cancel_purchase_and_stock_return` (DELETE - 200 / 204)

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

# Métricas e Resultados

## Cobertura de Testes (Test Coverage)

Para garantir a robustez e a qualidade da API, a estratégia de automação foi baseada na metodologia de **Operator Coverage**, que mede a abrangência dos métodos HTTP testados em relação aos endpoints disponíveis.

### Mapa de Cobertura de Operações

| Endpoint | POST | GET | PUT | DELETE | Total de Operações |
| :--- | :---: | :---: | :---: | :---: | :---: |
| `/login` | ✅ | - | - | - | 1 |
| `/usuarios` | ✅ | ✅ | - | ✅ | 3 |
| `/produtos` | ✅ | ✅ | ✅ | ✅ | 4 |
| `/carrinhos` | ✅ | - | - | ✅ | 2 |
| **Totais** | **4** | **2** | **1** | **3** | **10 / 10** |

### Metodologia de Cálculo
O cálculo foi realizado através da fórmula de *Operator Coverage*:

Como a API expõe 10 operações distintas entre os quatro recursos (Login, Usuários, Produtos e Carrinhos) e todos os fluxos foram devidamente automatizados, a suíte alcançou **100% de cobertura de operações**.

---
## Qualidade Além da Cobertura
Além de atingir 100% de cobertura de operadores, a suíte foca na **qualidade das validações**:

* **Status Code Coverage:** Validação de fluxos de sucesso (200, 201) e cenários de erro esperados (400, 401, 403, 404).
* **JSON Schema Validation:** Garantia de que o contrato da API está sendo respeitado em cada resposta, assegurando que o Front-End receba os dados esperados.
* **Fluxo de Integração:** Testes que orquestram dependências, como a criação de um produto para uso posterior na criação de um carrinho, simulando o uso real do sistema.

--- 

---

## Principais Desafios e Aprendizados

| Desafio | Solução Encontrada |
| :--- | :--- |
| **Validação de Contratos (JSON Schema)** | Mapeamento das chaves de erro específicas por campo (ex: `"nome": "nome é obrigatório"`) em vez de chaves genéricas, facilitando o tratamento de erros no Front-End. |
| **Retorno de Verbos HTTP** | Separação estratégica entre *Schemas* de mutação (apenas `message` e `_id`) e *Schemas* de leitura (objeto completo), ajustando as expectativas das validações. |
| **Encadeamento de Requisições** | Implementação de fluxos encadeados (POST Produto > Extração de `_id` > Montagem de Payload > POST Carrinho) para respeitar a arquitetura relacional da API. |
| **Mutabilidade de Dados** | Utilização do método `.copy()` em dicionários para evitar que a manipulação de dados em um teste afetasse o estado original da *fixture* nos testes subsequentes. |
| **Rigor do Pytest (Nomenclatura)** | Desenvolvimento de leitura crítica dos logs de erro do Pytest para identificar rapidamente desvios de nomenclatura entre fixtures e parâmetros injetados. |
| **Ambiente de Desenvolvimento** | Reforço da atenção aos indicadores visuais de salvamento de arquivos no VS Code, eliminando falsos positivos causados por execução de código desatualizado. |

---

## Conclusão

O desafio foi foi um verdadeiro mergulho no que significa, na prática, ser uma *Quality Engineer*. Entendi que, quando estruturamos testes bem organizados, não estamos apenas automatizando o trabalho manual, mas construindo uma camada de segurança que dá confiança para o time evoluir o produto sem medo.

A refatoração que fiz nesta quarta semana me trouxe um *insight* valioso: a qualidade é parte intrínseca da arquitetura. Quando tratamos os contratos de API com o mesmo cuidado que tratamos a interface do usuário, garantimos que o ecossistema inteiro — do *back-end* ao *front-end* — fale a mesma língua. Sigo motivada e com a bagagem reforçada, pronta para aplicar essa visão estratégica de QA em cenários ainda mais complexos.

---

## Referências Utilizadas em Consultas

* [AAA Pattern in Test Automation - Semaphore CI](https://semaphore.io/blog/aaa-pattern-test-automation)
* [Pytest Documentation & Best Practices](https://docs.pytest.org/)
* [Pytest: Help your tests do more](https://docs.pytest.org/en/stable/explanation/goodpractices.html)
* [Como verificar a cobertura de testes de APIs REST](https://medium.com/revista-dtar/como-verificar-a-cobertura-de-testes-da-api-rest-9e2f745564b)
* [Test Coverage Criteria for RESTful Web APIs](https://www.researchgate.net/publication/327774902_Test_coverage_criteria_for_RESTful_web_APIs) 
* [Introduction to Contract Testing](https://martinfowler.com/articles/contract-testing.html)
* [Understanding JSON Schema](https://json-schema.org/learn/getting-started-step-by-step) 

---

### Como Executar este Repositório

1. **Clone o repositório:**
```bash
git clone <https://github.com/codebyfernanda/bootcamp-QA-2026-desafios.git>
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
