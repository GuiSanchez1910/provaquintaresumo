# 📚 RESUMÃO FERA — PROVA DE CIÊNCIA DE DADOS

> **Objetivo:** revisar rapidamente os conteúdos das aulas e, principalmente, saber **o que cada código faz, quando usar e como interpretar o resultado**.
>
> **Ordem mental para a prova:**  
> **Dados → descrição → visualização → limpeza/outliers → inferência → correlação → previsão/IA → LGPD/governança.**

---

---

# 🧭 COMO USAR ESTE RESUMO

> **🔴 DECORAR:** fórmulas, comandos e regras de decisão  
> **🟡 ENTENDER:** interpretação dos resultados  
> **🟢 IDENTIFICAR:** qual ferramenta usar em cada questão

### ⚡ Atalho mental

```text
DESCREVER     → mean / median / mode / std / describe
DIVIDIR       → Q1 / Q2 / Q3
ACHAR OUTLIER → IQR + cercas
VISUALIZAR    → boxplot / histplot / scatterplot
COMPARAR      → ttest_ind + p-valor
RELACIONAR    → corr + Pearson/Spearman
PREVER        → LinearRegression
AVALIAR       → R² + RMSE
PROTEGER      → LGPD + governança
```

### 🎯 Regra de ouro

> **Se a questão mostrar um código, não tente decorar tudo de uma vez. Pergunte:**
>
> **1. O que entra? → 2. O que o código calcula? → 3. O que sai? → 4. Como interpreto?**

---

---

# 🧠 0. MAPA MENTAL DA PROVA

| Assunto | Ideia principal | Código-chave |
|---|---|---|
| DataFrame | Tabela de dados no Pandas | `pd.DataFrame(...)` |
| Média | Centro aritmético | `.mean()` |
| Mediana | Valor central ordenado | `.median()` |
| Moda | Valor mais frequente | `.mode()[0]` |
| Variância | Dispersão ao quadrado | `.var()` |
| Desvio padrão | Dispersão na unidade original | `.std()` |
| Quartis | Divide os dados em 4 partes | `.quantile()` |
| IQR | Miolo de 50% dos dados | `Q3 - Q1` |
| Outlier | Valor fora das cercas | `Q1 - 1.5IQR` / `Q3 + 1.5IQR` |
| Boxplot | Mostra quartis/outliers | `sns.boxplot()` |
| Histograma | Mostra distribuição | `sns.histplot()` |
| Sturges | Calcula nº de bins | `1 + 3.322*log10(N)` |
| Teste t | Compara médias de 2 grupos | `stats.ttest_ind()` |
| p-valor | Evidência contra H0 | `p < 0.05` |
| Correlação | Mede associação | `.corr()` |
| Pearson | Associação linear | `method='pearson'` |
| Spearman | Associação monotônica/ranking | `method='spearman'` |
| Scatterplot | Visualiza relação X × Y | `sns.scatterplot()` |
| Heatmap | Visualiza matriz | `sns.heatmap()` |
| Regressão linear | Aprende relação X → y | `LinearRegression()` |
| R² | Quanto o modelo explica | `r2_score()` |
| RMSE | Erro médio na unidade de y | `root_mean_squared_error()` |
| LGPD | Proteção de dados pessoais | minimizar/excluir PII |
| Governança | Regras, papéis e controles dos dados | qualidade, segurança, responsabilidade |

---

---

# 1. 📊 ESTATÍSTICA DESCRITIVA


## 1.1 Média, mediana e moda

### Média

Soma dos valores dividida pela quantidade.

```python
media = df['coluna'].mean()
```

**Problema:** é muito sensível a valores extremos.

Exemplo:

```text
1000, 1100, 1200, 1300, 1.000.000
```

A média fica artificialmente alta por causa do `1.000.000`.

### Mediana

Valor que fica no meio quando os dados estão ordenados.

```python
mediana = df['coluna'].median()
```

É muito mais resistente a outliers.

### Moda

Valor que mais aparece.

```python
moda = df['coluna'].mode()[0]
```

Para descobrir a região mais frequente:

```python
moda_regiao = df['Regiao'].mode()[0]
```

### ⚠️ Decore

```text
MÉDIA   → centro aritmético → sensível a outliers
MEDIANA → centro da fila   → resistente a outliers
MODA    → mais frequente
```

---

---

> 🧠 **DECORA:** `desvio padrão = √variância` e fica na mesma unidade dos dados.

# 2. 📏 VARIÂNCIA E DESVIO PADRÃO


## Variância

Mede o quanto os valores se afastam da média, usando os desvios ao quadrado.

```python
variancia = df['coluna'].var()
```

A unidade fica elevada ao quadrado.

Exemplo: se os dados estão em reais, a variância fica em `R$²`, por isso é menos intuitiva.


## Desvio padrão

É a raiz da variância.

```python
desvio = df['coluna'].std()
```

Volta para a unidade original.

Exemplo:

```text
Média = R$ 5.000
Desvio padrão = R$ 800
```

Interpretação simplificada: os valores costumam se espalhar em torno da média em uma escala de aproximadamente R$ 800.

### ⚠️ Relação

```text
VARIÂNCIA = dispersão ao quadrado
DESVIO PADRÃO = √variância
```

---

---

# 3. 🔬 `.describe()` — RAIO-X DOS DADOS

O comando mais importante para estatística descritiva:

```python
df['salario'].describe()
```

ou:

```python
resumo = df['salario'].describe()
print(resumo)
```

Ele mostra:

```text
count → quantidade de registros
mean  → média
std   → desvio padrão
min   → mínimo
25%   → Q1
50%   → Q2 / mediana
75%   → Q3
max   → máximo
```

### Exemplo

```python
resumo = df['salario'].describe()

q1 = resumo['25%']
q2 = resumo['50%']
q3 = resumo['75%']
```

---

---

> 🧠 **DECORA:** `Q1 = 25%` · `Q2 = 50% = mediana` · `Q3 = 75%`.

# 4. 🥧 QUARTIS

Quartis dividem os dados ordenados em quatro partes.

```text
MIN ───── Q1 ───── Q2 ───── Q3 ───── MAX
       25%       50%       75%
```

### Q1

25% dos dados estão abaixo dele.

```python
q1 = df['coluna'].quantile(0.25)
```

### Q2

É a mediana.

```python
q2 = df['coluna'].quantile(0.50)
```

### Q3

75% dos dados estão abaixo dele.

```python
q3 = df['coluna'].quantile(0.75)
```

### Exemplo manual

Dados:

```text
45, 12, 60, 20, 30, 15, 80
```

Ordenando:

```text
12, 15, 20, 30, 45, 60, 80
```

Resultado:

```text
Q1 = 15
Q2 = 30
Q3 = 60
```

---

---

> 🧠 **FÓRMULA:** `IQR = Q3 - Q1`

# 5. 📦 IQR — INTERVALO INTERQUARTIL

O IQR representa o intervalo dos **50% centrais** dos dados.


## Fórmula

```text
IQR = Q3 - Q1
```

Código:

```python
iqr = q3 - q1
```

Exemplo:

```text
Q1 = 150
Q3 = 250

IQR = 250 - 150
IQR = 100
```

Isso significa que os 50% centrais estão entre `150` e `250`, uma distância de `100`.

### ⚠️ Decore

```text
IQR = Q3 - Q1
```

---

---

> 🚨 **OUTLIER:** abaixo de `Q1 - 1.5×IQR` ou acima de `Q3 + 1.5×IQR`.

# 6. 🚨 OUTLIERS E CERCAS MATEMÁTICAS

Um outlier é um valor muito distante do comportamento central.

As cercas usam o IQR.


## Limite inferior

```text
LI = Q1 - 1.5 × IQR
```


## Limite superior

```text
LS = Q3 + 1.5 × IQR
```

Código:

```python
limite_inferior = q1 - 1.5 * iqr
limite_superior = q3 + 1.5 * iqr
```

Um valor:

```text
< limite_inferior → outlier
> limite_superior → outlier
```

### Exemplo

```text
Q1 = 150
Q3 = 250
IQR = 100

LI = 150 - 1.5 × 100
LI = 0

LS = 250 + 1.5 × 100
LS = 400
```

Logo:

```text
valor > 400 → possível outlier
valor < 0   → possível outlier
```

---

---

# 7. 🧹 LIMPEZA DE OUTLIERS COM PANDAS

Exemplo:

```python
df_limpo = df[
    (df['ticket'] <= limite_superior) &
    (df['ticket'] >= limite_inferior)
]
```

Ou quando o limite superior já foi definido:

```python
limite_superior = 6300

df_limpo = df[df['salario'] <= limite_superior]
```

Quantidade removida:

```python
removidos = len(df) - len(df_limpo)
```

Quantidade nova:

```python
len(df_limpo)
```

### ⚠️ Importante

Não basta simplesmente "apagar números estranhos". Primeiro deve existir um critério estatístico ou de negócio para justificar a remoção.

---

---

# 8. 📦 BOXPLOT

O boxplot é excelente para visualizar:

- Q1
- mediana
- Q3
- dispersão
- possíveis outliers

Código:

```python
sns.boxplot(x=df['salario'])
plt.show()
```

Por grupo:

```python
sns.boxplot(
    data=df_limpo,
    x='versao',
    y='ticket'
)
plt.show()
```

### Como ler

```text
        •       ← possível outlier
        |
    ┌───┤       ← bigode
    │   │
    │───│       ← mediana
    │   │
    └───┘
```

A caixa vai de `Q1` até `Q3`.

A linha dentro da caixa é a mediana.

Pontos isolados podem representar outliers.

---

---

# 9. 📊 HISTOGRAMA

Histograma mostra a distribuição dos dados dividindo valores em intervalos chamados **bins**.

```python
sns.histplot(x=df['salario'], bins=10)
plt.show()
```


## Regra de Sturges

Usada para estimar a quantidade de bins.

```text
k = 1 + 3.322 × log10(N)
```

