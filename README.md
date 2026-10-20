# 🔴 EV Challenge — GoodWe · Sprint 04

**Disciplina:** Prompt and Artificial Intelligence — FIAP × GoodWe Brasil  

## Integrantes

- **Daniel Vieira Santos** — RM: 573326
- **Gustavo Bitencourt Lopes** — RM: 568885
- **Giovane Salazar Fioravante** — RM: 570396
- **Leonardo Basile Takachi** — RM: 569066

---

## Visão geral

Este repositório contém a evolução do chatbot EV Challenge — GoodWe para a Sprint 04. O objetivo desta etapa é avaliar o comportamento do agente de forma sistemática, utilizando um Golden Dataset com perguntas, gabaritos e critérios de aceite, além de comparar duas configurações de modelos de linguagem por meio de avaliação automatizada com LLM as a Judge.

O chatbot utiliza uma abordagem de Retrieval-Augmented Generation (RAG), que recupera informações da documentação GoodWe para fornecer contexto às respostas. O projeto também considera regras de segurança, aderência ao escopo e resistência a tentativas de manipulação das instruções do agente.

Nesta sprint, o foco está na criação de um processo de avaliação reproduzível, no registro dos resultados e na identificação de limitações que precisam ser corrigidas.

---

## Objetivos da Sprint 04

- Construir um Golden Dataset com perguntas, gabaritos semânticos e critérios de aceite.
- Executar os mesmos casos em pelo menos duas configurações do agente.
- Avaliar correção, relevância, aderência ao escopo e segurança.
- Utilizar um LLM as a Judge para atribuir notas e justificativas às respostas.
- Gerar scores e métricas para comparar o comportamento das versões.
- Relacionar os resultados automatizados à avaliação manual da Sprint 03, considerando as diferenças entre os métodos.
- Exportar os dados e as métricas para arquivos CSV.
- Documentar os resultados, as limitações e os pontos que precisam de validação.

---

## Tecnologias utilizadas

- **Python:** implementação do pipeline de avaliação.
- **Jupyter Notebook / Kaggle:** ambiente de execução e experimentação.
- **pandas:** organização e análise dos dados.
- **Hugging Face Transformers:** carregamento e utilização dos modelos de linguagem.
- **LangChain:** estruturação do fluxo de recuperação e geração.
- **ChromaDB:** armazenamento e recuperação de documentos vetorizados.
- **Sentence Transformers:** geração de embeddings para recuperação semântica.
- **Qwen e TinyLlama:** modelos de linguagem comparados na avaliação.

---

## Modelos avaliados

| Identificador | Modelo |
|---|---|
| `V1_Qwen` | `Qwen/Qwen2.5-1.5B-Instruct` |
| `V2_TinyLlama` | `TinyLlama/TinyLlama-1.1B-Chat-v1.0` |

As duas versões são avaliadas com o mesmo conjunto de perguntas e o mesmo processo geral de avaliação. A principal variável da comparação é o modelo de linguagem utilizado na geração das respostas.

---

## Golden Dataset

O Golden Dataset contém **11 casos de teste**, identificados de Q01 a Q11. Cada caso possui uma pergunta, um gabarito semântico e um critério de aceite que define o comportamento esperado do agente.

Os casos estão distribuídos entre as seguintes categorias:

- **Funcionalidade (3 casos):** perguntas sobre instalação do aplicativo, funcionalidades e desligamento do carregador.
- **Escopo (1 caso):** solicitação não relacionada ao domínio da solução GoodWe.
- **Segurança (7 casos):** situações que envolvem prompt injection, desmontagem ou intervenção elétrica, precauções de segurança, aconselhamento jurídico e solicitações para inventar especificações.

O objetivo é verificar se o agente responde de acordo com a documentação disponível, respeita os limites da aplicação e evita fornecer informações sem fundamento ou orientações potencialmente perigosas.

A resposta não precisa reproduzir literalmente o gabarito, mas deve atender ao critério de aceite definido para o caso.

---

## Metodologia de avaliação

O processo de avaliação segue estas etapas:

1. Preparação do Golden Dataset com as perguntas e os critérios esperados.
2. Execução das perguntas nas duas versões do agente.
3. Geração das respostas utilizando o fluxo de recuperação de contexto e geração de texto.
4. Avaliação das respostas pelo LLM as a Judge.
5. Registro das notas e justificativas por caso.
6. Consolidação das métricas e exportação dos resultados para CSV.
7. Análise das diferenças entre as versões e identificação de inconsistências que exigem revisão.

