# Análise da Relação Populacional e Empresas Ativas no Setor da Construção (2007–2022)

Este projeto analisa a **razão entre a população com idade entre 38 e 58 anos e o número de empresas ativas** no setor da construção civil no Brasil, de 2007 a 2022, segmentado por região.

> **Objetivo:** Identificar regiões mais saturadas e aquelas com maiores oportunidades de crescimento no setor da construção.

---
## Sobre o Autor

Projeto desenvolvido por **Mileno Epifanio** — Analista de Dados com foco em soluções orientadas por dados para tomada de decisão.  
[LinkedIn](https://www.linkedin.com/in/milenoepifanio) • [GitHub](https://github.com/milenoepifanio)

---

## Objetivos

- Obter e tratar os dados populacionais da faixa etária predominante no mercado imobiliário (38–58 anos).
- Coletar o número de empresas ativas no setor de construção (via API SIDRA/IBGE).
- **Interpolar dados faltantes para os anos de 2021 e 2022.**
- Calcular e visualizar a **razão população/empresa ativa** por região.
- Salvar os dados tratados e gerar visualizações limpas e informativas.

---

## Tecnologias e Ferramentas

- `Python 3.11`
- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `requests`
- `openpyxl`

---

## Estrutura do Projeto
```bash
├── data/
│ └── 0 - empresas_ativas_por_regiao.csv
│ └── 0 - tabela de idades.xlsx
│ ├── 1 - populacao_ibge.csv
│ ├── 1 - empresas_ativas_interpoladas.csv
│ └── 2 - relacao_populacao_empresas.csv

├── scr/
│ └── 1 - consultando_ibge.ipynb
│ └── 2 - tratando_dados_população.ipynb
│ └── 3 - interpolacao_empresas.ipynb
│ └── 4 - arquivo_final.ipynb
├── .gitignore
├── LICENSE.txt
└── README.md
├── requirements.in
├── requirements.txt
```

---

## Como Executar

1. Clone este repositório:

```bash
git clone git@github.com:milenoepifanio/analise-populacional-imobiliaria-brasil.git
```


2. Crie um ambiente virtual e instale as dependências:
```bash
pip install -r requirements.txt
```

3. Execute os scripts da pasta scr/ para gerar os arquivos tratados e visualizações.

---

## Ordem de Execução dos Notebooks

Os notebooks foram organizados numericamente para refletir a **sequência lógica do processo de análise**. Abaixo, você encontra o propósito de cada um:

| Arquivo                                 | Descrição                                                                 |
|----------------------------------------|---------------------------------------------------------------------------|
| `1 - consultando_ibge.ipynb`           | Realiza a coleta de dados de empresas ativas na construção via API SIDRA |
| `2 - tratando_dados_população.ipynb`   | Faz o tratamento da base de população por idade com base na planilha IBGE |
| `3 - interpolacao_empresas.ipynb` | Interpola os dados de 2021 e 2022 por região usando polinômios de grau 2 |
| `4 - arquivo_final.ipynb`              | Consolida os passos e permite reexecutar a análise completa de forma sequencial |

Todos os arquivos intermediários são salvos na pasta `/data`.

| Arquivo                                 | Descrição                                                                 |
|----------------------------------------|---------------------------------------------------------------------------|
| `0 - empresas_ativas_por_regiao.csv`           | output das quantidade de empresas ativas por região, vindas da via API SIDRA, gerado por meio do script `1 - consultando_ibge.ipynb` |
| `0 - tabela de idades.xlsx`   | Base de população por idade com base na planilha IBGE. |
| `1 - empresas_ativas_interpoladas.csv` | Base oriunda da  `0 - empresas_ativas_por_regiao.csv`, onde é realizado a interpolação para os anos de 2021 e 2022. |
| `1 - populacao_ibge.csv` | Organiza, filtra, trata a base que vem da `0 - tabela de idades.xlsx`, deixando no formato adequado para o resultado final, organizado por meio do script  `2 - tratando_dados_população.ipynb`. |
| `2 - relacao_populacao_empresas.csv`              | Consolida as bases iniciadas por "1" e nos retorna a relação solicitada no case, por meio do script `4 - arquivo_final.ipynb`. |

Essa organização facilita a reprodução da análise por outros usuários ou pela banca avaliadora.


## Interpolação Adotada

Para estimar os valores de 2021 e 2022 (ausentes nos dados brutos do IBGE), foi utilizada a técnica de Interpolação Polinomial de Grau 2:

| Modelo                                           | Descrição                                                                      | Quando é adequado                                                                |
|--------------------------------------------------|--------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **Linear**                                       | Ajuste com reta (regressão linear).                                            | Tendência estável ou suavemente crescente/decrescente                            |
| 💡 **Polinomial (grau 2 ou 3)**                  | Permite curvaturas e inflexões, se ajustando a séries que sobem e depois caem. | **Quando há aceleração/desaceleração na tendência (ex: crescimento e depois queda)** |
| **Spline (Interpolação cúbica)**                 | Interpolação suave entre pontos, útil para suavizar flutuações.                | Se quiser manter fidelidade aos valores reais e suavizar ruído                   |
| **Exponencial ou logarítmico**                   | Crescimento rápido no início e saturação depois (ou vice-versa).               | Quando o crescimento é claramente exponencial ou log-logístico                   |
| **Modelos de séries temporais (ARIMA, Prophet)** | Modelos mais robustos, capturam tendência, sazonalidade e ruído.               | Se tiver mais pontos de dados e deseja modelagem preditiva avançada              |

Essa abordagem foi escolhida com base nas seguintes razões:

- Os dados históricos mostraram **curvaturas claras na série temporal**, com ciclos de **crescimento, estabilização e recuo** em algumas regiões.
- A interpolação polinomial de segundo grau se ajusta melhor a esse tipo de comportamento do que uma reta simples (linear).
- Evitamos modelos mais complexos como séries temporais (ARIMA, Prophet), pois o número de pontos por região (14 anos) ainda é relativamente limitado.


A interpolação foi aplicada separadamente por região, garantindo que cada curva seguisse a tendência histórica observada em seus próprios dados.

---

## Análise dos Resultados

Observa-se as seguintes tendências:


### Norte e Nordeste: tendência crescente

- Apresentam um **forte crescimento** na razão população/empresa ativa entre 2016 e 2022.
- Sinalizam possível **saturação do mercado** — a população nessa faixa etária cresce mais rápido do que o número de empresas.


### Sul e Sudeste: estabilidade e maturidade

- Exibem as **menores razões** ao longo de todo o período analisado.
- A **região Sul** se destaca por ter **menos de 300 pessoas por empresa ativa em 2022**, indicando um setor mais bem distribuído e maduro.
- Tendência de estabilidade desde 2015, com pequenas oscilações.


### Centro-Oeste: equilíbrio

- Indica um **mercado em equilíbrio**, com espaço para crescimento sem sinais de saturação imediata.


## Conclusão Estratégica

- **Oportunidades**: Regiões como **Sul** e **Centro-Oeste** mostram um mercado mais equilibrado e ainda com potencial de expansão empresarial.
- **Norte** e **Nordeste** requerem cuidado estratégico, pois a demanda populacional está crescendo mais rápido do que a oferta de empresas no setor.


---

## Licença

**Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**

Você tem a liberdade de:

- **Compartilhar** — copiar e redistribuir o material em qualquer meio ou formato.
- **Adaptar** — remixar, transformar e criar a partir do material.

Desde que respeite os seguintes termos:

- **Atribuição** — Você deve dar o crédito apropriado, fornecer um link para a licença e indicar se mudanças foram feitas.
- **Uso Não Comercial** — Você não pode usar o material para fins comerciais.

Leia a licença completa em: [https://creativecommons.org/licenses/by-nc/4.0/legalcode](https://creativecommons.org/licenses/by-nc/4.0/legalcode)
