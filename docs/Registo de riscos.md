# Registo de Riscos do Projeto (E1 ┬À Vis├úo e Plano)
## Sistema Universit├írio de Gest├úo e Agendamento de Salas

---

## 1. Introdu├º├úo e Metodologia

Este documento formaliza o processo cont├¡nuo de gest├úo de riscos do sistema de gest├úo e agendamento de salas universit├írias, identificando e gerindo eventos incertos futuros com potencial para afetar os objetivos de prazo, or├ºamento, qualidade, seguran├ºa ou taxa de ado├º├úo do produto.

### 1.1. Escalas de Avalia├º├úo

#### Probabilidade
* **Baixa (1):** Improv├ível de ocorrer durante o ciclo de desenvolvimento e lan├ºamento.
* **M├®dia (2):** Evento plaus├¡vel com probabilidade moderada de manifesta├º├úo.
* **Alta (3):** Forte probabilidade de ocorr├¬ncia sem a aplica├º├úo de medidas preventivas ativas.

#### Impacto
* **Baixo / Insignificante (1):** Desvios m├¡nimos ou perturba├º├Áes negligenci├íveis, sem degrada├º├úo das fun├º├Áes cr├¡ticas de agendamento.
* **Moderado (2):** Degrada├º├úo tempor├íria de funcionalidades ou atrasos contorn├íveis no fluxo de trabalho.
* **Alto / Catastr├│fico (3):** Bloqueio de atividades letivas presenciais, conflitos graves de hor├írios, brechas de seguran├ºa ou paragem cr├¡tica do sistema.

#### Matriz de N├¡vel de Risco Global (Probabilidade ├ù Impacto)

| Probabilidade \ Impacto | Baixo (1) | Moderado (2) | Alto (3) |
|:---|:---:|:---:|:---:|
| **Alta (3)** | Moderado (3) | Alto (6) | Alto / Cr├¡tico (9) |
| **M├®dia (2)** | Baixo (2) | Moderado (4) | Alto (6) |
| **Baixa (1)** | Baixo (1) | Baixo (2) | Moderado (3) |

* **Baixo (1ÔÇô2):** Risco aceit├ível com monitoriza├º├úo peri├│dica regular.
* **Moderado (3ÔÇô4):** Exige medidas preventivas ativas e plano de conting├¬ncia preparado.
* **Alto (6ÔÇô9):** Prioridade cr├¡tica de gest├úo; requer interven├º├úo arquitetural ou de processo imediata.

---

### 1.2. Tipologia de Estrat├®gias de Gest├úo de Risco
* **Evitar:** Eliminar a causa-raiz ou redesenhar a arquitetura e fluxos de neg├│cio para mitigar o risco.
* **Mitigar:** Implementar a├º├Áes preventivas para diminuir a probabilidade de ocorr├¬ncia ou atenuar o impacto caso ocorra.
* **Transferir:** Alocar a responsabilidade operacional ou de suporte a servi├ºos externos ou entidades terceiras.
* **Aceitar:** Reconhecer a possibilidade do risco e definir um plano de conting├¬ncia acion├ível caso se justifique pelo custo/benef├¡cio.

---

### 1.3. Abordagem aos Riscos de Intelig├¬ncia Artificial
O projeto enquadra o uso de Intelig├¬ncia Artificial em duas vertentes complementares:
1. **IA no Produto (Assistente de Agendamento):** Tratado atrav├®s de arquitetura determin├¡stica RAG com *Tool Calling/Function Calling*. A IA atua unicamente como interface de linguagem natural, enquanto a verifica├º├úo de disponibilidade, regras de neg├│cio e escrita na base de dados permanecem sob controlo exclusivo de APIs determin├¡sticas.
2. **IA no Processo de Desenvolvimento:** Mitigado por pol├¡ticas rigorosas de desenvolvimento seguro, *Code Review* obrigat├│rio por pares humanos para qualquer c├│digo assistido por IA e valida├º├úo automatizada em esteira de CI/CD (testes unit├írios, de integra├º├úo e ferramentas SAST).

---

## 2. Matriz Resumo dos Riscos Identificados

