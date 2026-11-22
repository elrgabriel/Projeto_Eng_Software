# Registo de Riscos do Projeto (E1 — Visão e Plano)
## Sistema Universitário de Gestão e Agendamento de Salas

---

## 1. Introdução e Metodologia

Este documento formaliza o processo contínuo de gestão de riscos do sistema de gestão e agendamento de salas universitárias, identificando e gerindo eventos incertos futuros com potencial para afetar os objetivos de prazo, orçamento, qualidade, segurança ou taxa de adoção do produto.

### 1.1. Escalas de Avaliação

#### Probabilidade
- **Baixa (1):** Improvável de ocorrer durante o ciclo de desenvolvimento e lançamento.
- **Média (2):** Evento plausível com probabilidade moderada de manifestação.
- **Alta (3):** Forte probabilidade de ocorrência sem a aplicação de medidas preventivas ativas.

#### Impacto
- **Baixo / Insignificante (1):** Desvios mínimos ou perturbações negligenciáveis, sem degradação das funções críticas de agendamento.
- **Moderado (2):** Degradação temporária de funcionalidades ou atrasos contornáveis no fluxo de trabalho.
- **Alto / Catastrófico (3):** Bloqueio de atividades letivas presenciais, conflitos graves de horários, brechas de segurança ou paragem crítica do sistema.

#### Matriz de Nível de Risco Global (Probabilidade × Impacto)

| Probabilidade \ Impacto | Baixo (1) | Moderado (2) | Alto (3) |
|:---|:---:|:---:|:---:|
| **Alta (3)** | Moderado (3) | Alto (6) | Alto / Crítico (9) |
| **Média (2)** | Baixo (2) | Moderado (4) | Alto (6) |
| **Baixa (1)** | Baixo (1) | Baixo (2) | Moderado (3) |

- **Baixo (1–2):** Risco aceitável com monitorização periódica regular.
- **Moderado (3–4):** Exige medidas preventivas ativas e plano de contingência preparado.
- **Alto (6–9):** Prioridade crítica de gestão; requer intervenção arquitetural ou de processo imediata.

---

### 1.2. Tipologia de Estratégias de Gestão de Risco
- **Evitar:** Eliminar a causa-raiz ou redesenhar a arquitetura e fluxos de negócio para mitigar o risco.
- **Mitigar:** Implementar ações preventivas para diminuir a probabilidade de ocorrência ou atenuar o impacto caso ocorra.
- **Transferir:** Alocar a responsabilidade operacional ou de suporte a serviços externos ou entidades terceiras.
- **Aceitar:** Reconhecer a possibilidade do risco e definir um plano de contingência acionável caso se justifique pelo custo/benefício.

---

### 1.3. Abordagem aos Riscos de Inteligência Artificial
O projeto enquadra o uso de Inteligência Artificial em duas vertentes complementares:
1. **IA no Produto (Assistente de Agendamento):** Tratado através de arquitetura determinística RAG com *Tool Calling/Function Calling*. A IA atua unicamente como interface de linguagem natural, enquanto a verificação de disponibilidade, regras de negócio e escrita na base de dados permanecem sob controlo exclusivo de APIs determinísticas.
2. **IA no Processo de Desenvolvimento:** Mitigado por políticas rigorosas de desenvolvimento seguro, *Code Review* obrigatório por pares humanos para qualquer código assistido por IA e validação automatizada em esteira de CI/CD (testes unitários, de integração e ferramentas SAST).

---

## 2. Matriz Resumo dos Riscos Identificados

| ID | Título do Risco | Categoria | Probabilidade | Impacto | Nível Global | Estratégia | Responsável |
|:---|:---|:---|:---:|:---:|:---:|:---:|:---|
| **R-01** | Concorrência e *race conditions* em reservas de última hora | Tecnológico / Concorrência | Alta | Alto | **Alto** | Evitar | Arquiteto de Software |
| **R-02** | Conflito de prioridades entre perfis (Docente vs. Aluno) | Regras de Negócio / Stakeholders | Média | Alto | **Alto** | Mitigar | Analista de Requisitos |
| **R-03** | Instabilidade ou dessincronização do ERP académico | Dependência Externa / Integração | Média | Alto | **Alto** | Mitigar | Engenheiro de Integração |
| **R-04** | Alucinações e reservas inválidas pelo assistente de IA | Risco de IA (Produto) | Média | Moderado | **Moderado** | Evitar | Engenheiro de IA / QA |
| **R-05** | Vulnerabilidades em código gerado por assistentes de IA | Risco de IA (Desenvolvimento) | Alta | Alto | **Alto** | Mitigar | Tech Lead / Qualidade |
| **R-06** | Inconsistência entre recursos registados e estado real das salas | Operacional / Integridade de Dados | Média | Moderado | **Moderado** | Mitigar | Administrador de Sistemas |
| **R-07** | Degradação de desempenho em picos sazonais de exames | Tecnológico / Desempenho | Alta | Moderado | **Alto** | Mitigar | Arquiteto de Software / DevOps |
| **R-08** | Resistência de secretarias e departamentos ao fluxo digital | Organizacional / Gestão da Mudança | Média | Moderado | **Moderado** | Mitigar | Gestor de Mudança / PO |

