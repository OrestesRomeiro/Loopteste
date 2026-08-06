---
artefato: expectativa-selada
empresa: PETR4
classe_validade: F
finalidade: teste-de-ferramenta          # §10.5 do mestre + ADR-016
as_of: AAAA-MM-DD                        # data em que o sponsor preenche
status: Vigente
depende_de: []
executor: {modelo: nenhum — preenchido pelo sponsor, revisor: Orestes}
selagem: {metodo: commit em main antes de qualquer execução de E0 sobre PETR4, commit_sha: <preencher>}
---

# Expectativa selada de veredito — PETR4 (controle negativo do gate E0)

**ADR-015.** Escopo restrito a E0. **Sem qualquer intenção de investimento.** Nenhum conteúdo deste
arquivo é recomendação, avaliação de investimento ou decisão da gestora (§10 do mestre).

---

## 0. Regras de uso — ler antes de preencher

1. **Preenchido pelo sponsor, não pelo modelo.** Um modelo que preenchesse este arquivo estaria
   produzindo a expectativa e o resultado com a mesma cabeça; a comparação não mediria nada.
2. **Selagem = commit.** O arquivo é commitado em `main` **antes** de qualquer execução de E0 sobre
   PETR4. O timestamp do commit é o selo; nenhum mecanismo adicional é necessário. Registre o SHA no
   cabeçalho depois do commit.
3. **A sessão que executa E0 sobre PETR4 não recebe este arquivo.** Nem em anexo, nem em resumo, nem
   por paráfrase no prompt. Este arquivo só é reaberto na sessão de avaliação de M5, junto com a
   ficha já produzida.
4. **Todo o conteúdo aqui é `[J]` julgamento por natureza** — é a expectativa prévia do sponsor, não
   um levantamento documental. Não é preciso marcar linha a linha, mas nenhuma afirmação daqui pode
   ser reaproveitada como `[F]` em outro artefato.
5. **Não pesquise antes de preencher.** A expectativa vale exatamente por ser a que o sponsor já
   tem. Se você sentir necessidade de abrir documentos para responder, escreva "sem expectativa
   formada" — é uma resposta válida e informativa sobre o gate.
6. **Preencher leva minutos, não horas.** Precisão não é o objetivo; comprometimento prévio é.

---

## 1. Veredito esperado

Marque **um**:

- [ ] **GO**
- [ ] **NO-GO**
- [ ] **AMBÍGUO → julgamento humano**

**Confiança nessa expectativa:** [ ] alta  [ ] média  [ ] baixa

**Em uma frase, por quê:**

>

---

## 2. Expectativa gate a gate

<!-- A lista de gates abaixo é transcrição literal da §3/E0 do mestre — não é o Anexo E0, que só
     será escrito por destilação do passe 1 (§6.4). Se o passe 1 mostrar que a decomposição real dos
     gates é outra, este quadro é remapeado na avaliação de M5, e o remapeamento é registrado. -->

Para cada gate: **P** = passa, **R** = reprova/trava, **A** = ambíguo, **—** = sem expectativa formada.

### Bloco A — Transmissão de valor

| Gate | Expectativa | Observação (opcional, 1 linha) |
|---|---|---|
| A1 — Histórico do controlador sob conflito de interesse | | |
| A2 — Partes relacionadas | | |
| A3 — Track record de alocação de capital | | |
| A4 — Arquitetura de direitos do minoritário (tag along, listagem, free float, estatuto) | | |
| A5 — Incentivos da gestão | | |
| A6 — Taxa base da classe de referência (estrutura de controle análoga no Brasil) | | |

### Bloco B — Defensibilidade do múltiplo

| Gate | Expectativa | Observação (opcional, 1 linha) |
|---|---|---|
| B1 — Natureza da vantagem competitiva (estrutural vs circunstancial) | | |
| B2 — "O que impede um entrante bem capitalizado?" | | |
| B3 — Vulnerabilidade a substituição por baixo | | |
| B4 — Sinais de mudança de regime | | |

### Pre-mortem

**"A empresa entregou tudo e perdemos dinheiro — por quê?"** — resposta esperada em 1–3 linhas:

>

---

## 3. Tripwires que o sponsor espera ver

Quais condições observáveis (§4.6) você esperaria que a ficha produzisse? Máx. 3.

| # | Condição observável esperada |
|---|---|
| 1 | |
| 2 | |
| 3 | |

---

## 4. O que seria surpresa

**Que resultado da execução faria você desconfiar do gate (e não da empresa)?**

>

**Que resultado faria você rever sua própria expectativa?**

>

---

## 5. Avaliação de M5

<!-- PREENCHER SOMENTE APÓS a execução do passe 4. Até lá, seção intocada. -->

| Item | Esperado | Obtido | Coincide? |
|---|---|---|---|
| Veredito final | | | |
| Gates reprovados (Bloco A) | | | |
| Gates reprovados (Bloco B) | | | |
| Tripwires gerados | | | |

**Comparação com o caso principal (ANIM3):** vereditos diferentes? [ ] sim [ ] não
<!-- Veredito idêntico nos dois casos = gate não validado; o Anexo E0 volta para revisão (§6.4, passe 4). -->

**As justificativas por gate explicam a diferença entre os dois casos?** [ ] sim [ ] não

**Veredito sobre M5:** [ ] gate validado  [ ] gate não validado → revisar Anexo E0

**Leitura da divergência (se houver):**

>

<!-- Divergência entre expectativa e resultado NÃO é falha automática. Três leituras possíveis:
     (a) o gate viu algo que o sponsor não via — ganho de informação, o mais valioso;
     (b) o gate errou por especificação frouxa — corrige-se o anexo;
     (c) a expectativa do sponsor estava mal formada — registra-se e segue.
     Escolher entre as três exige ler as justificativas por gate, não só o veredito. -->

---

## 6. Changelog

| Versão | Data | Mudanças | Autor |
|---|---|---|---|
| 0.1 | 2026-08-06 | Formulário criado vazio pela sessão de chat. **Nenhum campo de expectativa preenchido.** | Opus (sessão de chat) |
