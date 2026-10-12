# Predição de Prematuridade no Rio de Janeiro

Projeto de Machine Learning desenvolvido para investigar a capacidade de identificar **padrões associados à ocorrência de nascimentos prematuros** a partir de informações maternas, territoriais e da infraestrutura de saúde pública.

O estudo utiliza dados públicos brasileiros, com foco no **estado do Rio de Janeiro e na região Sudeste**, integrando informações do **SINASC, CNES e IBGE**.

O objetivo central não é reproduzir uma avaliação obstétrica durante o pré-natal, mas investigar se características disponíveis **antes ou independentemente do acompanhamento pré-natal** são capazes de antecipar a ocorrência de prematuridade em novos registros.

---

## Objetivo

Avaliar se informações históricas sobre a mãe, o contexto socioeconômico e geográfico de sua região e a estrutura assistencial disponível podem contribuir para estimar a probabilidade de um nascimento ser prematuro.

O projeto também busca compreender como essa capacidade preditiva varia entre diferentes perfis populacionais, municípios e contextos assistenciais.

A aplicação posterior das previsões pode apoiar análises como:

- identificação de perfis com maior probabilidade de prematuridade;
- identificação de territórios com maior concentração de casos;
- estimativa agregada de demanda assistencial;
- análise da capacidade instalada das redes hospitalares.

Essas aplicações são secundárias ao objetivo principal: **avaliar a capacidade de antecipação do desfecho por meio de Machine Learning**.

---

## Bases de dados

### SINASC

O **Sistema de Informação sobre Nascidos Vivos** constitui a principal fonte do projeto, fornecendo informações sobre características maternas, histórico obstétrico, nascimento e localização.

Entre as variáveis de interesse estão:

- idade materna;
- escolaridade;
- estado civil;
- raça/cor;
- ocupação;
- número de gestações anteriores;
- partos anteriores;
- filhos vivos e perdas anteriores;
- município de residência;
- características da gestação disponíveis no momento definido para previsão.

A **idade gestacional** será utilizada para construção da variável-alvo de prematuridade, e não como preditora.

Variáveis conhecidas apenas durante ou após a gestação, como número final de consultas pré-natais, tipo de parto, peso ao nascer e Apgar, serão avaliadas como potenciais fontes de *temporal leakage* e não deverão compor o modelo principal.

> O SINASC público não permite reconstruir de maneira confiável todo o histórico longitudinal de uma mesma mãe. Portanto, o projeto trabalha principalmente com o histórico obstétrico informado no registro atual.

### CNES

O **Cadastro Nacional de Estabelecimentos de Saúde** será utilizado para representar o contexto da rede assistencial.

Serão investigadas características como:

- leitos obstétricos;
- leitos neonatais;
- UTI e UCI neonatal;
- serviços especializados;
- capacidade instalada;
- características dos estabelecimentos;
- disponibilidade regional de recursos assistenciais.

O CNES deverá ser associado de maneira **temporalmente compatível** com cada observação.

O estabelecimento onde o nascimento ocorreu não será interpretado automaticamente como causa do desfecho, devido à possibilidade de seleção e encaminhamento de gestantes de maior risco para unidades de referência.

### IBGE

O **IBGE** será utilizado para representar características socioeconômicas, demográficas e territoriais dos municípios.

Entre os possíveis grupos de variáveis estão:

- renda e indicadores econômicos;
- escolaridade;
- saneamento;
- características domiciliares;
- densidade populacional;
- urbanização;
- indicadores demográficos.

Sempre que possível, serão utilizados indicadores compatíveis com o período da observação, evitando informações posteriores ao evento analisado.

---

## Variável-alvo

O desfecho principal será a ocorrência de nascimento prematuro:

\[
Y =
\begin{cases}
1, & \text{idade gestacional < 37 semanas}\\
0, & \text{idade gestacional ≥ 37 semanas}
\end{cases}
\]

O problema será tratado como **classificação binária**, podendo utilizar regressão logística como modelo estatístico de referência.

---

## Hipóteses

O projeto será orientado pelas seguintes hipóteses:

**H1.** Características maternas e histórico obstétrico disponíveis antes do acompanhamento pré-natal possuem capacidade preditiva significativa para a ocorrência de prematuridade.

**H2.** Características socioeconômicas e demográficas do município acrescentam informação preditiva às características individuais da mãe.

**H3.** A estrutura assistencial disponível na região acrescenta informação além das características maternas e territoriais.

**H4.** A ocorrência de prematuridade apresenta heterogeneidade espacial, com municípios ou regiões apresentando padrões significativamente distintos.

**H5.** A capacidade preditiva varia entre diferentes perfis socioeconômicos, geográficos e demográficos.