Código:

```python
N = len(df)

k_matematico = 1 + 3.322 * math.log10(N)
k_bins = round(k_matematico)

print(k_bins)
```

Depois:

```python
sns.histplot(
    x=df['salario'],
    bins=k_bins
)
plt.show()
```

### Exemplo da aula

```text
N = 8065
```

A fórmula produz aproximadamente a quantidade adequada de intervalos para representar a distribuição.

### ⚠️ Decore

```text
BOXPLOT     → quartis + outliers
HISTOGRAMA  → formato/distribuição dos dados
STURGES     → quantidade de bins
```

---

---

# 10. 🐼 PANDAS — COMANDOS QUE PRECISA SABER


## Criar DataFrame

```python
df = pd.DataFrame({
    'versao': ['A', 'B'],
    'ticket': [1200, 1220]
})
```


## Selecionar coluna

```python
df['ticket']
```


## Filtrar linhas

```python
df[df['ticket'] > 1000]
```


## Filtrar por categoria

```python
df[df['versao'] == 'A']
```


## Vários critérios

```python
df[
    (df['ticket'] <= limite_superior) &
    (df['ticket'] >= limite_inferior)
]
```


## Remover coluna

```python
df_seguro = df.drop(columns=['Email'])
```


## Primeiras linhas

```python
df.head()
```


## Quantidade

```python
len(df)
```


## Estatísticas

```python
df['ticket'].describe()
```


## Agrupamento

```python
df.groupby('versao')['ticket'].describe()
```

Isso gera estatísticas separadas para A e B.

---

---

# 11. 🎲 NUMPY E GERAÇÃO DE DADOS


## Travar aleatoriedade

```python
np.random.seed(42)
```

Significa que os números aleatórios serão reproduzíveis.


## Distribuição normal

```python
np.random.normal(
    loc=1200,
    scale=350,
    size=500
)
```

- `loc` = média
- `scale` = desvio padrão
- `size` = quantidade

Exemplo:

```python
ticket_A = np.random.normal(
    loc=1200,
    scale=350,
    size=500
)
```


## Uniforme

```python
np.random.uniform(
    500,
    900,
    size=50
)
```

Gera valores distribuídos uniformemente entre os limites.


## Concatenar

```python
todos_salarios = np.concatenate([
    salarios_normais,
    outliers_baixos,
    outliers_altos
])
```

Junta arrays.


## Repetição de listas

```python
['A'] * 500
```

gera 500 valores `"A"`.


## Linspace

```python
x_base = np.linspace(0, 100, 100)
```

Gera 100 valores igualmente espaçados entre 0 e 100.


## Inteiros aleatórios

```python
idades = np.random.randint(18, 65, 1000)
```


## Escolha aleatória

```python
regioes = np.random.choice(
    ['Sul', 'Sudeste', 'Nordeste'],
    1000,
    p=[0.2, 0.5, 0.3]
)
```

`p` define as probabilidades.

---

---

# 12. 📈 VISUALIZAÇÃO — SEABORN + MATPLOTLIB


## Configuração

```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_theme(style="whitegrid")
```


## Barplot

Usado para comparar valores agregados, normalmente médias:

```python
sns.barplot(
    data=df,
    x='versao',
    y='ticket'
)
plt.show()
```


## Boxplot

```python
sns.boxplot(
    data=df,
    x='versao',
    y='ticket'
)
plt.show()
```


## Scatterplot

Mostra a relação entre duas variáveis:

```python
sns.scatterplot(
    data=df,
    x='Tempo_Tela_Min',
    y='Gasto_Site_R$'
)
plt.show()
```

Com transparência:

```python
sns.scatterplot(
    data=df,
    x='Renda_Bairro',
    y='Preco',
    alpha=0.3
)
```

`alpha=0.3` deixa os pontos mais transparentes.


## Heatmap

```python
sns.heatmap(
    matriz_correlacao,
    annot=True,
    fmt=".2f"
)
plt.show()
```

- `annot=True` → mostra os números
- `fmt=".2f"` → 2 casas decimais

---

---

# 13. 🧪 INFERÊNCIA ESTATÍSTICA

Estatística descritiva descreve o que temos.

Inferência tenta avaliar se uma diferença observada pode ser explicada pelo acaso.

Exemplo clássico das aulas:

```text
Teste A/B

A = versão antiga
B = versão nova
```

Queremos saber se a diferença entre A e B é estatisticamente significativa.

---

---

# 14. ⚖️ HIPÓTESES H0 E H1


## H0 — hipótese nula

Não existe diferença real.

Exemplo:

```text
H0: média A = média B
```


## H1 — hipótese alternativa

Existe diferença.

```text
H1: média A ≠ média B
```

A decisão usa o **p-valor**.

---

---

> 🎯 **DECISÃO:** `p < 0.05 → rejeita H0` | `p ≥ 0.05 → não rejeita H0`.

# 15. 🎯 P-VALOR

O p-valor ajuda a avaliar a evidência contra H0.

Na aula:

```text
α = 0.05
```

Regra:

```text
p < 0.05
→ rejeita H0
→ resultado estatisticamente significativo

p >= 0.05
→ não rejeita H0
→ não há evidência suficiente de diferença
```

### ⚠️ Cuidado

`p >= 0.05` não significa "provei que H0 é verdadeira".

Significa que **não houve evidência estatística suficiente para rejeitá-la**.

---

---

# 16. 🧪 TESTE T DE STUDENT PARA DOIS GRUPOS

Biblioteca:

```python
from scipy import stats
```

Separando os grupos:

```python
array_A = df[df['versao'] == 'A']['ticket']
array_B = df[df['versao'] == 'B']['ticket']
```

Teste:

```python
estatistica, p_valor = stats.ttest_ind(
    array_A,
    array_B
)
```

Imprimir:

```python
print(f"P-Valor: {p_valor:.4f}")
```

Decisão:

```python
if p_valor < 0.05:
    print("Diferença estatisticamente significativa.")
else:
    print("Diferença não significativa.")
```

### Fluxo para decorar

```text
FILTRA A
   ↓
FILTRA B
   ↓
ttest_ind(A, B)
   ↓
pega p_valor
   ↓
compara com 0.05
```

---

---

# 17. 🧹 TESTE A/B + OUTLIERS

Um problema importante da aula:

- média é sensível a outliers;
- desvio padrão é sensível a outliers;
- muita variabilidade pode dificultar encontrar diferença;
- por isso a limpeza pode ser feita antes da análise, quando estatisticamente justificável.

Código completo:

```python
q1 = df['ticket'].quantile(0.25)
q3 = df['ticket'].quantile(0.75)

iqr = q3 - q1

limite_superior = q3 + 1.5 * iqr
limite_inferior = q1 - 1.5 * iqr

df_limpo = df[
    (df['ticket'] <= limite_superior) &
    (df['ticket'] >= limite_inferior)
]

grupo_A_limpo = df_limpo[
    df_limpo['versao'] == 'A'
]['ticket']

grupo_B_limpo = df_limpo[
    df_limpo['versao'] == 'B'
]['ticket']

t_stat_limpo, p_valor_limpo = stats.ttest_ind(
    grupo_A_limpo,
    grupo_B_limpo
)
```

Depois:

```python
print(f"P-Valor: {p_valor_limpo:.4f}")
```

---

---

# 18. 📋 GROUPBY + DESCRIBE

Para fazer um "raio-X executivo":

```python
resumo_executivo = (
    df_limpo
    .groupby('versao')['ticket']
    .describe()
)

display(resumo_executivo)
```

Você consegue comparar:

```text
count
mean
std
min
25%
50%
75%
max
```

por grupo.

---

---

# 19. 🔗 CORRELAÇÃO

Correlação mede a **associação** entre duas variáveis.

Ela não prova causalidade.

### Exemplos

```text
mais tempo no site → mais gasto
```

pode haver associação.

Mas isso não significa automaticamente:

```text
tempo no site CAUSA o gasto
```

---

---

> 🔗 **DECORA:** `+1 = positiva forte` · `0 = pouca relação linear` · `-1 = negativa forte`.

# 20. 📐 COEFICIENTE DE CORRELAÇÃO

O coeficiente normalmente varia entre:

```text
-1 ───────── 0 ───────── +1
```

### Próximo de +1

Forte relação positiva.

```text
X sobe → Y tende a subir
```

### Próximo de -1

Forte relação negativa.

```text
X sobe → Y tende a cair
```

### Próximo de 0

Pouca ou nenhuma relação linear.

### ⚠️ Importante

Correlação ≠ causalidade.

---

---

# 21. 📏 PEARSON

Pearson mede principalmente **relação linear**.

Fórmula:

```text
r = Cov(X,Y) / (sx × sy)
```

No Pandas:

```python
correlacao = df['X'].corr(
    df['Y'],
    method='pearson'
)
```

Ou:

```python
correlacao = df['X'].corr(df['Y'])
```

Interpretação:

```text
r ≈ +1 → relação linear positiva forte
r ≈ -1 → relação linear negativa forte
r ≈ 0  → pouca relação linear
```

---

---

# 22. 🏆 SPEARMAN

Spearman trabalha com **ranks/ordenação**.

É útil quando a relação é monotônica, mas não necessariamente uma reta.

Exemplo:

```python
spearman = df['X'].corr(
    df['Y'],
    method='spearman'
)
```

Comparação:

```text
PEARSON   → relação linear
SPEARMAN  → relação monotônica / ranking
```

### Exemplo da aula

```python
x_curva = np.arange(1, 20)
y_curva = x_curva ** 3

df_curva = pd.DataFrame({
    'X': x_curva,
    'Y': y_curva
})

pearson = df_curva['X'].corr(
    df_curva['Y'],
    method='pearson'
)

spearman = df_curva['X'].corr(
    df_curva['Y'],
    method='spearman'
)
```

Mesmo sendo uma curva, a ordenação continua perfeita, então Spearman consegue capturar a relação monotônica.

