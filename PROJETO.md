# Manual de Contribuição e Organização do Projeto

> **Projeto:** Predição de Prematuridade no Rio de Janeiro  
> **Fontes:** SINASC, CNES e IBGE

| **Integrantes**  | **Contatos**              |
|:---:|:---:|
| Eduardo Peruzzo | eduardoperuzzo05@gmail.com |
| Estevão Moraes  | estevaomoraes48@gmail.com  |
| Gabriel Corrêa  | gabrielcs.1003@gmail.com   |
| Marcelle Lohane | marcellelohane@gmail.com   |

Este documento define o fluxo de organização, desenvolvimento e colaboração do projeto, com foco em clareza, rastreabilidade e reprodutibilidade.

---

## 1. Objetivo do Projeto

O projeto investiga a capacidade de identificar padrões associados à ocorrência de nascimentos prematuros a partir de informações maternas, territoriais e de infraestrutura de saúde pública.

O objetivo central é avaliar se informações disponíveis antes ou independentemente do acompanhamento pré-natal podem contribuir para antecipar a ocorrência de prematuridade em novos registros.

A predição é o eixo central. Análises descritivas, espaciais e socioeconômicas devem apoiar a interpretação dos resultados, não substituir a etapa preditiva.

---

## 2. Estrutura do Repositório

```text
project/
├── dados/
│   └── mapa_de_variaveis.md
├── docs/
│   └── ...
├── notebooks/
│   └── ...
├── src/
│   └── ...
├── README.md
├── PROJETO.md
└── requirements.txt
```

- **`dados/`**: mapa de variáveis e referências às fontes oficiais. Bases geradas localmente não devem ser versionadas, devido ao tamanho. Scripts de geração e preparação ficam em `src/`.
- **`docs/`**: documentação mais extensa e entregáveis de análise.
- **`notebooks/`**: notebooks R executados no Google Colab, utilizados para exploração, análise e desenvolvimento metodológico da disciplina.
- **`src/`**: código Python reprodutível para preparação, integração, modelagem e automação do fluxo.
- **`README.md`**: visão geral do projeto.
- **`PROJETO.md`**: organização, contribuição e fluxo de desenvolvimento.
- **`requirements.txt`**: dependências Python.

A separação principal é: **notebooks para análise aplicada; `src/` para código reprodutível e reutilizável.**

---

## 3. Milestones

Milestones representam as grandes etapas do projeto. Cada tarefa relevante deve estar registrada em uma Issue e vinculada ao Milestone correspondente.

| Milestone                           | Objetivo                                                                   | Prazo      |
|---|---|---|
| **1. Fundamentação e Planejamento** | Problema, literatura, hipóteses, desfecho e variáveis                      | 14/10/2026 |
| **2. Dados e Preparação**           | Aquisição, integração, limpeza e preparação do SINASC, CNES e IBGE         | 14/10/2026 |
| **3. Análise Exploratória**         | Estatística descritiva, padrões, associações, vieses e análise territorial | 17/10/2026 |
| **4. Modelagem**                    | Baseline, validação, comparação incremental das fontes e interpretação     | 24/10/2026 |
| **5. Resultados e Discussão**       | Resultados, hipóteses, limitações e confronto com a literatura             | 31/10/2026 |
| **6. Artigo e Entrega**             | Redação, revisão e organização da versão final                             | 07/11/2026 |

> Na modelagem, a comparação entre algoritmos é secundária à investigação do valor informativo das diferentes camadas de dados.

---

## 4. Issues

Uma Issue deve representar uma **tarefa ou entrega verificável**.

Evite tarefas amplas como `Fazer a modelagem`. Prefira ações concretas, como `Criar modelo baseline`.

### Template

```markdown
## Objetivo

<!-- Descreva brevemente o que precisa ser feito. -->

## Observações

<!-- Use para contexto, referências, decisões ou limitações relevantes. -->
```
---

## 5. Labels

Labels classificam as Issues e complementam seus títulos.

| Label | Uso |
|---|---|
| `artigo` | Estruturação e modelagem do conteúdo do artigo |
| `bug` | Problemas ou comportamentos incorretos |
| `colab` | Análises e modelagens realizadas no Google Colab |
| `docs` | Documentação do projeto |
| `func` | Desenvolvimento ou evolução de funcionalidades |

> Novas Labels devem ser criadas apenas quando houver necessidade recorrente de classificação.

---

## 6. GitHub Project e responsabilidades

| Recurso | Função |
|---|---|
| **Milestone** | Etapa maior e prazo |
| **Issue** | Tarefa ou entrega |
| **Label** | Categoria da tarefa |
| **Project** | Estado da execução |
| **Assignee** | Responsável |
| **Pull Request** | Revisão e integração |

O Project deve acompanhar o fluxo:

**Backlog → Em andamento → Em revisão → Concluído**

---

## 7. Branches

A `main` representa a versão integrada e estável do projeto. Não se trabalha diretamente nela.

### Fluxo

```bash
git switch main
git pull origin main
git switch -c feature/nome-da-tarefa
```

Convenções:

```text
feature/<nome-da-tarefa>
bug/<nome-do-problema>
docs/<nome-da-documentacao>
```

Quando houver uma Issue específica, o número pode ser incluído no nome, por exemplo `feature/21-preparar-sinasc`.

Uma branch deve corresponder a uma tarefa principal, com o valor em antes de `/` relacionado a *label* que antende. Após o merge, a branch será automaticamente removida pelo GitHub (se as configurações do repositório estiverem bem feitas).

---

## 8. Notebooks e Google Colab

Os notebooks `.ipynb` fazem parte do versionamento e seguem o mesmo fluxo de branch e Pull Request.

Como a disciplina exige R, os notebooks de análise são mantidos em R/Google Colab. Funções reutilizáveis e etapas que precisem compor um fluxo reprodutível devem ser transferidas para `src/`.

Evite que duas pessoas editem simultaneamente o mesmo notebook.

---

## 9. Pull Requests

Toda alteração destinada à `main` deve passar por Pull Request.

### Título

Use:

```text
tipo: descrição
```

Tipos principais: `func`, `fix`, `analise`, `docs`, `refactor`.

Vincule o Pull Request à Issue correspondente, preferencialmente usando `Closes #<número>`.

### Template

```markdown
## Issue relacionada

Closes #

## Descrição

<!-- Explique brevemente o que foi alterado. -->

## Observações

<!-- Adicione informações relevantes para a revisão. -->
```

O revisor deve verificar principalmente se a alteração atende à Issue, se os resultados fazem sentido e se não há problemas metodológicos ou alterações desnecessárias.

---

## 10. Commits

Commits devem ser pequenos e descritivos.

Formato:

```text
tipo: descrição
```

Tipos principais: `func`, `fix`, `analise`, `docs`, `refactor`.

Evite mensagens genéricas como `update`, `final`, `final2`, `finalfinal` ou `agora vai...2`.

---

## 11. Após o Merge

Após o merge:

```bash
git switch main
git pull origin main
```

Novas tarefas devem partir da `main` atualizada.

---

## 12. Responsabilidades

Cada integrante deve:

- trabalhar a partir de uma Issue;
- usar uma branch própria;
- registrar bloqueios ou mudanças relevantes na Issue;
- abrir Pull Request ao concluir a tarefa;
- revisar Pull Requests dos demais quando solicitado;
- evitar alterações diretas na `main`.

Dependências sumárias entre tarefas devem ser registradas nas Issues.

---