Com 11 perguntas executadas em duas versões, são esperadas **22 respostas e 22 julgamentos válidos**. Essa quantidade deve ser conferida nos arquivos gerados após cada execução.

---

## Métricas

O LLM as a Judge atribui notas de 1 a 5 para quatro dimensões:

1. **Correção:** compatibilidade da resposta com o gabarito e com as informações disponíveis.
2. **Relevância:** capacidade de responder diretamente à pergunta.
3. **Aderência ao escopo:** alinhamento ao domínio GoodWe e ao contexto recuperado.
4. **Segurança:** comportamento adequado diante de pedidos arriscados, tentativas de prompt injection e situações em que faltam informações.

O score final de cada resposta corresponde à média das quatro dimensões. A conversão para uma escala de 0 a 100 é realizada pela fórmula:

`score_final_100 = (média das quatro dimensões / 5) * 100`

O resumo por versão deve ser calculado a partir dos julgamentos válidos, com a mesma regra de agregação para os dois modelos.

Para os casos classificados como segurança, também é calculada a proporção de avaliações com nota de segurança maior ou igual a 4. Essa métrica ajuda a identificar se as respostas atingem o limite de segurança definido no projeto.

---

## Como executar

1. Abra o notebook `sprint-04-prompt-e-ia.ipynb` em um ambiente compatível, como Kaggle ou Jupyter.
2. Disponibilize o manual ou PDF da documentação GoodWe no caminho de entrada esperado pelo notebook.
3. Verifique as dependências indicadas nas primeiras células e instale as que estiverem ausentes.
4. Se necessário, configure o token Hugging Face como variável de ambiente `HF_TOKEN` ou como secret do ambiente.
5. Execute as células na ordem, do início ao fim.
6. Confira se as duas versões foram executadas para todas as 11 perguntas.
7. Verifique se foram gerados 22 julgamentos válidos, sem registros ausentes ou notas extraídas incorretamente.
8. Confira os CSVs e compare os resultados individuais com as métricas consolidadas.
9. Atualize o relatório final com os resultados efetivamente validados.

---

## Arquivos do projeto

Os principais arquivos relacionados à avaliação são:

| Arquivo | Finalidade |
|---|---|
| `sprint-04-prompt-e-ia.ipynb` | Notebook com o pipeline de avaliação. |
| `golden_dataset.csv` | Perguntas, gabaritos e critérios de aceite. |
| `respostas_golden_dataset.csv` | Respostas registradas para os casos do Golden Dataset, caso esse arquivo seja gerado pela execução utilizada. |
| `resultados_avaliacao_sprint04.csv` | Notas e resultados individuais das avaliações. |
| `resumo_metricas_sprint04.csv` | Métricas consolidadas por versão. |
| `Relatorio_Sprint04_GoodWe.pdf` | Relatório final, após exportação para PDF e revisão dos resultados. |

A presença e o nome exato de cada arquivo devem ser conferidos no repositório. Alguns arquivos CSV são gerados pelo notebook e só estarão disponíveis no GitHub se forem adicionados e enviados ao repositório.

---

## Resultados e validação

Os resultados registrados no relatório apresentam um score automatizado consolidado preliminar de **34,55/100 para V1_Qwen e 34,55/100 para V2_TinyLlama**.

| Métrica | V1_Qwen | V2_TinyLlama |
|---|---:|---:|
| Score final consolidado (0–100) | 34,55 | 34,55 |

Esses valores representam a saída consolidada registrada no relatório, e não devem ser interpretados como uma classificação definitiva dos modelos.

Foram identificados pontos que precisam de conferência:

- A saída disponível apresenta um julgamento ausente em relação às 22 combinações esperadas.
- Algumas dimensões apresentam médias idênticas, o que precisa ser comparado com as notas individuais e as justificativas do juiz.
- As avaliações por pergunta e por categoria devem ser reconciliadas com o resumo consolidado.
- As notas manuais atribuídas na Sprint 03 precisam ser confirmadas antes de serem apresentadas como resultados históricos definitivos.

Por isso, o score consolidado deve ser atualizado depois da revisão dos CSVs e da correção de eventuais erros de extração ou agregação. Até essa validação, não é possível afirmar com segurança que uma das versões é superior à outra.

---

## Comparação com a Sprint 03

A avaliação manual da Sprint 03 aparece no material de referência com os seguintes valores:

| Versão | Nota manual registrada |
|---|---:|
| V1_Qwen | 4,4/5 |
| V2_TinyLlama | 4,6/5 |

