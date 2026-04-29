# PBL Fase 6 — Visão Computacional na FarmTech Solutions

> Projeto acadêmico da FIAP — disciplina de IA / Visão Computacional.
> Cliente fictício: **FarmTech Solutions**.
> Tarefa: detectar e classificar dois objetos (`faca` e `celular`) usando três abordagens diferentes.

---

## Equipe

| Aluno | RM | Responsabilidade |
|---|---|---|
| Yasmin Nesili | _(RM)_ | Entrega 1 — YOLO Customizado |
| **Jonathan Gomes Ribeiro Franco** | **567109** | Entrega 2 — Comparativo (YOLO Padrão + CNN do Zero) |

---

## Visão geral do projeto

A FarmTech Solutions ampliou seu portfólio de IA para além do agronegócio e está prospectando um cliente que quer entender, na prática, como funciona um sistema de visão computacional. Para esse pitch, o time da FarmTech montou um dataset de **80 imagens** (40 facas + 40 celulares) e está comparando três abordagens distintas:

1. **YOLO Customizado** — YOLOv8n com fine-tuning sobre as nossas imagens (Entrega 1).
2. **YOLO Padrão** — `yolov8n.pt` original do COCO, sem nenhum treino adicional.
3. **CNN do Zero** — rede convolucional construída e treinada do zero.

A comparação avalia **facilidade de uso, precisão, tempo de treinamento e tempo de inferência**.

---

## Estrutura do repositório

```
.
├── README.md                                        ← você está aqui
├── entrega1_yolo_customizado/
│   └── flexmediafiap.ipynb                          ← notebook da Entrega 1 (Yasmin)
└── entrega2_comparativo/
    └── JonathanGomesRibeiroFranco_rm567109_pbl_fase6.ipynb
```

---

## Como reproduzir

### Pré-requisitos

1. Acesso ao Google Drive contendo a pasta `fiap/` com o dataset rotulado (estrutura padrão YOLO: `train/images`, `train/labels`, `valid/images`, `valid/labels`, `data.yaml`).
2. Conta no Google Colab (de preferência com runtime GPU `T4` para reduzir o tempo de treino).

### Run unificado (Entrega 1 + Entrega 2 num único notebook)

O notebook da Entrega 2 foi escrito para rodar **as duas entregas em sequência**: ele detecta se o `best.pt` da Entrega 1 (Yasmin) já existe em `/content/drive/MyDrive/fiap/train_60/weights/best.pt` e, se não existir, **re-treina o YOLOv8n com os mesmos hiperparâmetros (60 épocas, imgsz 640, batch 8)**. Em seguida, executa a Entrega 2 (CNN do Zero + YOLO Padrão) e imprime o resumo comparativo final.

A Seção **2.2** acrescenta a comparação **30 vs 60 épocas** exigida pelo enunciado: roda um treino auxiliar com 30 épocas (também idempotente — só treina se `train_30/weights/best.pt` ainda não existir) e imprime uma tabela com mAP50, mAP50-95, Precision e Recall lado a lado. A Seção **1.2** garante o `data.yaml` automaticamente via `gdown` se ele ainda não estiver no Drive.

### Passo a passo

1. Abra o notebook da Entrega 2 no Colab pelo link abaixo.
2. Em `Runtime → Change runtime type`, selecione **T4 GPU**.
3. Execute as células em ordem. O Drive será montado, o `data.yaml` baixado se faltar, o dataset reorganizado, o YOLO Customizado treinado (ou reaproveitado) e a CNN treinada — tudo em sequência.
4. A célula **2.2** dispara um treino adicional de 30 épocas (~3-4 min em T4) e imprime a tabela 30 vs 60.
5. Ao final, a célula **"Resumo Comparativo"** imprime os três tempos de inferência (YOLO Customizado, CNN do Zero, YOLO Padrão) e o tempo de treino (YOLO Customizado e CNN do Zero) lado a lado, prontos para serem citados na análise crítica.
6. A Seção **8 — Conclusão e recomendação** fecha o trabalho com a recomendação final justificada das três abordagens.

---

## Links

- **Notebook da Entrega 1** (YOLO Customizado): [`flexmediafiap.ipynb`](https://colab.research.google.com/drive/1h1QxcJ60BhpxJAFZL8Rsakuj4Xa7mQtA)
- **Notebook da Entrega 2** (Comparativo): `entrega2_comparativo/JonathanGomesRibeiroFranco_rm567109_pbl_fase6.ipynb`
- **Vídeo demonstrativo (YouTube, não listado):** _[a inserir após a gravação]_

---

## Resultados em uma frase

O **YOLO Customizado** vence a comparação no nosso problema, com `mAP50` geral de **0.645** (faca 0.928, celular 0.363). O **YOLO Padrão** funciona como baseline barato porque o COCO contém `knife` e `cell phone`, mas não generalizaria para objetos fora dessas 80 classes. A **CNN do Zero** evidencia o limite teórico de treinar uma rede convolucional sem transfer learning em um dataset de 80 imagens — útil pedagogicamente, não em produção.

A maior alavanca de melhoria identificada **não é o modelo, é o dataset**: ampliar a coleção de imagens de `celular` em contextos variados (mãos, mesas, capas, frente/verso, iluminação variável) tende a elevar o `mAP50` dessa classe para 0.7+ sem qualquer mudança arquitetural.

---

## Conclusão e limitações

A análise crítica completa, com explicações teóricas (transfer learning, overfitting com poucas amostras, generalização fora do COCO), está nas células markdown finais do notebook da Entrega 2. As limitações reconhecidas (dataset pequeno, comparação cross-task entre detecção e classificação, conjunto de teste de apenas 8 imagens) também estão documentadas lá.