---

---

# 23. 🕸️ COVARIÂNCIA

Covariância mostra como duas variáveis variam juntas.

```text
positiva → tendem a subir juntas
negativa → uma sobe enquanto outra tende a cair
próxima de zero → pouca relação linear conjunta
```

Diferença importante:

```text
COVARIÂNCIA → depende da escala/unidade
CORRELAÇÃO  → normalizada, facilitando comparação
```

---

---

# 24. 📍 SCATTERPLOT

O gráfico de dispersão mostra visualmente a relação entre X e Y.

```python
sns.scatterplot(
    data=df,
    x='Tempo_Tela_Min',
    y='Gasto_Site_R$'
)
plt.show()
```

### Leitura

```text
↗ pontos subindo → correlação positiva
↘ pontos descendo → correlação negativa
☁ pontos espalhados → pouca correlação
```

---

---

# 25. 🔥 HEATMAP / MATRIZ DE CORRELAÇÃO

Primeiro:

```python
matriz_correlacao = df.corr()
```

Ver a correlação com uma variável:

```python
print(
    matriz_correlacao['Preco']
    .sort_values(ascending=False)
)
```

Heatmap:

```python
sns.heatmap(
    matriz_correlacao,
    annot=True,
    fmt=".2f"
)
plt.show()
```

### Por que usar?

Porque uma matriz grande fica difícil de interpretar apenas olhando números.

O heatmap transforma os valores em um mapa visual.

---

---

# 26. 🏠 CALIFORNIA HOUSING — EXEMPLO DA AULA

Importação:

```python
from sklearn.datasets import fetch_california_housing
```

Carregar:

```python
data = fetch_california_housing()
```

DataFrame:

```python
df = pd.DataFrame(
    data.data,
    columns=data.feature_names
)
```

Preço:

```python
df['Preco'] = data.target * 100000
```

Renomeando:

```python
df = df.rename(columns={
    'MedInc': 'Renda_Bairro',
    'HouseAge': 'Idade_Casa',
    'AveRooms': 'Qtd_Quartos',
    'AveBedrms': 'Qtd_Suites',
    'Population': 'Populacao',
    'AveOccup': 'Ocupacao_Media',
    'Latitude': 'Latitude',
    'Longitude': 'Longitude'
})
```

Correlação:

```python
matriz_correlacao = df.corr()

print(
    matriz_correlacao['Preco']
    .sort_values(ascending=False)
)
```

A aula destaca que `Renda_Bairro` apresenta uma correlação positiva relevante com `Preco`.

---

---

# 27. 🧹 LIMPEZA DE DADOS E CORRELAÇÃO

Na base imobiliária existe um problema de teto artificial no preço.

Filtro:

```python
df_limpo = df[
    df['Preco'] < 500000
]
```

Nova matriz:

```python
matriz_limpa = df_limpo.corr()
```

Extraindo Pearson:

```python
coeficiente_puro = matriz_limpa.loc[
    'Renda_Bairro',
    'Preco'
]
```

Comparando:

```python
print(
    matriz_correlacao.loc[
        'Renda_Bairro',
        'Preco'
    ]
)

print(
    matriz_limpa.loc[
        'Renda_Bairro',
        'Preco'
    ]
)
```

### Ideia da aula

Ruídos/anomalias podem alterar a correlação observada.

Por isso:

```text
dados sujos
   ↓
correlação
   ↓
pode ficar distorcida

dados tratados
   ↓
correlação
   ↓
pode representar melhor a relação
```

---

---

# 28. 🤖 REGRESSÃO LINEAR

Agora saímos de:

```text
"Existe relação?"
```

para:

```text
"Consigo prever Y usando X?"
```

Modelo:

```text
Y = aX + b
```

Onde:

```text
a = peso / coeficiente
b = intercepto / viés
```

---

---

# 29. 🧠 LINEARREGRESSION

Importação:

```python
from sklearn.linear_model import LinearRegression
```

Criar modelo:

```python
modelo = LinearRegression()
```

Treinar:

```python
modelo.fit(X, y)
```

Prever:

```python
previsoes = modelo.predict(X)
```

---

---

# 30. X E y — PEGADINHA DE PROVA

No exemplo:

```python
X = df_limpo[['Tempo_Tela_Min']]
y = df_limpo['Gasto_Site_R$']
```

### X

É a variável de entrada/preditora.

```text
Tempo_Tela_Min
```

### y

É a variável que queremos prever.

```text
Gasto_Site_R$
```

### Por que `[['Tempo_Tela_Min']]`?

Porque o `scikit-learn` espera X como uma matriz 2D:

```text
[[15],
 [20],
 [30]]
```

Enquanto `y` pode ser uma série 1D.

---

---

> 📊 **R² = capacidade explicativa** | **RMSE = tamanho do erro**.

# 31. MÉTRICA R²

Importação:

```python
from sklearn.metrics import r2_score
```

Cálculo:

```python
r2 = r2_score(y, previsoes)
```

O R² indica quanto da variação de `y` é explicada pelo modelo.

Forma intuitiva:

```text
R² próximo de 1 → modelo explica bastante
R² próximo de 0 → modelo explica pouco
```

Em determinados contextos o R² pode ser negativo em dados de teste, mas para a revisão principal memorize a interpretação acima.

---

---

# 32. RMSE

RMSE = Root Mean Squared Error.

Ele mede o tamanho típico do erro na mesma unidade de `y`.

Importação usada na aula:

```python
from sklearn.metrics import root_mean_squared_error
```

Cálculo:

```python
rmse = root_mean_squared_error(
    y,
    previsoes
)
```

Se:

```text
RMSE = R$ 100
```

a interpretação prática é que o erro típico do modelo está na ordem de R$ 100.

### ⚠️ Decore

```text
R²   → qualidade explicativa
RMSE → tamanho do erro
```

---

---

# 33. PESO E VIÉS DO MODELO

Depois do treinamento:

```python
peso = modelo.coef_[0]
vies = modelo.intercept_
```

Equação:

```python
print(
    f"Gasto = ({peso:.2f} * Minutos) + {vies:.2f}"
)
```

Se o modelo produzir:

```text
Gasto = 24.50 × Minutos + 150
```

Então:

- `24.50` = coeficiente/peso
- `150` = intercepto/viés

---

---

# 34. 🔮 PREVISÃO PARA UM NOVO CLIENTE

Para 45 minutos:

```python
previsao_45_min = modelo.predict(
    [[45]]
)[0]
```

O `[0]` pega o primeiro valor da previsão.

---

---

# 35. 📈 CENÁRIOS USANDO RMSE

Previsão central:

```python
previsao_45_min
```

Teto:

```python
teto = previsao_45_min + rmse
```

Piso:

```python
piso = previsao_45_min - rmse
```

Relatório:

```python
print(f"Previsão: R$ {previsao_45_min:.2f}")
print(f"Otimista: R$ {teto:.2f}")
print(f"Pessimista: R$ {piso:.2f}")
```

### ⚠️ Atenção

Isso é uma forma didática de apresentar a margem de erro do modelo. Não significa que `previsão ± RMSE` seja automaticamente um intervalo de confiança ou previsão formal.

---

---

# 36. 🧪 CASO TECHMARKET — REVISÃO COMPLETA

A revisão da aula 07 junta praticamente tudo.


## Setup

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from scipy import stats
from sklearn.linear_model import LinearRegression
from sklearn.metrics import root_mean_squared_error, r2_score

sns.set_theme(style="whitegrid")

np.random.seed(42)

n_clientes = 1000

idades = np.random.randint(18, 65, n_clientes)

emails = [
    f"cliente_{i}@email.com"
    for i in range(n_clientes)
]

regioes = np.random.choice(
    ['Sul', 'Sudeste', 'Nordeste'],
    n_clientes,
    p=[0.2, 0.5, 0.3]
)

versoes = np.random.choice(
    ['A', 'B'],
    n_clientes
)

tempo_A = np.random.normal(15, 5, 500)
tempo_B = np.random.normal(17, 5, 500)

tempo_tela = np.concatenate([
    tempo_A,
    tempo_B
])

np.random.shuffle(tempo_tela)

gasto = (
    tempo_tela * 25
    + 150
    + np.random.normal(0, 100, n_clientes)
)

gasto[10] = 9500
gasto[50] = 8800

df = pd.DataFrame({
    'Idade': idades,
    'Email': emails,
    'Regiao': regioes,
    'Versao_Site': versoes,
    'Tempo_Tela_Min': np.abs(tempo_tela),
    'Gasto_Site_R$': np.abs(gasto)
})
```

---

---

# 37. 🔐 FASE LGPD — REMOVER PII

A base possui `Email`, que identifica diretamente o cliente.

Se a análise não precisa do e-mail, ele deve ser removido da base analítica:

```python
df_seguro = df.drop(
    columns=['Email']
)

df_seguro.head()
```

### Ideia

```text
Se não precisa do dado pessoal:
→ não exponha
→ minimize a coleta/uso
→ reduza o risco
```

Isso está relacionado ao princípio de **necessidade/minimização** no tratamento de dados pessoais.

---

---

# 38. 📊 FASE ESTATÍSTICA TECHMARKET

Moda:

```python
moda_regiao = df_seguro[
    'Regiao'
].mode()[0]

print(moda_regiao)
```

Média:

```python
media_gasto = df_seguro[
    'Gasto_Site_R$'
].mean()
```

Mediana:

```python
mediana_gasto = df_seguro[
    'Gasto_Site_R$'
].median()
```

### Interpretação

Se:

```text
média > mediana
```

pode indicar assimetria à direita, especialmente se existirem valores extremos altos.

No exemplo foram inseridos:

```text
R$ 8.800
R$ 9.500
```

---

---

# 39. 📊 FASE VISUAL TECHMARKET

Barplot:

```python
plt.figure(figsize=(8, 5))

sns.barplot(
    data=df_seguro,
    x='Regiao',
    y='Gasto_Site_R$'
)