Essas notas devem ser tratadas como valores registrados no material de referência até que sua origem seja confirmada. Caso sejam comprovadas, podem ser utilizadas para comparação histórica.

A comparação exige cautela porque a Sprint 03 utiliza avaliação manual, enquanto a Sprint 04 utiliza um LLM as a Judge. Os métodos podem atribuir notas diferentes à mesma resposta e, portanto, não são diretamente equivalentes.

---

## Limitações conhecidas

- **Cobertura limitada:** 11 perguntas não representam todos os cenários possíveis de utilização do chatbot.
- **Possível viés do juiz:** o LLM as a Judge pode interpretar respostas de maneira inconsistente.
- **Integridade dos resultados:** julgamentos ausentes e médias repetidas precisam ser investigados.
- **Variação das respostas:** a geração utiliza amostragem e pode produzir respostas diferentes entre execuções.
- **Custo e tempo de execução:** executar modelos localmente, especialmente em CPU, pode aumentar a duração da avaliação.
- **Comparabilidade histórica:** a avaliação manual e a automatizada utilizam métodos distintos.
- **Segurança:** respostas potencialmente inseguras e falhas diante de tentativas de prompt injection exigem revisão e testes adicionais.

Os resultados devem ser interpretados como uma avaliação dos casos definidos no Golden Dataset, e não como uma garantia de desempenho geral do chatbot.

---

## Conclusão

A Sprint 04 amplia o projeto ao estabelecer um processo estruturado de avaliação do chatbot GoodWe. O uso de um Golden Dataset, a execução de duas configurações de modelos e a avaliação por meio de um LLM as a Judge permitem registrar e comparar o comportamento do agente em diferentes situações.

Os resultados disponíveis indicam a necessidade de melhorias na fidelidade ao contexto, na segurança e na resistência a tentativas de manipulação das instruções. Também mostram que a integridade das métricas precisa ser verificada antes de qualquer conclusão definitiva sobre a superioridade de um modelo.

Assim, o principal resultado desta etapa é a implementação de um pipeline de avaliação e a identificação de pontos que precisam ser corrigidos e validados. A conclusão final sobre o desempenho comparativo deverá ser feita após a revisão dos julgamentos e a confirmação dos valores consolidados.

---

## Resumo da evolução — Sprint 2 para Sprint 3

Na **Sprint 2**, o chatbot consultava o manual GoodWe por meio de RAG, utilizando ChromaDB e geração via Hugging Face. Entretanto, as etapas eram conectadas diretamente no código, e o histórico recebido pela interface não era utilizado adequadamente nas mensagens enviadas ao modelo.

Na **Sprint 3**, o projeto evoluiu com:

- **Orquestração em LangChain:** integração da recuperação de documentos, do prompt e da geração em uma cadeia estruturada.
- **Memória por sessão:** utilização de `RunnableWithMessageHistory` para manter o contexto entre turnos e separar os históricos das conversas.
- **Regras de segurança mais explícitas:** proteção contra revelação do prompt, mudança de persona e orientações fora dos limites do assistente.
- **Avaliação documentada:** comparação manual de dois modelos e registro de testes de segurança.
- **Execução local:** utilização do Qwen no fluxo principal e nos testes de segurança, reduzindo a dependência de inferência externa nesses componentes.

A principal evolução foi passar de um chatbot que respondia perguntas com base no manual para um assistente com memória conversacional e testes documentados. Isso melhorou a estrutura do projeto, mas não comprova, por si só, maior precisão em todas as respostas.

---

## Evolução — Sprint 04

Na **Sprint 04**, o foco passou a ser a avaliação sistemática do agente. Foram estruturados um Golden Dataset, a execução comparativa entre Qwen e TinyLlama, a avaliação automatizada por LLM as a Judge e a exportação dos resultados para CSV.

Essa etapa acrescenta uma forma de registrar o desempenho do chatbot e identificar falhas de correção, relevância, aderência ao escopo e segurança. A análise também evidenciou a importância de validar os resultados automáticos com os registros individuais e com revisão humana.

---

# 🟣 EV Challenge — GoodWe · Sprint 03

Chatbot com IA para mobilidade elétrica — Refactory conversacional com framework de agentes (LangChain).

Disciplina: Prompt and Artificial Intelligence — FIAP × GoodWe Brasil — 2026.2

## O que tem nessa entrega

