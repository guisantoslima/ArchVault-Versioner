<!--
  📌 COMO USAR ESTE TEMPLATE

  1. Copie este arquivo para docs/adrs/ARCH-NNN-titulo-curto.md
     (NNN = próximo número sequencial disponível).
  2. Preencha todas as seções — não delete seções, apenas indique "N/A"
     quando genuinamente não se aplicar.
  3. Vincule diagramas em docs/diagramas/ e visões em docs/visoes/ por
     referência (link relativo), nunca duplique o conteúdo aqui.
  4. Este template segue a estrutura do TOGAF ADM (Architecture
     Development Method), adaptada para registrar decisões de mudança
     em processos de negócio. Ver TOGAF 9.1, Parte II, Capítulo 8
     (Phase B: Business Architecture) e Capítulo 17 (Requirements
     Management).
  5. Ao concluir, siga o fluxo do CONTRIBUTING.md: branch
     `feat/adr-NNN-descricao-curta`, commit Conventional Commits,
     atualização do CHANGELOG.md, e Pull Request com revisão de pelo
     menos 1 arquiteto ou tech lead.
-->

# ADR-NNN — [Título curto da decisão]

| Campo | Valor |
| --- | --- |
| **Status** | `Proposto` \| `Em revisão` \| `Aprovado` \| `Rejeitado` \| `Substituído` |
| **Fase ADM** | `B — Business Architecture` |
| **Versão do documento** | `0.1` |
| **Data** | AAAA-MM-DD |
| **Responsável (Architecture Owner)** | @usuario |
| **Stakeholders envolvidos** | Nome/papel — Nome/papel |
| **Solution Request / Card** | [SR-XXXX](link) |
| **Substitui / Substituído por** | ADR-XXX (se aplicável) |

---

## 1. 🎯 Contexto e Objetivos

> Baseado em TOGAF ADM — Fase B, Seção 8.1 (Objectives) e Seção 8.2 (Approach).

### 1.1 Problema / Motivação
<!-- Qual necessidade de negócio, dor operacional ou driver estratégico motiva esta mudança? -->



### 1.2 Objetivo da decisão
<!-- O que esta ADR busca resolver? Como ela apoia os objetivos de negócio e a Architecture Vision vigente? -->



### 1.3 Vínculo com a Visão Arquitetural
<!-- Link para o documento em docs/visoes/ que esta decisão refina ou implementa -->

- Visão relacionada: [`docs/visoes/...`](../visoes/)
- Architecture Vision / Statement of Architecture Work: [link]

---

## 2. 📥 Entradas (Inputs)

> Baseado em TOGAF ADM — Fase B, Seção 8.3 (Inputs).

| Tipo de entrada | Descrição | Fonte |
| --- | --- | --- |
| Request for Architecture Work | | |
| Princípios de negócio / drivers | | |
| Arquitetura Baseline (atual) | Processo(s) de negócio hoje | `docs/visoes/visao-logica.md` ou catálogo de processos |
| Arquitetura Target (desejada) | Versão preliminar, se já existir | |
| Restrições conhecidas | Regulatórias, orçamentárias, de prazo | |

---

## 3. 🔍 Decisão Arquitetural

### 3.1 Descrição da Baseline (estado atual)
<!-- Como o processo de negócio funciona hoje. Referencie diagramas existentes em vez de redesenhá-los aqui. -->

- Diagrama de processo atual: [`docs/diagramas/...`](../diagramas/)



### 3.2 Descrição do Target (estado proposto)
<!-- Como o processo deve funcionar após esta decisão. -->

- Diagrama de processo proposto: [`docs/diagramas/...`](../diagramas/)



### 3.3 Alternativas consideradas
<!-- TOGAF recomenda documentar as opções avaliadas e por que foram descartadas, garantindo rastreabilidade da decisão. -->

| Alternativa | Descrição | Motivo da rejeição |
| --- | --- | --- |
| A | | |
| B | | |

### 3.4 Decisão tomada
<!-- Declare explicitamente a decisão e a justificativa. -->



---

## 4. 📊 Gap Analysis

> Baseado em TOGAF ADM — Fase B, Seção 8.4.4 (Perform Gap Analysis) e Parte III, Capítulo 27.

