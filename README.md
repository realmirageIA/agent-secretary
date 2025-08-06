#Projeto: Agent SecretaryV1.0.0

## Objetivo
Automatizar funções de secretaria executiva via **Zaia.app (Google Calendar API)**, registros em **Supabase** e automação de e-mails via **n8n**.

## Componentes Principais
- **Zaia.app / Google Calendar API** para gerenciamento de compromissos
- **Supabase** para registro de atividades, tarefas e logs
- **n8n**
  - Webhook ou IMAP/SMTP para e-mails
  - Nodes de HTTP para integração com Zaia.app
  - Nodes de Supabase (query/insert/update)
  - Logic e Function para decisões e seguimentos
  - Grafos de controle de tarefa

## Fluxo de Processos
1. **Sincronização da Agenda**
   - Periodicamente (cron) sincroniza eventos do Google Calendar via Zaia.app → atualiza `calendar_events`
2. **Checagem de E-mails**
   - IMAP/SMTP ou filtro de webhook detecta mensagens importantes
   - Decide ação: registro de tarefa, agenda ou resposta automática
3. **Gestão de Tarefas**
   - Tarefas são criadas ou atualizadas em Supabase
   - Envia lembretes automáticos conforme `due_date`
4. **Modo Relatório (“Secretaria”)**
   - Ao receber essa palavra em e-mail ou chat, gera resumo diário:
     - Agenda do dia
     - Tarefas pendentes
     - Emails não processados
5. **Interação com o Usuário**
   - Responde via e-mail com feedback profissional e confirmações

## Nós Principais do n8n
| Node | Função |
|------|--------|
| Cron de Sincronização | Chama API do Zaia para listar eventos |
| Zaia.app HTTP Request | Baixa eventos do Google Calendar |
| Supabase Query/Upsert | Inserção e atualização de `calendar_events` |
| IMAP Trigger ou Webhook Email | Recebe novos e-mails |
| Function Node de lógica | Decide ação com base no conteúdo do e-mail |
| Supabase Insert Task | Cria tarefas ou logs |
| SMTP Node / ou chat | Envia e-mails de resposta ou lembrete |
| Webhook / Chatbot | Atende palavra "Secretaria" para gerar relatório |
| AI opcional | Geração de texto mais natural para e-mails ou resumos |