---

## 3. Fichas Detalhadas de Risco

### [R-01] Concorrência e *Race Conditions* em Reservas Simultâneas de Última Hora
- **Categoria:** Risco Tecnológico / Concorrência e Transações
- **Descrição:** Dois ou mais utilizadores (como delegados de turma ou docentes) tentarem reservar a mesma sala ou laboratório informático exatamente no mesmo segundo para um horário de elevada procura, resultando em sobreposição (*double-booking*) se o sistema não isolar adequadamente as transações concorrentes na base de dados.
- **Probabilidade:** Alta
- **Impacto:** Alto (Inviabiliza a utilização da sala e gera conflito institucional no local)
- **Nível de Risco:** **Alto**
- **Estratégia:** **Evitar**
- **Plano de Mitigação:**
  - **Ações Preventivas:** Utilização de mecanismos de bloqueio pessimista ou transações com isolamento *Serializable* na alocação do intervalo temporal; introdução de reserva temporária (*lease lock* de 3 minutos via Redis) enquanto o requerente preenche o formulário final.
  - **Plano de Contingência:** Mecanismo automático de desempate transacional que notifica de imediato o segundo utilizador com opções de salas equivalentes livres no mesmo pavilhão/bloco.
- **Responsável:** Arquiteto de Software / Lead Backend

---

### [R-02] Conflito de Prioridades e Sobreposição de Perfis (Docente vs. Aluno)
- **Categoria:** Conflito de Stakeholders / Regras de Negócio
- **Descrição:** Necessidade urgente de um docente ou da secretaria agendar uma aula de reposição ou avaliação intercalar numa sala previamente reservada por alunos para trabalho de grupo, gerando indefinição de autoridade e insatisfação entre membros da comunidade universitária.
- **Probabilidade:** Média
- **Impacto:** Alto
- **Nível de Risco:** **Alto**
- **Estratégia:** **Mitigar**
- **Plano de Mitigação:**
  - **Ações Preventivas:** Definição explícita de uma matriz RBAC (*Role-Based Access Control*) com níveis de precedência (Aulas Oficiais > Avaliações > Eventos Departamentais > Reservas de Estudantes) e prazos mínimos de cancelamento com aviso prévio (24 horas).
  - **Plano de Contingência:** Realocação algorítmica automática da reserva dos alunos para outra sala de capacidade idêntica, acompanhada de notificação imediata por e-mail e push notification com pedido de confirmação.
- **Responsável:** Analista de Requisitos / Gestor de Produto

---

### [R-03] Falhas de Sincronização ou Indisponibilidade do Sistema Académico Legado
- **Categoria:** Dependência Externa / Integração
- **Descrição:** A API de horários e matrículas da universidade apresentar instabilidade, alterações no esquema de dados ou lentidão extrema, provocando dessincronização entre as turmas oficiais curriculares e os blocos de horário disponíveis no sistema de salas.
- **Probabilidade:** Média
- **Impacto:** Alto
- **Nível de Risco:** **Alto**
- **Estratégia:** **Mitigar**
- **Plano de Mitigação:**
  - **Ações Preventivas:** Criação de camada de abstração com *Circuit Breaker*, cache local sincronizada via *jobs* agendados fora das horas de ponta e validação estrita de esquemas de dados com JSON Schema / Pydantic.
  - **Plano de Contingência:** Ativação automática de "Modo Resiliente Offline", informando no cabeçalho do sistema a data/hora do último catálogo sincronizado enquanto novas tentativas assíncronas são geridas em fila (*retry queue* com *backoff* exponencial).
- **Responsável:** Engenheiro de Integração / DevOps

---

### [R-04] Alucinações e Sugestões Inválidas do Assistente de Agendamento por IA
- **Categoria:** Risco de IA (Produto / Funcionalidade)
- **Descrição:** O assistente conversacional inteligente sugerir salas com capacidade insuficiente, ignorar restrições físicas (acessibilidade para pessoas com mobilidade reduzida, ausência de tomadas elétricas ou projetor) ou assumir horários fora do expediente do campus devido a alucinações de LLM ou ambiguidades no diálogo.
- **Probabilidade:** Média
- **Impacto:** Moderado
- **Nível de Risco:** **Moderado**
- **Estratégia:** **Evitar**
- **Plano de Mitigação:**
  - **Ações Preventivas:** O modelo de IA não toma decisões finais de reserva nem acede à base de dados para escrita; atua estritamente via *Function Calling*, limitando-se a extrair parâmetros (data, intervalo, recursos pretendidos). A validação das regras de negócio é 100% determinística através das APIs centrais.
  - **Plano de Contingência:** Obrigatoriedade de confirmação visual através de um ecrã de revisão (*Human-in-the-Loop*), onde o utilizador valida explicitamente a sala e o horário propostos antes de qualquer gravação no sistema.
