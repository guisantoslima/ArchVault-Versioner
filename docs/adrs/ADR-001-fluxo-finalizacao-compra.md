# ARCH-001 — Fluxo de Finalização de Compra com Reserva de Estoque e Pagamento Assíncrono

| Campo                                | Valor                                                         |
| ------------------------------------ | ------------------------------------------------------------- |
| **Status**                           | `Aprovado`                                                    |
| **Fase**                             | `Business Architecture`                                       |
| **Versão do documento**              | `1.0`                                                         |
| **Data**                             | 2026-05-12                                                    |
| **Responsável (Architecture Owner)** | @ana.arquiteta                                                |
| **Stakeholders envolvidos**          | João Lima (Product Owner) — Carla Souza (Operações/Logística) |
| **Solution Request / Card**          | [SR-2201](https://jira.shopsimples.com/browse/SR-2201)        |
| **Substitui / Substituído por**      | N/A                                                           |

---

## 1. 🎯 Contexto e Objetivos

> Baseado em TOGAF ADM — Fase B, Seção 8.1 (Objectives) e Seção 8.2 (Approach).

### 1.1 Problema / Motivação

O processo atual de finalização de compra do ShopSimples reserva o estoque **somente
depois** da confirmação do pagamento, de forma totalmente síncrona. Isso tem causado
dois problemas recorrentes: (1) clientes recebem erro de "produto indisponível" só
depois de já terem pago, exigindo reembolso manual; e (2) em picos de acesso, o tempo
de resposta da finalização de compra ultrapassa 8 segundos, gerando abandono de carrinho.

### 1.2 Objetivo da decisão

Redesenhar o processo de negócio de finalização de compra para que a reserva de
estoque ocorra **antes** da cobrança, e que etapas não essenciais à confirmação
imediata do pedido (envio de e-mail, emissão de nota fiscal) sejam processadas de
forma assíncrona — reduzindo o tempo percebido pelo cliente e eliminando reembolsos
por falta de estoque.

### 1.3 Vínculo com a Visão Arquitetural

- Visão relacionada: [`docs/visoes/visao-processos.md`](../visoes/visao-processos.md)
- Architecture Vision / Statement of Architecture Work: Visão de evolução do ShopSimples
  para suportar picos de tráfego sazonais (ex.: Black Friday) sem degradação de
  experiência do cliente.

---

## 2. 📥 Entradas (Inputs)

> Baseado em TOGAF ADM — Fase B, Seção 8.3 (Inputs).

| Tipo de entrada | Descrição | Fonte |
| --- | --- | --- |
| Request for Architecture Work | Reduzir tempo de finalização de compra e eliminar reembolsos por falta de estoque | SR-2201 |
| Princípios de negócio / drivers | "O cliente nunca deve pagar por um produto que não pode ser entregue" | Time de Produto |
| Arquitetura Baseline (atual) | Reserva de estoque ocorre após confirmação de pagamento (síncrono, sem fila) | `docs/visoes/visao-logica.md` |
| Arquitetura Target (desejada) | Reserva de estoque antes da cobrança; pós-processamento assíncrono via fila | Proposta desta ADR |
| Restrições conhecidas | Gateway de Pagamento externo não pode ser alterado (contrato vigente) | Operações |

---

## 3. 🔍 Decisão Arquitetural

### 3.1 Descrição da Baseline (estado atual)

No fluxo atual: o cliente finaliza a compra → a API chama o Gateway de Pagamento →
somente após a confirmação do pagamento a API verifica e debita o estoque → se não
houver estoque, o pedido é cancelado e o reembolso é solicitado manualmente pela
equipe de operações. Todo o fluxo, incluindo envio de e-mail e geração de nota fiscal,
é executado de forma síncrona na mesma requisição HTTP.

- Diagrama de processo atual: não documentado formalmente antes desta ADR (processo
  legado, mantido apenas em código).

### 3.2 Descrição do Target (estado proposto)

No fluxo proposto: a reserva de estoque passa a ocorrer **antes** da chamada ao
Gateway de Pagamento, dentro de uma transação no banco de dados. Após a confirmação
do pagamento, o pedido é persistido com status `PAGO` e um evento
`NovoPedidoConfirmado` é publicado em uma fila (RabbitMQ). Um worker consome esse
evento de forma assíncrona para emitir a nota fiscal e enviar o e-mail de confirmação,
sem impactar o tempo de resposta percebido pelo cliente.

- Diagrama de processo proposto: [`docs/diagramas/c2-container.puml`](../diagramas/c2-container.puml)
  e [`docs/diagramas/c3-component-api.puml`](../diagramas/c3-component-api.puml)
- Diagrama de sequência detalhado: [`docs/visoes/visao-processos.md`](../visoes/visao-processos.md)

### 3.3 Alternativas consideradas

| Alternativa | Descrição | Motivo da rejeição |
| --- | --- | --- |
| A — Manter fluxo síncrono, apenas otimizar timeout do Gateway | Reduzir apenas o tempo de timeout da chamada de pagamento | Não resolve o problema de reserva de estoque tardia; resolveria só parcialmente a lentidão |
| B — Reservar estoque por tempo limitado já na adição ao carrinho | Bloquear estoque assim que o item entra no carrinho | Geraria falsa indisponibilidade para outros clientes navegando, prejudicando conversão |
| C (escolhida) — Reservar estoque apenas na finalização, antes da cobrança, com pós-processamento assíncrono | Conforme descrito em 3.2 | Melhor equilíbrio entre experiência do cliente, consistência de estoque e desempenho |

### 3.4 Decisão tomada

Adotar a Alternativa C: reserva de estoque transacional na finalização da compra,
antes da cobrança, com pós-processamento assíncrono via fila para tarefas não
essenciais à confirmação imediata do pedido.

---

## 4. 📊 Gap Analysis

> Baseado em TOGAF ADM — Fase B, Seção 8.4.4 (Perform Gap Analysis) e Parte III, Capítulo 27.

| Elemento (processo, papel, regra, dado) | Existe na Baseline? | Existe no Target? | Gap identificado | Ação proposta |
| --- | --- | --- | --- | --- |
| Reserva de estoque antes da cobrança | Não | Sim | Estoque só era validado após pagamento | Implementar transação de reserva em `pedido.service.ts` antes da chamada ao Gateway |
| Processamento assíncrono pós-pedido | Não | Sim | E-mail e NF eram síncronos, aumentando latência | Introduzir fila RabbitMQ e worker consumidor |
| Liberação automática de estoque reservado não pago | Não | Sim | Reservas "presas" exigiam ação manual | Job de expiração de reserva após timeout configurável |

---

## 5. 🧩 Artefatos Afetados

> Baseado em TOGAF ADM — Fase B, Seção 8.5 (Outputs): catálogos, matrizes e diagramas de Business Architecture.

### 5.1 Catálogos impactados
- [ ] Catálogo de Organização/Atores
- [ ] Catálogo de Papéis (Role catalog)
- [x] Catálogo de Processo/Evento/Controle/Produto — processo "Finalização de Compra" redefinido
- [ ] Catálogo de Serviços de Negócio

### 5.2 Matrizes impactadas
- [ ] Matriz de Interação de Negócio (Business Interaction matrix)
- [ ] Matriz Ator/Papel

### 5.3 Diagramas impactados (`docs/diagramas/`)
- [x] Diagrama de Fluxo de Processo (Process Flow diagram) — ver Visão de Processos
- [ ] Business Footprint diagram
- [ ] Diagrama de Decomposição Funcional
- [x] Outro: Diagrama de Container (C2) e Componentes (C3) — inclusão da Fila de Processamento de Pedidos

### 5.4 Visões impactadas (`docs/visoes/`)
- [x] Visão lógica — regra de negócio "pedido só é criado com estoque suficiente" reforçada
- [x] Visão de processos de negócio — novo fluxo de sequência documentado
- [x] Outra: Visão Física (novo container RabbitMQ) e Visão de Cenários (cenário "Finalização de Compra")

---

## 6. ⚖️ Consequências e Impactos

> Baseado em TOGAF ADM — Fase B, Seção 8.4.6 (Resolve Impacts Across the Architecture Landscape).

### 6.1 Impactos em outros domínios da arquitetura

| Domínio | Impacto | Observações |
| --- | --- | --- |
| Dados | Nova coluna `status` com valor `AGUARDANDO_PAGAMENTO` e job de expiração de reserva | Requer migration no PostgreSQL |
| Aplicações | Novo módulo de mensageria (`messaging/`) na API | Ver Visão de Desenvolvimento |
| Tecnologia | Introdução do RabbitMQ como novo container de infraestrutura | Ver Visão Física |
| Segurança | Nenhum dado sensível adicional transita pela fila | Mensagens da fila não contêm dados de cartão |

### 6.2 Trade-offs assumidos

Ganha-se em desempenho percebido pelo cliente e em consistência de estoque, mas
adiciona-se complexidade operacional (um novo componente de infraestrutura — fila —
para monitorar e manter disponível).

### 6.3 Riscos e mitigação

| Risco | Probabilidade | Impacto | Mitigação |
| --- | --- | --- | --- |
| Fila indisponível impede notificação de pedidos | Baixa | Médio | Retry automático + alerta de monitoramento; pedido já está confirmado independente da fila |
| Reserva de estoque "presa" por falha no worker de expiração | Baixa | Médio | Job de expiração com idempotência e execução redundante |

---

## 7. 🗺️ Roadmap e Plano de Transição

> Baseado em TOGAF ADM — Fase E (Opportunities & Solutions) e Fase F (Migration Planning).

### 7.1 Componentes do roadmap

| Work Package | Descrição | Dependências | Arquitetura de Transição |
| --- | --- | --- | --- |
| WP1 — Infraestrutura de Mensageria | Provisionar RabbitMQ no cluster Kubernetes | Nenhuma | v1.1.0 |
| WP2 — Reserva de estoque transacional | Mover validação/reserva de estoque para antes da cobrança | WP1 | v1.1.0 |
| WP3 — Worker assíncrono | Consumidor de e-mail e nota fiscal | WP1, WP2 | v1.2.0 |
| WP4 — Job de expiração de reserva | Liberação automática de estoque não pago | WP2 | v1.2.0 |

### 7.2 Vínculo com o roadmap do projeto

- Roadmap relacionado: [`docs/roadmaps/roadmap-2026.md`](../roadmaps/roadmap-2026.md)
- Marco/release alvo: `v1.2.0`

### 7.3 Plano de implementação e migração

WP1 e WP2 podem ser implementados em paralelo, já que a infraestrutura de fila não
depende da lógica de reserva. WP3 depende de ambos estarem em produção. WP4 pode ser
entregue em paralelo ao WP3. Critério de prontidão para release: testes de carga
simulando pico de Black Friday sem erros de reembolso por falta de estoque.

---

## 8. ✅ Revisão e Governança

> Baseado em TOGAF ADM — Fase B, Seção 8.4.7 (Conduct Formal Stakeholder Review) e Fase G (Implementation Governance).

- [x] Revisão técnica por arquiteto ou tech lead responsável.
- [x] Validação com stakeholders de negócio impactados.
- [x] Verificação de conformidade com princípios arquiteturais vigentes.
- [x] Diagramas referenciados renderizam corretamente.
- [ ] `CHANGELOG.md` atualizado na seção `[Unreleased]`.

| Revisor | Papel | Parecer | Data |
| --- | --- | --- | --- |
| Pedro Tech Lead | Tech Lead | Aprovado | 2026-05-10 |
| Carla Souza | Operações/Logística | Aprovado, com ressalva sobre monitoramento da fila | 2026-05-11 |

---

## 9. 📚 Referências

- TOGAF® 9.1 — Part II: Architecture Development Method (ADM), Chapter 8 (Phase B: Business Architecture) e Chapter 17 (ADM Architecture Requirements Management).
- ADRs relacionadas: N/A (primeira ADR do projeto)
- Issue / Solution Request: [SR-2201](https://jira.shopsimples.com/browse/SR-2201)

---

## 10. 📝 Notas de Requirements Management

> Baseado em TOGAF ADM — Capítulo 17: a gestão de requisitos é contínua e atravessa todas as fases.

| Requisito identificado | Dentro do escopo desta ADR? | Destino |
| --- | --- | --- |
| Notificação por SMS além de e-mail | Não | Movido para backlog (SR-2245) |
| Dashboard de monitoramento da fila para operações | Não | Movido para backlog (SR-2246) |