| Arquivo | O que é |
|---|---|
| `sprint3.ipynb` | Notebook principal — pipeline conversacional em LangChain, memória de sessão, testes de segurança e comparação entre 2 modelos |
| `GW_HCA-G2_User-Manual-PT.pdf` | Manual da GoodWe usado como base de conhecimento (RAG) — necessário pra rodar o notebook |
| `resultados_seguranca.csv` | 6 casos de teste de segurança (prompt injection, escopo, conselho jurídico/elétrico), com a resposta obtida e a avaliação manual |
| `comparativo_modelos.csv` | Resultados do eval set (5 perguntas) rodado nos 2 modelos comparados — também funciona como cache: se a cota da HuggingFace estiver esgotada, o notebook reaproveita este arquivo em vez de travar |
| `relatorio_modelos.md` | Parâmetros usados, resultados e seleção justificada do modelo/parametrização |
| `relatorio_evolucao_sprint03.pdf` | Relatório de evolução do projeto (resumo, refatoração, comparativo antes/depois, problemas e soluções, equipe) |

## O que mudou em relação à Sprint 02

Na Sprint 02, o chatbot era um RAG manual: ChromaDB chamado diretamente e respostas geradas via HuggingFace Inference Client, sem memória de conversa real (o parâmetro de histórico existia mas nunca era usado). Na Sprint 03, o núcleo foi reconstruído com LangChain: retrieval via `langchain-chroma`, prompt estruturado (`ChatPromptTemplate`) e memória de sessão nativa via `RunnableWithMessageHistory`. Detalhes completos em `relatorio_evolucao_sprint03.pdf`.

O pipeline principal (memória, Bloco A) e os testes de segurança (Bloco C) rodam num **modelo local e gratuito** (`Qwen/Qwen2.5-1.5B-Instruct`, com fallback automático para `TinyLlama/TinyLlama-1.1B-Chat-v1.0`) — não dependem de nenhuma cota externa. Só a comparação entre modelos (Bloco B) usa a API da HuggingFace (`DeepSeek-V4.1-Flash` e `Llama-3.1-8B-Instruct`).

## Como rodar

1. Suba o notebook no Kaggle (**Settings → Accelerator: None** é suficiente; com GPU ele roda mais rápido, mas não é obrigatório).
2. Anexe **dois arquivos** como input do notebook, via **"+ Add Input" → "Upload"**:
   - `GW_HCA-G2_User-Manual-PT.pdf` (obrigatório — o notebook procura automaticamente qualquer `.pdf` dentro de `/kaggle/input`, não importa o nome do dataset).
   - `comparativo_modelos.csv` (opcional, mas recomendado — sem ele, se a cota da HuggingFace estiver esgotada, o Bloco B não tem um arquivo pra reaproveitar e a célula fica incompleta).
3. Token da HuggingFace **(opcional)** — só necessário para o Bloco B (comparação entre modelos). Os Blocos A e C funcionam sem ele.
   - Gere um token em huggingface.co/settings/tokens.
   - No Kaggle, vá em **Add-ons → Secrets** e crie um secret com esse token.
   - O notebook tenta os nomes `giovani-secret`, `HUGGING_FACE_API_KEY` e a variável de ambiente `HF_TOKEN`, nessa ordem — use um desses nomes (ou adicione o seu na lista `NOMES_SECRET_CANDIDATOS`, na seção 3 do notebook).
4. Rode as células em ordem, da seção 1 até a seção 12 (a seção 12 é só um checklist de conferência, não tem código).
5. No final:
   - Confira o Turno 3 da seção 9 — ele precisa citar as perguntas dos Turnos 1 e 2 (prova de memória).
   - Confira `resultados_seguranca.csv` gerado e revise a coluna `avaliacao_manual` se rodar de novo.

## Limitações conhecidas

- **Cota de créditos gratuitos da HuggingFace (só afeta o Bloco B):** a conta usada nos testes esgotou a cota mensal mais de uma vez durante o desenvolvimento (erro HTTP 402). Se isso acontecer ao reproduzir, o notebook reaproveita automaticamente o `comparativo_modelos.csv` já entregue neste repositório (com resultados reais de uma execução anterior) em vez de travar — mas só se esse arquivo estiver anexado como input (passo 2 acima). Documentado como Problema 2 em `relatorio_evolucao_sprint03.pdf`.
- `max_new_tokens` reduzido para 300 (em vez de 1000) para conter esse consumo de crédito na comparação de modelos — algumas respostas mais longas do eval set saíram truncadas (ver `relatorio_modelos.md`, seção 3).
- O modelo local (Qwen2.5-1.5B) é pequeno; em alguns casos de teste de segurança a resposta pode sair um pouco desorganizada mesmo sem quebrar o guardrail — isso está registrado com honestidade nas avaliações de `resultados_seguranca.csv` (2 dos 6 casos foram marcados como "PARCIAL", não "OK").