| ID | T├¡tulo do Risco | Categoria | Probabilidade | Impacto | N├¡vel Global | Estrat├®gia | Respons├ível |
|:---|:---|:---|:---:|:---:|:---:|:---:|:---|
| **R-01** | Concorr├¬ncia e *race conditions* em reservas de ├║ltima hora | Tecnol├│gico / Concorr├¬ncia | Alta | Alto | **Alto** | Evitar | Arquiteto de Software |
| **R-02** | Conflito de prioridades entre perfis (Docente vs. Aluno) | Regras de Neg├│cio / Stakeholders | M├®dia | Alto | **Alto** | Mitigar | Analista de Requisitos |
| **R-03** | Instabilidade ou dessincroniza├º├úo do ERP acad├®mico | Depend├¬ncia Externa / Integra├º├úo | M├®dia | Alto | **Alto** | Mitigar | Engenheiro de Integra├º├úo |
| **R-04** | Alucina├º├Áes e reservas inv├ílidas pelo assistente de IA | Risco de IA (Produto) | M├®dia | Moderado | **Moderado** | Evitar | Engenheiro de IA / QA |
| **R-05** | Vulnerabilidades em c├│digo gerado por assistentes de IA | Risco de IA (Desenvolvimento) | Alta | Alto | **Alto** | Mitigar | Tech Lead / Qualidade |
| **R-06** | Inconsist├¬ncia entre recursos registados e estado real das salas | Operacional / Integridade de Dados | M├®dia | Moderado | **Moderado** | Mitigar | Administrador de Sistemas |
| **R-07** | Degrada├º├úo de desempenho em picos sazonais de exames | Tecnol├│gico / Desempenho | Alta | Moderado | **Alto** | Mitigar | Arquiteto de Software / DevOps |
| **R-08** | Resist├¬ncia de secretarias e departamentos ao fluxo digital | Organizacional / Gest├úo da Mudan├ºa | M├®dia | Moderado | **Moderado** | Mitigar | Gestor de Mudan├ºa / PO |

---

## 3. Fichas Detalhadas de Risco

### [R-01] Concorr├¬ncia e *Race Conditions* em Reservas Simult├óneas de ├Ültima Hora
* **Categoria:** Risco Tecnol├│gico / Concorr├¬ncia e Transa├º├Áes
* **Descri├º├úo:** Dois ou mais utilizadores (como delegados de turma ou docentes) tentarem reservar a mesma sala ou laborat├│rio inform├ítico exatamente no mesmo segundo para um hor├írio de elevada procura, resultando em sobreposi├º├úo (*double-booking*) se o sistema n├úo isolar adequadamente as transa├º├Áes concorrentes na base de dados.
* **Probabilidade:** Alta
* **Impacto:** Alto (Inviabiliza a utiliza├º├úo da sala e gera conflito institucional no local)
* **N├¡vel de Risco:** **Alto**
* **Estrat├®gia:** **Evitar**
* **Plano de Mitiga├º├úo:**
  * **A├º├Áes Preventivas:** Utiliza├º├úo de mecanismos de bloqueio pessimista ou transa├º├Áes com isolamento *Serializable* na aloca├º├úo do intervalo temporal; introdu├º├úo de reserva tempor├íria (*lease lock* de 3 minutos via Redis) enquanto o requerente preenche o formul├írio final.
  * **Plano de Conting├¬ncia:** Mecanismo autom├ítico de desempate transacional que notifica de imediato o segundo utilizador com op├º├Áes de salas equivalentes livres no mesmo pavilh├úo/bloco.
* **Respons├ível:** Arquiteto de Software / Lead Backend

---

### [R-02] Conflito de Prioridades e Sobreposi├º├úo de Perfis (Docente vs. Aluno)
* **Categoria:** Conflito de Stakeholders / Regras de Neg├│cio
* **Descri├º├úo:** Necessidade urgente de um docente ou da secretaria agendar uma aula de reposi├º├úo ou avalia├º├úo intercalar numa sala previamente reservada por alunos para trabalho de grupo, gerando indefini├º├úo de autoridade e insatisfa├º├úo entre membros da comunidade universit├íria.
* **Probabilidade:** M├®dia
* **Impacto:** Alto
* **N├¡vel de Risco:** **Alto**
* **Estrat├®gia:** **Mitigar**
* **Plano de Mitiga├º├úo:**
  * **A├º├Áes Preventivas:** Defini├º├úo expl├¡cita de uma matriz RBAC (*Role-Based Access Control*) com n├¡veis de preced├¬ncia (Aulas Oficiais > Avalia├º├Áes > Eventos Departamentais > Reservas de Estudantes) e prazos m├¡nimos de cancelamento com aviso pr├®vio (24 horas).
  * **Plano de Conting├¬ncia:** Realoca├º├úo algor├¡tmica autom├ítica da reserva dos alunos para outra sala de capacidade id├¬ntica, acompanhada de notifica├º├úo imediata por e-mail e push notification com pedido de confirma├º├úo.
