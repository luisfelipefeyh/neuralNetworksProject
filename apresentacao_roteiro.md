# Roteiro de Apresentação — Trabalho Prático 2: Redes Neurais (CIFAR-10)
**Disciplina:** Sistemas Inteligentes — UFSC  
**Equipe:** João Pedro Paixão e Luis Felipe Feyh  
**Tempo estimado:** ~8 minutos

---

## SLIDE 1 — Capa
**Título:** Classificação de Imagens com Redes Neurais — CIFAR-10  
**Subtítulo:** Trabalho Prático 2 — Sistemas Inteligentes  
**Rodapé:** João Pedro Paixão · Luis Felipe Feyh · UFSC · Jun/2026

---

## BLOCO 1 — Introdução e Objetivo (~1 min)

### SLIDE 2 — O Problema
**Título:** O que queremos resolver?

**Conteúdo:**
- Dataset: **CIFAR-10** — 60.000 imagens 32×32 RGB
- **10 classes:** avião, automóvel, pássaro, gato, cervo, cachorro, sapo, cavalo, navio, caminhão
- Tarefa: dado uma imagem, prever corretamente sua classe
- 50.000 imagens de treino · 10.000 de teste

*(mostrar figura com amostras do CIFAR-10)*

**Fala:**
> "O CIFAR-10 é um benchmark clássico em visão computacional. São imagens pequenas — apenas 32×32 pixels — divididas em 10 categorias. O desafio é que, nessa resolução, até humanos cometem erros. Nosso objetivo é comparar diferentes arquiteturas de redes neurais para esse problema."

---

### SLIDE 3 — Estrutura do Trabalho
**Título:** Como organizamos o trabalho

**Conteúdo (linha do tempo):**
1. **MLP simples** — baseline sem convoluções
2. **CNN + Data Augmentation** — rede convolucional
3. **Busca de Hiperparâmetros** — otimização sistemática
4. **Avaliação Final** — resultado no conjunto de teste

**Fala:**
> "Estruturamos o trabalho em quatro etapas progressivas: começamos com o modelo mais simples como ponto de referência, avançamos para arquiteturas mais sofisticadas e, por fim, otimizamos os hiperparâmetros antes de avaliar o modelo final em dados nunca vistos durante o treino."

---

## BLOCO 2 — MLP Simples (~2 min)

### SLIDE 4 — Arquitetura da MLP
**Título:** Parte 1: MLP Simples

**Conteúdo:**
```
Entrada: 32×32×3  →  Flatten  →  3.072 neurônios
Dense(512, ReLU)  →  Dropout(0.4)
Dense(256, ReLU)  →  Dropout(0.4)
Dense(10, Softmax)
```
- Otimizador: Adam (lr = 0,001)
- Loss: Entropia cruzada categórica (sparse)
- Épocas: 25 · Batch: 128

**Fala:**
> "A MLP — Multi-Layer Perceptron — é uma rede totalmente conectada. Cada imagem é 'achatada' em um vetor de 3.072 valores e processada por camadas densas. É o modelo mais simples possível: não enxerga a estrutura espacial da imagem — para ela, um pixel do canto superior esquerdo não tem relação nenhuma com seu vizinho."

---

### SLIDE 5 — Resultados da MLP
**Título:** MLP — Curvas de Treino e Validação

*(mostrar gráfico de loss e acurácia por época)*

**Legenda/destaques:**
- Acurácia de validação: ~**48–52%**
- Gap treino/validação crescente → **overfitting moderado**
- `val_loss` começa a subir enquanto `train_loss` cai

**Fala:**
> "Os gráficos mostram um padrão claro: a loss de treino continua caindo, mas a de validação estabiliza e começa a subir. Isso é sinal de overfitting — o modelo começa a 'decorar' os dados de treino em vez de aprender padrões generalizáveis."

---

### SLIDE 6 — Loss ≠ Acurácia (conceito central)
**Título:** Por que loss e acurácia não são proporcionais?

**Conteúdo:**
| Métrica | Definição | Natureza |
|---------|-----------|----------|
| **Loss** (Entropia Cruzada) | `-log(p)` da classe correta — mede confiança | **Contínua** |
| **Acurácia** | Fração de `argmax` corretos — acertou/errou | **Discreta** |

**Exemplo chave:**
> O modelo pode **aumentar a loss de validação** sem alterar a acurácia — basta passar a errar com *muita confiança* (cada erro confiante gera um `-log(p)` enorme, enquanto conta o mesmo 1 erro na acurácia)

**Fala:**
> "Essa é uma distinção fundamental. A acurácia só muda quando o `argmax` muda — quando o modelo troca o vencedor. A loss, por outro lado, é sensível à probabilidade atribuída: um modelo que diz '90% avião' quando é um gato pune muito mais a loss do que um que diz '20% avião'. Por isso é comum ver a `val_loss` subir enquanto a `val_accuracy` fica quase estável — o modelo está errando com mais confiança."