plt.title('Gasto Médio por Região')
plt.show()
```

Boxplot:

```python
plt.figure(figsize=(8, 3))

sns.boxplot(
    data=df_seguro,
    x='Gasto_Site_R$'
)

plt.title(
    'Distribuição Geral dos Gastos (BoxPlot)'
)

plt.show()
```

O boxplot permite identificar visualmente os valores extremos.

---

---

# 40. 🧪 FASE TESTE A/B TECHMARKET

Separação:

```python
tempo_A = df_seguro[
    df_seguro['Versao_Site'] == 'A'
]['Tempo_Tela_Min']

tempo_B = df_seguro[
    df_seguro['Versao_Site'] == 'B'
]['Tempo_Tela_Min']
```

Teste:

```python
resultado_teste = stats.ttest_ind(
    tempo_A,
    tempo_B
)

p_valor = resultado_teste.pvalue
```

Decisão:

```python
if p_valor < 0.05:
    print(
        "Diferença estatisticamente significativa."
    )
else:
    print(
        "Diferença não significativa."
    )
```

---

---

# 41. 🔗 FASE CORRELAÇÃO TECHMARKET

```python
correlacao = df_seguro[
    'Tempo_Tela_Min'
].corr(
    df_seguro['Gasto_Site_R$']
)

print(
    f"Correlação de Pearson: {correlacao:.2f}"
)
```

Visual:

```python
sns.scatterplot(
    data=df_seguro,
    x='Tempo_Tela_Min',
    y='Gasto_Site_R$'
)

plt.title(
    'Correlação: Tempo de Tela vs Gasto no Site'
)

plt.show()
```

---

---

# 42. 🤖 FASE IA TECHMARKET

Limpeza dos outliers de gasto:

```python
df_limpo = df_seguro[
    df_seguro['Gasto_Site_R$'] < 8000
]
```

X:

```python
X = df_limpo[
    ['Tempo_Tela_Min']
]
```

y:

```python
y = df_limpo[
    'Gasto_Site_R$'
]
```

Modelo:

```python
modelo = LinearRegression()

modelo.fit(X, y)
```

Previsões:

```python
previsoes = modelo.predict(X)
```

Métricas:

```python
r2 = r2_score(
    y,
    previsoes
)

rmse = root_mean_squared_error(
    y,
    previsoes
)
```

Peso:

```python
peso = modelo.coef_[0]
```

Viés:

```python
vies = modelo.intercept_
```

Equação:

```python
print(
    f"Gasto = ({peso:.2f} * Minutos) + {vies:.2f}"
)
```

---

---

# 43. 🔮 PREVISÃO TECHMARKET — 45 MINUTOS

```python
previsao_45_min = modelo.predict(
    [[45]]
)[0]

teto = previsao_45_min + rmse

piso = previsao_45_min - rmse

print(
    f"Previsão: R$ {previsao_45_min:.2f}"
)

print(
    f"Teto: R$ {teto:.2f}"
)

print(
    f"Piso: R$ {piso:.2f}"
)
```

### Fluxo completo

```text
BASE
 ↓
REMOVE PII
 ↓
DESCREVE
 ↓
VISUALIZA
 ↓
IDENTIFICA OUTLIERS
 ↓
TESTE A/B
 ↓
CORRELAÇÃO
 ↓
LIMPA DADOS
 ↓
REGRESSÃO LINEAR
 ↓
R² + RMSE
 ↓
PREVISÃO
```

---

---

# 44. 🛡️ GOVERNANÇA DE DADOS

Governança de dados é o conjunto de **regras, papéis, responsabilidades, processos e controles** usados para garantir que os dados sejam administrados corretamente.

Objetivos típicos:

- qualidade;
- segurança;
- disponibilidade;
- integridade;
- privacidade;
- rastreabilidade;
- conformidade;
- responsabilidade sobre os dados.


## Governança ≠ apenas segurança

```text
Governança
→ quem pode usar?
→ para quê?
→ quais regras?
→ quem é responsável?
→ como garantir qualidade?
→ como monitorar?

Segurança
→ como proteger?
→ contra acesso indevido?
→ contra alteração?
→ contra perda?
```

Segurança é parte importante da governança, mas governança é mais ampla.

---

---

# 45. 🔐 SEGURANÇA DE DADOS

Os três pilares clássicos:


## Confidencialidade

Somente pessoas autorizadas acessam.

```text
"Quem pode ver?"
```


## Integridade

Dados não devem ser alterados indevidamente.

```text
"Os dados continuam corretos?"
```


## Disponibilidade

Dados/sistemas devem estar acessíveis quando necessários.

```text
"Consigo acessar quando preciso?"
```

### CIA

```text
C = Confidencialidade
I = Integridade
A = Disponibilidade
```

---

---

# 46. 🇧🇷 LGPD

A LGPD é a Lei Geral de Proteção de Dados Pessoais.

Ela trata do uso/tratamento de dados pessoais.


## Dado pessoal

Informação relacionada a uma pessoa natural identificada ou identificável.

Exemplos:

```text
nome
CPF
e-mail
telefone
endereço
```


## Dado pessoal sensível

Categoria que recebe proteção reforçada.

Exemplos incluem:

```text
origem racial/étnica
religião
opinião política
saúde
vida sexual
dado genético
dado biométrico
```

---

---

> 🔐 **PALAVRA-CHAVE:** finalidade + adequação + necessidade + transparência + segurança + prevenção + não discriminação + responsabilização.

# 47. 🎯 PRINCÍPIOS IMPORTANTES DA LGPD

Para prova, associe:

### Finalidade

Usar o dado para propósitos legítimos e informados.

### Adequação

O tratamento deve ser compatível com a finalidade.

### Necessidade

Usar somente o necessário.

### Transparência

O titular deve receber informações claras.

### Segurança

Proteger os dados.

### Prevenção

Adotar medidas para evitar danos.

### Não discriminação

Não usar os dados para fins discriminatórios ilícitos/abusivos.

### Responsabilização e prestação de contas

Demonstrar que as medidas de proteção e conformidade são efetivas.

---

---

# 48. 🔑 BASE LEGAL ≠ CONSENTIMENTO SEMPRE

Um erro comum:

> "LGPD = sempre pedir consentimento."

Não.

A LGPD possui diferentes bases legais para tratamento.

Consentimento é uma delas.

Outras hipóteses legais podem permitir o tratamento conforme o contexto.

### Para prova

```text
LGPD não significa:
"sempre pedir autorização"

LGPD significa:
"ter fundamento jurídico + finalidade + necessidade
+ proteção + transparência + responsabilidade"
```

---

---

# 49. 👤 ANONIMIZAÇÃO / PSEUDONIMIZAÇÃO

### Anonimização

Processo que busca impedir que o dado seja associado, por meios razoáveis, a uma pessoa.

### Pseudonimização

Substitui identificadores diretos por códigos, mas pode existir possibilidade de reidentificação dependendo das informações adicionais.

### Na aula

Quando o algoritmo não precisa saber quem é o cliente:

```python
df_seguro = df.drop(columns=['Email'])
```

é uma forma simples de **minimização/exclusão de identificador desnecessário**.

---

---

# 50. 🧠 GOVERNANÇA × SEGURANÇA × LGPD

```text
GOVERNANÇA
    ↓
regras + responsabilidades + processos + qualidade

SEGURANÇA
    ↓
proteção dos dados e sistemas

LGPD
    ↓
regras legais para tratamento de dados pessoais
```

Eles se relacionam, mas não são sinônimos.

---

---

# 51. 🧩 PEGADINHAS QUE PODEM CAIR


## "Média maior que mediana significa automaticamente que existem outliers?"

Não automaticamente.

Pode indicar assimetria à direita e pode ser compatível com outliers, mas deve-se investigar a distribuição.

---


## "Correlação prova causalidade?"

**Não.**

```text
Correlação ≠ causalidade
```

---


## "p = 0.03 significa 97% de chance de H1 ser verdadeira?"

**Não.**

O p-valor não é a probabilidade de a hipótese ser verdadeira.

---


## "p = 0.20 prova que não existe diferença?"

Não.

Significa que não houve evidência suficiente para rejeitar H0 no nível de significância adotado.

---


## "R² é o erro do modelo?"

Não.

```text
R² → capacidade explicativa
RMSE → tamanho do erro
```

---


## "Pearson e Spearman são iguais?"

Não.

```text
Pearson  → relação linear
Spearman → ranking/relação monotônica
```

---


## "Boxplot e histograma mostram a mesma coisa?"

Não.

```text
Boxplot → resumo por quartis + outliers
Histograma → distribuição por intervalos
```

---

---

# 52. ⚡ CÓDIGOS PARA DECORAR


## Estatística

```python
df['x'].mean()
df['x'].median()
df['x'].mode()[0]
df['x'].var()
df['x'].std()
df['x'].describe()

df['x'].quantile(0.25)
df['x'].quantile(0.50)
df['x'].quantile(0.75)
```


## IQR

```python
q1 = df['x'].quantile(0.25)
q3 = df['x'].quantile(0.75)

iqr = q3 - q1

li = q1 - 1.5 * iqr
ls = q3 + 1.5 * iqr
```


## Limpeza

```python
df_limpo = df[
    (df['x'] >= li) &
    (df['x'] <= ls)
]
```


## Visualização

```python
sns.barplot(data=df, x='grupo', y='valor')

sns.boxplot(data=df, x='grupo', y='valor')

sns.histplot(data=df, x='valor', bins=10)

sns.scatterplot(data=df, x='x', y='y')

sns.heatmap(df.corr(), annot=True)
```


## Teste t

```python
from scipy import stats

a = df[df['grupo'] == 'A']['valor']
b = df[df['grupo'] == 'B']['valor']

estatistica, p = stats.ttest_ind(a, b)

if p < 0.05:
    print("Significativo")
else:
    print("Não significativo")
