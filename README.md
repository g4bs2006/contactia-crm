# CRM de leads para clínicas

CRM que acompanha o lead de uma clínica odontológica do primeiro contato até o
fechamento do tratamento, com a agenda da clínica no mesmo lugar. Ele abre dentro
da plataforma de mensagens que a clínica já usa, então a equipe atende e move o
card sem trocar de tela.

> Este repositório é uma vitrine: mostra as telas e o desenho do sistema. O código
> é privado. Todos os dados dos prints são fictícios, de uma clínica de demonstração.

![Quadro do CRC1, com os leads por etapa](telas/01-quadro-crc1.png)

## O que ele resolve

A clínica recebia leads de anúncios e controlava tudo em planilha: quem chamou,
quem agendou, quem faltou, quem fechou. O CRM troca a planilha por dois painéis,
um para cada momento do funil, e mede o resultado sozinho.

- **CRC1, agendamento:** do lead que chegou até a avaliação marcada.
- **CRC2, fechamento:** do paciente que compareceu até o tratamento fechado.
- **Agenda:** cada consulta anda junto com o card do paciente.
- **Dashboard e metas:** o gestor vê o funil, a receita e o que precisa de ação.

## Telas

### Quadro de leads

Cada coluna é uma etapa. O card mostra a origem do lead (o anúncio de onde veio),
as etiquetas, a unidade, quem é o responsável, as ligações feitas e a consulta
marcada. A barra colorida avisa quanto tempo o lead está parado.

| Kanban do CRC2 | Lista |
|---|---|
| ![Quadro do CRC2](telas/06-quadro-crc2.png) | ![Leads em lista](telas/02-quadro-lista.png) |

| Filtros em gaveta | Novo lead |
|---|---|
| ![Gaveta de filtros](telas/03-quadro-filtros.png) | ![Diálogo de novo lead](telas/05-novo-lead.png) |

### Ficha do card

Tudo sobre o lead em uma gaveta lateral: dados, campanha de origem, responsáveis,
anotações, anexos e o histórico completo do que aconteceu com ele. As saídas do
funil (compareceu, não é lead, perda) ficam no alto.

![Ficha do card](telas/04-ficha-do-card.png)

### Dashboard

Oito visões sobre o mesmo período, por unidade. Cada número conta pela data em
que aconteceu, e cada indicador explica como é calculado.

![Visão geral do dashboard](telas/07-dashboard-visao-geral.png)

| Meta vs realizado | CRC1, agendamento |
|---|---|
| ![Meta vs realizado](telas/08-dashboard-meta-vs-realizado.png) | ![Dashboard do CRC1](telas/09-dashboard-crc1.png) |

| CRC2, fechamento | Funil por campanha |
|---|---|
| ![Dashboard do CRC2](telas/10-dashboard-crc2.png) | ![Funil por campanha](telas/11-dashboard-funil-por-campanha.png) |

| Cortes por pessoa, campanha e origem | Receita |
|---|---|
| ![Cortes](telas/12-dashboard-cortes.png) | ![Receita](telas/13-dashboard-receita.png) |

| Para agir | Metas do mês |
|---|---|
| ![Para agir](telas/14-dashboard-para-agir.png) | ![Metas](telas/15-metas.png) |

### Configuração do gestor

O gestor ajusta o CRM da própria clínica sem depender de suporte: sequências de
mensagens de cada coluna, horários de atendimento, etiquetas, campanhas, motivos
de perda e a foto e a cor de cada pessoa da equipe.

![Configuração](telas/16-configuracao.png)

### Agenda

A agenda da clínica por profissional, em dia, semana ou lista. Marcar uma consulta
move o card no funil, e o comparecimento volta para o CRM.

![Agenda do dia](telas/20-agenda-dia.png)

| Semana | Lista |
|---|---|
| ![Agenda da semana](telas/21-agenda-semana.png) | ![Próximas consultas](telas/22-agenda-lista.png) |

| Novo agendamento | Configurações da agenda |
|---|---|
| ![Novo agendamento](telas/23-agenda-agendar.png) | ![Configurações gerais](telas/27-agenda-configuracoes-geral.png) |

| Procedimentos | Profissionais |
|---|---|
| ![Procedimentos](telas/27-agenda-configuracoes-procedimentos.png) | ![Profissionais](telas/27-agenda-configuracoes-profissionais.png) |

### Pacientes

O cadastro do paciente junta consultas, indicadores, retornos, anamnese e
arquivos. A aba de retornos mostra quem está na hora de voltar.

| Pacientes | Ficha do paciente |
|---|---|
| ![Lista de pacientes](telas/24-pacientes.png) | ![Ficha do paciente](telas/25-ficha-do-paciente.png) |

| Retornos | Anamnese |
|---|---|
| ![Retornos](telas/26-retornos.png) | ![Anamnese](telas/27-agenda-configuracoes-anamnese.png) |

### Integrações e API

API pública documentada em OpenAPI, com chaves por clínica e webhooks de saída.
A página de teste deixa quem integra experimentar cada chamada antes de montar a
automação.

| Testar integração | Documentação da API |
|---|---|
| ![Testar integração](telas/30-integracao.png) | ![Documentação da API](telas/31-api-docs.png) |

### Setup

A tela da equipe que implanta o sistema: cria a clínica, liga os produtos, as
unidades, os painéis e as chaves da API.

| Entrada | Clínica no setup |
|---|---|
| ![Entrada do setup](telas/32-setup-entrada.png) | ![Setup da clínica](telas/33-setup.png) |

## Principais funcionalidades

- Dois painéis de funil (agendamento e fechamento) com arrastar e soltar, além de
  painéis personalizados por clínica.
- Origem do lead identificada pelo anúncio, com funil por campanha.
- Histórico completo de cada card: toda mudança vira um evento.
- Sequências de mensagens disparadas pela etapa do card.
- Agenda própria, com profissionais, cadeiras, procedimentos, bloqueios e
  expediente, ligada ao funil.
- Integração em mão dupla com o prontuário da clínica.
- Cadastro de pacientes com anamnese, retornos, arquivos e importação por planilha.
- Dashboard com metas por unidade e dias de trabalho.
- Várias clínicas e várias unidades no mesmo sistema, cada uma isolada da outra.
- API pública com contrato OpenAPI, chaves por clínica e webhooks.

## Como foi construído

Um app Next.js e um processo de tarefas em segundo plano, do mesmo código.

- **Clean Architecture por módulo:** domínio puro, casos de uso e adaptadores. Uma
  verificação automática barra qualquer dependência fora da regra.
- **Toda escrita passa por um caso de uso**, que grava a mudança, o evento de
  histórico e o webhook de saída na mesma transação.
- **API com um esquema só:** o mesmo esquema valida a chamada e gera a documentação.
- **Regras no banco testadas de verdade:** os testes aplicam as migrações reais em
  um Postgres que roda dentro do processo.
- **Decisões registradas:** mais de 30 registros de decisão de arquitetura (ADRs).

| Camada | Tecnologias |
|---|---|
| Telas | Next.js 16, React 19, Tailwind 4, shadcn/ui, TanStack Query, dnd-kit |
| API | Hono, zod, OpenAPI 3.1, Scalar |
| Dados | PostgreSQL, Drizzle, pg-boss |
| Qualidade | TypeScript, Vitest, ESLint, dependency-cruiser, CI no GitHub Actions |
| Entrega | Docker, nginx, VPS |