* **Respons├ível:** Analista de Requisitos / Gestor de Produto

---

### [R-03] Falhas de Sincroniza├º├úo ou Indisponibilidade do Sistema Acad├®mico Legado
* **Categoria:** Depend├¬ncia Externa / Integra├º├úo
* **Descri├º├úo:** A API de hor├írios e matr├¡culas da universidade apresentar instabilidade, altera├º├Áes no esquema de dados ou lentid├úo extrema, provocando dessincroniza├º├úo entre as turmas oficiais curriculares e os blocos de hor├írio dispon├¡veis no sistema de salas.
* **Probabilidade:** M├®dia
* **Impacto:** Alto
* **N├¡vel de Risco:** **Alto**
* **Estrat├®gia:** **Mitigar**
* **Plano de Mitiga├º├úo:**
  * **A├º├Áes Preventivas:** Cria├º├úo de camada de abstra├º├úo com *Circuit Breaker*, cache local sincronizada via *jobs* agendados fora das horas de ponta e valida├º├úo estrita de esquemas de dados com JSON Schema / Pydantic.
  * **Plano de Conting├¬ncia:** Ativa├º├úo autom├ítica de "Modo Resiliente Offline", informando no cabe├ºalho do sistema a data/hora do ├║ltimo cat├ílogo sincronizado enquanto novas tentativas ass├¡ncronas s├úo geridas em fila (*retry queue* com *backoff* exponencial).
* **Respons├ível:** Engenheiro de Integra├º├úo / DevOps

---

### [R-04] Alucina├º├Áes e Sugest├Áes Inv├ílidas do Assistente de Agendamento por IA
* **Categoria:** Risco de IA (Produto / Funcionalidade)
* **Descri├º├úo:** O assistente conversacional inteligente sugerir salas com capacidade insuficiente, ignorar restri├º├Áes f├¡sicas (acessibilidade para pessoas com mobilidade reduzida, aus├¬ncia de tomadas el├®tricas ou projetor) ou assumir hor├írios fora do expediente do campus devido a alucina├º├Áes de LLM ou ambiguidades no di├ílogo.
* **Probabilidade:** M├®dia
* **Impacto:** Moderado
* **N├¡vel de Risco:** **Moderado**
* **Estrat├®gia:** **Evitar**
* **Plano de Mitiga├º├úo:**
  * **A├º├Áes Preventivas:** O modelo de IA n├úo toma decis├Áes finais de reserva nem acede ├á base de dados para escrita; atua estritamente via *Function Calling*, limitando-se a extrair par├ómetros (data, intervalo, recursos pretendidos). A valida├º├úo das regras de neg├│cio ├® 100% determin├¡stica atrav├®s das APIs centrais.
  * **Plano de Conting├¬ncia:** Obrigatoriedade de confirma├º├úo visual atrav├®s de um ecr├ú de revis├úo (*Human-in-the-Loop*), onde o utilizador valida explicitamente a sala e o hor├írio propostos antes de qualquer grava├º├úo no sistema.
* **Respons├ível:** Engenheiro de IA / Especialista em QA

---

### [R-05] Introdu├º├úo de Falhas de Seguran├ºa e L├│gica em C├│digo Gerado por IA
* **Categoria:** Risco de IA (Processo de Desenvolvimento)
* **Descri├º├úo:** A utiliza├º├úo de ferramentas geradoras de c├│digo (como GitHub Copilot ou modelos generativos) durante o desenvolvimento introduzir vulnerabilidades de seguran├ºa (ex.: *IDOR*, *SQL Injection*, falhas de autentica├º├úo em rotas sens├¡veis ou valida├º├Áes deficientes de par├ómetros de reserva).
* **Probabilidade:** Alta
* **Impacto:** Alto
* **N├¡vel de Risco:** **Alto**
* **Estrat├®gia:** **Mitigar**
* **Plano de Mitiga├º├úo:**
  * **A├º├Áes Preventivas:** Pol├¡tica formal de revis├úo de c├│digo obrigat├│ria (*Code Review* com aprova├º├úo de pelo menos um revisor s├®nior humano); integra├º├úo no pipeline de CI/CD de ferramentas de an├ílise est├ítica e de seguran├ºa (SAST) como SonarQube ou Semgrep; exig├¬ncia de cobertura m├¡nima de testes unit├írios para toda a l├│gica de neg├│cio.
  * **Plano de Conting├¬ncia:** Auditorias regulares de seguran├ºa e execu├º├úo de testes de penetra├º├úo espec├¡ficos focados nas rotas de autentica├º├úo, reservas e atribui├º├úo de permiss├Áes antes de cada marco de entrega (E2 e E3).