```


## Correlação

```python
df['x'].corr(df['y'])
```

Pearson:

```python
df['x'].corr(df['y'], method='pearson')
```

Spearman:

```python
df['x'].corr(df['y'], method='spearman')
```


## Regressão

```python
from sklearn.linear_model import LinearRegression

X = df[['x']]
y = df['y']

modelo = LinearRegression()
modelo.fit(X, y)

previsoes = modelo.predict(X)
```


## Métricas

```python
from sklearn.metrics import r2_score
from sklearn.metrics import root_mean_squared_error

r2 = r2_score(y, previsoes)

rmse = root_mean_squared_error(
    y,
    previsoes
)
```


## Modelo

```python
modelo.coef_
modelo.intercept_
```


## Nova previsão

```python
modelo.predict([[45]])
```

---

---

# 53. 🧠 COMO RESPONDER QUESTÕES TEÓRICAS


## Se perguntarem "qual medida é mais resistente a outliers?"

**Mediana.**


## "Qual medida é mais afetada por outliers?"

**Média** e **desvio padrão**.


## "Como encontrar outliers pelo método das cercas?"

```text
IQR = Q3 - Q1

LI = Q1 - 1.5IQR
LS = Q3 + 1.5IQR
```


## "Como testar diferença entre duas médias?"

```python
stats.ttest_ind(grupo_A, grupo_B)
```


## "Qual o corte usado na aula?"

```text
α = 0.05
```


## "p < 0.05?"

```text
rejeita H0
→ diferença estatisticamente significativa
```


## "p >= 0.05?"

```text
não rejeita H0
→ evidência insuficiente de diferença
```


## "Como medir correlação?"

```python
df['A'].corr(df['B'])
```


## "Pearson ou Spearman?"

```text
reta/linear → Pearson
ranking/monotônica → Spearman
```


## "Qual gráfico para relação entre duas variáveis?"

```text
Scatterplot
```


## "Qual gráfico mostra outliers?"

```text
Boxplot
```


## "Qual gráfico mostra distribuição?"

```text
Histograma
```


## "Qual métrica mede erro da regressão na unidade original?"

```text
RMSE
```


## "Qual métrica mostra quanto da variação é explicada?"

```text
R²
```

---

---

# 54. 🚀 REVISÃO DE 5 MINUTOS ANTES DA PROVA

> ⭐ **LEIA ESTA PARTE POR ÚLTIMO, ANTES DE IR PARA A PROVA.**

Se tiver pouquíssimo tempo, memorize isto:

```text
MÉDIA
→ soma / quantidade
→ sensível a outliers

MEDIANA
→ valor central
→ resistente a outliers

MODA
→ valor mais frequente

VARIÂNCIA
→ dispersão ao quadrado

DESVIO PADRÃO
→ √variância
→ mesma unidade dos dados

Q1
→ 25%

Q2
→ 50% = mediana

Q3
→ 75%

IQR
→ Q3 - Q1

LIMITE INFERIOR
→ Q1 - 1.5IQR

LIMITE SUPERIOR
→ Q3 + 1.5IQR

BOXPLOT
→ quartis + mediana + outliers

HISTOGRAMA
→ distribuição

STURGES
→ k = 1 + 3.322 log10(N)

TESTE T
→ compara dois grupos

P < 0.05
→ rejeita H0

CORRELAÇÃO
→ associação, NÃO causalidade

PEARSON
→ linear

SPEARMAN
→ ranking/monotônica

SCATTERPLOT
→ X × Y

HEATMAP
→ matriz visual

X
→ entrada/preditor

y
→ variável a prever

REGRESSÃO LINEAR
→ aprende X → y

R²
→ poder explicativo

RMSE
→ tamanho típico do erro

LGPD
→ proteção de dados pessoais

NECESSIDADE
→ usar apenas o necessário

GOVERNANÇA
→ regras + papéis + processos + controles
```

---

---

# 55. 🏁 FLUXO COMPLETO PARA DECORAR

```text
                 DADOS
                   │
                   ▼
          ┌─────────────────┐
          │  DataFrame      │
          │     Pandas      │
          └────────┬────────┘
                   │
                   ▼
          ESTATÍSTICA DESCRITIVA
          │
          ├── média
          ├── mediana
          ├── moda
          ├── std
          └── describe
                   │
                   ▼
             VISUALIZAÇÃO
          │
          ├── boxplot
          ├── histplot
          ├── barplot
          └── scatterplot
                   │
                   ▼
              OUTLIERS?
                   │
                   ▼
             Q1 / Q3 / IQR
                   │
                   ▼
          CERCAS MATEMÁTICAS
                   │
                   ▼
              DADOS LIMPOS
                   │
          ┌────────┴─────────┐
          ▼                  ▼
       TESTE A/B         CORRELAÇÃO
          │                  │
          ▼                  ▼
       p-valor          Pearson/Spearman
          │                  │
          └────────┬─────────┘
                   ▼
             REGRESSÃO
                   │
                   ▼
              R² + RMSE
                   │
                   ▼
               PREVISÃO
```

---

---

# 56. 🔥 DECORAR COM ASSOCIAÇÕES

```text
"QUANTO?"                  → describe()

"NO MEIO?"                 → median()

"MAIS FREQUENTE?"          → mode()

"DISPERSÃO?"               → std()

"25 / 50 / 75%?"           → quantile()

"50% DO MEIO?"             → IQR

"FORA DA CERCA?"           → outlier

"CAIXA?"                   → boxplot

"FORMATO DA DISTRIBUIÇÃO?" → histplot

"DOIS GRUPOS?"              → ttest_ind()

"p < 0.05?"                 → rejeitar H0

"RELAÇÃO?"                 → corr()

"LINHA?"                   → Pearson

"RANKING?"                 → Spearman

"NUVEM DE PONTOS?"         → scatterplot

"MAPA DE CORRELAÇÃO?"      → heatmap

"PREVER?"                  → LinearRegression

"EXPLICA QUANTO?"          → R²

"ERRO EM REAIS?"           → RMSE

"DADO PESSOAL DESNECESSÁRIO?"
                           → remover/minimizar

"REGRAS E RESPONSABILIDADES?"
                           → governança
```

---

---

# 57. ⚠️ ÚLTIMO ALERTA — NÃO CONFUNDA

| Conceito | NÃO confundir com |
|---|---|
| Média | Mediana |
| Variância | Desvio padrão |
| Q1 | Q3 |
| IQR | Limite superior |
| Outlier | Qualquer valor alto |
| Boxplot | Histograma |
| p-valor | Probabilidade de H0 ser verdadeira |
| Correlação | Causalidade |
| Pearson | Spearman |
| R² | RMSE |
| LGPD | Apenas consentimento |
| Governança | Apenas segurança |

---

---

# 🎯 FRASE FINAL PARA A PROVA

> **Primeiro eu entendo os dados com estatística descritiva, depois visualizo, identifico e trato anomalias, faço inferência quando preciso comparar grupos, verifico correlações sem confundir correlação com causalidade e, se houver relação adequada, posso construir um modelo preditivo avaliando-o com R² e RMSE. Tudo isso deve respeitar segurança, governança e proteção de dados pessoais.**



---

---

# 📎 APÊNDICE — CÓDIGOS DOS NOTEBOOKS DA REVISÃO



## `Cópia_de_Data_Science_Aula_4_comentado.ipynb`

### Célula 1

```python
Claro. Essa linha faz duas coisas na coluna salario:

df['salario'] = df['salario'].clip(lower=500).round(2)
1. clip(lower=500)

Define 500 como valor mínimo.

Ou seja, qualquer salário menor que 500 passa a ser 500.

Exemplo:

Antes:
300
450
500
1200
2500

Depois do clip:
500
500
500
1200
2500

É como dizer:

"Nenhum valor pode ficar abaixo de 500."

2. .round(2)

Arredonda os valores para 2 casas decimais.

Exemplo:

1234.5678 → 1234.57
987.1234  → 987.12
500.999   → 501.00
3. O resultado completo
df['salario'] = df['salario'].clip(lower=500).round(2)

Lendo da esquerda para direita:

Pegue a coluna salario → não deixe nenhum valor abaixo de 500 → arredonde para 2 casas decimais → salve novamente na coluna salario.

Exemplo

Se tivermos:

salario
-------
350.456
480.789
1200.567
2500.123

Depois:

salario
-------
500.00
500.00
1200.57
2500.12

Resumo:

.clip(lower=500)  # mínimo = 500
.round(2)         # 2 casas decimais

E o df['salario'] = é importante porque substitui a coluna original pelo resultado dessas operações
```

### Célula 2

```python
import pandas as pd
import numpy as np
import math
import seaborn as sns
import matplotlib.pyplot as plt

---

# 1. A Semente Mágica: Trava a aleatoriedade (todos terão a mesma base)
np.random.seed(42)

---

# 2. O Miolo da População: 8.000 salários (Média 4k, Desvio 800)
salarios_normais = np.random.normal(loc=4000, scale=800, size=8000)

---

# 3. As Anomalias (Outliers para estourar as Cercas)
---

# np.random.uniform(início, fim, quantidade)
outliers_baixos = np.random.uniform(500, 900, size=50)       # 50 Estagiários
outliers_altos = np.random.uniform(25000, 150000, size=15)   # 15 CEOs/Diretores

---

# 4. Consolidando o DataFrame
todos_salarios = np.concatenate([salarios_normais, outliers_baixos, outliers_altos])
df = pd.DataFrame({'salario': todos_salarios})

---

# Limpeza e formatação
df['salario'] = df['salario'].clip(lower=500).round(2)

print(f"Base gerada com sucesso! Total de registros: {len(df)}")
```

### Célula 4

```python
metricas_salario = df['salario'].describe()

print(metricas_salario)
```

### Célula 6

```python
q1 = 3443.88
q3 = 4537.91

---

# Calcule o IQR e o Limite Superior abaixo:
iqr = q3 - q1
limite_suprior = q3 + (1.5 * iqr)

