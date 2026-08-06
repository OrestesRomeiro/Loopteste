---
artefato: dossie-vivo
empresa: <TICKER>
classe_validade: C          # PENDENTE ADR-016 — ver §8.3 deste arquivo
finalidade: analise         # analise | teste-de-ferramenta — PENDENTE ADR-016
as_of: AAAA-MM-DD           # data da última atualização deste arquivo
status: Vigente
depende_de: []              # o dossiê agrega; dependências vivem em cada artefato indexado
executor: {modelo: <nome+versão>, revisor: <humano|Opus>}
---

# Dossiê Vivo — <RAZÃO SOCIAL> (<TICKER>)

> **Este arquivo é índice e memória, não é artefato de etapa.** Fichas, mapas, memos e notas vivem
> como arquivos próprios em `/artefatos/`; aqui ficam apenas o estado, os ponteiros e o log.
> Exceção deliberada: a **§7 — Log de eventos** só existe aqui.
> Regras de escrita em §8.

---

## 1. Sumário executivo

<!-- Máx. 1 página (§8.3 do mestre). É a única seção que uma sessão de triagem de E5 precisa ler. -->

### 1.1 Identificação

| Campo | Valor | Marca | Fonte |
|---|---|---|---|
| Razão social | | `[F]` | |
| Ticker / classe | | `[F]` | |
| Setor / subsetor | | `[F]` | |
| Segmento de listagem B3 | | `[F]` | |
| Estrutura de controle (1 linha) | | `[F]` | |
| Free float | | `[F]` | |

<!-- Campo de Fato sem referência resolvível escreve literalmente SEM FONTE (§4.4, regra 1). -->

### 1.2 Estado do caso

<!-- Máx. 5 linhas. Reescrito a cada atualização (não é append-only). -->

- **Veredito de fundação vigente:** <GO | NO-GO | AMBÍGUO | não executado> (<data>)
- **Decisão vigente:** <comprar | vender | manter | passar | nenhuma> (<data>, memo <arquivo>)
- **Hipótese principal ativa:** <uma linha> `[J]`
- **Próxima revisão programada:** <data e motivo>

### 1.3 Tabela de Estado dos Artefatos

<!-- §4.5, regra 2. Primeira coisa lida por qualquer sessão de E5. -->

| Artefato | Arquivo | Classe | `as_of` | Vence por / válido até | Status |
|---|---|---|---|---|---|
| Ficha de Fundação | | F | | evento (ver §2.1 e §4.5 do mestre) | |
| Mapa de Expectativas | | P | | | |
| Registro de Hipóteses | | — | | | |
| Memo de Decisão | | D | | | |

<!-- Nenhuma etapa consome artefato com status ≠ Vigente (§4.5, regra 3).
     Artefato vencido permanece na tabela com status Vencido/Substituído — nunca é removido. -->

### 1.4 Alertas ativos

| ID | Tipo | Tocado em | Fato disparador | Ação pendente | Dono |
|---|---|---|---|---|---|
| | tripwire / kill criteria / premissa E1 | | | | |

<!-- Sem alertas: escrever "Nenhum alerta ativo em <data>". Nunca deixar a tabela vazia sem essa linha. -->

---

## 2. Fundação (E0)

**Ficha vigente:** `<arquivo>` — veredito **<...>**, `as_of` <data>
**Fichas anteriores:** <lista ou "nenhuma">

**Resumo do veredito (máx. 3 linhas):**
<!-- Copiado da ficha, não reescrito. Se divergir da ficha, a ficha prevalece. -->

### 2.1 Tripwires ativos

<!-- Formato canônico §4.6 do mestre. Condição que exija interpretação é inválida. -->

| ID | Condição observável | Fonte de verificação | Frequência | Ação se violado | Última verificação | Estado |
|---|---|---|---|---|---|---|
| TW-<TICKER>-01 | | | | | | Não violado / Violado em <data> |

