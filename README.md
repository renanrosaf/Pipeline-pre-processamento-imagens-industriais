markdown
# 🏭 Pipeline de Pré-processamento de Imagens Industriais

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=flat-square&logo=python)
![OpenCV](https://img.shields.io/badge/OpenCV-4.x-green?style=flat-square&logo=opencv)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?style=flat-square&logo=jupyter)
![License](https://img.shields.io/badge/License-Educacional-lightgrey?style=flat-square)

## 📖 Sobre o Projeto

Este projeto foi desenvolvido como parte do **Mini-Projeto Avaliativo — Módulo 2 (Semana 07)** da disciplina de **Machine Learning e Visão Computacional**.

O objetivo **não é classificar** se uma peça possui defeito, mas sim atuar como **Analista de Visão Computacional Júnior**, construindo um *pipeline* em Python com a biblioteca **OpenCV** para realizar o pré-processamento em lote de imagens reais de peças de fundição metálicas.

O pipeline destaca **ranhuras estruturais e possíveis falhas** nas peças, padronizando e "limpando" os dados para que, futuramente, a equipe de Machine Learning possa treinar um modelo preditivo eficiente.

---

## 📊 Dataset Utilizado

- **Nome:** Casting Product Image Data for Quality Inspection
- **Fonte:** [Kaggle](https://www.kaggle.com/datasets/ravirajsinh45/real-life-industrial-dataset-of-casting-product)
- **Descrição:** Imagens reais de peças de fundição classificadas em duas categorias:
  - `ok_front` → peças sem defeito
  - `def_front` → peças com defeito

---

## 🗂️ Estrutura do Repositório

```text
Pipeline-pre-processamento/
│
├── .venv/                          # Ambiente virtual Python
├── data/
│   └── raw_images/                 # Imagens originais do dataset
│       ├── def_front/              # Peças com defeito
│       └── ok_front/               # Peças sem defeito
│
├── output/
│   └── processed_images/           # Imagens após o pipeline
│       ├── def_front/              # Defeituosas processadas
│       ├── ok_front/               # Sem defeito processadas
│       └── test/                   # Testes com parâmetros individuais
│
├── pipeline_processamento.ipynb    # Notebook com o pipeline completo
├── README.md                       # Este arquivo
├── requirements.txt                # Dependências do projeto
└── .gitignore                      # Arquivos ignorados pelo Git
```

---

## ⚙️ Pipeline de Pré-processamento

Cada imagem passa por **8 etapas sequenciais**, implementadas como métodos privados da classe `pipeline_Processor`:

| # | Etapa | Função OpenCV | Parâmetros | Sprint |
|---|-------|---------------|------------|--------|
| 1 | Leitura em lote | `cv2.imread` + `os.listdir` | — | Sprint 2 |
| 2 | Grayscale | `cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)` | — | Sprint 3 |
| 3 | Suavização (Blur) | `cv2.GaussianBlur` | kernel `(5, 5)`, σ = 0 | Sprint 3 |
| 4 | Limiarização (Otsu) | `cv2.threshold(..., THRESH_OTSU)` | automático | Sprint 4 |
| 5 | Morfologia | `cv2.morphologyEx` (OPEN + CLOSE) | kernel elíptico `(3, 3)` | Sprint 5 |
| 6 | Máscara + Bitwise | `cv2.bitwise_and(blurred, mask)` | — | Sprint 5 |
| 7 | Detecção de Bordas | `cv2.Canny` | 50 / 150 | Sprint 4 |
| 8 | Resize + Salvamento | `cv2.resize` + `cv2.imwrite` | 256 × 256 px | Sprint 5/6 |

### 🧠 Por que essa ordem?

A ordem foi ajustada após testes empíricos para **evitar a destruição das bordas finas** geradas pelo Canny:

1. A **morfologia** é aplicada sobre a **máscara binária** (resultado do Otsu), onde faz sentido remover ruídos e fechar buracos.
2. A máscara refinada é usada em `cv2.bitwise_and` para **isolar a peça** do fundo.
3. O **Canny** é aplicado sobre a **imagem mascarada**, preservando as **ranhuras internas** (anéis concêntricos) sem capturar ruído do fundo.
4. Por fim, a imagem é **redimensionada** para 256 × 256, padronizando a entrada.

> ⚠️ **Atenção:** aplicar morfologia *depois* do Canny destrói as bordas (linhas de 1 pixel são apagadas pela erosão). Por isso a ordem é crítica.

---

## 🚀 Como Executar

### 1️⃣ Clonar o repositório

```bash
git clone https://github.com/renanrosaf/Pipeline-pre-processamento.git
cd Pipeline-pre-processamento
```

### 2️⃣ Criar e ativar o ambiente virtual

**Windows:**
```bash
python -m venv .venv
.venv\Scripts\activate
```

**Linux / macOS:**
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3️⃣ Instalar as dependências

```bash
pip install -r requirements.txt
```

Ou manualmente:
```bash
pip install opencv-python numpy
```

### 4️⃣ Baixar o dataset

Baixe o dataset do [Kaggle](https://www.kaggle.com/datasets/ravirajsinh45/real-life-industrial-dataset-of-casting-product) e organize as imagens em:

```text
data/raw_images/def_front/   # imagens com defeito
data/raw_images/ok_front/    # imagens sem defeito
```

### 5️⃣ Executar o pipeline

Abra o notebook `pipeline_processamento.ipynb` no Jupyter ou VSCode e execute todas as células:

```bash
jupyter notebook pipeline_processamento.ipynb
```

As imagens processadas serão salvas automaticamente em:

```text
output/processed_images/def_front/
output/processed_images/ok_front/
```

---

## 🧪 Testes com Imagem Única

Antes de rodar o pipeline no lote inteiro, foram realizados testes individuais para **calibração dos parâmetros** (kernel do blur, limiares do Canny, tamanho do kernel morfológico). Esses testes estão salvos em:

```text
output/processed_images/test/
```

O método `process_single_image()` permite visualizar **todas as etapas** com `mostrar=True`:

```python
pipeline = pipeline_Processor()
pipeline.process_single_image(
    "data/raw_images/def_front/cast_def_0_9477.jpeg",
    "output/processed_images/test/resultado.jpeg",
    mostrar=True
)
```

---

## ⚡ Processamento Concorrente

O processamento em lote utiliza `ThreadPoolExecutor` para paralelizar a leitura/escrita das imagens:

```python
pipeline.process_batch_concurrent(
    "data/raw_images/def_front",
    "output/processed_images/def_front"
)
```

**Por que ThreadPool e não ProcessPool?**

- ThreadPool **não exige pickling** de objetos, evitando erros de `ProcessPoolExecutor` no Windows (`"terminated abruptly"`).
- O OpenCV **libera o GIL** em operações como `imread`, `cvtColor`, `GaussianBlur`, `Canny` e `resize`, permitindo paralelismo real.
- Compatibilidade total com **Windows + VSCode + Jupyter**.

---

## 📝 Divisão das Tarefas (Sprints)

| Sprint | Descrição | Status |
|--------|-----------|--------|
| **1** | Configuração do Git, branch `development`, ambiente virtual e download do dataset | ✅ |
| **2** | Estruturação de pastas (`raw_images` / `processed_images`) e leitura em lote | ✅ |
| **3** | Grayscale + Gaussian Blur | ✅ |
| **4** | Threshold Otsu + Detecção de Bordas (Canny) | ✅ |
| **5** | Morfologia + Resize (256×256) | ✅ |
| **6** | Salvamento em lote, documentação (README) e vídeo de apresentação | ✅ |

---

## 🌿 Versionamento e Branches

O desenvolvimento seguiu o modelo **Git Flow** simplificado:

| Branch | Descrição |
|--------|-----------|
| `main` | Versão final estável entregue |
| `development` | Branch de integração das funcionalidades |
| `feature/sprint-N-*` | Branches específicas por sprint (ex: `feature/sprint3-preprocessing`) |

Commits seguem o padrão **Conventional Commits** (`feat:`, `fix:`, `docs:`, `refactor:`).

---

## 🔮 Melhorias Futuras

- [ ] Ajuste **dinâmico** dos limiares do Canny com base no histograma da imagem.
- [ ] Suporte a **data augmentation** para treinamento posterior.
- [ ] Substituição do `ThreadPoolExecutor` por `ProcessPoolExecutor` após diagnóstico do ambiente.
- [ ] Implementação de **interface CLI** com `argparse` para escolher pastas de entrada/saída.
- [ ] Exportação de relatório **CSV** com métricas de processamento (tempo, taxa de sucesso).

---

## 📹 Vídeo de Apresentação

🔗 **[Inserir link do vídeo no Google Drive — modo leitor para qualquer pessoa com o link]**

O vídeo aborda:

1. Objetivo do sistema e demonstração de funcionamento;
2. Passos para execução;
3. Organização das tarefas antes do desenvolvimento;
4. Estrutura de branches criadas;
5. Pontos de melhoria identificados no código.

---

## 🏷️ Autor

**Renan Rosa Ferreira**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/renanrosaferreira/)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/renanrosaf)

## 📄 Licença

Projeto desenvolvido para fins **educacionais**, sem fins comerciais.

Dataset de terceiros: [Kaggle — Casting Product Image Data](https://www.kaggle.com/datasets/ravirajsinh45/real-life-industrial-dataset-of-casting-product).

---

*Última atualização: setembro de 2026*