print(f"IQR = {iqr:.2f}\nLimite Superior = {limite_suprior:.2f}")
```

### Célula 8

```python
sns.boxplot(x=df['salario'])

---

# plt.xlim(0, 10000)

plt.show()
```

### Célula 10

```python
---

# Pega a quantidade total de linhas do DataFrame.
---

# Ou seja, quantos salários existem no conjunto de dados.
N = len(df)

---

# Calcula a quantidade ideal de "bins" (intervalos) para o histograma
---

# usando a Regra de Sturges.
#
---

# Fórmula:
---

# k = 1 + 3.322 * log10(N)
#
---

# N = quantidade de dados
---

# log10 = logaritmo na base 10
k_matematico = 1 + 3.322 * math.log10(N)

---

# Arredonda o resultado da fórmula para obter um número inteiro de bins.
k_bins = round(k_matematico)

---

# Mostra no terminal quantos bins foram calculados.
print(f"O número de bins calculado pela Regra de Sturges é: {k_bins}")

---

# Define o tamanho da figura do gráfico:
---

# 10 = largura
---

# 5 = altura
plt.figure(figsize=(10, 5))

---

# Cria o histograma dos salários.
#
---

# x=df['salario'] -> usa a coluna salario do DataFrame
---

# bins=k_bins -> divide os salários na quantidade de intervalos calculada
---

# color='skyblue' -> define a cor das barras
sns.histplot(x=df['salario'], bins=k_bins, color='skyblue')

---

# Define o título do gráfico.
---

# O valor de k_bins é colocado automaticamente no título.
plt.title(f'Histograma de Salários com {k_bins} Bins')

---

# Exibe o gráfico na tela.
plt.show()
```

### Célula 12

```python
---

# Define o maior salário que será permitido na nova base.
---

# Neste caso, salários acima de 6300 serão considerados
---

# valores que queremos remover.
limite_superior = 6300

---

# Filtra o DataFrame original.
---

# Mantém somente as linhas em que o salário é menor ou igual a 6300.
#
---

# df['salario'] <= limite_superior
---

#       ↓
---

# Cria uma condição True/False para cada linha.
#
---

# True  -> mantém a linha
---

# False -> remove a linha
df_limpo = df[df['salario'] <= limite_superior]

---

# Conta quantas linhas existem na nova base depois do filtro.
tamanho_novo = len(df_limpo)

---

# Mostra no terminal o tamanho da nova base.
print(f"O tamanho da nova base df_limpo é: {tamanho_novo} registros.")
```



## `Cópia_de_Data_Science_Aula_4_normal.ipynb`

### Célula 1

```python
import pandas as pd
import numpy as np
import math
import seaborn as sns
import matplotlib.pyplot as plt

---

# 1. A Semente Mágica: Trava a aleatoriedade (todos terão a mesma base)
np.random.seed(42)

---

# 2. O Miolo da População: 8.000 salários (Média 4k, Desvio 800)
salarios_normais = np.random.normal(loc=4000, scale=800, size=8000)

---

# 3. As Anomalias (Outliers para estourar as Cercas)
outliers_baixos = np.random.uniform(500, 900, size=50)       # 50 Estagiários
outliers_altos = np.random.uniform(25000, 150000, size=15)   # 15 CEOs/Diretores

---

# 4. Consolidando o DataFrame
todos_salarios = np.concatenate([salarios_normais, outliers_baixos, outliers_altos])
df = pd.DataFrame({'salario': todos_salarios})

---

# Limpeza e formatação
df['salario'] = df['salario'].clip(lower=500).round(2)

print(f"Base gerada com sucesso! Total de registros: {len(df)}")
```

### Célula 4

```python
metricas_salario = df['salario'].describe()

print(metricas_salario)
```

### Célula 6

```python
q1 = 3443.88
q3 = 4537.91

---

# Calcule o IQR e o Limite Superior abaixo:
iqr = q3 - q1
limite_suprior = q3 + (1.5 * iqr)

print(f"IQR = {iqr:.2f}\nLimite Superior = {limite_suprior:.2f}")
```

### Célula 8

```python
sns.boxplot(x=df['salario'])

---

# plt.xlim(0, 10000)

plt.show()
```

### Célula 10

```python
N = len(df)

k_matematico = 1 + 3.322 * math.log10(N)

k_bins = round(k_matematico)
print(f"O número de bins calculado pela Regra de Sturges é: {k_bins}")

plt.figure(figsize=(10, 5))

sns.histplot(x=df['salario'], bins=k_bins, color='skyblue')

plt.title(f'Histograma de Salários com {k_bins} Bins')

plt.show()
```

### Célula 12

```python
limite_superior = 6300

df_limpo = df[df['salario'] <= limite_superior]

tamanho_novo = len(df_limpo)

print(f"O tamanho da nova base df_limpo é: {tamanho_novo} registros.")
```



## `Aula_05_comentado.ipynb`

### Célula 2

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```

### Célula 3

```python
---

# Trava a aleatoriedade.
---

# Assim, toda vez que o código for executado, os mesmos
---

# números aleatórios serão gerados.
np.random.seed(42)

---

# Gera 500 valores para a versão A.
#
---

# loc=1200  -> média dos valores
---

# scale=350 -> desvio padrão
---

# size=500  -> quantidade de valores gerados
ticket_A = np.random.normal(loc=1200, scale=350, size=500)

---

# Gera 500 valores para a versão B.
#
---

# loc=1220  -> média dos valores
---

# scale=40  -> desvio padrão
---

# size=500  -> quantidade de valores gerados
ticket_B = np.random.normal(loc=1220, scale=40, size=500)

---

# Cria um DataFrame juntando as duas versões.
df = pd.DataFrame({
    
    # Cria a coluna 'versao'.
    # ['A'] * 500  -> cria 500 letras A
    # ['B'] * 500  -> cria 500 letras B
    #
    # Resultado:
    # A
    # A
    # ...
    # A   (500 vezes)
    # B
    # B
    # ...
    # B   (500 vezes)
    'versao': ['A'] * 500 + ['B'] * 500,

    # Junta os 500 valores de A com os 500 valores de B
    # em uma única coluna chamada 'ticket'.
    #
    # np.concatenate() significa "concatenar/juntar".
    'ticket': np.concatenate([ticket_A, ticket_B])
})
```

### Célula 5

```python
---

# Passo 2: O Barplot
sns.barplot(data=df, x='versao', y='ticket')
plt.show()
---

# VISUAL: A barra B vai parecer discretamente mais alta na tela.
```

### Célula 7

```python
from scipy import stats

---

# Filtramos as arrays puras
array_A = df[df['versao'] == 'A']['ticket']
array_B = df[df['versao'] == 'B']['ticket']

---

# Injetamos na máquina
estatistica, p_valor = stats.ttest_ind(array_A, array_B)

print(f"P-Valor extraído: {p_valor:.4f}")
if p_valor < 0.05:
    print("A diferença é real. Dêem o bônus.")
else:
    print("É ruído (Sorte). A tela nova é inútil.")


#Output do Console: P-Valor: 0.1384 -> É ruído. Tela inútil.
```

### Célula 10

```python
---

# 1. Achando os Quartis
q1 = df['ticket'].quantile(0.25)
q3 = df['ticket'].quantile(0.75)
iqr = q3 - q1

---

# 2. Criando o Limite Superior
limite_superior = q3 + 1.5 * iqr
print(f"O Limite Superior matemático é: R$ {limite_superior:.2f}")

limite_inferior = q1 - 1.5 * iqr
print(f"O Limite inferior matemático é: R$ {limite_inferior:.2f}")

---

# 3. A Cirurgia (Removendo Outliers)
df_limpo = df[(df['ticket'] <= limite_superior) & (df['ticket'] >= limite_inferior)]
print(f"Registros removidos: {len(df) - len(df_limpo)}")

---

# 4. Refazendo a separação dos grupos com a base limpa
grupo_A_limpo = df_limpo[df_limpo['versao'] == 'A']['ticket']
grupo_B_limpo = df_limpo[df_limpo['versao'] == 'B']['ticket']

---

# 5. O Novo Julgamento Computacional
t_stat_limpo, p_valor_limpo = stats.ttest_ind(grupo_A_limpo, grupo_B_limpo)

print(f"\nNOVO P-Valor: {p_valor_limpo:.4f}")
if p_valor_limpo < 0.05:
    print("Veredito Final: Com o ruído removido, o ganho se mostrou estatisticamente sólido! Bônus aprovado.")
else:
    print("Veredito Final: Mesmo sem outliers, foi pura sorte. A tela nova é inútil.")
```

### Célula 12

```python
---

# 1. A Prova Numérica
print("--- RAIO-X DESCRITIVO PÓS-CIRURGIA ---")
resumo_executivo = df_limpo.groupby('versao')['ticket'].describe()
display(resumo_executivo)

---

# 2. A Prova Visual
plt.figure(figsize=(8, 5))
sns.boxplot(
    data=df_limpo,
    x='versao',
    y='ticket',
    palette='Set2'
)
plt.title('Distribuição Limpa do Ticket Médio - Teste A/B', fontsize=14, fontweight='bold')
plt.ylabel('Valor da Compra (R$)')
plt.xlabel('Design da Interface')
plt.show()
```

### Célula 14

```python
---

# 2. A Prova Visual (Barplot do ticket médio após limpeza)
plt.figure(figsize=(9, 6))
sns.barplot(x=resumo_executivo.index, y=resumo_executivo['mean'], palette='viridis')
plt.title('Ticket Médio por Versão (Dados Limpos)', fontsize=16, fontweight='bold')
plt.ylabel('Ticket Médio (R$)', fontsize=12)
plt.xlabel('Versão', fontsize=12)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.ylim(resumo_executivo['mean'].min() * 0.9, resumo_executivo['mean'].max() * 1.1) # Ajusta o limite Y para melhor visualização
plt.show()

---

# Exibir a tabela descritiva novamente para referência numérica
display(resumo_executivo)
```