---

## 3. Expectativas do preço (E1)

**Mapa vigente:** `<arquivo>` — `as_of_preco` <preço/data>, válido até <data>

- **O que o preço embute (3 linhas):**
- **CAP implícito:**
- **Variáveis de maior sensibilidade:** 1. … 2. … 3. …

<!-- Gatilhos de reexecução (classe P): ±15% do as_of_preco, 90 dias, ou divulgação de resultado
     trimestral — o que ocorrer primeiro (§4.5 do mestre). -->

---

## 4. Hipóteses (E2)

| ID | Hipótese (falsificável) | Horizonte | Status | Evidência discriminante pendente | Artefato |
|---|---|---|---|---|---|
| H-01 | | | ativa / confirmada / refutada / arquivada | | |

<!-- Hipótese refutada não é apagada: muda de status e permanece. O histórico de refutações é
     insumo de calibração e é parte do estoque. -->

---

## 5. Pesquisa (E3)

**Fila de pesquisa vigente:** `<arquivo>`

| Data | Nota | Hipótese endereçada | Resultado (confirma/refuta/inconclusivo) | Arquivo |
|---|---|---|---|---|

---

## 6. Decisões (E4)

| Data | Decisão | Sizing | Kill criteria (resumo) | Data de revisão | Arquivo | Decisor humano |
|---|---|---|---|---|---|---|

<!-- §10.4 do mestre: autoria e responsabilidade são humanas; o decisor é nomeado no memo e aqui. -->

---

## 7. Log de eventos com leitura (E5)

<!-- APPEND-ONLY. Mais recente no topo. Nenhuma linha é editada ou removida depois de escrita.
     "Leitura" = classificação do fato contra (a) hipóteses ativas, (b) tripwires, (c) premissas do
     Mapa de Expectativas (§3 E5 do mestre). Fato sem leitura é fato não processado — não entra. -->

| Data | Evento / fato | Fonte `[F]` | Hipótese afetada | Leitura | Tripwire tocado | Premissa E1 afetada | Ação | Executor |
|---|---|---|---|---|---|---|---|---|
| | | | H-xx / nenhuma | confirma / refuta / neutro | não / TW-xx | não / qual | | |

---

## 8. Convenções deste arquivo

1. **Marcação obrigatória (§4.4 do mestre).** Todo conteúdo é `[F]` fato com referência resolvível,
   `[I]` inferência nomeando de que fatos deriva, ou `[J]` julgamento. Fato sem fonte escreve
   `SEM FONTE` — nunca estimativa nem memória do modelo.
2. **Índice, não cópia.** Onde há artefato próprio, o dossiê guarda ponteiro + resumo curto. Em
   divergência entre o resumo aqui e o artefato, **o artefato prevalece** e o resumo é corrigido.
3. **Seções append-only:** §7 (log) e §9 (changelog). Todas as demais são reescritas a cada
   atualização — exceto linhas de tabela com status, que mudam de status em vez de sumir (§4.5, regra 1).
4. **Ordem de leitura para uma sessão de E5:** §1.3 → §1.4 → §2.1 → §7 (últimas entradas). Só se
   houver alerta é que se abre artefato completo.

### 8.3 Pendências normativas deste template

- **`classe_validade: C`** não existe hoje na §4.5 do mestre (só P/F/D). O Dossiê Vivo é contêiner:
  não vence por preço, por evento nem por data de revisão própria — vence por vencimento dos
  artefatos que indexa. Proposto como **ADR-016**, pendente de aprovação do sponsor.
- **Campo `finalidade`** é exigido pela §10.5 do mestre para artefatos de teste, mas não consta do
  cabeçalho normativo da §4.5. Proposto como parte da **ADR-016** (obrigatório em todo artefato,
  default `analise`).

---

## 9. Changelog do dossiê

| Versão | Data | Mudanças | Executor |
|---|---|---|---|
| | | | |
