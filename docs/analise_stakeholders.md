# Análise de Stakeholders
 
Sistema web de reserva de salas da faculdade. Os stakeholders são fictícios, mas plausíveis.

## 1. Identificar os stakeholders
 
ID - Stakeholder - Tipo 

S1 - Serviços Académicos (gestão de salas) - Equipa interna
S2 - Direção da Faculdade - Cliente
S3 - Docentes - Utilizadores
S4 - Estudantes - Utilizadores / comunidade
S5 - Serviços Informáticos - Equipa de manutenção
S6 - Fornecedor de alojamento - Fornecedor

## 2. Avaliar influência e interesse
 
ID - Poder de decisão - Interesse - Impacto do sistema

S1 - Alto - Alto - Positivo: menos conflitos para resolver à mão.
S2 - Alto - Baixo - Positivo: melhor uso das salas.
S3 - Baixo - Alto - Positivo: reservas mais rápidas.
S4 - Baixo - Alto - Positivo ou negativo: mais acesso às salas, mas podem perder reservas para usos prioritários.
S5 - Alto - Baixo - Negativo: mais um sistema para manter.
S6 - Baixo - Baixo - Neutro.
 
## 3. Compreender expectativas e riscos
 
ID - Necessidades - Preocupações - Apoio ou resistência

S1 - Ver todas as reservas e resolver conflitos. - Reservas que nunca são usadas. - Apoio
S2 - Regras de prioridade claras. - Decisões vistas como injustas. - Apoio
S3 - Reservar rapidamente. - Perder a reserva sem aviso. - Apoio
S4 - Ver salas livres e cancelar facilmente. - Serem sempre preteridos. - Apoio com reservas
S5 - Sistema simples e seguro. - Código gerado por IA difícil de manter. - Possível resistência
S6 - Uso de recursos dentro do contratado. - Picos de utilização. - Neutro
 
Conflito previsível: os Estudantes (S4) reservam uma sala para estudo e os Serviços Académicos (S1) precisam dela para uma aula de 
reposição. 
Proposta: definir prioridades (exames, aulas, reuniões, eventos, estudo) e avisar quem perde a reserva, sugerindo outra sala.
 
## 4. Plano de envolvimento
 
Estratégia - Stakeholders - Formato - Frequência

Gerir de perto - S1 - Reuniões e validação de requisitos - Semanal
Manter satisfeito - S2, S5 - Relatórios e documentação - Mensal ou quando necessário
Manter informado - S3, S4 - Testes de utilização e avisos - A cada entrega
Monitorizar - S6 - Email - Quando necessário