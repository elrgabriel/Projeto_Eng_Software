# 4. Modelo de processo escolhido e justifica├º├úo

**Projeto:** Reserva de salas da Faculdade
**Entrega:** E1 ┬À Vis├úo e plano (ficheiro sugerido: `docs/processo.md`)

---

## 4.1 Decis├úo

**Modelo escolhido: iterativo e incremental, gerido com Scrum e com pr├íticas t├®cnicas de XP.**

| Camada | O que usamos | Para qu├¬ no nosso projeto |
|---|---|---|
| Modelo de processo | Iterativo e incremental | Entregar o sistema por partes e corrigir o rumo com feedback a cada ciclo |
| Framework de gest├úo | Scrum (Sprints, Product Backlog, Sprint Review, Retrospetiva) | Organizar o trabalho do grupo e tornar vis├¡vel o progresso |
| Pr├íticas t├®cnicas | TDD, revis├úo por pares, integra├º├úo cont├¡nua (XP) | Garantir que a l├│gica de conflitos e prioridades est├í correta |



---

## 4.2 Porqu├¬ este modelo: o contexto do nosso projeto

A escolha parte das tr├¬s dificuldades principais do dom├¡nio: **conflitos de hor├írio, prioridades e cancelamentos**.

### a) As regras de prioridade ainda n├úo s├úo conhecidas
O sistema s├│ funciona se soubermos quem ganha quando dois pedidos disputam a mesma sala (aula curricular, exame, reuni├úo, evento de associa├º├úo de estudantes). Essas regras v├úo ser negociadas com stakeholders que t├¬m interesses opostos. Por exemplo, os docentes querem reservar com meses de anteced├¬ncia e os servi├ºos querem libertar as salas ├á semana. Este conflito est├í identificado na an├ílise de stakeholders (ponto 2).

Requisitos que s├│ se compreendem a negociar n├úo podem ficar fechados num documento no in├¡cio. Num processo iterativo, a primeira vers├úo serve para **validar** esses requisitos (a aula 3 refere isto expressamente), e cada Sprint Review permite ajustar a ordem de prioridade com quem decide.

### b) A parte mais dif├¡cil tecnicamente ├® a que mais vai mudar
Detetar conflitos de hor├írio (sobreposi├º├Áes, salas com capacidade insuficiente, reservas recorrentes) e gerir cancelamentos (libertar a sala, reatribuir a quem estava em espera) ├® a l├│gica central do sistema. Erros aqui tornam o sistema in├║til: uma sala reservada duas vezes ├® pior do que n├úo ter sistema.

Com ciclos curtos, esta l├│gica ├® constru├¡da e testada **logo no primeiro Sprint**, e n├úo s├│ no fim do semestre. O erro mais caro ├® descoberto cedo, quando ainda ├® barato corrigi-lo.

### c) Separar funcionalidade de pol├¡tica de neg├│cio
Seguimos o princ├¡pio da aula 4: "cancelar" ├® uma funcionalidade; "s├│ se pode cancelar at├® X horas antes" ou "uma aula curricular prevalece sobre um evento" s├úo **regras de neg├│cio** que podem mudar sem mudar a funcionalidade. Num modelo iterativo, cada regra entra no backlog como item pr├│prio, com crit├®rios de aceita├º├úo test├íveis, e pode ser alterada num Sprint seguinte sem refazer o sistema.

### d) Os utilizadores existem e podem validar
Alunos, docentes e servi├ºos acad├®micos s├úo utilizadores reais e acess├¡veis. Um modelo iterativo exige um cliente dispon├¡vel a cada ciclo, e neste dom├¡nio isso ├® poss├¡vel: cada Sprint Review pode ser feita com um representante de cada grupo.

### e) O prazo ├® um semestre e a equipa ├® pequena e inexperiente
Temos um prazo fixo e curto, e uma equipa de estudantes. Isto favorece **├ómbito vari├ível** (a caracter├¡stica ├ígil de fixar prazo e custo e ajustar o que se entrega): se faltar tempo, adiamos os itens *Could* e nunca a qualidade nem a funcionalidade *Must*. Ao fim de cada Sprint existe sempre uma vers├úo utiliz├ível, o que protege o grupo de chegar ao fim com um sistema incompleto.

