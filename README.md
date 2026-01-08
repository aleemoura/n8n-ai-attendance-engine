# AI Attendance Engine – n8n + Evolution API

Motor de atendimento automatizado **100% orientado por IA**, desenvolvido em **n8n**, com suporte a mensagens **texto, áudio e imagem**, controle de contexto, memória conversacional, delays humanizados e handoff inteligente para atendimento humano.

Este projeto representa uma **arquitetura real de produção**, disponibilizada aqui em versão **sanitizada** para fins de portfólio.

---

## Objetivo do Workflow

Automatizar o atendimento inbound via WhatsApp, qualificando leads de forma natural e contextualizada, utilizando IA para:

* Interpretar mensagens multimodais (texto, áudio e imagem)
* Manter coerência de conversa com memória
* Evitar conflitos entre bot e humano
* Humanizar o tempo e formato das respostas
* Encaminhar leads qualificados para atendimento humano

---

## Visão Geral da Arquitetura

Fluxo resumido:

1. Recebe mensagens via **Webhook** (Evolution API)
2. Identifica se a mensagem é:

   * Texto
   * Áudio (transcrição automática)
   * Imagem (análise via IA)
3. Unifica e acumula mensagens enviadas em sequência
4. Aguarda uma janela de tempo para evitar respostas fragmentadas
5. Envia o contexto completo para o **AI Agent**
6. Aplica regras de negócio, tom e restrições
7. Divide respostas longas e envia com **delay humanizado**
8. Detecta intervenção humana e pausa o bot automaticamente
9. Mantém histórico e contexto via **Redis (TTL)**

---

## Stack Utilizada

* **n8n** (self-hosted ou cloud)
* **Evolution API** (WhatsApp)
* **OpenAI / LLM compatível**
* **Redis** (memória e controle de estado)
* **Webhooks**
* **LangChain Nodes**
* **Multimodal AI (texto, áudio, imagem)**

---

## Principais Componentes Técnicos

### Detecção de Tipo de Mensagem

* Switch identifica `conversation`, `audioMessage` e `imageMessage`
* Áudio → transcrição automática
* Imagem → análise semântica via IA

### Acúmulo Inteligente de Mensagens

* Mensagens curtas consecutivas são acumuladas
* Janela de espera configurável evita respostas quebradas
* Contexto final é enviado como uma única entrada para a IA

### Memória Conversacional

* Implementada com **Redis Chat Memory**
* TTL configurado para evitar crescimento infinito
* Mantém coerência entre perguntas e respostas

### Controle Bot vs Humano

* Se um humano responde pelo WhatsApp:

  * Bot entra em modo de bloqueio temporário
  * Redis armazena estado de pausa
  * Evita colisão de mensagens

### Humanização de Respostas

* Respostas longas são divididas automaticamente
* Envio em batches
* Delay aleatório simula digitação humana

---

## Segurança e Boas Práticas

* Nenhuma credencial versionada
* Workflow desacoplado de ambiente
* Dados de empresa **fictícios**
* Tokens e IDs removidos
* Pronto para importação em qualquer instância n8n

---

## Estrutura do Repositório

```
.
├── workflow/
│   └── ai-attendance-engine.json
├── assets/
│   └── flow-overview.png
└── README.md
```

---

## Pré-requisitos para Execução

Este workflow requer a configuração local das seguintes credenciais no n8n:

* OpenAI (ou LLM compatível)
* Evolution API
* Redis

Além disso:

* Uma instância ativa do n8n
* Evolution API conectada a um número WhatsApp

---

## Importação do Workflow

1. Acesse o n8n
2. Menu → Import workflow
3. Selecione o arquivo `ai-attendance-engine.json`
4. Configure as credenciais localmente
5. Ative o workflow

---

## Status do Projeto

* Arquitetura validada
* Fluxo funcional
* Pronto para adaptação em outros nichos
* Projeto demonstrativo (portfólio)

---

## Observação Final

Este repositório representa uma **arquitetura avançada de automação com IA**, aplicável a diversos segmentos como:

* Concessionárias
* Imobiliárias
* Clínicas
* Serviços em geral
* Funis de vendas inbound

O foco é demonstrar **lógica, estrutura e boas práticas**, não um produto fechado.

---

### Autor

Projeto desenvolvido para fins de portfólio técnico em **Automação, IA e Integrações com n8n**.