**H6.** Modelos que combinam diferentes camadas de informação apresentam desempenho superior ao modelo baseado exclusivamente em características maternas.

---

## Principais dimensões investigadas

A análise será organizada em quatro grupos:

### Perfil materno

Características demográficas, sociais e histórico obstétrico previamente conhecido.

### Contexto territorial

Características socioeconômicas, demográficas e geográficas do município de residência.

### Contexto assistencial

Disponibilidade e capacidade da rede hospitalar e neonatal da região.

### Contexto temporal

Variações entre anos, períodos, municípios e mudanças na estrutura da rede de saúde.

---

## Tratamento temporal

A validade temporal será uma preocupação central do projeto.

Para cada nascimento, as informações utilizadas como preditoras devem representar aquilo que poderia estar disponível **antes do desfecho**.

Serão especialmente avaliados:

- informações do CNES posteriores ao nascimento;
- indicadores territoriais publicados após o período analisado;
- agregações que utilizem informações do próprio período ou de períodos futuros;
- variáveis registradas somente durante ou após o acompanhamento pré-natal;
- informações diretamente derivadas do próprio nascimento.

Sempre que forem utilizadas estatísticas históricas do município, como taxa de prematuridade, elas deverão ser calculadas utilizando apenas períodos anteriores à observação.

---

## Modelagem conceitual

A evolução do estudo seguirá uma abordagem incremental:

```text
Características maternas
        ↓
+ Contexto socioeconômico e territorial
        ↓
+ Estrutura assistencial
        ↓
Modelo preditivo
        ↓
Análise dos padrões e limitações
```

O desempenho de cada etapa será comparado para avaliar **quanto cada fonte de informação acrescenta à capacidade de antecipação do desfecho**.

A comparação entre algoritmos é secundária à investigação do valor informativo das diferentes camadas de dados.

---

## Análise de clusters e distribuição espacial

A análise exploratória espacial será utilizada como apoio à modelagem.

Podem ser investigados:

- agrupamentos de municípios com maiores taxas históricas de prematuridade;
- diferenças entre regiões;
- perfis maternos predominantes em determinados territórios;
- concentração de recursos assistenciais;
- relação entre indicadores socioeconômicos e ocorrência do desfecho.

Técnicas de agrupamento e análise espacial serão utilizadas principalmente para **interpretar os padrões encontrados pelo modelo**, e não como substitutas da etapa preditiva.

---

## Observações esperadas

O estudo busca investigar principalmente:

1. quais características apresentam maior associação com a prematuridade;
2. quanto o contexto territorial acrescenta à previsão individual;
3. quanto a estrutura assistencial acrescenta ao modelo;
4. em quais regiões o modelo apresenta melhor ou pior desempenho;
5. quais perfis apresentam maior concentração do desfecho;
6. se os padrões identificados permanecem estáveis ao longo do tempo;
7. se as probabilidades individuais agregadas podem auxiliar na estimativa da demanda assistencial.

Os resultados serão interpretados como **associações e capacidade preditiva**, não como evidência causal.

---

## Limitações esperadas

O projeto possui algumas limitações estruturais:

- ausência de histórico longitudinal completo da mãe nos dados públicos do SINASC;
- ausência de diversas variáveis clínicas relevantes para prematuridade;
- possibilidade de diferenças de cobertura e qualidade entre municípios;
- limitações na representação individual por indicadores municipais;
- possibilidade de viés de encaminhamento na relação entre maternidades e prematuridade;
- necessidade de compatibilização temporal entre diferentes bases;
- impossibilidade de interpretar importância preditiva como causalidade.

Essas limitações serão consideradas na avaliação dos resultados e na interpretação dos modelos.

---

## Etapas do projeto

```text
1. Aquisição e documentação das bases
2. Integração SINASC + CNES + IBGE
3. Definição temporal das variáveis
4. Tratamento de inconsistências e dados ausentes
5. Construção da variável-alvo
6. Análise exploratória e espacial
7. Construção do baseline
8. Inclusão incremental das diferentes fontes de informação
9. Avaliação temporal e comparação dos modelos
10. Análise de perfis, clusters e distribuição geográfica
11. Interpretação dos resultados
12. Discussão das limitações e aplicações
```

---

## Escopo do projeto

O trabalho não pretende substituir avaliação médica ou estabelecer fatores causais para a prematuridade.

Seu objetivo é investigar, utilizando dados públicos brasileiros, **se padrões maternos, territoriais e assistenciais podem ser utilizados para antecipar a ocorrência de prematuridade e caracterizar como essa capacidade varia entre diferentes contextos**.

A análise descritiva, espacial e socioeconômica será utilizada para explicar e contextualizar os resultados do modelo, mantendo a **predição da prematuridade como eixo central do estudo**.