# 🎬 Visão de Cenários (+1) — ShopSimples

> Parte do Modelo 4+1 de Visões Arquiteturais. Esta visão usa **cenários arquiteturais
> críticos** para validar e integrar as outras quatro visões (Lógica, Processos,
> Desenvolvimento e Física), funcionando como prova de consistência entre elas.

---

## 1. Objetivo

Demonstrar, por meio de cenários de ponta a ponta, que as decisões registradas nas
quatro visões funcionam de forma coerente quando combinadas — e servir de roteiro
para testes de aceitação e testes de carga.

---

## 2. Cenário crítico: Finalização de Compra

> Este é o cenário arquitetural mais importante do ShopSimples: envolve todas as
> camadas do sistema e várias preocupações não-funcionais simultaneamente.

### 2.1 Descrição do cenário

Um cliente com 3 itens no carrinho — incluindo um produto com apenas 1 unidade em
estoque — finaliza a compra pagando com cartão de crédito, durante um período de
pico de acessos (ex.: Black Friday).

### 2.2 Como cada visão é exercitada

| Visão | O que este cenário valida |
| --- | --- |
| **Lógica** | Regra de negócio "pedido só é criado se houver estoque suficiente" ([visão-logica.md](visao-logica.md)) é respeitada mesmo sob concorrência |
| **Processos** | Fluxo de Reserva de Estoque → Cobrança → Evento Assíncrono ([visão-processos.md](visao-processos.md)) se completa corretamente e dentro das metas de tempo |
| **Desenvolvimento** | A separação `controllers → services → repositories` ([visao-desenvolvimento.md](visao-desenvolvimento.md)) permite testar a regra de estoque isoladamente, sem subir a API completa |
| **Física** | A API escalada horizontalmente em 2+ réplicas no Kubernetes ([visao-fisica.md](visao-fisica.md)) responde dentro do SLA mesmo sob pico de acesso |

### 2.3 Critérios de aceitação (rastreáveis às visões)

- [ ] Se dois clientes tentam comprar a última unidade simultaneamente, apenas um pedido é confirmado (Visão de Processos — lock otimista).
- [ ] O tempo de resposta da finalização de compra permanece abaixo de 3 segundos no p95, mesmo com as réplicas da API sob carga (Visão Física — HPA).
- [ ] Se o Gateway de Pagamento falhar, o pedido fica em `AGUARDANDO_PAGAMENTO` e o estoque é liberado após timeout (Visão Lógica + Processos).
- [ ] O e-mail de confirmação é enviado de forma assíncrona, sem impactar o tempo de resposta percebido pelo cliente (Visão de Processos — fila RabbitMQ).

---

## 3. Cenário de evolução: Operador cadastra novo produto com variação de preço promocional

### 3.1 Descrição do cenário

O operador da loja cadastra um novo produto e define um preço promocional válido por
tempo limitado, que deve refletir imediatamente no catálogo visível ao cliente.

### 3.2 Como cada visão é exercitada

| Visão | O que este cenário valida |
| --- | --- |
| **Lógica** | Funcionalidade "Gestão de Catálogo" ([visao-logica.md](visao-logica.md)) permite criar produto com preço e período promocional |
| **Processos** | Atualização do catálogo não exige reprocessamento assíncrono — é uma escrita síncrona simples, sem impacto em pedidos já existentes |
| **Desenvolvimento** | A regra de "preço vigente" fica isolada em `services/catalogo.service.ts` ([visao-desenvolvimento.md](visao-desenvolvimento.md)), sem vazar para o controller |
| **Física** | Nenhuma infraestrutura adicional é necessária — reaproveita os mesmos pods da API ([visao-fisica.md](visao-fisica.md)) |

---

## 4. Matriz de rastreabilidade entre cenários e ADRs

| Cenário | ADR relacionada | Status |
| --- | --- | --- |
| Finalização de Compra | [`ARCH-001 — Fluxo de Finalização de Compra`](../adrs/ARCH-001-fluxo-finalizacao-compra.md) | Aprovado |
| Gestão de Catálogo com preço promocional | _A definir_ | Proposto |

---

## 5. Como usar esta visão

Sempre que uma nova ADR alterar um processo de negócio (ver
[`_template.md`](../adrs/_template.md)), recomenda-se:

1. Verificar se o cenário afetado já existe aqui — se não, adicioná-lo.
2. Validar que as quatro visões (Lógica, Processos, Desenvolvimento, Física) continuam
   coerentes entre si à luz do novo cenário.
3. Referenciar o cenário na seção "Revisão e Governança" da ADR correspondente.
