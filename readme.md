
# PROJETO APLICADO 3

#### UNIVERSIDADE PRESBITERIANA MACKENZIE - FACULDADE DE COMPUTAÇÃO E INFORMÁTICA

## SISTEMA DE RECOMENDAÇÃO DE CURSOS ONLINE
### Uma abordagem por KNN baseada em conteúdo usando Python

Orientadora: Prof.ª Dr.ª Carolina Toledo Ferraz — 2026

---

### GRUPO 2

### Eduardo Pinheiro Canas - RA 10184419
### Hugo de Moraes Holzer - RA 10142961
### Luiz Rodrigo Alves Vergino - RA 10176038

---
## Objetivo

Desenvolver, implementar e avaliar um **sistema de recomendação de cursos online** baseado em filtragem por conteúdo (*content-based filtering*), utilizando o algoritmo **K-Nearest Neighbors (KNN)** com vetorização **TF-IDF** aplicado ao [Coursera Courses Dataset 2021](https://www.kaggle.com/datasets/khusheekapoor/coursera-courses-dataset-2021).

O sistema é capaz de sugerir cursos relevantes a partir de **dois modos de consulta**:
- **Por nome de curso** — encontra cursos similares ao informado
- **Por skill/interesse livre** — vetoriza o texto da consulta em tempo real e busca os cursos mais próximos

O projeto está alinhado aos **Objetivos de Desenvolvimento Sustentável da ONU**:
- **ODS 4 — Educação de Qualidade**: democratiza o acesso a trilhas de aprendizado personalizadas, independentemente de renda ou orientação educacional especializada
- **ODS 8 — Trabalho Decente e Crescimento Econômico**: auxilia na identificação de caminhos de qualificação profissional em um cenário de transformação digital acelerada

---

## Dataset

| Atributo | Descrição |
|---|---|
| **Fonte** | [Kaggle — Coursera Courses Dataset 2021](https://www.kaggle.com/datasets/khusheekapoor/coursera-courses-dataset-2021) |
| **Registros** | 3.522 cursos |
| **Colunas** | `name`, `university`, `difficulty`, `rating`, `url`, `description`, `skills` |
| **Acesso** | API Kaggle (`kaggle datasets download`) |

Não há avaliações individuais de usuários na base — característica que motivou a escolha da filtragem por conteúdo em detrimento da filtragem colaborativa.

---

## Metodologia

```
Definição do Problema
        ↓
Coleta de Dados (API Kaggle)
        ↓
Pré-processamento e Limpeza
        ↓
Divisão dos Dados (sem split supervisionado)
        ↓
Seleção do Algoritmo (TF-IDF + KNN cosine)
        ↓
Treinamento do Modelo
        ↓
Função de Recomendação (recomendar_cursos)
        ↓
Avaliação do Desempenho (4 métricas offline)
        ↓
Otimização e Ajustes (hiperparâmetros analíticos)
        ↓
Implantação e Monitoramento
```

### Parâmetros principais

| Componente | Configuração |
|---|---|
| `TfidfVectorizer` | `max_features=5000`, `ngram_range=(1,2)`, `min_df=2`, `stop_words='english'` |
| `NearestNeighbors` | `metric='cosine'`, `algorithm='brute'`, `n_neighbors=11` |
| Matriz resultante | 3.522 × 5.000 termos |

---

## Resultados

| Métrica | Valor | Interpretação |
|---|---|---|
| Cobertura do catálogo | **39,0%** | 1.374 cursos distintos sugeridos em 200 consultas |
| Similaridade média (cosseno) | **43,9%** | Coerência temática moderada — vocabulário técnico diverso |
| Diversidade intra-lista | **66,0%** | Listas variadas, sem concentração em subtema único |
| Nota média recomendada | **4,57** vs. 4,56 global | Leve viés positivo de qualidade |

### Comparação com baselines

| Abordagem | Cobertura | Similaridade | Diversidade |
|---|---|---|---|
| Aleatória | ~100% | ~5–10% | ~90%+ |
| Popularidade | ~1–3% | variável | muito baixa |
| **KNN TF-IDF (este projeto)** | **39%** | **43,9%** | **66%** |

---

## Como executar

### Pré-requisitos

```bash
pip install pandas numpy scikit-learn matplotlib seaborn kaggle
```

### Configurar API do Kaggle

```bash
# Coloque kaggle.json em ~/.kaggle/
kaggle datasets download -d khusheekapoor/coursera-courses-dataset-2021 --unzip
```

### Executar no Google Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/canasep/mackprojeto3/blob/main/PROJETO_APLICADO_3.ipynb)

### Usar a função de recomendação

```python
# Por nome de curso
recomendar_cursos('Machine Learning', n=5)

# Por skill/interesse livre
recomendar_cursos('python data analysis', n=5, por_skill=True)
```

---

## Estrutura do repositório

```
mackprojeto3/
│
├── PROJETO_APLICADO_3.ipynb     # Notebook principal (pipeline completo)
├── readme.md                    # Este arquivo
│
└── codes/
│   └──readme_codes.md
└── dataset/
│   ├── arquivos/
│   │   ├── Coursera.csv
│   │   └── arquivos.md
│   └── readme_dataset.md
└── entregas/
│   └──PROJETOAPLICADO_MODULO1.pdf
│   └──PROJETOAPLICADO_MODULO2.pdf
│   └──PROJETOAPLICADO_MODULO3.pdf
│   └──PROJETOAPLICADO_MODULO4.pdf
│   └──readme_entregas.md
└── graficos/
│   └──readme_graficos.md
└── imagens/
│   └──readme_imagens.md
└── pdfs/
│   └──PROJETOAPLICADO_MODULO1.pdf
│   └──PROJETOAPLICADO_MODULO2.pdf
│   └──PROJETOAPLICADO_MODULO3.pdf
│   └──PROJETOAPLICADO_MODULO4.pdf
│   └──readme_pdfs.md
└── tables/
│   └──readme_tables.md
└── words/
    └── PROJETOAPLICADO_FINAL.docx
```

---

## Referências principais

- HERLOCKER, J. L. et al. Evaluating collaborative filtering recommender systems. *ACM TOIS*, 2004.
- LOPS, P.; DE GEMMIS, M.; SEMERARO, G. Content-based Recommender Systems. In: *Recommender Systems Handbook*. Springer, 2011.
- MANNING, C. D.; RAGHAVAN, P.; SCHÜTZE, H. *Introduction to Information Retrieval*. Cambridge, 2008.
- RICCI, F. et al. *Recommender Systems Handbook*. Springer, 2011.
- SHANI, G.; GUNAWARDANA, A. Evaluating Recommendation Systems. In: *Recommender Systems Handbook*. Springer, 2011.
- SALTON, G.; BUCKLEY, C. Term-weighting approaches in automatic text retrieval. *IPM*, 1988.
