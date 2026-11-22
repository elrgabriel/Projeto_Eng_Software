# 1. Visão do Sistema

## 1.1 Contexto e Problema
Numa instituição de ensino superior, a gestão de salas e laboratórios é frequentemente descentralizada ou depende de processos manuais, 
gerando conflitos de agendamento, subutilização de espaços e dificuldade em identificar salas que cumpram requisitos específicos de equipamento 
(ex.: projetores, computadores ou bancadas de laboratório). 

Além disso, existe uma necessidade institucional clara de gerir prioridades de utilização: as atividades letivas ou reuniões de docentes têm precedência sobre sessões de 
estudo autónomo ou trabalhos de grupo de alunos. Atualmente, quando um docente necessita de uma sala já reservada por um estudante, a resolução do conflito é morosa e carece 
de um mecanismo automático de realocação e aviso atempado.

O objetivo deste sistema é centralizar a consulta e reserva de espaços académicos numa plataforma única, permitindo filtrar salas 
pelas suas características físicas e equipamentos disponíveis, gerir reservas de forma transparente e aplicar automaticamente regras de prioridade entre perfis de utilizador, 
notificando os afetados sempre que ocorra uma sobreposição.

## 1.2 Utilizadores do Sistema
O sistema destina-se a três perfis de utilizadores internos da instituição:

* **Alunos:** Consultam a disponibilidade dos espaços aplicando filtros (capacidade e equipamentos) e efetuam reservas para estudo ou trabalho académico, 
estando sujeitos à cedência do espaço caso um docente necessite da mesma sala no mesmo horário.
* **Professores (Docentes):** Consultam e reservam salas para aulas de substituição, orientações ou atividades académicas. Possuem prioridade sobre as reservas de alunos, 
podendo sobrepor-se a marcações existentes feitas por estudantes.
* **Administradores (Serviços Académicos / Gestão de Espaços):** Gerem o catálogo de salas da instituição 
(criação, edição de lotação e inventário de equipamentos) e supervisionam o estado geral das ocupações e regras de funcionamento.

## 1.3 O que fica fora do âmbito (Exclusões)
Para garantir que o sistema se mantém focado no núcleo essencial do problema (3 a 5 casos de uso executáveis de ponta a ponta) e é viável dentro do calendário do projeto, 
ficam explicitamente excluídos desta versão:

* **Integração com sistemas externos da instituição:** Não haverá sincronização automática com o sistema de horários letivos oficiais,
 nem autenticação federada (ex.: SSO/LDAP institucional), utilizando-se um mecanismo próprio de perfis fictícios.
* **Notificações por canais externos:** Os avisos aos alunos sobre alteração ou cancelamento de reserva por prioridade docente 
serão registados e apresentados dentro da própria aplicação (notificação interna), ficando excluído o envio de emails reais ou SMS.
* **Controlo físico de acessos e IoT:** O sistema gere apenas a reserva lógica da sala; não inclui integração com fechaduras eletrónicas, 
leitores de cartões de estudante ou sensores de presença.
* **Gestão de avarias e manutenção de equipamentos:** O administrador regista os equipamentos existentes na sala, 
mas o fluxo de reporte técnico de avarias ou pedidos de reparação ao Helpdesk fica fora do âmbito.
* **Pagamentos ou faturação:** Sendo um sistema interno académico, não contempla alugueres comerciais a entidades externas nem processamento financeiro.