| Elemento (processo, papel, regra, dado) | Existe na Baseline? | Existe no Target? | Gap identificado | Ação proposta |
| --- | --- | --- | --- | --- |
| | | | | |

---

## 5. 🧩 Artefatos Afetados

> Baseado em TOGAF ADM — Fase B, Seção 8.5 (Outputs): catálogos, matrizes e diagramas de Business Architecture.

### 5.1 Catálogos impactados
- [ ] Catálogo de Organização/Atores
- [ ] Catálogo de Papéis (Role catalog)
- [ ] Catálogo de Processo/Evento/Controle/Produto
- [ ] Catálogo de Serviços de Negócio

### 5.2 Matrizes impactadas
- [ ] Matriz de Interação de Negócio (Business Interaction matrix)
- [ ] Matriz Ator/Papel

### 5.3 Diagramas impactados (`docs/diagramas/`)
- [ ] Diagrama de Fluxo de Processo (Process Flow diagram)
- [ ] Business Footprint diagram
- [ ] Diagrama de Decomposição Funcional
- [ ] Outro: \_\_\_\_\_\_\_\_\_\_\_\_

### 5.4 Visões impactadas (`docs/visoes/`)
- [ ] Visão lógica
- [ ] Visão de processos de negócio

---

## 6. ⚖️ Consequências e Impactos

> Baseado em TOGAF ADM — Fase B, Seção 8.4.6 (Resolve Impacts Across the Architecture Landscape).

### 6.1 Impactos em outros domínios da arquitetura
| Domínio | Impacto | Observações |
| --- | --- | --- |
| Dados | | |
| Aplicações | | |
| Tecnologia | | |
| Segurança | | |

### 6.2 Trade-offs assumidos
<!-- O que se ganha e o que se perde com esta decisão? -->



### 6.3 Riscos e mitigação
| Risco | Probabilidade | Impacto | Mitigação |
| --- | --- | --- | --- |
| | | | |

---

## 7. 🗺️ Roadmap e Plano de Transição

> Baseado em TOGAF ADM — Fase E (Opportunities & Solutions) e Fase F (Migration Planning).

### 7.1 Componentes do roadmap
<!-- Pacotes de trabalho (work packages) necessários para sair da Baseline e chegar ao Target. -->

| Work Package | Descrição | Dependências | Arquitetura de Transição |
| --- | --- | --- | --- |
| | | | |

### 7.2 Vínculo com o roadmap do projeto
- Roadmap relacionado: [`docs/roadmaps/...`](../roadmaps/)
- Marco/release alvo: `vX.Y.0`

### 7.3 Plano de implementação e migração
<!-- Sequenciamento de alto nível: o que muda primeiro, o que depende do quê, critérios de prontidão. -->



---

## 8. ✅ Revisão e Governança

> Baseado em TOGAF ADM — Fase B, Seção 8.4.7 (Conduct Formal Stakeholder Review) e Fase G (Implementation Governance).

- [ ] Revisão técnica por arquiteto ou tech lead responsável.
- [ ] Validação com stakeholders de negócio impactados.
- [ ] Verificação de conformidade com princípios arquiteturais vigentes.
- [ ] Diagramas referenciados renderizam corretamente.
- [ ] `CHANGELOG.md` atualizado na seção `[Unreleased]`.

| Revisor | Papel | Parecer | Data |
| --- | --- | --- | --- |
| | | | |

---

## 9. 📚 Referências

<!-- Links para Solution Request, discussões, ADRs relacionadas, material externo. -->

- TOGAF® 9.1 — Part II: Architecture Development Method (ADM), Chapter 8 (Phase B: Business Architecture) e Chapter 17 (ADM Architecture Requirements Management).
- ADRs relacionadas: ADR-XXX
- Issue / Solution Request: [link]

---

## 10. 📝 Notas de Requirements Management

> Baseado em TOGAF ADM — Capítulo 17: a gestão de requisitos é contínua e atravessa todas as fases.

<!-- Novos requisitos identificados durante esta decisão, mas fora do escopo atual, devem ser registrados aqui e movidos para o backlog/Requirements Repository do projeto — não descartados. -->

| Requisito identificado | Dentro do escopo desta ADR? | Destino |
| --- | --- | --- |
| | Sim / Não | upstream / backlog |