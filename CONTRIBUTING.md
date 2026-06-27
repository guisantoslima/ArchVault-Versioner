# 🤝 Guia de Contribuição — ArchVault Versioner

Obrigado por contribuir com o **ArchVault Versioner**! Este guia define como propor mudanças
em ADRs, diagramas, visões arquiteturais e demais artefatos do repositório, mantendo o mesmo
rigor de versionamento aplicado a código-fonte.

---

## 📑 Sumário

| 🚀 [Antes de começar](#-antes-de-começar) | 🌿 [Branches](#-convenção-de-branches) | 📝 [Commits](#-conventional-commits) | 🔁 [Pull Request](#-abrindo-um-pull-request) | ✅ [Checklist](#-checklist-de-revisão) | 🏷️ [Versionamento](#️-versionamento-semântico) |
| --- | --- | --- | --- | --- | --- |

---

## 🚀 Antes de começar

1. Verifique se já existe uma [issue](https://github.com/guisantoslima/ArchVault-Versioner/issues) aberta para a mudança que você quer propor.
2. Caso não exista, abra uma issue usando o template `nova-decisao-arquitetural.md` ou `revisao-de-documento.md`.
3. Aguarde um *sinal verde* de um arquiteto ou tech lead antes de iniciar mudanças estruturais grandes (ex.: nova visão arquitetural, reorganização de pastas).
4. Para correções pequenas (typo, link quebrado, formatação), pode abrir o Pull Request diretamente.

---

## 🌿 Convenção de branches

Use o padrão `tipo/escopo-descricao-curta`, alinhado aos tipos de commit:

| Branch | Quando usar | Exemplo |
| --- | --- | --- |
| `feat/adr-NNN-*` | Nova decisão arquitetural | `feat/adr-003-message-broker` |
| `docs/diag-*` | Novo ou atualizado diagrama | `docs/diag-c4-container` |
| `docs/visao-*` | Nova ou atualizada visão arquitetural | `docs/visao-seguranca` |
| `fix/gloss-*` | Correção no glossário ou conteúdo existente | `fix/gloss-definicao-saga` |
| `chore/tools-*` | Scripts, automações e ferramentas | `chore/tools-script-links` |

---

## 📝 Conventional Commits

Todos os commits devem seguir [Conventional Commits](https://www.conventionalcommits.org/pt-br/v1.0.0/),
conforme já definido no README do projeto:

```
<tipo>(<escopo>): <descrição curta no imperativo>
```

| Tipo | Escopo sugerido | Exemplo |
| --- | --- | --- |
| `feat` | `adr` | `feat(adr): adiciona ADR-003 sobre escolha de message broker` |
| `docs` | `diag`, `visao` | `docs(diag): atualiza C4 de containers` |
| `fix` | `gloss`, `adr`, `visao` | `fix(gloss): corrige definição de saga` |
| `chore` | `tools`, `ci` | `chore(tools): adiciona script de verificação de links` |
| `refactor` | `docs` | `refactor(docs): reorganiza seções da visão lógica` |

> 💡 Commits pequenos e atômicos facilitam a revisão e a geração automática do changelog.

---

## 🔁 Abrindo um Pull Request

1. Crie a branch a partir da `main`, seguindo a [convenção de branches](#-convenção-de-branches).
2. Faça commits seguindo [Conventional Commits](#-conventional-commits).
3. Adicione uma linha na tabela **`[Unreleased]`** do `CHANGELOG.md`, com Versão, Data, Solution Request, Feature, Branch, Responsável e o link do PR.
4. Abra o Pull Request usando o template em `.github/PULL_REQUEST_TEMPLATE.md`.
5. Solicite revisão de pelo menos **1 arquiteto ou tech lead**.
6. Responda aos comentários da revisão na própria branch — evite *force push* depois que a revisão já começou.

---

## ✅ Checklist de revisão

Antes de solicitar revisão, confirme que:

- [ ] O documento segue o template correspondente (`_template.md`, ADR, visão, etc.).
- [ ] Links internos e externos foram verificados.
- [ ] Diagramas (PlantUML/Mermaid) foram renderizados sem erros.
- [ ] O `CHANGELOG.md` foi atualizado na seção `[Unreleased]`.
- [ ] A versão semântica foi avaliada (ver seção abaixo) e ajustada, se necessário.
- [ ] O commit segue o padrão Conventional Commits.

---

## 🏷️ Versionamento semântico

Mudanças no conteúdo do repositório seguem a mesma lógica de **MAJOR.MINOR.PATCH** já definida no README:

| Versão | Significado | Exemplo |
| --- | --- | --- |
| `MAJOR` | Arquitetura completamente redesenhada / mudança que quebra referências existentes | `v2.0.0` |
| `MINOR` | Nova ADR, nova visão, novo diagrama | `v1.3.0` |
| `PATCH` | Correções, revisões menores, typo | `v1.3.2` |

Quem decide o tipo de versão é o revisor responsável pelo merge, com base no impacto da mudança.

---

## 🙋 Dúvidas?

Abra uma [issue](https://github.com/guisantoslima/ArchVault-Versioner/issues) com a tag `question`
ou inicie uma conversa na aba [Discussions](https://github.com/guisantoslima/ArchVault-Versioner/discussions) do repositório, se disponível.

Toda contribuição — grande ou pequena — ajuda a manter a documentação arquitetural viva. 🏛️