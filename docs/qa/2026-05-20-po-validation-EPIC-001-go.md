# @po Validation — EPIC-001 (VM→PaaS Workshop) — GO transition

> **Validator:** Pax (@po) · **Date:** 2026-05-20 · **Trigger:** owner aprovou avanço pós-freshness audit 2026-05-15
> **Scope:** Stories 1.1, 1.2, 1.3, 1.4 — todas Draft → Ready
> **Previous audit:** [docs/qa/2026-05-15-po-validation-EPIC-001.md](2026-05-15-po-validation-EPIC-001.md)

---

## Veredito consolidado

| Story | Score | Verdict | Status anterior | Status novo |
|---|---|---|---|---|
| 1.1 — Deploy inicial em 3 VMs | **9/10** | ✅ GO | Draft | **Ready** |
| 1.2 — Migrar Backend para Web App | **9/10** | ✅ GO | Draft | **Ready** |
| 1.3 — Migrar Frontend para Web App | **9/10** | ✅ GO | Draft | **Ready** |
| 1.4 — Migrar SQL Server para Azure SQL | **9/10** | ✅ GO | Draft | **Ready** |

**Confidence Level:** High em todas. Diferença de 1 ponto (9 vs 10) = gaps estruturais mecânicos (não conteúdo) que foram **corrigidos nesta sessão**.

---

## Checklist 10-pontos (cada story)

| # | Critério | 1.1 | 1.2 | 1.3 | 1.4 |
|---|---|:-:|:-:|:-:|:-:|
| 1 | Título claro e objetivo | ✅ | ✅ | ✅ | ✅ |
| 2 | Story bem formada (As a/I want/So that) | ✅ | ✅ | ✅ | ✅ |
| 3 | AC testáveis | ✅ | ✅ | ✅ | ✅ |
| 4 | Tasks executáveis | ✅ 32 | ✅ ~15 | ✅ ~12 | ✅ ~18 |
| 5 | Dependências mapeadas | ✅ | ✅ | ✅ | ✅ |
| 6 | Estimativa de complexidade | ✅ 45min | ✅ 30min | ✅ 30min | ✅ 45min |
| 7 | Valor de negócio explícito | ✅ | ✅ | ✅ | ✅ |
| 8 | Riscos | ⚠️ Epic | ⚠️ Epic | ⚠️ Epic | ⚠️ Epic |
| 9 | DoD / smoke claros | ✅ | ✅ | ✅ | ✅ |
| 10 | Alinhamento c/ Epic | ✅ | ✅ | ✅ | ✅ |

Legenda: ⚠️ Epic = riscos documentados no `EPIC-001`, não duplicados na story (aceitável).

## Anti-Hallucination (Art. IV — No Invention)

- ✅ Contagens (104 jogos / 49 seleções / 17 estádios) verificadas contra prod live + Story 0.10.
- ✅ Todos os artefatos referenciados existem em disco (5/5 para 1.1, +6 para 1.2-1.4): `DEPLOY_IIS_SIMPLIFICADO.md`, `.env.example`, `schema.sql`, `bacpac`, `DEPLOY.md`, `web-app-backend.bicep`, `web-app-frontend.bicep`, `sql-database.bicep`, `provision.sh`, `deploy-backend.yml`, `deploy-frontend.yml`.
- ✅ Sem invenção: passos rastreiam aos roteiros existentes (Lovable/DEPLOY_IIS_SIMPLIFICADO.md, infra/main.bicep).
- 📌 **Owner-owned (FORA do escopo do agente, decisão owner 2026-05-15):** regeneração do bacpac + verificação do login admin = manual no final, se/quando o owner rodar o dry-run.

## Fixes estruturais aplicados nesta sessão

Cada story tinha 2 gaps mecânicos do template original (não-conteúdo):

1. **Change Log section ausente** — bloqueava Step 12 pre-check do `validate-next-story.md`.
2. **🤖 CodeRabbit Integration section ausente** — `coderabbit_integration.enabled: true` em `core-config.yaml` exige a seção mesmo que N/A.

**Resolução (idêntica nas 4 stories):**
- Anexada seção `🤖 CodeRabbit Integration` com nota **N/A — story instrucional** (justificada pela frase do Epic: *"@dev implementa só se algo do app precisar mudar (não esperado)"*).
- Anexada seção `Change Log` com 3 entradas (draft → freshness audit → GO).

## Issues remanescentes (não-bloqueantes)

| ID | Severidade | Descrição | Ação |
|---|---|---|---|
| S-1 | Nice-to-have | Riscos não duplicados dentro das stories (estão no Epic) | Manter como está — duplicação seria DRY violation |
| S-2 | Nice-to-have | Tasks/Subtasks não linkam checkboxes a ACs específicos | Pode melhorar rastreabilidade em PR review; opcional |
| S-3 | Owner-owned | Bacpac stale + admin login não verificáveis sem DB | Owner trata no dry-run; runbook em [audit anterior](2026-05-15-po-validation-EPIC-001.md) |

---

## Próximos passos

| # | Quem | Ação |
|---|---|---|
| 1 | Owner | Decidir quando rodar o dry-run do workshop (regenera bacpac antes, à mão) |
| 2 | @dev | **Opcional** — só se o owner identificar algo do app que precisa mudar para a didática funcionar. Não esperado pelo Epic. |
| 3 | Aluno (no evento) | Executa as 4 stories sequencialmente; cada uma tem rollback independente |
| 4 | Owner / @qa | Pós-dry-run: documentar lições aprendidas em `docs/qa/` para iterar nas stories antes do evento real |

## Handoff

```yaml
next_agent: owner (ou @dev se patch no app necessário)
next_command: dry-run manual (workshop) ou *develop {story-id} (se patch necessário)
condition: 4 stories em Ready, sem blockers do agente
alternatives:
  - agent: @sm, command: *draft, condition: stories precisam reescrita (não aplicável agora)
  - agent: @qa, command: *qa-gate {story-id}, condition: pós-execução do workshop, se quiser auditar
```