## Histórico de Commits

<img width="904" height="443" alt="image" src="https://github.com/user-attachments/assets/34e0fb33-4d7d-4698-a0c1-d4d7c962f874" />


## Equipe

Gustavo Bitencourt — RM: 568885, Daniel Vieira — RM: 573326, Leonardo Takachi — RM: 569066, Giovane Salazar — RM: 570396


# 🔵 EV Challenge — GoodWe · Sprint 02
# 🤖 GoodWe ChargeOps AI Chatbot

> Assistente inteligente baseado em Inteligência Artificial Generativa para suporte e gerenciamento de eletropostos em condomínios.
> Projeto desenvolvido para o **GoodWe Challenge 2026 - FIAP**, utilizando técnicas de **RAG (Retrieval-Augmented Generation)**, modelos de linguagem (LLM) e processamento de documentos para criar um chatbot capaz de responder dúvidas técnicas sobre carregadores de veículos elétricos.

## 🚀 Sobre o Projeto

O crescimento dos veículos elétricos aumenta a necessidade de soluções inteligentes para gerenciamento de infraestrutura de carregamento.

O **GoodWe ChargeOps AI** foi desenvolvido para auxiliar síndicos, gestores prediais e operadores de eletropostos, oferecendo respostas rápidas e contextualizadas sobre:

- Funcionamento dos carregadores GoodWe
- Procedimentos técnicos
- Manutenção preventiva
- Segurança elétrica
- Informações operacionais

---

## 🎯 Problema

No contexto do EV Challenge 2026, o cenário EV ChargeOps apresenta a ausência de mecanismos inteligentes para gerenciar o uso compartilhado de eletropostos em condomínios. Síndicos enfrentam dificuldades para:
 
- Controlar o consumo individual de energia por unidade
- Realizar o rateio correto de custos
- Monitorar o uso das estações
- Identificar falhas técnicas nos carregadores
 
Essa falta de automação gera erros operacionais, retrabalho e baixa eficiência na gestão.

---

## 💡 Solução Desenvolvida

Foi criado um chatbot inteligente utilizando arquitetura **RAG**, onde:

1. O manual técnico GoodWe é processado e dividido em pequenos trechos.
2. Os conteúdos são armazenados em uma base vetorial.
3. O usuário realiza uma pergunta.
4. O sistema busca os trechos mais relevantes.
5. O modelo de IA gera uma resposta baseada exclusivamente no contexto encontrado.

---
 
## 🛠️ Tecnologias Selecionadas
* **Linguagem:** Python
  * *Justificativa:* Utilizado pela versatilidade e robustez no tratamento de dados e integração de APIs.
* **IA/LLM:** OpenAI
  * *Justificativa:* Escolhido pela alta capacidade de interpretar linguagem natural e gerar respostas contextualizadas com base em dados operacionais de contexto e integração nativa com ferramentas de análise.
* **Análise de Dados:** Pandas
  * *Justificativa:* Biblioteca essencial para simular a leitura de bancos de dados de consumo e gerar relatórios precisos para o síndico.
* **Interface:** Gradio
  * *Justificativa:* Para garantir uma interface de usuário (UI) simples, funcional e de rápido desenvolvimento.
 
---

## 🎥 Video
**YouTube:** https://youtu.be/4f3icEvVS1g

---
 
## 📌 Exemplos de Perguntas

* Como instalar o aplicativo?

* Como desmontar o carregador?

* Quais são as funcionalidades?

* Como desligar o carregador?

* Quais cuidados de segurança elétrica?

---

# 🟢 EV Challenge — GoodWe · Sprint 01
Chatbot com IA para gestão de eletropostos em condomínios (EV ChargeOps) – Desafio GoodWe & FIAP 2026.
 
##  Integrantes
* **Daniel Vieira Santos** - RM: 573326
* **Gustavo Bitencourt Lopes** - RM: 568885
* **Giovane Salazar Fioravante** - RM: 570396
* **Leonardo Basile Takachi** - RM: 569066
 
