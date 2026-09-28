# Checkpoint 02 — APIs, energias renováveis e aprendizado de máquina

**Aluno:** Patrick <sobrenome> — RM572899 — Turma 1CCPW
**Disciplina:** Soluções em Energias Renováveis e Sustentáveis — Prof. André Tritiack

## Objetivo

Consultar duas APIs públicas de dados abertos e resolver duas tarefas de aprendizado de
máquina, comparando três algoritmos em cada uma:

1. **Classificação:** prever a fonte de um empreendimento de geração (Solar, Eólica ou
   Hidráulica) a partir de potência outorgada, latitude e longitude.
2. **Regressão:** estimar a radiação solar horária em Petrolina (PE) a partir de
   temperatura, umidade, cobertura de nuvens, vento e hora do dia.

## Dados

| Tarefa | Fonte | Período / recorte | Arquivo |
|---|---|---|---|
| Classificação | [SIGA — ANEEL](https://dadosabertos.aneel.gov.br/dataset/siga-sistema-de-informacoes-de-geracao-da-aneel) (API CKAN) | Até 1200 registros por sigla (UFV, EOL, UHE, PCH, CGH); 3876 empreendimentos válidos | `aneel_classificacao_orange.csv` |
| Regressão | [Open-Meteo — histórico](https://open-meteo.com/en/docs/historical-weather-api) | 01/04/2025 a 30/06/2025, horas de 7h a 17h, fuso America/Recife; 1001 horas | `meteo_regressao_orange.csv` |

Nenhuma das consultas exige token. Os CSVs estão no repositório e também são
regenerados pelo notebook a partir das APIs. Como a base da ANEEL é atualizada, uma
nova consulta pode retornar registros um pouco diferentes.

## Como executar

1. Abra `Aula_APIs_Energia_Renovavel_ML.ipynb` no Google Colab ou em um Jupyter local
   (Python 3.10+).
2. A primeira célula instala as dependências: `pandas`, `matplotlib`, `seaborn`,
   `scikit-learn`.
3. Execute todas as células em ordem (*Reiniciar sessão e executar tudo*). O notebook
   consulta as APIs, gera os CSVs, treina os seis modelos e exibe tabelas, gráficos e
   conclusões.

## Resultados

### Tarefa 1 — Classificação (divisão estratificada 80/20, `random_state=42`, média macro)

| Modelo | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| Regressão Logística | 0,825 | 0,828 | 0,821 | 0,820 |
| KNN (k=5) | 0,965 | 0,966 | 0,964 | 0,965 |
| Random Forest | **0,976** | **0,977** | **0,974** | **0,975** |

Random Forest foi o melhor modelo. Solar é a classe mais confundida, sobretudo com
Eólica na Regressão Logística, porque as duas coexistem no interior do Nordeste.
Potência e localização não descrevem o recurso físico da usina, e unidades de um mesmo
complexo têm coordenadas quase idênticas, o que tende a inflar o desempenho medido.

### Tarefa 2 — Regressão (divisão temporal 80/20 sem embaralhar)

| Modelo | MAE (W/m²) | MSE ((W/m²)²) | R² |
|---|---|---|---|
| Regressão Linear | 145,2 | 30.034 | 0,360 |
| KNN (k=5) | 68,3 | 7.608 | 0,838 |
| Random Forest | **66,3** | **7.251** | **0,845** |

Random Forest foi o melhor modelo. A hora do dia é a variável mais importante (0,49),
porque define o teto físico de radiação; a Regressão Linear falha por tentar uma reta
numa relação em forma de sino. Estimar radiação não equivale a prever geração elétrica:
a energia produzida depende ainda de área, inclinação e eficiência dos painéis,
temperatura dos módulos, sombreamento e perdas do inversor.

## Atividade complementar — Orange Data Mining

<em construção — prints dos fluxos e análise serão adicionados aqui>

## Estrutura do repositório

CP2_SERS_PRONTO.ipynb
aneel_classificacao_orange.csv
meteo_regressao_orange.csv
README.md
