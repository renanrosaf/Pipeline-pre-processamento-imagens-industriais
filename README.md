# 🏭 Pipeline de Pré-processamento de Imagens Industriais (Visão Computacional)

## 📖 Sobre o Projeto
Este projeto foi desenvolvido como parte do Mini-Projeto Avaliativo do Módulo 2 (Machine Learning e Visão Computacional). 

O objetivo principal não é classificar se uma peça possui defeito, mas sim atuar como um Analista de Visão Computacional Júnior, desenvolvendo um *pipeline* em Python utilizando a biblioteca OpenCV. O script realiza o pré-processamento em lote de imagens reais de peças de fundição (metálicas), destacando ranhuras e defeitos estruturais. Isso padroniza e "limpa" os dados para que, futuramente, a equipe de Machine Learning possa treinar um modelo preditivo eficiente.

## 📊 Dataset Utilizado
Utilizou-se o dataset público **Casting Product Image Data for Quality Inspection**.
- **Fonte:** [Kaggle](https://www.kaggle.com/datasets/ravirajsinh45/real-life-industrial-dataset-of-casting-product)
- O dataset contém imagens reais de peças de fundição com e sem defeitos.

## 📝 Divisão das Tarefas
- **Sprint 1 - Configuração e Versionamento:** Inicializa um repositório Git, cria uma branch development, configura o ambiente virtual Python e faz o download do dataset sugerido.
- **Sprint 2 - Estruturação de Dados e Leitura:** Cria uma estrutura de pastas (`/raw_images` e `/processed_images`). Desenvolve a lógica para ler múltiplas imagens do diretório de entrada em lote (batch).
- **Sprint 3 - Pipeline de Pré-processamento Base:** Implementa conversão para escala de cinza (Grayscale) e aplicação de filtro para redução de ruído (ex: Gaussian Blur ou Median Blur).
- **Sprint 4 - Segmentação e Destaque de Características:** Aplica técnicas de limiarização (Thresholding, como o método de Otsu) e detecção de bordas (ex: Canny ou Sobel) para destacar os contornos da peça e possíveis falhas.
- **Sprint 5 - Refinamento Morfológico e Padronização:** Utiliza operações morfológicas (Erosão/Dilatação) para remover ruídos da segmentação e redimensionar (Resize) todas as imagens para um tamanho padrão (ex: 256x256 pixels).
- **Sprint 6 - Gravação e Documentação:** Salva as imagens processadas no diretório de saída, consolida a branch no GitHub, escreve o readme.md e grava o vídeo de apresentação.

## ⚙️ Funcionalidades e Pipeline (Resumo)
O desenvolvimento seguiu uma ordem lógica de Sprints, aplicando as seguintes técnicas em cada imagem do lote:

1. **Leitura em Lote (Batch):** Carregamento autônomo das imagens da pasta de entrada.
2. **Grayscale:** Conversão do espaço de cor (RGB para escala de cinza).
3. **Suavização (Blur):** Aplicação de filtros para atenuação e redução de ruídos (ex: Gaussian Blur ou Median Blur).
4. **Limiarização (Thresholding):** Binarização para isolar o objeto do fundo (ex: Método de Otsu).
5. **Detecção de Bordas:** Algoritmos (Canny/Sobel) para destacar contornos e falhas.
6. **Refinamento Morfológico:** Operações de Erosão e Dilatação para eliminar ruídos residuais.
7. **Padronização:** Redimensionamento (Resize) de todas as imagens para o tamanho de 256x256 pixels.
8. **Exportação:** Salvamento dos resultados na pasta de saída.

## 📁 Estrutura do Diretório
O projeto está organizado da seguinte forma:
```text
├── /raw_images           # Pasta contendo as imagens originais do dataset
├── /processed_images     # Pasta onde as imagens tratadas e padronizadas são salvas
├── main.py               # Script principal do pipeline de visão computacional
├── README.md             # Documentação do projeto
└── requirements.txt      # Dependências do projeto (OpenCV, etc.)
```

## 🚀 Como Executar
1. Clone o repositório:

Bash
git clone [https://github.com/seu-usuario/nome-do-repositorio.git](https://github.com/seu-usuario/nome-do-repositorio.git)
cd nome-do-repositorio

2. Prepare os dados:
Baixe o dataset do Kaggle (Casting Product Image Data) e coloque as imagens na pasta raw_images/.

3. Execute o script:
```python
Bash
python main.py
```

## 🔧 Funcionamento do Script:

1. Ler todas as imagens (.jpg, .png, etc.) de raw_images/;

2.  Processar cada uma conforme o pipeline;

3. Salvar os resultados em processed_images/ com o prefixo processed_.

4. Verifique a saída na pasta processed_images/.

## 📹 Vídeo de Apresentação
🔗 [link do vídeo aqui]

O vídeo aborda:

Objetivo do sistema e demonstração de funcionamento;

Passos para execução;

Organização das tarefas antes do desenvolvimento;

Estrutura de branches criadas;

Pontos de melhoria identificados.


## 🏷️ Autor: 
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/renanrosaferreira/)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/renanrosaf)