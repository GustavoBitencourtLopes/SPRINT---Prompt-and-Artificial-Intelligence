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
