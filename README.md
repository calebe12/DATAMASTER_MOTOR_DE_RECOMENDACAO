# 🏦 Motor de Recomendação de Produtos Bancários

> Projeto desenvolvido para o programa **Datamaster** — Sistema completo de recomendação de produtos bancários baseado em Machine Learning, com geração de dados sintéticos, clusterização K-Means, RFM Score, Score de Propensão e entrega executiva via relatório HTML interativo + Excel.

---

## 📋 Sobre o Projeto

Sistema que segmenta **300.000 clientes sintéticos** em 5 perfis distintos utilizando K-Means, calcula scores RFM e de propensão por produto, e gera recomendações personalizadas com explicabilidade. Todo o pipeline é executado automaticamente a partir de um único comando.

### 👤 Perfis de Clientes

| Perfil | % Base | Renda Mensal | Foco Comercial |
|---|---|---|---|
| 🔵 Primeiros Passos | 5% | R$ 800 – R$ 2.500 | Inclusão financeira |
| 🟢 Trajetória Crescente | 35% | R$ 2.500 – R$ 8.000 | Cross-sell clássico |
| 🟡 Potencial Oculto | 30% | R$ 6.000 – R$ 20.000 | Ativação de investimentos |
| 🟣 Self Made | 25% | R$ 15.000 – R$ 60.000 | Produtos premium |
| 🔴 Old Money | 5% | R$ 50.000 – R$ 120.000 | Upgrades exclusivos |

---

## 🏗️ Arquitetura do Projeto
DATAMASTER_MOTOR_DE_RECOMENDACAO/
├── notebooks/
│   ├── data/raw/                     # Excel IBGE + GeoJSON Brasil (gerados automaticamente)
│   ├── 01_geracao_dados.ipynb        # Geração de 300k clientes sintéticos + SQLite
│   ├── 02_clusterizacao.ipynb        # K-Means 5 clusters + PCA
│   ├── 03_rfm_score.ipynb            # RFM Score + segmentos + prioridade de ataque
│   ├── 04_score_propensao.ipynb      # Regressão Logística por produto (AUC-ROC > 0.75)
│   ├── 05_depara_segmentacao.ipynb   # De-Para regras de negócio vs K-Means
│   ├── 06_pipeline_incremental.ipynb # Classifica novos clientes sem retreinar
│   ├── 07_recomendacao.ipynb         # Motor de recomendação com explicabilidade
│   └── 08_produto_final.ipynb        # HTML interativo + Excel + Email
├── .env                              # Credenciais — preencha antes de rodar
├── .gitignore
├── requirements.txt
├── X_Projeto_Motor_Produtos.ipynb    # Executa tudo automaticamente
└── README.md

---

## 🗄️ Banco de Dados (SQLite — gerado automaticamente)

| Tabela | Registros | Descrição |
|---|---|---|
| tb_clientes | 300.000 | Base principal de clientes sintéticos |
| tb_ibge_salarios | 69 | Profissões e salários PNAD Contínua 2024 |
| tb_estados_cidades | 64 | Localidades com peso geográfico |
| tb_clusters | 300.000 | Resultado da clusterização K-Means |
| tb_perfil_clusters | 5 | Resumo estatístico dos perfis |
| tb_rfm | 300.000 | Scores RFM e segmentos |
| tb_propensao | 300.000 | Propensão por produto (%) |
| tb_depara | 5 | Regras de negócio por segmento |
| tb_depara_clientes | 300.000 | Classificação De-Para por cliente |
| tb_pipeline_incremental | ~2.785 | Novos clientes mensais classificados |
| tb_recomendacoes | 300.000 | Recomendações finais por cliente |

---

## 🤖 Performance dos Modelos

| Modelo | Métrica | Resultado |
|---|---|---|
| K-Means (k=5) | Silhouette Score | 0.1494 |
| K-Means (k=5) | Davies-Bouldin | 1.6087 |
| Propensão Câmbio | AUC-ROC | 0.84 |
| Propensão Imobiliário | AUC-ROC | 0.81 |
| Propensão Investimento | AUC-ROC | 0.81 |
| Propensão Previdência | AUC-ROC | 0.80 |
| Propensão Cartão | AUC-ROC | 0.79 |
| Propensão Seguro | AUC-ROC | 0.76 |
| Concordância De-Para vs K-Means | % | 40.7% |