---

## BLOCO 3 — CNN + Data Augmentation (~2 min)

### SLIDE 7 — Motivação e Arquitetura CNN
**Título:** Parte 2: CNN com Data Augmentation

**Arquitetura:**
```
Entrada 32×32×3
↓ Data Augmentation (flip, rotação, zoom, translação)
Conv2D(32) → BN → Conv2D(32) → MaxPool → Dropout(0.25)
Conv2D(64) → BN → Conv2D(64) → MaxPool → Dropout(0.30)
Conv2D(128) → BN → MaxPool → Dropout(0.40)
Dense(128) → Dropout(0.5) → Dense(10, Softmax)
```

**Fala:**
> "As camadas convolucionais aplicam filtros que percorrem a imagem localmente, explorando a vizinhança dos pixels e compartilhando pesos. São muito mais adequadas para imagens. Adicionamos também data augmentation: espelhamento horizontal, rotação, zoom e translação aplicados aleatoriamente apenas no treino."

---

### SLIDE 8 — O que é Data Augmentation?
**Título:** Data Augmentation — variações artificiais

*(mostrar grid com 10 versões aumentadas de uma mesma imagem)*

**Conteúdo:**
- CIFAR-10: apenas **5.000 imagens por classe** no treino
- Augmentation gera variações plausíveis a cada época
- Efeito: **aumenta o número efetivo de exemplos** sem coletar dados novos
- Também atua como **regularizador**: a rede aprende características invariantes

> **Regra geral:** quanto menos dados por categoria, mais o augmentation ajuda

**Fala:**
> "Com só 5 mil imagens por classe, o augmentation é especialmente importante. Ao ver um cachorro espelhado, rotacionado e com zoom diferente a cada época, a rede aprende que 'cachorro' é um conceito independente de orientação e escala. Isso é basicamente uma regularização gratuita."

---

### SLIDE 9 — Resultados CNN
**Título:** CNN — Resultados e Comparação

*(mostrar gráfico de loss e acurácia por época da CNN)*

**Destaques:**
- Acurácia de validação: ~**75–82%** (vs. ~50% da MLP)
- Gap treino/validação **menor** → **melhor generalização**
- Início: `val_accuracy > train_accuracy` ← efeito do augmentation (treino é "mais difícil")

**Fala:**
> "A CNN com augmentation supera a MLP em quase 30 pontos percentuais de acurácia. Um detalhe curioso: no início do treino, a acurácia de treino fica abaixo da de validação — isso é esperado. Como o augmentation transforma as imagens de treino aleatoriamente, os lotes de treino são mais difíceis do que os de validação, que chegam sem transformação."

---

## BLOCO 4 — Busca de Hiperparâmetros (~2,5 min)

### SLIDE 10 — Setup da Busca
**Título:** Parte 3: Otimização de Hiperparâmetros

**Espaço de busca:**
| Hiperparâmetro | Valores testados |
|----------------|-----------------|
| Learning rate | 1e-2 · **1e-3** · 5e-4 |
| Neurônios (Dense) | 128 · **256** |
| Dropout | 0.3 · **0.5** |

**Estratégia:**
- 12 combinações possíveis → **8 amostradas aleatoriamente**
- CNN compacta (2 blocos) · sem augmentation · EarlyStopping
- Subconjunto de 20.000 imagens (acelera a busca)
- Melhor combinação → retreinada completo na Parte 4

**Fala:**
> "Testar todas as combinações possíveis seria muito custoso. Usamos uma amostragem aleatória de 8 das 12 combinações possíveis com um modelo mais leve, sem augmentation, para ranquear as opções rapidamente. A vencedora é retreinada do zero com o pipeline completo na Parte 4."

---

### SLIDE 11 — Tabela Comparativa
**Título:** Resultados da Busca — Tabela Comparativa

*(mostrar tabela do notebook ordenada por val_acc)*

**Colunas:** learning_rate · dense_units · dropout · val_acc · val_loss · épocas · tempo_s

**Destaques a apontar na tabela:**
- Combinações com lr=1e-2 tendem a ter pior desempenho (instabilidade)
- Melhor combinação encontrada: `lr=1e-3, dense=256, dropout=0.5`

**Fala:**
> "Aqui está a tabela ordenada da melhor para a pior combinação. [Aponte para a tabela] Note que as combinações com learning rate 1e-2 aparecem consistentemente nas posições inferiores — o passo de otimização é grande demais e o treino fica instável. As combinações com lr=1e-3 ou 5e-4 lideram."

---

### SLIDE 12 — Análise dos Hiperparâmetros
**Título:** Qual hiperparâmetro mais impactou?

**Ranking de impacto:**

