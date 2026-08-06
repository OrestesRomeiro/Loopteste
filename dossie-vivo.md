---
artefato: dossie-vivo
empresa: ANIM3
classe_validade: C          # PENDENTE ADR-016
finalidade: analise         # PENDENTE ADR-016
as_of: 2026-08-06
status: Vigente
depende_de: [fontes/indice-fontes.md]
executor: {modelo: Claude Opus 4.5 (sessão de chat), revisor: pendente — sponsor}
---

# Dossiê Vivo — ANIMA HOLDING S.A. (ANIM3)

> Criado vazio no item 3 do backlog (§9.1 do mestre); campos `[F]` preenchidos na sessão 1 do passe 1
> de E0, exclusivamente a partir de documentos abertos naquela sessão (§4.4, regra 2).
> Fontes e URLs em `fontes/indice-fontes.md`. Convenções de escrita em §8.

---

## 1. Sumário executivo

### 1.1 Identificação

| Campo | Valor | Marca | Fonte |
|---|---|---|---|
| Razão social | ANIMA HOLDING S.A. — CNPJ 09.288.252/0001-32 | `[F]` | Estatuto Social \| 2025-03-11 \| preâmbulo |
| Ticker / classe | ANIM3 — 403.868.805 ações ordinárias, todas nominativas; vedada a emissão de preferenciais | `[F]` | Estatuto Social \| 2025-03-11 \| Art. 5º e §3º |
| Setor / subsetor | SEM FONTE | `[F]` | não coletado — classificação B3 não foi aberta |
| Segmento de listagem B3 | Novo Mercado | `[F]` | Estatuto Social \| 2025-03-11 \| Art. 1º, par. único |
| Estrutura de controle (1 linha) | **Em aberto.** Estatuto legisla sobre "Acionista Controlador"; Acionistas Originais somam 29,7% | `[I]` | deriva de Estatuto Art. 21/22 + Composição Acionária Apr/26 — ver log do passe 1, §5 |
| Free float | 256.720.748 ONs em circulação, ~63,6% do capital, em 17/04/2026 | `[F]` | página de RI — Composição Acionária \| 2026-04-17 \| nota (1) |

### 1.2 Estado do caso

- **Veredito de fundação vigente:** não executado — passe 1 de E0 em curso, interrompido por falta de fonte (log do passe 1, achado F3)
- **Decisão vigente:** nenhuma
- **Hipótese principal ativa:** nenhuma
- **Próxima revisão programada:** próxima sessão do passe 1, condicionada ao anexo dos trechos do FRE listados no log (§6)

### 1.3 Tabela de Estado dos Artefatos

| Artefato | Arquivo | Classe | `as_of` | Vence por / válido até | Status |
|---|---|---|---|---|---|
| Log do passe 1 — E0 | `artefatos/log-passe1-E0-ANIM3-2026-08-06.md` | C | 2026-08-06 | — | Vigente (parcial) |
| Índice de fontes | `fontes/indice-fontes.md` | C | 2026-08-06 | — | Vigente |
| Ficha de Fundação | — | F | — | — | Não iniciado — bloqueado por coleta incompleta |
| Mapa de Expectativas | — | P | — | — | Não iniciado |
| Registro de Hipóteses | — | — | — | — | Não iniciado |
| Memo de Decisão | — | D | — | — | Não iniciado |

### 1.4 Alertas ativos

Nenhum alerta ativo em 2026-08-06 — não há tripwires nem kill criteria definidos, porque E0 e E4
ainda não foram executados. Pendência registrada (não é alerta): identificar quem exerce o controle,
antes de fechar o gate A4.

---

## 2. Fundação (E0)

**Ficha vigente:** nenhuma — etapa em execução (passe 1), sem veredito.
**Fichas anteriores:** nenhuma.

**Estado da coleta:** gate A4 com material parcial; A1, A2, A3, A5, A6 e Bloco B não coletados.
Detalhe e extrações em `artefatos/log-passe1-E0-ANIM3-2026-08-06.md`.

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
| 2026-08-06 | Sessão 1 do passe 1 de E0: Estatuto Social e Composição Acionária lidos; FRE truncado em ~p. 24 de 313 | S-01, S-02, S-03 | nenhuma | neutro | não | não | anexar trechos do FRE listados no log §6 | Opus (sessão de chat) |
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
| 0.2 | 2026-08-06 | Sessão 1 do passe 1 de E0. §1.1 preenchida a partir de S-01 e S-02; tabela de artefatos, §2 e log atualizados. Estrutura de controle registrada como `[I]` em aberto. | Opus (sessão de chat) |