- **Responsável:** Engenheiro de IA / Especialista em QA

---

### [R-05] Introdução de Falhas de Segurança e Lógica em Código Gerado por IA
- **Categoria:** Risco de IA (Processo de Desenvolvimento)
- **Descrição:** A utilização de ferramentas geradoras de código (como GitHub Copilot ou modelos generativos) durante o desenvolvimento introduzir vulnerabilidades de segurança (ex.: *IDOR*, *SQL Injection*, falhas de autenticação em rotas sensíveis ou validações deficientes de parâmetros de reserva).
- **Probabilidade:** Alta
- **Impacto:** Alto
- **Nível de Risco:** **Alto**
- **Estratégia:** **Mitigar**
- **Plano de Mitigação:**
  - **Ações Preventivas:** Política formal de revisão de código obrigatória (*Code Review* com aprovação de pelo menos um revisor sénior humano); integração no pipeline de CI/CD de ferramentas de análise estática e de segurança (SAST) como SonarQube ou Semgrep; exigência de cobertura mínima de testes unitários para toda a lógica de negócio.
  - **Plano de Contingência:** Auditorias regulares de segurança e execução de testes de penetração específicos focados nas rotas de autenticação, reservas e atribuição de permissões antes de cada marco de entrega (E2 e E3).
- **Responsável:** Tech Lead / Engenheiro de Qualidade de Software

---

### [R-06] Inconsistência entre Atributos Físicos Registados e Estado Real das Salas
- **Categoria:** Operacional / Integridade de Dados
- **Descrição:** Uma sala ser reservada assumindo a presença de equipamentos específicos (como projetor interativo, sistema de ar condicionado ou 30 computadores funcionais) e o utilizador deparar-se com equipamentos inoperacionais ou sala trancada para manutenção não registada.
- **Probabilidade:** Média
- **Impacto:** Moderado
- **Nível de Risco:** **Moderado**
- **Estratégia:** **Mitigar**
- **Plano de Mitigação:**
  - **Ações Preventivas:** Módulo administrativo rápido para equipas de suporte técnico e portaria sinalizarem avarias ou manutenções pontuais com desativação temporária dos recursos afetados; funcionalidade de *check-in* rápido via QR Code na entrada da sala.
  - **Plano de Contingência:** Ao ser reportada avaria crítica num recurso, o sistema envia um alerta prioritário a todos os utilizadores com reservas futuras dependentes dessa característica, sugerindo realocação proativa.
- **Responsável:** Administrador de Sistemas / Apoio ao Utilizador

---

### [R-07] Degradação de Desempenho e Indisponibilidade em Picos Sazonais
- **Categoria:** Tecnológico / Desempenho e Escalabilidade
- **Descrição:** Picos severos de acessos em simultâneo (ex.: primeiro dia de aulas de cada semestre ou abertura do agendamento de gabinetes de estudo para épocas de exames) provocarem latência excessiva, erros de *timeout* ou indisponibilidade da aplicação.
- **Probabilidade:** Alta
- **Impacto:** Moderado
- **Nível de Risco:** **Alto**
- **Estratégia:** **Mitigar**
- **Plano de Mitigação:**
  - **Ações Preventivas:** Criação de índices específicos nas tabelas de reservas por data, hora e sala; camada de *caching* distribuída (Redis) para a consulta de disponibilidade pública; realização de testes de stress automatizados (K6 ou JMeter) simulando 3× o tráfego estimado.
  - **Plano de Contingência:** Arquitetura com contentorização preparada para escalonamento horizontal e ativação de *rate limiting* e fila de espera virtual caso a taxa de requisições exceda a capacidade do nó principal.
- **Responsável:** Arquiteto de Software / DevOps

---

### [R-08] Resistência de Secretarias e Departamentos ao Novo Fluxo de Trabalho
- **Categoria:** Organizacional / Gestão da Mudança e Adoção
- **Descrição:** Funcionários das secretarias departamentais continuarem a registar marcações por canais informais (papel, e-mail direto ou telefone) sem registo centralizado, provocando desconfiança na fiabilidade do sistema e duplicidade de agendamentos no terreno.
- **Probabilidade:** Média
- **Impacto:** Moderado
- **Nível de Risco:** **Moderado**
- **Estratégia:** **Mitigar**
- **Plano de Mitigação:**
  - **Ações Preventivas:** Sessões práticas de demonstração focadas na redução de esforço burocrático; disponibilização de uma interface de balcão de atendimento com criação acelerada de reservas para as secretarias; diretiva institucional que define a plataforma como meio oficial único de agendamento.
  - **Plano de Contingência:** Designação de um facilitador de apoio presencial durante as primeiras duas semanas de adoção para suporte imediato às equipas de secretaria.
- **Responsável:** Gestor de Mudança / Product Owner