## `Aula_05_normal.ipynb`

### Célula 2

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
```

### Célula 3

```python
np.random.seed(42) # Trava o acaso
---

# Gerando 500 compras (A: média 120 / B: média 122) com muita variação
---

# loc = média; scale = desvio padrão; size = quantidade
ticket_A = np.random.normal(loc=1200, scale=350, size=500)
ticket_B = np.random.normal(loc=1220, scale=40, size=500)

---

# Unificando no DataFrame
df = pd.DataFrame({
    'versao': ['A']*500 + ['B']*500,
    'ticket': np.concatenate([ticket_A, ticket_B])
})
```

### Célula 5

```python
---

# Passo 2: O Barplot
sns.barplot(data=df, x='versao', y='ticket')
plt.show()
---

# VISUAL: A barra B vai parecer discretamente mais alta na tela.
```

### Célula 7

```python
from scipy import stats

---

# Filtramos as arrays puras
array_A = df[df['versao'] == 'A']['ticket']
array_B = df[df['versao'] == 'B']['ticket']

---

# Injetamos na máquina
estatistica, p_valor = stats.ttest_ind(array_A, array_B)

print(f"P-Valor extraído: {p_valor:.4f}")
if p_valor < 0.05:
    print("A diferença é real. Dêem o bônus.")
else:
    print("É ruído (Sorte). A tela nova é inútil.")


#Output do Console: P-Valor: 0.1384 -> É ruído. Tela inútil.
```

### Célula 10

```python
---

# 1. Achando os Quartis
q1 = df['ticket'].quantile(0.25)
q3 = df['ticket'].quantile(0.75)
iqr = q3 - q1

---

# 2. Criando o Limite Superior
limite_superior = q3 + 1.5 * iqr
print(f"O Limite Superior matemático é: R$ {limite_superior:.2f}")

limite_inferior = q1 - 1.5 * iqr
print(f"O Limite inferior matemático é: R$ {limite_inferior:.2f}")

---

# 3. A Cirurgia (Removendo Outliers)
df_limpo = df[(df['ticket'] <= limite_superior) & (df['ticket'] >= limite_inferior)]
print(f"Registros removidos: {len(df) - len(df_limpo)}")

---

# 4. Refazendo a separação dos grupos com a base limpa
grupo_A_limpo = df_limpo[df_limpo['versao'] == 'A']['ticket']
grupo_B_limpo = df_limpo[df_limpo['versao'] == 'B']['ticket']

---

# 5. O Novo Julgamento Computacional
t_stat_limpo, p_valor_limpo = stats.ttest_ind(grupo_A_limpo, grupo_B_limpo)

print(f"\nNOVO P-Valor: {p_valor_limpo:.4f}")
if p_valor_limpo < 0.05:
    print("Veredito Final: Com o ruído removido, o ganho se mostrou estatisticamente sólido! Bônus aprovado.")
else:
    print("Veredito Final: Mesmo sem outliers, foi pura sorte. A tela nova é inútil.")
```

### Célula 12

```python
---

# 1. A Prova Numérica
print("--- RAIO-X DESCRITIVO PÓS-CIRURGIA ---")
resumo_executivo = df_limpo.groupby('versao')['ticket'].describe()
display(resumo_executivo)

---

# 2. A Prova Visual
plt.figure(figsize=(8, 5))
sns.boxplot(
    data=df_limpo,
    x='versao',
    y='ticket',
    palette='Set2'
)
plt.title('Distribuição Limpa do Ticket Médio - Teste A/B', fontsize=14, fontweight='bold')
plt.ylabel('Valor da Compra (R$)')
plt.xlabel('Design da Interface')
plt.show()
```

### Célula 14

```python
---

# 2. A Prova Visual (Barplot do ticket médio após limpeza)
plt.figure(figsize=(9, 6))
sns.barplot(x=resumo_executivo.index, y=resumo_executivo['mean'], palette='viridis')
plt.title('Ticket Médio por Versão (Dados Limpos)', fontsize=16, fontweight='bold')
plt.ylabel('Ticket Médio (R$)', fontsize=12)
plt.xlabel('Versão', fontsize=12)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.ylim(resumo_executivo['mean'].min() * 0.9, resumo_executivo['mean'].max() * 1.1) # Ajusta o limite Y para melhor visualização
plt.show()

---

# Exibir a tabela descritiva novamente para referência numérica
display(resumo_executivo)
```



## `Data Science_Aula 06_Colab.ipynb`

### Célula 0

```python
---

# [CÉLULA 1] SETUP DO AMBIENTE E BIBLIOTECAS
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

---

# Configurando o visual dos gráficos para apresentação
sns.set_theme(style="whitegrid")
```

### Célula 1

```python
---

# [CÉLULA 2] SIMULAÇÃO: LENDO A NUVEM DE PONTOS
---

# Travando a aleatoriedade para o gráfico ser sempre igual
np.random.seed(42)
x_base = np.linspace(0, 100, 100)

---

# 1. Tendência Positiva
y_positivo = x_base + np.random.normal(0, 10, 100)

---

# 2. Tendência Negativa
y_negativo = 100 - x_base + np.random.normal(0, 10, 100)

---

# 3. Caos Total
x_caos = np.random.normal(50, 20, 100)
y_caos = np.random.normal(50, 20, 100)

---

# Plotando os cenários lado a lado
fig, axs = plt.subplots(1, 3, figsize=(18, 5))
sns.scatterplot(x=x_base, y=y_positivo, ax=axs[0], color='blue').set_title('Correlação Positiva (+)')
sns.scatterplot(x=x_base, y=y_negativo, ax=axs[1], color='red').set_title('Correlação Negativa (-)')
sns.scatterplot(x=x_caos, y=y_caos, ax=axs[2], color='gray').set_title('Sem Correlação (Caos)')
plt.show()
```

### Célula 3

```python
---

# [CÉLULA 4] O DUELO: PEARSON VS SPEARMAN
---

# Criando uma curva exponencial (y = x ao cubo)
x_curva = np.arange(1, 20)
y_curva = x_curva ** 3  
df_curva = pd.DataFrame({'X': x_curva, 'Y': y_curva})

---

# Calculando os coeficientes via Pandas
pearson = df_curva['X'].corr(df_curva['Y'], method='pearson')
spearman = df_curva['X'].corr(df_curva['Y'], method='spearman')

print(f"Força segundo PEARSON (A Reta): {pearson:.3f}")
print(f"Força segundo SPEARMAN (O Ranking): {spearman:.3f}")

plt.figure(figsize=(6,4))
sns.lineplot(x=x_curva, y=y_curva, marker='o', color='purple')
plt.title('A Cegueira do Pearson em Curvas Exponenciais')
plt.show()
```

### Célula 4

```python
---

# [CÉLULA 5] SETUP DA OFICINA PBL (CALIFORNIA HOUSING)
from sklearn.datasets import fetch_california_housing

---

# Baixando os dados embutidos na biblioteca
data = fetch_california_housing()
df = pd.DataFrame(data.data, columns=data.feature_names)

---

# Ajustando o preço para escala real (A base original estava reduzida)
df['Preco'] = data.target * 100000 

---

# Renomeando para português
df = df.rename(columns={
    'MedInc': 'Renda_Bairro',
    'HouseAge': 'Idade_Casa',
    'AveRooms': 'Qtd_Quartos',
    'AveBedrms': 'Qtd_Suites',
    'Population': 'Populacao',
    'AveOccup': 'Ocupacao_Media',
    'Latitude': 'Latitude',
    'Longitude': 'Longitude'
})

df.head()
```

### Célula 6

```python
---

# [CÉLULA 7] RESOLUÇÃO - DESAFIO 1
matriz_correlacao = df.corr()
print(matriz_correlacao['Preco'].sort_values(ascending=False))

---

# ===== INTERPRETAÇÃO DOS RESULTADOS =====
---

# A 'Renda_Bairro' possui o maior peso preditivo positivo (~ 0.68).
---

# Fatores físicos como a 'Idade_Casa' demonstraram pouquíssima relevância linear (~ 0.10).
---

# O modelo matemático indica que a riqueza da vizinhança sobrepuja a estrutura do imóvel.
```

### Célula 8

```python
---

# [CÉLULA 9] RESOLUÇÃO - DESAFIO 2
plt.figure(figsize=(10, 8))
sns.heatmap(matriz_correlacao, annot=True, cmap='coolwarm', fmt=".2f")
plt.title('Heatmap Imobiliário')
plt.show()

---

# ===== INTERPRETAÇÃO DOS RESULTADOS =====
---

# O gráfico evidencia uma correlação extrema (+0.85) entre Qtd_Quartos e Qtd_Suites.
---

# Lógica: Projetos arquitetônicos com área para múltiplos quartos naturalmente alocam mais suítes.
```

### Célula 10

```python
---

# [CÉLULA 11] RESOLUÇÃO - DESAFIO 3
plt.figure(figsize=(8, 6))
sns.scatterplot(data=df, x='Renda_Bairro', y='Preco', alpha=0.3)
plt.title('Dispersão: Renda vs Preço')
plt.show()

---

# ===== INTERPRETAÇÃO DOS RESULTADOS =====
---

# A nuvem de pontos ascende de maneira linear, validando o número de Pearson.
---

# Ruído identificado: Há um traço horizontal massivo fixado em US$ 500.000.
---

# Isso indica um teto artificial do sistema de captação que censurou propriedades mais caras.
```

### Célula 12

```python
---

# [CÉLULA 13] RESOLUÇÃO - DESAFIO 4

---

# 1. Filtro Booleano de Saneamento
df_limpo = df[df['Preco'] < 500000]

---

# 2. Recálculo da Matriz
matriz_limpa = df_limpo.corr()
coeficiente_puro = matriz_limpa.loc['Renda_Bairro', 'Preco']

