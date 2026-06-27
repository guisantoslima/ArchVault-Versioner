# 🏛️ ArchVault Versioner

<p align="center">
  
  <img src="https://img.shields.io/badge/Git-2.40+-F05032?logo=git&logoColor=white" alt="Git 2.40+">
  
  <a href="https://github.com/guisantoslima/ArchVault-Versioner">
  <img src="https://img.shields.io/badge/GitHub-Repo-181717?logo=github&logoColor=white" alt="GitHub Repo">
  </a>
  
  <img src="https://img.shields.io/badge/Markdown-Docs-000000?logo=markdown&logoColor=white" alt="Markdown Docs">
  <img src="https://img.shields.io/badge/Conventional%20Commits-1.0.0-FE5196?logo=conventionalcommits&logoColor=white" alt="Conventional Commits">
  <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License MIT">
</p>

<p align="center">
  <strong>Versionamento inteligente de documentação arquitetural usando Git & GitHub como fonte única da verdade.</strong>
</p>

---

## 📑 Navegue pelo projeto

| 🚀 [Início rápido](#-início-rápido) | 🏗️ [Estrutura](#-estrutura-do-repositório) | 📋 [Workflow](#-workflow-de-versionamento) | 🤝 [Contribuição](#-como-contribuir) | 📜 [Changelog](#-changelog) |
|:-----------------------------------:|:------------------------------------------:|:-------------------------------------------:|:------------------------------------:|:---------------------------:|

---

## 🎯 Visão geral

O **ArchDoc Versioner** é um repositório modelo para organizar, rastrear e evoluir documentação arquitetural de software com o mesmo rigor usado para código-fonte, utilizando o poder de correlação e centralização de documentação da ferramenta Obsidian.

> *"Arquitetura que não é versionada é arquitetura que não existe."*

### ✨ Por que versionar documentação arquitetural?

- **Rastreabilidade**: saiba quem mudou o quê, quando e por quê.
- **Revisão colaborativa**: Pull Requests para decisões arquiteturais.
- **Histórico evolutivo**: entenda como a arquitetura mudou ao longo do tempo.
- **Fonte única da verdade**: diagramas, ADRs e glossário sempre sincronizados.
- **Integração com CI/CD**: valide links, diagramas e estilo automaticamente.

---

## 🚀 Início rápido

### 1. Clone o template

```bash
git clone https://github.com/guisantoslima/ArchVault-Versioner.git meu-projeto-arquitetura
cd meu-projeto-arquitetura
```

### 2. Configure seu ambiente

```bash
git config --local user.name "Seu Nome"
git config --local user.email "seu.email@example.com"
```

### 3. Crie sua primeira decisão arquitetural

```bash
git checkout -b feat/arch-001-escolha-banco-dados
cp docs/archs/_template.md docs/adrs/ARCH-001-escolha-banco-de-dados.md
# edite o arquivo...
git add .
git commit -m "feat(arch): adiciona ARCH-001 sobre escolha do banco de dados"
```

---

## 🏗️ Estrutura do repositório

```text
📦 archvault-versioner
├── 📁 docs/
│   ├── 📁 archs/                    # Architecture Decision Records
│   │   ├── ARCH-001-exemplo.md
│   │   └── _template.md
│   ├── 📁 diagramas/               # C4, UML, fluxos de dados
│   │   ├── c4-contexto.puml
│   │   └── c4-container.puml
│   ├── 📁 visoes/                  # Visões arquiteturais (lógica, física, etc.)
│   │   ├── visao-logica.md
│   │   └── visao-deploy.md
│   ├── 📁 glossario/               # Termos do domínio
│   │   └── termos.md
│   └── README.md                   # Guia de navegação dos docs
├── 📁 .github/
│   ├── 📁 workflows/               # CI/CD para docs
│   │   ├── links.yml
│   │   └── diagramas.yml
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── ISSUE_TEMPLATE/
│       ├── nova-decisao-arquitetural.md
│       └── revisao-de-documento.md
├── 📁 scripts/
│   ├── novo-adr.sh
│   └── verificar-links.sh
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE.md
└── README.md
```

---

## 🔄 Workflow de versionamento

```mermaid
graph LR
    A[Identifica necessidade<br/>de mudança] --> B{Cria novo ARCH<br/>ou atualiza doc?}
    B -->|Novo| C[Copia template<br/>de ARCH]
    B -->|Atualiza| D[Edita documento<br/>existente]
    C --> E[Abre branch<br/>feat/arch-NNN]
    D --> E
    E --> F[Commit com<br/>Conventional Commits]
    F --> G[Abre Pull Request]
    G --> H[Revisão por pares]
    H -->|Aprovado| I[Merge na main]
    H -->|Solicita ajustes| E
    I --> J[Atualiza CHANGELOG.md]
    J --> K[Tag semântica<br/>vX.Y.Z]
```

### 📌 Convenção de commits

Use **Conventional Commits** para manter o histórico legível e gerar changelog automaticamente.

| Tipo           | Quando usar                 | Exemplo                                                 |
| -------------- | --------------------------- | ------------------------------------------------------- |
| `feat(arch)`   | Nova decisão arquitetural   | `feat(adr): ADR-003 escolha de message broker`          |
| `docs(diag)`   | Novo ou atualizado diagrama | `docs(diag): atualiza C4 de containers`                 |
| `docs(visao)`  | Nova visão arquitetural     | `docs(visao): adiciona visão de segurança`              |
| `fix(gloss)`   | Correção no glossário       | `fix(gloss): corrige definição de saga`                 |
| `chore(tools)` | Ferramentas e scripts       | `chore(tools): adiciona script de verificação de links` |

### 🏷️ Versionamento semântico para documentação

Aplicamos versionamento semântico ao conteúdo do repositório:

| Versão  | Significado                           | Exemplo  |
| ------- | ------------------------------------- | -------- |
| `MAJOR` | Arquitetura completamente redesenhada | `v2.0.0` |
| `MINOR` | Nova ARCH, nova visão, novo diagrama  | `v1.3.0` |
| `PATCH` | Correções, revisões menores, typo     | `v1.3.2` |

---

## 📝 Templates prontos

### 🏛️ ADR — Architecture Decision Record

Acesse o template completo em [`docs/archs/_template.md`](_template.md) ou use o script:

```bash
./scripts/novo-arch.sh "escolha do message broker"
```

### 🖼️ Diagrama C4 — PlantUML

```plantuml
@startuml C4_Contexto
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

Person(usuario, "Usuário", "Usuário final do sistema")
System(sistema, "Sistema", "Plataforma de versionamento de docs")
System_Ext(github, "GitHub", "Repositório e revisão colaborativa")

Rel(usuario, sistema, "Acessa")
Rel(sistema, github, "Sincroniza docs")

SHOW_LEGEND()
@enduml
```

---

## 🤝 Como contribuir

1. Leia o [`CONTRIBUTING.md`](CONTRIBUTING.md).
2. Verifique se já existe uma [issue](https://github.com/seu-org/archdoc-versioner/issues) relacionada.
3. Crie uma branch a partir da `main`: `feat/arch-NNN-descricao-curta`.
4. Faça commits claros com Conventional Commits.
5. Abra um Pull Request usando o template fornecido.
6. Solicite revisão de pelo menos **1 arquiteto ou tech lead**.

### ✅ Checklist de Pull Request

- [ ] O documento segue o template correspondente.
- [ ] Links internos e externos foram verificados.
- [ ] Diagramas foram renderizados sem erros.
- [ ] O CHANGELOG.md foi atualizado.
- [ ] A versão semântica foi atualizada, se necessário.

---

## 📜 Changelog

Mudanças documentadas seguem o formato [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/).

```text
## [Unreleased]

## [1.0.0] - 2026-06-27
### Added
- Estrutura inicial do repositório.
- Template de ARCH.
- Workflows de CI para verificação de links e diagramas.
- Scripts auxiliares para criação de ARCHs.
```

Veja o histórico completo em [`CHANGELOG.md`](_changelog.md).

---

## 🛠️ Ferramentas recomendadas

| Ferramenta   | Uso                            | Link                                        |
| ------------ | ------------------------------ | ------------------------------------------- |
| Git          | Versionamento de código e docs | https://git-scm.com                         |
| GitHub       | Repositório, PRs e Issues      | https://github.com                          |
| PlantUML     | Diagramas C4 e UML             | https://plantuml.com                        |
| Mermaid      | Diagramas em Markdown          | https://mermaid-js.github.io                |
| Markdownlint | Padronização de Markdown       | https://github.com/DavidAnson/markdownlint  |
| Obsidian     | Anotações Markdown Link        | [https://obsidian.md](https://obsidian.md/) |

---

## 📚 Leitura recomendada

- 📖 [Documenting Software Architectures](https://www.amazon.com/Documenting-Software-Architectures-Views-Beyond/dp/0321552687)
- 📖 [Architecture Decision Records](https://adr.github.io/)
- 📖 [Conventional Commits](https://www.conventionalcommits.org/pt-br/v1.0.0/)
- 📖 [C4 Model](https://c4model.com/)

---

## 📄 Licença

Este projeto está licenciado sob a [Licença MIT](LICENSE).

---

<p align="center">
  Feito com ❤️ para arquitetos de software que acreditam em documentação viva.
</p>

<p align="center">
  <a href="#-archdoc-versioner">⬆️ Voltar ao topo</a>
</p>

---