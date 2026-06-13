# Automação de API ServeRest com Python e Pytest

Este projeto é uma suíte de testes automatizados desenvolvida em **Python** utilizando o framework **Pytest**. O objetivo é validar o comportamento dos endpoints de Usuários e Produtos da API [ServeRest](https://compassuol.serverest.dev/), garantindo a integridade dos dados e a conformidade das respostas.

## Tecnologias Utilizadas

* **Linguagem:** Python 3.13.13
* **Framework de Testes:** Pytest
* **Requisições HTTP:** Requests

## Cenários de Teste Implementados

A presente suíte cobre os fluxos principais de CRUD (CREATE-READ-UPDATE-DELETE) para usuários e produtos, garantindo que erros de regra de negócio sejam devidamente reportados.

| Módulo | Cenários |
| :--- | :--- |
| **Status** | Verificação de disponibilidade da API |
| **Usuários** | Cadastro (válido/duplicado), busca por ID (existe/não existe), atualização e exclusão. |
| **Produtos** | Listagem, busca por ID, atualização e exclusão. |

## Como executar

1. **Clone o repositório:**
```bash
git clone [https://github.com/codebyfernanda/bootcamp-QA-2026-desafios.git](https://github.com/codebyfernanda/bootcamp-QA-2026-desafios.git)
cd bootcamp-QA-2026-desafios

```

2. **Crie e ative um ambiente virtual:**

```bash
# Linux/macOS
python -m venv venv
source venv/bin/activate

# Windows
python -m venv venv
venv\Scripts\activate

```

3. **Instale as dependências:**

```bash
pip install -r requirements.txt

```

4. **Execute os testes:**

```bash
pytest -v

```

## Pontos de Atenção (Arquitetura)

* **Massa de Dados Dinâmica:** Utilizei e-mails e nomes gerados aleatoriamente a cada execução para evitar conflitos de duplicidade na base da ServeRest.
* **Asserts Robustos:** Além de validar o status code, verifiquei a estrutura do contrato (JSON response) para garantir que a API está devolvendo os dados conforme o esperado.

## Sobre a Autora

O presente projeto foi desenvolvido por **Fernanda Bastos dos Santos** [(@codebyfernanda)](https://github.com/codebyfernanda), estudante de **Análise e Desenvolvimento de Sistemas** no Mackenzie, durante o **BOOTCAMP | AWS AI FDE DRIVEN QUALITY ENGINEERING**, da Compass UOL com [AI/R Company](https://aircompany.ai/).

## Agradecimentos Especiais
Gostaria de expressar meus agradecimentos à Squad 2 pelo engajamento, por toda troca de conhecimento e, também, pelo suporte durante o processo do nosso Bootcamp. Deixo aqui um agradecimento especial aos meus colegas [Renan Pacheco](https://github.com/Renanpacheco) e [Vitor Kunicki](https://github.com/vitto2099). A paciência, a disponibilidade e o auxílio técnico de vocês foram fundamentais para a superação dos desafios dessa entrega. Muito obrigada! :)