---

## 🚀 Como Executar

### Pré-requisitos
- Python 3.10 ou superior
- Git instalado
- Conta no [Mailtrap](https://mailtrap.io) (opcional — apenas para envio de email)

### Passo 1 — Clone o repositório

```bash
git clone https://github.com/calebe12/DATAMASTER_MOTOR_DE_RECOMENDACAO.git
cd DATAMASTER_MOTOR_DE_RECOMENDACAO
```

### Passo 2 — Crie o ambiente virtual

```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# Linux / Mac
python -m venv .venv
source .venv/bin/activate
```

### Passo 3 — Instale as dependências

```bash
pip install -r requirements.txt
```

### Passo 4 — Configure o arquivo .env

Crie um arquivo `.env` na raiz do projeto com o seguinte conteúdo:
Banco de dados
DATABASE_URL=sqlite:///data/banco.db
Email (Mailtrap) — opcional
MAILTRAP_HOST=sandbox.smtp.mailtrap.io
MAILTRAP_PORT=587
MAILTRAP_USER=seu_usuario_mailtrap
MAILTRAP_PASS=sua_senha_mailtrap
Destinatário do relatório
EMAIL_REMETENTE=seu@email.com
EMAIL_DESTINATARIO=destinatario@email.com

### Passo 5 — Execute tudo automaticamente

Abra o arquivo `X_Projeto_Motor_Produtos.ipynb` no Jupyter e execute todas as células. Ele irá:

1. ✅ Instalar todas as dependências do `requirements.txt`
2. ✅ Executar os 8 notebooks em sequência
3. ✅ Abrir o README no navegador
4. ✅ Abrir o relatório HTML interativo
5. ✅ Abrir o repositório no GitHub

---

## 📊 Entregas do Projeto

### Relatório HTML Interativo
- Mapa de calor do Brasil com distribuição de clientes por estado
- Distribuição dos 5 perfis por estado ao passar o mouse
- Cards com métricas por cluster
- Score de propensão por produto e cluster
- Ranking de recomendações
- Top clientes para ataque comercial por perfil
- Lista de clientes elegíveis para upgrade

### Excel com 10 Abas
- Visão Geral Executiva com KPIs
- 5 abas com clientes por cluster
- Top 100 clientes para ataque comercial
- Top 100 clientes com upgrades disponíveis
- Score de propensão por cluster
- Distribuição geográfica por estado

### Email Automático
- Relatório HTML como corpo do email
- Excel resumido top 20 por cluster como anexo
- Enviado via Mailtrap SMTP

---

## 🔄 Pipeline de Dados
Faker pt_BR                    IBGE (PNAD 2024)
↓                               ↓
Gera clientes sintéticos      Excel de profissões
↓                               ↓
SQLite (banco.db)
↓
K-Means (5 clusters)
↓
RFM Score
↓
Score de Propensão
↓
De-Para (regras de negócio)
↓
Pipeline Incremental (novos clientes)
↓
Motor de Recomendação
↓
HTML + Excel + Email

---

## 🛠️ Tecnologias Utilizadas

| Categoria | Tecnologias |
|---|---|
| Linguagem | Python 3.10+ |
| Machine Learning | Scikit-learn — K-Means, Regressão Logística, PCA |
| Dados | Pandas, NumPy, Faker pt_BR |
| Banco de Dados | SQLite + SQLAlchemy |
| Visualização | Matplotlib, Seaborn |
| Relatório | HTML, CSS, JavaScript, SVG |
| Excel | OpenPyXL |
| Email | Mailtrap SMTP |
| Ambiente | Jupyter Notebook, VS Code |

---

## 📁 Arquivos Gerados Automaticamente

Os seguintes arquivos são gerados ao executar o projeto e não estão no repositório:

- `data/banco.db` — Banco SQLite com 11 tabelas e 300k registros
- `data/raw/ibge_salarios_profissoes.xlsx` — Excel de profissões IBGE
- `data/raw/brasil_estados.geojson` — GeoJSON do Brasil
- `models/` — Modelos K-Means e Scaler em pickle
- `docs/` — Relatório HTML, Excel e gráficos

---

## 👤 Autor

**Calebe Bizarro**
Programa Datamaster — 2025
[GitHub](https://github.com/calebe12)
