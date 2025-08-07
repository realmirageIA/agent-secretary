# Objetivo

Criar Requisitos Funcionais concisos para o seguinte serviços de agentes de IA.

# Contexto

Criar um agente de AI para funções básicas de uma secretária para atendimento interno e externo.
  
## Informações gerais
- Será utilizada a plataforma www.zaia.app (consulte a [documentação](https://zaiadocs.gitbook.io/recursos)) para configurar os agentes de IA
- Será utilizado o [Make](https://www.make.com/en)/n8n para as integrações:
	- Google Calendar
	- Gmail


# Especificações funcionais para o Agente de IA

## Secretária
### Contexto geral:
- Gerenciar agenda de compromissos:
	- Criar tarefas/eventos no calendário
	- Enviar lembrete/confirmação
- Gerenciar email:
	- Ler e responder emails
- Gerar relatórios
### Integrações:
- Google calendar
- Gmail
- Supabase?

### Contexto específicos:
- Gerenciamento de agenda/email:
	-  Pedido de reunião:
		- Se interno (@realmirage.com.br) adicionar evento no calendário com os envolvidos
		- Se externo, enviar link do [cal](call.com):
			- Demo: https://cal.com/real-mirage/demo
	- Enviar lembrete/confirmação no dia util anterior a um evento
- Relatórios:
	- Frequência configurável:
		- Diário
		- Semanal
		- Quinzenal
		- Mensal
	- Conteúdo:
		- Resumo dos últimos email no período (de acordo com a frequência configurada)

# Observações técnicas:
```
GET /calendar/events → Consulta agenda
POST /calendar/create → Cria evento
DELETE /calendar/event/:id → Cancela evento
```