* **Respons├ível:** Tech Lead / Engenheiro de Qualidade de Software

---

### [R-06] Inconsist├¬ncia entre Atributos F├¡sicos Registados e Estado Real das Salas
* **Categoria:** Operacional / Integridade de Dados
* **Descri├º├úo:** Uma sala ser reservada assumindo a presen├ºa de equipamentos espec├¡ficos (como projetor interativo, sistema de ar condicionado ou 30 computadores funcionais) e o utilizador deparar-se com equipamentos inoperacionais ou sala trancada para manuten├º├úo n├úo registada.
* **Probabilidade:** M├®dia
* **Impacto:** Moderado
* **N├¡vel de Risco:** **Moderado**
* **Estrat├®gia:** **Mitigar**
* **Plano de Mitiga├º├úo:**
  * **A├º├Áes Preventivas:** M├│dulo administrativo r├ípido para equipas de suporte t├®cnico e portaria sinalizarem avarias ou manuten├º├Áes pontuais com desativa├º├úo tempor├íria dos recursos afetados; funcionalidade de *check-in* r├ípido via QR Code na entrada da sala.
  * **Plano de Conting├¬ncia:** Ao ser reportada avaria cr├¡tica num recurso, o sistema envia um alerta priorit├írio a todos os utilizadores com reservas futuras dependentes dessa caracter├¡stica, sugerindo realoca├º├úo proativa.
* **Respons├ível:** Administrador de Sistemas / Apoio ao Utilizador

---

### [R-07] Degrada├º├úo de Desempenho e Indisponibilidade em Picos Sazonais
* **Categoria:** Tecnol├│gico / Desempenho e Escalabilidade
* **Descri├º├úo:** Picos severos de acessos em simult├óneo (ex.: primeiro dia de aulas de cada semestre ou abertura do agendamento de gabinetes de estudo para ├®pocas de exames) provocarem lat├¬ncia excessiva, erros de *timeout* ou indisponibilidade da aplica├º├úo.
* **Probabilidade:** Alta
* **Impacto:** Moderado
* **N├¡vel de Risco:** **Alto**
* **Estrat├®gia:** **Mitigar**
* **Plano de Mitiga├º├úo:**
  * **A├º├Áes Preventivas:** Cria├º├úo de ├¡ndices espec├¡ficos nas tabelas de reservas por data, hora e sala; camada de *caching* distribu├¡da (Redis) para a consulta de disponibilidade p├║blica; realiza├º├úo de testes de stress automatizados (K6 ou JMeter) simulando 3├ù o tr├ífego estimado.
  * **Plano de Conting├¬ncia:** Arquitetura com contentoriza├º├úo preparada para escalonamento horizontal e ativa├º├úo de *rate limiting* e fila de espera virtual caso a taxa de requisi├º├Áes exceda a capacidade do n├│ principal.
* **Respons├ível:** Arquiteto de Software / DevOps

---

### [R-08] Resist├¬ncia de Secretarias e Departamentos ao Novo Fluxo de Trabalho
* **Categoria:** Organizacional / Gest├úo da Mudan├ºa e Ado├º├úo
* **Descri├º├úo:** Funcion├írios das secretarias departamentais continuarem a registar marca├º├Áes por canais informais (papel, e-mail direto ou telefone) sem registo centralizado, provocando desconfian├ºa na fiabilidade do sistema e duplicidade de agendamentos no terreno.
* **Probabilidade:** M├®dia
* **Impacto:** Moderado
* **N├¡vel de Risco:** **Moderado**
* **Estrat├®gia:** **Mitigar**
* **Plano de Mitiga├º├úo:**
  * **A├º├Áes Preventivas:** Sess├Áes pr├íticas de demonstra├º├úo focadas na redu├º├úo de esfor├ºo burocr├ítico; disponibiliza├º├úo de uma interface de balc├úo de atendimento com cria├º├úo acelerada de reservas para as secretarias; diretiva institucional que define a plataforma como meio oficial ├║nico de agendamento.
  * **Plano de Conting├¬ncia:** Designa├º├úo de um facilitador de apoio presencial durante as primeiras duas semanas de ado├º├úo para suporte imediato ├ás equipas de secretaria.
* **Respons├ível:** Gestor de Mudan├ºa / Product Owner
