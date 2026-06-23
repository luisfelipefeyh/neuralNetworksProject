# Trabalho Prático 2 — Redes Neurais

Trabalho da disciplina de **Sistemas Inteligentes** (UFSC, Curso de Sistemas de Informação), cujo objetivo é a **classificação de imagens do conjunto de dados CIFAR-10** (32×32, RGB, 10 classes) usando redes neurais artificiais.

O desenvolvimento é feito em um **notebook Jupyter** (`.ipynb`), com **TensorFlow + Keras**, executado no **Google Colab**.

## Etapas

1. **MLP simples** — rede totalmente conectada (sem camadas convolucionais); análise de loss e acurácia (treino × validação) por época.
2. **CNN + data augmentation** — rede convolucional com aumento de dados; mesma análise de métricas e generalização.
3. **Busca de hiperparâmetros** — teste de combinações (camadas, neurônios, learning rate etc.) com tabela comparativa dos resultados.
4. **Avaliação final** — melhor modelo avaliado no conjunto de teste, com discussão da capacidade de generalização.

## Entregáveis

- Notebook `.ipynb` com as saídas salvas (gráficos, métricas e tabelas).
- Vídeo explicativo de até 10 minutos.

## Como executar (Google Colab)

O notebook é o arquivo [`TrabalhoPratico2_RedesNeurais_CIFAR10.ipynb`](TrabalhoPratico2_RedesNeurais_CIFAR10.ipynb).

1. Abra o notebook no [Google Colab](https://colab.research.google.com/) (*Arquivo → Fazer upload de notebook*).
2. Ative a GPU em *Ambiente de execução → Alterar o tipo de ambiente de execução → GPU*.
3. Rode tudo com *Ambiente de execução → Executar tudo* (~20–35 min).
4. Preencha o nome(s)/matrícula(s) da equipe na primeira célula.
5. Salve com *Arquivo → Fazer download → Fazer o download do .ipynb* para entregar **com as saídas**.

> As bibliotecas (TensorFlow/Keras, scikit-learn, etc.) já vêm instaladas no Colab — não é preciso
> instalar nada. Localmente este repositório **não** roda (sem TensorFlow e com Python mais novo que
> o suportado pela biblioteca).