print(f"Força de Pearson anterior: {matriz_correlacao.loc['Renda_Bairro', 'Preco']:.4f}")
print(f"Força de Pearson com o ruído cortado: {coeficiente_puro:.4f}")

---

# ===== INTERPRETAÇÃO DOS RESULTADOS =====
---

# A correlação genuína ampliou de 0.68 para quase 0.70.
---

# Anomalias em bancos de dados enfraquecem a confiabilidade de modelos lineares.
---

# Higienizar a base é etapa mandatória antes de inserir os dados na Inteligência Artificial.
```



## `Desafio e Revisao - Aula 07 (1).ipynb`

### Célula 1

```python
---

# RODAR ESTA CÉLULA PARA INICIALIZAR O AMBIENTE (CÓDIGO FORNECIDO)

import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
from scipy import stats
from sklearn.linear_model import LinearRegression
from sklearn.metrics import root_mean_squared_error, r2_score

sns.set_theme(style="whitegrid")

---

# --- GERANDO A BASE SINTÉTICA DA TECHMARKET ---
np.random.seed(42)
n_clientes = 1000

idades = np.random.randint(18, 65, n_clientes)
emails = [f"cliente_{i}@email.com" for i in range(n_clientes)]
regioes = np.random.choice(['Sul', 'Sudeste', 'Nordeste'], n_clientes, p=[0.2, 0.5, 0.3])
versoes = np.random.choice(['A', 'B'], n_clientes)

tempo_A = np.random.normal(15, 5, 500) 
tempo_B = np.random.normal(17, 5, 500) 
tempo_tela = np.concatenate([tempo_A, tempo_B])

---

# shuffle -> embaralha
np.random.shuffle(tempo_tela)


gasto = (tempo_tela * 25) + 150 + np.random.normal(0, 100, n_clientes)

---

# Criando anomalias/outliers para atrapalhar as análises
gasto[10] = 9500
gasto[50] = 8800

df = pd.DataFrame({
    'Idade': idades,
    'Email': emails,
    'Regiao': regioes,
    'Versao_Site': versoes,
    'Tempo_Tela_Min': np.abs(tempo_tela),
    'Gasto_Site_R$': np.abs(gasto)
})

print("Base gerada com sucesso! Bem-vindo à TechMarket.")
df.head()
```

### Célula 3

```python
---

# MISSÃO 1:
---

# 1. Exclua a coluna que expõe a identidade do cliente usando a função apropriada do Pandas.
---

# 2. Salve o resultado numa variável chamada 'df_seguro'.
---

# 3. Mostre as primeiras linhas da nova base para confirmar.

---

# SEU CÓDIGO AQUI:
df_seguro = df.drop(columns=['Email'])
df_seguro.head()
```

### Célula 5

```python
---

# MISSÃO 2:
---

# 1. Qual é a Região que mais acessa o site? Encontre e imprima a MODA.
---

# 2. Qual é a MÉDIA da coluna 'Gasto_Site_R$'?
---

# 3. Qual é a MEDIANA da coluna 'Gasto_Site_R$'?
---

# 4. Comente (#) logo abaixo: Comparando a média e a mediana, a sua base parece ter Outliers? Por quê?

---

# SEU CÓDIGO E RESPOSTA AQUI:

---

# O .mode() encontra o valor que mais se repete. O [0] pega o primeiro resultado da moda.
moda_regiao = df_seguro['Regiao'].mode()[0]
print(f"região que mais acessa o site: {moda_regiao}")

media_gasto = df_seguro['Gasto_Site_R$'].mean()
print(f"Média do gasto: R$ {media_gasto: .2f}")

mediana_gasto = df_seguro['Gasto_Site_R$'].median()
print(f"Mediana do gasto: R$ {mediana_gasto: .2f}")

---

# Q2 é mediana
---

# a media é maior que a mediana, indicando forte assimetria
---

# à direita (presença de valores extremos puxando o cálculo)

---

#  A média é maior que a mediana, indicando que existem valores
#mais altos influenciando a média. Isso é compatível com a presença 
#de outliers na base, como os gastos de R$ 8.800 e R$ 9.500 que foram
#inseridos.
```

### Célula 7

```python
---

# MISSÃO 3:
---

# Usando o Seaborn (sns):
---

# 1. Plote um gráfico de Barras (barplot) mostrando o Gasto (y) separado por Regiao (x).
---

# 2. Plote um Gráfico de Caixa (boxplot) do Gasto geral do site.
---

# 3. Comente (#): O boxplot ajudou a confirmar a sua desconfiança da Fase 2 sobre os Outliers? 

---

# SEU CÓDIGO E RESPOSTA AQUI:

plt.figure(figsize=(8, 5))

sns.barplot(data=df_seguro, x='Regiao', y='Gasto_Site_R$')
plt.title('Gasto Médio por Região')
plt.show()

plt.figure(figsize=(8, 3))

sns.boxplot(data=df_seguro, x='Gasto_Site_R$')
plt.title('Distribuição Geral dos Gastos (BoxPlot)')
plt.show()


---

# Sim. O boxplot motra uma"caixa" azul espremida no lado esquerdo mostra onde estão os gastos reais da esmagadora 
---

# maioria dos clientes (próximo de zero a mil reais). Já os pequenos pontos isolados, 
---

# na casa dos R$ 8.000 e R$ 9.000, são a prova visual clara dos outliers que estavam a 
---

# distorcer a média.
```

### Célula 9

```python
---

# MISSÃO 4:
---

# 1. Isole em uma variável (tempo_A) todo o 'Tempo_Tela_Min' apenas de quem acessou a 'Versao_Site' A.
---

# 2. Faça o mesmo para a Versão B (tempo_B).
---

# 3. Use o stats.ttest_ind do SciPy para comparar os dois grupos e extrair o p-valor.
---

# 4. Crie um IF/ELSE usando a regra de alfa (0.05). Se p < 0.05, imprima que a diferença é real e faça o deploy. Senão, chancele como ruído.

---

# SEU CÓDIGO AQUI:

tempo_A = df_seguro[df_seguro['Versao_Site'] == 'A']['Tempo_Tela_Min']

tempo_B = df_seguro[df_seguro['Versao_Site'] == 'B']['Tempo_Tela_Min']

resultado_teste = stats.ttest_ind(tempo_A, tempo_B)
p_valor = resultado_teste.pvalue

print(f"O p-valor calculado pelo SciPy foi: {p_valor:.5f}")

if p_valor < 0.05:
    print("A diferença é real e estatisticamente significativa! Pode fazer o deploy da Versão B.")
else:
    print("A diferença não é significativa, apenas ruído (acaso). Mantenha a Versão A.")
```

### Célula 11

```python
---

# MISSÃO 5.1 (A Prova do Crime):
---

# 1. Calcule a Correlação de Pearson entre Tempo_Tela e Gasto_Site.
---

# 2. Desenhe o Scatterplot dessas duas colunas.

---

# SEU CÓDIGO AQUI:

correlacao = df_seguro['Tempo_Tela_Min'].corr(df_seguro['Gasto_Site_R$'])
print(f"A Correlação de Pearson é: {correlacao:.2f}")

plt.figure(figsize=(8, 5))
sns.scatterplot(data=df_seguro, x='Tempo_Tela_Min', y='Gasto_Site_R$')
plt.title('Correlação: Tempo de Tela vs Gasto no Site')
plt.show()
```

### Célula 12

```python
---

# MISSÃO 5.2 (O Treinamento e as Métricas):
---

# IMPORTANTE: Filtrar a base tirando os dois Outliers de gasto gigante (ex: < 8000) para não enburrecer a IA.
---

# 1. Separe X (Tempo_Tela 2D) e y (Gasto_Site 1D).
---

# 2. Instancie e treine o LinearRegression.
---

# 3. Extraia as previsões.
---

# 4. Calcule o R2 Score e o RMSE (Use a nova função root_mean_squared_error importada no setup).
---

# 5. Imprima o Peso (coef_) e o Viés (intercept_).

---

# SEU CÓDIGO AQUI:

df_limpo = df_seguro[df_seguro['Gasto_Site_R$'] < 8000]

X = df_limpo[['Tempo_Tela_Min']]
y = df_limpo['Gasto_Site_R$']

modelo = LinearRegression()
modelo.fit(X, y)

previsoes = modelo.predict(X)

r2 = r2_score(y, previsoes)
rmse = root_mean_squared_error(y, previsoes)

print(f"Métricas do Modelo:")
print(f"R² Score: {r2:.4f}")
print(f"RMSE (Margem de Erro Média): R$ {rmse:.2f}\n")

peso = modelo.coef_[0]
vies = modelo.intercept_
print(f"A equação da IA é: Gasto = ({peso:.2f} * Minutos) + {vies:.2f}")
```

### Célula 13

```python
---

# MISSÃO FINAL BOSS (O Uso em Produção):
---

# O Diretor de Marketing perguntou: "Se o cliente ficar exatamente 45 minutos no nosso site, qual a previsão exata de dinheiro que ele vai deixar com a gente? E qual a nossa margem de erro financeira?"

---

# 1. Use modelo.predict() passando [[45]] como matriz de entrada.
---

# 2. Calcule o Cenário Otimista (Teto = Previsão + RMSE).
---

# 3. Calcule o Cenário Pessimista (Piso = Previsão - RMSE).
---

# 4. Imprima os 3 resultados formatados em Reais (R$).

---

# SEU CÓDIGO AQUI:

previsao_45_min = modelo.predict([[45]])[0]

teto = previsao_45_min + rmse

piso = previsao_45_min - rmse

print("=== RELATÓRIO DE PREVISÃO DA TECHMARKET ===")
print(f"Tempo de tela analisado: 45 minutos\n")
print(f"Previsão Exata Central: R$ {previsao_45_min:.2f}")
print(f"Cenário Otimista (Teto): R$ {teto:.2f}")
print(f"Cenário Pessimista (Piso): R$ {piso:.2f}")
```
