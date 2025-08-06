# 🤖 Projeto: Agent SecretaryV1.0.0

## Objetivo
Automatizar funções de secretaria executiva usando **Zaia.app (Google Calendar API)**, **Supabase** e **n8n** para:

- Gerenciar agenda de compromissos.
- Registrar tarefas e follow-ups.
- Ler e responder e-mails automaticamente.
- Gerar relatórios diários quando solicitado com a palavra secreta **“Secretaria”**.

---

## ⚙️ Componentes do Sistema

1. **Zaia.app / Google Calendar API**
   - Criação, atualização e cancelamento de eventos.
   - Consulta da agenda para evitar conflitos.
   - Retorno em JSON com `event_id`, `summary`, `start` e `end`.

2. **Supabase**
   - Armazena:
     - Tarefas do dia (`tasks`)
     - Eventos sincronizados (`calendar_events`)
     - Logs de e-mails (`email_logs`)

3. **n8n**
   - Orquestrador principal:
     - Recebe e-mails (IMAP)
     - Consulta e atualiza Supabase
     - Chama API do Zaia.app
     - Envia respostas via SMTP

---

## 🔄 Fluxo do Sistema

1. **Sincronização da Agenda**
   - **Cron Node** dispara a cada hora.
   - **HTTP Node** chama a API do **Zaia.app**:  
     `GET https://api.zaia.app/calendar/events?token=<API_KEY>`
   - **Supabase Node** faz `upsert` na tabela `calendar_events`.

2. **Checagem e Processamento de E-mails**
   - **IMAP Trigger** lê novas mensagens.
   - **Function Node** classifica a mensagem:
     - Solicitação de reunião → Cria evento no Zaia.app.
     - Follow-up → Cria tarefa no Supabase.
     - Palavra secreta **“Secretaria”** → Gera relatório do dia.

3. **Criação de Tarefas**
   - **Supabase Insert** salva detalhes em `tasks`.
   - Tarefas com `waiting_info` são sinalizadas para acompanhamento.

4. **Resposta Automática**
   - **SMTP Node** envia e-mail de confirmação ou lembrete.
   - **Optional AI Node** gera mensagens mais naturais e profissionais.

---

## 📊 Diagrama Mermaid do Fluxo

```mermaid
flowchart TD
    %% INÍCIO DO FLUXO
    subgraph AGENDA["🕑 Sincronização de Agenda"]
        A1[Cron Job - a cada hora]
        A2[HTTP GET - Zaia.app API /calendar/events]
        A3[Supabase Upsert - calendar_events]
        A1 --> A2 --> A3
    end

    subgraph EMAIL["📩 Processamento de E-mails"]
        B1[IMAP Trigger - Novo E-mail]
        B2[Function - Classificar E-mail]
        B3[Supabase Insert - tasks]
        B4[SMTP Reply - Resposta Automática]
        B5[HTTP POST - Zaia.app API /calendar/create]
        
        %% Fluxos
        B1 --> B2
        B2 -->|Follow-up| B3
        B2 -->|Agendamento| B5 --> A3
        B2 -->|Resposta| B4
        B2 -->|Palavra 'Secretaria'| C1
    end

    subgraph RELATORIO["📊 Relatório Diário"]
        C1[Function - Gerar Relatório]
        C2[Supabase Query - tasks & calendar_events]
        C3[SMTP Reply com PDF/Resumo]
        C1 --> C2 --> C3
    end

    %% Fluxos principais
    AGENDA --> EMAIL
    EMAIL --> RELATORIO

```

## 🔑 Observações Técnicas
Zaia.app API Endpoints usados:
```
GET /calendar/events → Consulta agenda
POST /calendar/create → Cria evento
DELETE /calendar/event/:id → Cancela evento
```
## E-mails:
- Recebidos via IMAP Trigger
- Respondidos via SMTP Node

## Relatórios:
- Gerados sob demanda quando palavra "Secretaria" for detectada