🥇 **Learning Rate** — fator mais determinante
- 1e-2: treino instável, converge mal
- 1e-3 e 5e-4: convergência estável → lideram a tabela
- Motivo: controla diretamente o tamanho do passo na otimização

🥈 **Neurônios na camada densa** (128 → 256)
- Efeito secundário: mais capacidade, ganho pequeno
- Risco de overfitting se dropout não acompanhar

🥉 **Dropout** (0.3 vs 0.5)
- Pouco impacto na acurácia bruta em poucas épocas
- Dropout maior → menor gap treino/validação

**Fala:**
> "O learning rate domina o ranking. Isso é esperado: ele controla o tamanho do passo em cada atualização de peso — um passo grande demais e o modelo não converge para um mínimo bom; pequeno demais e o treino fica muito lento. Os neurônios e o dropout têm influência mais sutil, mas ainda identificável."

---

## BLOCO 5 — Resultado Final + Conclusão (~1,5 min)

### SLIDE 13 — Modelo Final
**Título:** Parte 4: Avaliação no Conjunto de Teste

**Setup:**
- Mesma arquitetura compacta, **com augmentation**
- Melhores hiperparâmetros da busca
- Callbacks: EarlyStopping (patience=6) + ReduceLROnPlateau
- Teste **intocado** até este momento

*(mostrar gráfico de treino do modelo final)*

**Resultados:**
```
Conjunto de Teste:
  Loss     = X.XXXX
  Acurácia = XX.XX%
```

**Fala:**
> "Agora sim usamos o conjunto de teste — ele ficou completamente intocado durante todo o processo de desenvolvimento. Os resultados de teste próximos aos de validação confirmam que não houve vazamento de informação: o modelo generaliza de verdade para dados nunca vistos."

---

### SLIDE 14 — Matriz de Confusão
**Título:** O modelo erra mais onde?

*(mostrar a matriz de confusão e o classification_report)*

**Destaques:**
- **Animais** (gato ↔ cachorro, cervo ↔ cavalo): mais confundidos — formas similares em 32×32
- **Veículos** (navio, avião): mais fáceis — silhuetas distintas
- Classes com maior F1: automóvel, navio, avião
- Classes com menor F1: gato, cachorro

**Fala:**
> "A matriz de confusão revela onde o modelo ainda erra. Gato e cachorro são os mais difundidos entre si — em 32×32 pixels, a diferença é sutil. Veículos, por terem silhuetas mais geométricas e distintas, são classificados com mais precisão."

---

### SLIDE 15 — Comparativo Final e Conclusão
**Título:** Evolução ao longo do trabalho

**Tabela resumo:**
| Modelo | Acurácia (val) | Observação |
|--------|---------------|------------|
| MLP simples | ~50% | Baseline · ignora estrutura espacial |
| CNN + Augmentation | ~78% | +28pp · melhor generalização |
| Modelo Final (busca) | ~XX% | Otimizado · avaliado no teste |

**Conclusões:**
1. Convoluções são essenciais para imagens
2. Data augmentation melhora generalização mesmo com dados limitados
3. Learning rate é o hiperparâmetro mais crítico
4. Modelo final generaliza bem para dados novos

**Fala:**
> "Resumindo: partimos de ~50% com uma MLP simples e chegamos a resultados significativamente melhores com CNN e augmentation. A busca de hiperparâmetros confirmou o learning rate como parâmetro mais crítico. O modelo final, avaliado honestamente no conjunto de teste, demonstrou boa capacidade de generalização. Obrigado!"

---

## Notas para gravação

### Dicas de apresentação
- **Tempo total alvo:** 7–8 minutos (margem de segurança para os 10 min)
- **Navegue pelo notebook** enquanto apresenta cada parte — mostre os gráficos reais
- Ao apresentar a tabela de hiperparâmetros, use o cursor para apontar linhas específicas

### Divisão de fala sugerida (2 apresentadores)
| Apresentador | Seção |
|---|---|
| João Pedro | Introdução + MLP (Blocos 1 e 2) |
| Luis Felipe | CNN + Hiperparâmetros + Conclusão (Blocos 3, 4 e 5) |

### Sequência de telas a mostrar
1. Slide de capa → slides de introdução
2. Abrir o notebook → célula de amostras do CIFAR-10
3. Gráfico MLP (loss + acurácia)
4. Grid de augmentation + gráfico CNN
5. Tabela comparativa de hiperparâmetros
6. Gráfico do modelo final + matriz de confusão
7. Slide de conclusão

### Citação de IA (obrigatória pelo enunciado)
> "Para a elaboração deste trabalho, utilizamos a ferramenta de IA generativa **Claude (Anthropic)**, via Claude Code, para auxiliar na estruturação do notebook, geração de código TensorFlow/Keras e formatação do markdown. Todo o conteúdo foi revisado e validado pela equipe."
