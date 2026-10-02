# Grandes Modelos de Linguagem (LLMs)

![Python](https://img.shields.io/badge/python-3.10%2B-3776AB?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-notebooks-F37626?logo=jupyter&logoColor=white)
![LaTeX](https://img.shields.io/badge/LaTeX-Beamer-008080?logo=latex&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green)

Material didático sobre Grandes Modelos de Linguagem, desenvolvido no **Laboratório de Simulações Numéricas e Inteligência Artificial**. O repositório reúne os slides da apresentação, em português e inglês, e uma coleção de exemplos práticos que aplicam IA generativa à Engenharia de Software: RAG, ajuste fino com LoRA, *tool calling*, MCP, visualização de atenção e TDD assistido por LLMs.

---

## Sumário

- [Visão geral](#visão-geral)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Apresentação](#apresentação)
- [Exemplos práticos](#exemplos-práticos)
- [Primeiros passos](#primeiros-passos)
- [Execução dos módulos](#execução-dos-módulos)
- [Autores](#autores)
- [Licença](#licença)

---

## Visão geral

A apresentação oferece uma visão conceitual e matemática de como funciona a IA generativa moderna, partindo das arquiteturas de redes neurais até os impactos sociotécnicos dos modelos. Os exemplos práticos levam esses conceitos para o código, cada um em um módulo independente, com notebook ou script próprio e, quando aplicável, `Dockerfile` e `requirements.txt`.

---

## Estrutura do repositório

```
.
├── code/
│   ├── attention-visualization/  # Visualização da atenção em Transformers (BERT, BERTimbau, GPT-2)
│   ├── fine-tuning-example/      # Ajuste fino completo e LoRA do CodeBERTa para detecção de vulnerabilidades
│   ├── rag-example/              # Retrieval-Augmented Generation sobre a especificação de uma API
│   ├── tool-calling-example/     # Function calling com LangChain para consultas em repositórios Git
│   ├── mcp-example/              # Servidor e cliente MCP expondo ferramentas de Git a um LLM
│   └── assured-tdd/              # Ciclos de TDD e refatoração conduzidos por LLM
└── presentation/
    ├── LLM-BR.tex / LLM-BR.pdf   # Slides em português
    ├── LLM-EN.tex / LLM-EN.pdf   # Slides em inglês
    └── pic/                      # Figuras utilizadas nos slides
```

---

## Apresentação

Os slides foram produzidos em LaTeX (Beamer) e estão disponíveis já compilados em [`presentation/LLM-BR.pdf`](presentation/LLM-BR.pdf) e [`presentation/LLM-EN.pdf`](presentation/LLM-EN.pdf).

O conteúdo começa pelas **arquiteturas de redes neurais**, comparando modelos baseados em *decoders*, *encoders* e *encoder-decoders*, e segue para a **geração condicional**, que formula tarefas como análise de sentimento e sumarização como predição sequencial de palavras. Em seguida, os **algoritmos de decodificação e amostragem** são analisados em detalhe, incluindo Top-k, Top-p (*nucleus sampling*) e o controle da distribuição por temperatura.

A parte central trata do **pré-treinamento e da engenharia de dados**, cobrindo o paradigma autossupervisionado, a minimização da entropia cruzada e a curadoria de grandes corpora como C4 e The Pile, e do **ajuste fino eficiente em parâmetros (PEFT)**, com a formulação matemática do LoRA (*Low-Rank Adaptation*). A apresentação encerra com **avaliação e leis de escala**, baseadas em perplexidade, e com os **danos sociais** associados aos LLMs, como alucinações, violação de direitos autorais, toxicidade e vazamento de privacidade.

Para recompilar os slides a partir do código-fonte:

```bash
cd presentation
latexmk -pdf LLM-BR.tex
latexmk -pdf LLM-EN.tex
```

---

## Exemplos práticos

| Módulo | Tema | Formato | Principais tecnologias |
|---|---|---|---|
| [`attention-visualization`](code/attention-visualization) | Tokenização, mapas de atenção por camada e cabeça, atenção causal | Notebook | PyTorch, Transformers, BertViz |
| [`fine-tuning-example`](code/fine-tuning-example) | Detecção de vulnerabilidades em C (Devign), ajuste completo vs. LoRA | Notebook | PyTorch, Transformers, PEFT, scikit-learn |
| [`rag-example`](code/rag-example) | Geração de código a partir da documentação recuperada de uma API | Notebook | LangChain, OpenAI |
| [`tool-calling-example`](code/tool-calling-example) | LLM invocando funções Python sobre um repositório Git | Notebook | LangChain, GitPython, OpenAI |
| [`mcp-example`](code/mcp-example) | Ferramentas de Git expostas via Model Context Protocol | Scripts | MCP (FastMCP), LangChain, OpenAI |
| [`assured-tdd`](code/assured-tdd) | Implementação e refatoração guiadas por testes unitários | Scripts | OpenAI, Lizard |

**`attention-visualization`** investiga para onde um modelo "olha" em uma frase. O notebook percorre a tokenização em subpalavras, visualiza a atenção do BERT com BertViz, reconstrói manualmente o cálculo de uma cabeça de atenção, compara padrões entre camadas e repete a análise em português com o BERTimbau e em um modelo gerador com atenção causal (GPT-2).

**`fine-tuning-example`** aplica a teoria de pré-treinamento, ajuste fino e LoRA a um problema real de segurança de software: classificar funções em C como seguras ou vulneráveis. O notebook estabelece linhas de base (classe majoritária e TF-IDF com regressão logística), trata o vazamento de dados entre *commits* na divisão treino/teste e compara o ajuste fino completo do CodeBERTa com o LoRA em métricas, curvas ROC e precisão-revocação.

**`rag-example`** implementa um pipeline de *Retrieval-Augmented Generation* sobre a especificação da API OpenDataHub, que fornece dados de turismo do Tirol do Sul (`opendatahub.json`). As definições das chamadas são indexadas com *embeddings*, os trechos relevantes para a tarefa são recuperados e o modelo gera um script Python fundamentado nessa documentação, que é salvo em `output/main.py` e executado no próprio notebook.

**`tool-calling-example`** demonstra como definir funções Python como ferramentas, vinculá-las ao modelo e deixar que ele decida quando chamá-las para responder perguntas sobre o histórico de um repositório Git (o projeto Defects4J é usado como alvo).

**`mcp-example`** separa as mesmas capacidades em um servidor MCP (`mcp-git-server.py`), que expõe ferramentas como listagem de *commits* por período ou por autor, e em um cliente interativo (`client.py`), que conecta um LLM ao servidor via `stdio`.

**`assured-tdd`** automatiza o ciclo de TDD: a partir de um arquivo de testes, o LLM escreve a implementação, executa os testes e corrige o código até que passem (`write_code`), podendo em seguida refatorá-lo preservando o comportamento (`refactor`). As interações são registradas em pastas de *log* para análise posterior.

---

## Primeiros passos

### Pré-requisitos

O ambiente recomendado é Python 3.10 ou superior com JupyterLab ou Jupyter Notebook. Os módulos `fine-tuning-example` e `attention-visualization` se beneficiam de GPU, mas foram parametrizados para rodar também em CPU, com tempo de execução maior. Os módulos `rag-example`, `tool-calling-example`, `mcp-example` e `assured-tdd` consomem a API da OpenAI e exigem uma chave válida.

### Instalação

```bash
git clone https://github.com/joaobezcerra/LLM.git
cd LLM
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install jupyterlab
```

As dependências são instaladas por módulo, seja pelo `requirements.txt` da pasta, seja pelas células `%pip install` no início de cada notebook.

### Credenciais

Os módulos que utilizam a OpenAI leem a chave da variável de ambiente `OPENAI_API_KEY`. Os notebooks solicitam a chave interativamente caso ela não esteja definida.

```bash
export OPENAI_API_KEY="sua-chave"
```

---

## Execução dos módulos

### Notebooks

```bash
jupyter lab code/
```

Abra o notebook do módulo desejado e execute as células em ordem. O notebook de ajuste fino baixa automaticamente o conjunto de dados Devign a partir do MADE-WIC, e o de visualização de atenção baixa os modelos do Hugging Face Hub na primeira execução.

### MCP

```bash
cd code/mcp-example
pip install -r requirements.txt
git clone https://github.com/rjust/defects4j.git
python client.py
```

O cliente inicia o servidor MCP automaticamente e abre um *prompt* interativo para consultas em linguagem natural sobre o repositório.

### TDD assistido

```bash
cd code/assured-tdd
pip install -r requirements.txt
python -m tdd.write_code -t test.py -c code.py      # implementa até os testes passarem
python -m tdd.refactor   -t test.py -c code.py      # refatora mantendo os testes verdes
```

Os parâmetros opcionais `--max_number_repetitions`, `--full_context`, `--print_context`, `--print_message` e `--logs_folder` controlam o número de tentativas, o contexto enviado ao modelo e o registro das interações.

### Docker

Os módulos `rag-example`, `tool-calling-example`, `mcp-example` e `assured-tdd` incluem um `Dockerfile` para execução isolada. Para os módulos baseados em notebook, o contêiner inicia um servidor Jupyter na porta 8888:

```bash
cd code/rag-example
docker build -t llm-rag .
docker run -p 8888:8888 -e OPENAI_API_KEY="$OPENAI_API_KEY" llm-rag
```

---

## Autores

- **João Gabriel de Morais Bezerra** — [@joaobezcerra](https://github.com/joaobezcerra)
- **Daniel Henrique Peres Servejeira** — [@DanielServejeira](https://github.com/DanielServejeira)
- **José Henrique Ioki Yamaoki**

Laboratório de Simulações Numéricas e Inteligência Artificial.

---

## Licença

Distribuído sob a licença MIT. Consulte o arquivo [`LICENSE`](LICENSE) para mais detalhes.