### f) Uso de IA no desenvolvimento
O grupo usa IA para gerar c├│digo e rascunhos de hist├│rias. A aula 4 alerta que, quando gerar c├│digo fica barato, **verificar** passa a ser o passo cr├¡tico. Por isso a nossa Definition of Done inclui: c├│digo gerado por IA lido e revisto por uma pessoa, testes autom├íticos a passar e pull request aprovado por outro membro. O TDD funciona como barreira: o teste ├® escrito por quem conhece a regra (por exemplo, "duas reservas n├úo podem sobrepor-se") e valida o que a IA gerou. Isto responde diretamente aos riscos ligados ├á IA do registo de riscos (ponto 3).

---

## 4.3 Plano de incrementos (como o modelo se aplica ao projeto)

A ordem dos Sprints segue o **risco e o valor**: primeiro o que ├® mais dif├¡cil e mais essencial.

| Incremento | Foco | Porqu├¬ nesta ordem |
|---|---|---|
| Sprint 0 | Prot├│tipo de baixa fidelidade (wireframes) dos ecr├ús de reserva e de conflito; valida├º├úo das regras de prioridade com stakeholders | Os requisitos de prioridade est├úo mal compreendidos; um prot├│tipo descart├ível clarifica antes de programar |
| Sprint 1 | Criar reserva + **dete├º├úo de conflitos de hor├írio** | ├ë o n├║cleo do sistema e o maior risco t├®cnico |
| Sprint 2 | **Cancelamentos** e liberta├º├úo da sala | Depende de j├í existirem reservas e conflitos bem definidos |
| Sprint 3 | **Regras de prioridade** entre tipos de utilizador/atividade | Precisa de feedback real dos Sprints 1 e 2 para afinar as regras |
| Sprint 4+ | Notifica├º├Áes, consulta de disponibilidade, melhorias de interface | Valor adicional (*Should*/*Could*); entra se houver capacidade |

Em cada Sprint, o ecr├ú e as regras j├í entregues podem ser **refeitos** com base no feedback (a vertente iterativa), enquanto se acrescentam funcionalidades novas (a vertente incremental).

---

## 4.4 Compara├º├úo com alternativas

### Alternativa 1 ┬À Cascata (e cascata revisto)

| Aspeto | Cascata | O que isto significa para o nosso projeto |
|---|---|---|
| Requisitos | Fechados e assinados no in├¡cio | Ter├¡amos de fixar as regras de prioridade antes de falarmos com quem as decide |
| Feedback | S├│ no fim, quando o software existe | Descobrir├¡amos que a dete├º├úo de conflitos falha na ├║ltima semana |
| Mudan├ºa | Dif├¡cil de acomodar | Qualquer altera├º├úo ├á pol├¡tica de cancelamentos obrigaria a rever fases anteriores |
| Documenta├º├úo | Extensa | ├Ütil, mas excessiva para um grupo pequeno e um prazo de um semestre |

**Porque a rejeit├ímos:** o cascata ├® adequado a requisitos est├íveis e a contextos regulados, e o nosso dom├¡nio tem o oposto: prioridades por negociar e interesses em conflito. O cascata revisto atenua o problema, mas continua a deixar o cliente sem ver software a funcionar at├® ao fim.
**O que aproveitamos:** a disciplina de documentar o essencial (vis├úo, riscos, regras de neg├│cio escritas e rastre├íveis).

### Alternativa 2 ┬À Espiral (Boehm)

| Aspeto | Espiral | O que isto significa para o nosso projeto |
|---|---|---|
| Gest├úo de risco | An├ílise formal de risco em cada volta | Adequada em teoria, dado que temos riscos reais |
| Custo e estrutura | Custo inicial elevado; exige gest├úo de risco experiente | A equipa ├® de estudantes, sem experi├¬ncia em an├ílise formal de risco |
| Dimens├úo do projeto | Pouco eficaz em projetos pequenos | Teria demasiada estrutura para o nosso ├ómbito |

**Porque a rejeit├ímos:** ├® indicada para projetos longos, caros e de alto risco (a aula usa o controlo de tr├ífego a├®reo como exemplo). O nosso risco ├® real mas **ger├¡vel**, e o custo do processo seria desproporcionado.
**O que aproveitamos:** a ideia de come├ºar cada ciclo por rever os riscos. Fazemo-lo de forma leve, revendo o registo de riscos em cada Sprint Planning e Retrospetiva.

### Alternativa 3 ┬À RUP

| Aspeto | RUP | O que isto significa para o nosso projeto |
|---|---|---|
| Estrutura | 4 fases, 9 disciplinas, muitos pap├®is formais | Pesado para uma equipa pequena |
| Documenta├º├úo | Extensa e rastre├ível | Consumiria tempo que precisamos para construir e testar |
| Indicado para | Projetos grandes e complexos, com grandes equipas | N├úo ├® o nosso caso |

**Porque a rejeit├ímos:** n├úo ├® ├ígil e ├® desenhado para outra escala de equipa e de projeto.

### Quadro-resumo

| Crit├®rio | Cascata | Espiral | RUP | **Iterativo + Scrum/XP** |
|---|---|---|---|---|
| Lida bem com regras de prioridade ainda por definir | N├úo | Sim | Parcialmente | **Sim** |
| Feedback frequente de utilizadores reais | N├úo (s├│ no fim) | Sim | Sim | **Sim (cada Sprint Review)** |
| Adequado a equipa pequena e prazo de um semestre | Parcialmente | N├úo | N├úo | **Sim** |
| Protege contra falhas no n├║cleo (conflitos de hor├írio) | Tarde demais | Sim, mas com custo elevado | Sim, mas com muito formalismo | **Sim, logo no Sprint 1** |
| Permite reduzir ├ómbito sem perder qualidade | Dif├¡cil | Dif├¡cil | Dif├¡cil | **Sim (prioridade MoSCoW)** |
| Integra verifica├º├úo do c├│digo gerado por IA | S├│ na fase de testes | Dependente da volta | Dependente da fase | **Sim (DoD + TDD a cada Sprint)** |

---

## 4.5 Riscos desta escolha e como os gerimos

| Risco do modelo | Mitiga├º├úo |
|---|---|
| Dif├¡cil estimar custo e prazo totais no in├¡cio | Prioridade MoSCoW: fixamos o prazo e ajustamos o ├ómbito; os itens *Won't* ficam registados |
| Arquitetura degradar-se a cada incremento | Refactoring frequente e revis├úo por pares; decis├Áes de arquitetura discutidas em equipa |
| Exige cliente/stakeholder dispon├¡vel em cada ciclo | Definir ├á partida um representante por grupo de utilizadores e fixar as datas das Sprint Reviews |
| Mudan├ºa descontrolada das regras de prioridade | Mudan├ºas entram no Product Backlog atrav├®s do Product Owner e s├│ se aplicam a Sprints futuros, nunca a meio de um Sprint |

---

## 4.6 Como saberemos que o processo est├í a funcionar

- Cada Sprint termina com um **incremento utiliz├ível** que cumpre a Definition of Done.
- Em cada Sprint Review demonstramos **software a funcionar** (n├úo slides) a pelo menos um stakeholder.
- Em cada Retrospetiva registamos pelo menos **uma a├º├úo concreta** de melhoria para o Sprint seguinte.
- Se uma regra de prioridade ou de cancelamento mudar, confirmamos que o impacto ficou limitado a **um item do backlog** e n├úo a uma reescrita (sinal de que separ├ímos bem funcionalidade e pol├¡tica).

---

## Nota final

N├úo existe um modelo "melhor" em abstrato: escolhemos este porque as tr├¬s dificuldades do nosso dom├¡nio (conflitos de hor├írio, prioridades e cancelamentos) dependem de regras que s├│ se estabilizam com feedback, e porque o n├║cleo t├®cnico precisa de ser testado cedo. Se o contexto mudasse (por exemplo, se a Faculdade impusesse regras fixas e documenta├º├úo formal), reavaliar├¡amos a escolha.