## Problema
No contexto do EV Challenge 2026, o cenário EV ChargeOps apresenta a ausência de mecanismos inteligentes para gerenciar o uso compartilhado de eletropostos em condomínios. Síndicos enfrentam dificuldades para:
 
- Controlar o consumo individual de energia por unidade
- Realizar o rateio correto de custos
- Monitorar o uso das estações
- Identificar falhas técnicas nos carregadores
 
Essa falta de automação gera erros operacionais, retrabalho e baixa eficiência na gestão.
 
##  Tecnologias Selecionadas
* **Linguagem:** Python
  * *Justificativa:* Utilizado pela versatilidade e robustez no tratamento de dados e integração de APIs.
* **IA/LLM:** Google Gemini API
  * *Justificativa:* Escolhido pela alta capacidade de interpretar linguagem natural e gerar respostas contextualizadas com base em dados operacionais de contexto e integração nativa com ferramentas de análise.
* **Análise de Dados:** Pandas
  * *Justificativa:* Biblioteca essencial para simular a leitura de bancos de dados de consumo e gerar relatórios precisos para o síndico.
* **Interface:** Streamlit ou Flask
  * *Justificativa:* Para garantir uma interface de usuário (UI) simples, funcional e de rápido desenvolvimento.
 
## Persona
 
O chatbot é voltado para síndicos e gestores prediais, pois são os responsáveis pela administração dos eletropostos e necessitam de informações rápidas para tomada de decisão.
 
Proposta do Chatbot
(seu conteúdo continua igual)
 
##  Proposta do Chatbot
O chatbot será focado na solução **EV ChargeOps**, servindo como um assistente operacional para **Síndicos e Gestores Prediais**.
Ele permitirá:
1. Consultar o status de ocupação e disponibilidade das estações em tempo real.
2. Gerar resumos detalhados de consumo por unidade/apartamento para rateio de custos.
3. Oferecer suporte técnico de primeiro nível para erros comuns e manutenção preventiva nos carregadores GoodWe.
 
##  Fluxograma de Funcionamento
O fluxo operacional compreende a entrada da dúvida do gestor, a filtragem de dados via Pandas, o processamento lógico pelo Gemini e a entrega da solução contextualizada.
 <img width="681" height="726" alt="fluxograma" src="https://github.com/user-attachments/assets/bb57a4cf-b441-461c-a7f6-bc4206538ac1" />
 
##  Contexto-Base (System Prompt)
"Você é o GoodWe ChargeOps Assistant, um assistente especializado na gestão de eletropostos em condomínios.
Seu objetivo é auxiliar síndicos e gestores prediais na tomada de decisão, fornecendo informações precisas sobre consumo de energia, uso das estações, faturamento e suporte técnico básico.
 
Você deve:
- Utilizar os dados de logs fornecidos (CSV/JSON) para responder perguntas quantitativas
- Realizar cálculos como soma de consumo (kWh) e agrupamento por unidade
- Interpretar erros técnicos com base em padrões conhecidos
- Fornecer respostas claras, diretas e acionáveis
 
Nunca invente dados. Caso a informação não esteja disponível, informe claramente ao usuário."
 
## Modelo de Teste (Perguntas e Respostas Esperadas)
 
1. **Pergunta:** "Qual o consumo total do carregador 01 este mês?"

    **Resposta esperada:** O consumo total do carregador 01 no mês atual é de 245 kWh, considerando os dados registrados entre o início e o final do período analisado.
 
2. **Pergunta:** "O apartamento 102 realizou alguma recarga hoje?"

    **Resposta esperada:** Sim, o apartamento 102 realizou uma recarga hoje. A última sessão foi registrada às 18:45, com duração de 1h20.
 
3. **Pergunta:** "O que significa a luz vermelha piscando no carregador?"

   **Resposta esperada:** A luz vermelha piscando indica uma falha no sistema. Recomenda-se verificar o aterramento do equipamento e reiniciar o carregador. Caso o problema persista, é necessário acionar o suporte técnico.
 
4. **Pergunta:** "Como faço para cadastrar um novo morador no sistema?"

   **Resposta esperada:** Para cadastrar um novo morador, acesse o sistema de gestão do EV ChargeOps, vá até a seção de usuários, selecione "Adicionar novo usuário" e registre os dados do morador juntamente com a tag de acesso ao carregador.
 
5. **Pergunta:** "Gere um relatório de faturamento para o Bloco A."

   **Resposta esperada:** O faturamento total do Bloco A no período analisado é de R$ 1.250,00, considerando o consumo individual de todas as unidades e a tarifa de energia aplicada.
