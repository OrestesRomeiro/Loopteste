---
artefato: dossie-vivo
empresa: ANIM3
classe_validade: C          # PENDENTE ADR-016
finalidade: analise         # PENDENTE ADR-016
as_of: 2026-08-06
status: Vigente
depende_de: []
executor: {modelo: Claude Opus 4.5 (sessão de chat), revisor: pendente — sponsor}
---

# Dossiê Vivo — SEM FONTE (ANIM3)

> Arquivo criado vazio no item 3 do backlog (§9.1 do mestre), antes de qualquer execução de etapa.
> Nenhum campo de fato foi preenchido: **nada aqui vem de documento carregado em sessão**, e a §4.4
> (regra 2) proíbe o executor de citar conhecimento próprio como fato. Os campos `[F]` só são
> preenchidos no passe 1 de E0, a partir dos documentos abertos naquela sessão.
> Convenções de escrita em §8.

---

## 1. Sumário executivo

### 1.1 Identificação

| Campo | Valor | Marca | Fonte |
|---|---|---|---|
| Razão social | SEM FONTE | `[F]` | a preencher no passe 1 de E0 |
| Ticker / classe | SEM FONTE | `[F]` | a preencher no passe 1 de E0 |
| Setor / subsetor | SEM FONTE | `[F]` | a preencher no passe 1 de E0 |
| Segmento de listagem B3 | SEM FONTE | `[F]` | a preencher no passe 1 de E0 |
| Estrutura de controle (1 linha) | SEM FONTE | `[F]` | a preencher no passe 1 de E0 |
| Free float | SEM FONTE | `[F]` | a preencher no passe 1 de E0 |

### 1.2 Estado do caso

- **Veredito de fundação vigente:** não executado
- **Decisão vigente:** nenhuma
- **Hipótese principal ativa:** nenhuma
- **Próxima revisão programada:** n/a — próximo passo é o passe 1 de E0 (§6.4 do mestre)

### 1.3 Tabela de Estado dos Artefatos

| Artefato | Arquivo | Classe | `as_of` | Vence por / válido até | Status |
|---|---|---|---|---|---|
| Ficha de Fundação | — | F | — | — | Não iniciado |
| Mapa de Expectativas | — | P | — | — | Não iniciado |
| Registro de Hipóteses | — | — | — | — | Não iniciado |
| Memo de Decisão | — | D | — | — | Não iniciado |

### 1.4 Alertas ativos

Nenhum alerta ativo em 2026-08-06 — não há tripwires nem kill criteria definidos, porque E0 e E4
ainda não foram executados.

---

## 2. Fundação (E0)

**Ficha vigente:** nenhuma — etapa não executada.
**Fichas anteriores:** nenhuma.

### 2.1 Tripwires ativos

Nenhum. Tripwires nascem da Ficha de Fundação (§4.6 do mestre).

| ID | Condição observável | Fonte de verificação | Frequência | Ação se violado | Última verificação | Estado |
|---|---|---|---|---|---|---|

---

## 3. Expectativas do preço (E1)

Não executada. Bloqueada até o Anexo E1 existir (§4.7, item 6 do mestre: sem o método de prêmio de
risco/beta fixado, nenhuma execução de E1 é válida) e até o consenso manual do sponsor (ADR-011).

---

## 4. Hipóteses (E2)

| ID | Hipótese (falsificável) | Horizonte | Status | Evidência discriminante pendente | Artefato |
|---|---|---|---|---|---|

---

## 5. Pesquisa (E3)

**Fila de pesquisa vigente:** nenhuma.

| Data | Nota | Hipótese endereçada | Resultado | Arquivo |
|---|---|---|---|---|

---

## 6. Decisões (E4)

| Data | Decisão | Sizing | Kill criteria (resumo) | Data de revisão | Arquivo | Decisor humano |
|---|---|---|---|---|---|---|

---

## 7. Log de eventos com leitura (E5)

<!-- APPEND-ONLY. Mais recente no topo. -->

| Data | Evento / fato | Fonte `[F]` | Hipótese afetada | Leitura | Tripwire tocado | Premissa E1 afetada | Ação | Executor |
|---|---|---|---|---|---|---|---|---|
| 2026-08-06 | Dossiê criado vazio (item 3 do backlog §9.1) | este documento | nenhuma | neutro | não | não | aguardar passe 1 de E0 | Opus (sessão de chat) |

---

## 8. Convenções deste arquivo

1. **Marcação obrigatória (§4.4 do mestre):** `[F]` com referência resolvível, `[I]` nomeando os
   fatos de origem, `[J]` julgamento. Fato sem fonte escreve `SEM FONTE`.
2. **Índice, não cópia.** Em divergência entre resumo aqui e artefato, o artefato prevalece.
3. **Append-only:** §7 e §9. Demais seções são reescritas; linhas com status mudam de status em vez
   de sumir.
4. **Ordem de leitura em sessão de E5:** §1.3 → §1.4 → §2.1 → §7.

---

## 9. Changelog do dossiê

| Versão | Data | Mudanças | Executor |
|---|---|---|---|
| 0.1 | 2026-08-06 | Criação do esqueleto a partir de `templates/dossie-vivo-TEMPLATE.md`. Nenhum campo de fato preenchido. | Opus (sessão de chat) |
