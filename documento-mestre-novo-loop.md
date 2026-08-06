# Documento Mestre — Projeto Novo Loop (MVP)

| Campo | Valor |
|---|---|
| Versão | 0.1 |
| Data | 2026-08-06 |
| Status | Rascunho — aguardando aprovação do sponsor |
| Sponsor / decisor | Orestes |
| Executores | Claude (Opus = design e julgamento; Sonnet = operação e manutenção) |
| Fonte da verdade | A versão mais recente deste arquivo. Anexos são arquivos separados, indexados na §6.3 |

---

## 0. Instruções para o modelo executor

Este bloco é lido por qualquer modelo (Opus ou Sonnet) que receber este documento no início de uma sessão.

1. Este documento é a **fonte única da verdade** do projeto. Em conflito entre este documento e memória/contexto anterior, este documento prevalece.
2. Antes de executar qualquer tarefa, identifique: (a) qual etapa do loop (§3) a tarefa pertence; (b) se existe anexo aprovado para a etapa (§6.3); (c) se você é o modelo recomendado para a tarefa (§8). Se não for, avise o usuário antes de prosseguir.
3. Toda proposta de mudança neste documento segue as regras da §7. Nunca altere o Registro de Decisões (§5) — decisões só são revertidas por nova ADR.
4. Saídas de ferramentas devem seguir exatamente os formatos definidos nos anexos. Formato é contrato.
5. Em ambiguidade material, pergunte ao sponsor. Em ambiguidade trivial, decida, registre a decisão tomada na resposta e siga.

---

## 1. Visão e definição de sucesso

**Problema.** O processo fundamentalista tradicional (coleta → análise → decisão) tem como gargalo a construção de entendimento por ativo — cara, episódica e não escalável. Parte relevante das horas de analista é gasta em trabalho escalável (extração, cálculo, monitoramento) que não gera edge.

**Visão.** Reestruturar o loop para que: (i) o entendimento profundo seja gasto apenas onde é contestado e decisivo (variant perception sobre o que o preço embute); (ii) o entendimento deixe de ser fluxo episódico e vire **estoque mantido continuamente** (dossiês vivos); (iii) casos estruturalmente problemáticos sejam eliminados **antes** do investimento de horas (fundação).

**Definição de sucesso do projeto.** Construir, ao longo de diversas interações, um "software" operado por LLMs + ferramentas replicáveis que execute o loop da §3 de ponta a ponta para o universo de cobertura, tal que:

- S1. O estágio de Fundação rode de forma semi-automática sobre candidatos, com veredito e tripwires.
- S2. Cada empresa em cobertura tenha um Dossiê Vivo atualizado a custo marginal baixo.
- S3. As horas humanas do analista se concentrem em: julgamento de casos ambíguos, variant perception, relacionamentos e decisão.
- S4. Toda decisão de compra/venda/manutenção tenha memo registrado com hipóteses falsificáveis e critérios de kill.

**Definição de sucesso do MVP.** O loop completo executado manualmente-assistido (sem automação de infraestrutura) para um piloto de 3–5 empresas, com todas as ferramentas dos anexos testadas ao menos uma vez e formatos de saída estabilizados.

---

## 2. Escopo do MVP

### 2.1 Dentro do escopo
- Especificação e execução das 6 etapas do loop (§3) para o universo piloto.
- Criação dos anexos de cada etapa conforme o modelo da §6, incluindo ferramentas replicáveis (prompts-padrão e, quando determinístico, scripts).
- Definição dos artefatos de dados (entradas e saídas) de cada etapa — formato é parte do escopo.
- Dossiês Vivos como arquivos Markdown estruturados por empresa.
- Roteamento de modelos para controle de custo (§8).

### 2.2 Fora do escopo (registrado para não perder)
- Interfaces de uso (dashboards, apps, alertas automáticos). MVP opera por sessões de chat + arquivos.
- Automação de coleta agendada (crawlers, pipelines). No MVP, a coleta é disparada manualmente.
- Integração com sistemas da gestora e com dados proprietários de relacionamento.
- Backtesting quantitativo do processo.
- Cobertura do universo completo da B3.

---

## 3. O Loop — especificação das etapas

Visão geral: **E0 Fundação → E1 Expectativas do Preço → E2 Hipóteses (Variant Perception) → E3 Pesquisa por VOI → E4 Decisão e Sizing → E5 Dossiê Vivo / Monitoramento → (realimenta E1–E4; tripwires podem reabrir E0)**

Cada etapa é especificada como um componente de software: entrada, processamento, saída. O detalhamento operacional de cada etapa viverá em anexo (§6).

### E0 — Fundação (gates desqualificadores)
- **Objetivo:** eliminar, antes do gasto de horas, os casos em que a tese pode estar certa e o retorno não chegar ao minoritário.
- **Entrada:** ticker candidato; documentos públicos (estatuto, formulário de referência, atas de assembleia, DFs e notas explicativas — partes relacionadas, ITRs, fatos relevantes; decisões CADE quando aplicável).
- **Processamento:** dois blocos de gates + pre-mortem obrigatório.
  - Bloco A — Transmissão de valor: histórico do controlador sob conflito de interesse; partes relacionadas; track record de alocação de capital; arquitetura de direitos do minoritário (tag along, listagem, free float, estatuto); incentivos da gestão; taxa base da classe de referência (estrutura de controle análoga no Brasil).
  - Bloco B — Defensibilidade do múltiplo: natureza da vantagem competitiva (estrutural vs circunstancial); pergunta desqualificadora "o que impede um entrante bem capitalizado?"; vulnerabilidade a substituição por baixo; sinais de mudança de regime.
  - Pre-mortem: "a empresa entregou tudo e perdemos dinheiro — por quê?".
- **Saída:** **Ficha de Fundação** — veredito {GO | NO-GO | AMBÍGUO→julgamento humano}, justificativa por gate, e lista de **tripwires** (condições de contorno que, se violadas, reabrem a fundação). Formato definido no Anexo E0.
- **Nota:** etapa mais automatizável do loop (baseada em documentos públicos). Fundação não é one-time: tripwires são monitorados em E5.

### E1 — Expectativas do Preço (engenharia reversa)
- **Objetivo:** ler o que o preço atual já embute, em vez de construir valuation do zero.
- **Entrada:** preço/múltiplos atuais; consenso disponível (sell-side, guidance); DFs históricas; custo de capital estimado.
- **Processamento:** DCF reverso simplificado; decomposição do preço em premissas (crescimento, margem, reinvestimento, **CAP — anos de retorno acima do custo de capital embutidos**); identificação das 2–3 variáveis às quais o valor é mais sensível.
- **Saída:** **Mapa de Expectativas** — o que o preço assume, CAP implícito, ranking de sensibilidade das variáveis. Formato no Anexo E1.

### E2 — Hipóteses e Variant Perception
- **Objetivo:** formular onde especificamente podemos saber melhor que o preço — e as hipóteses concorrentes.
- **Entrada:** Mapa de Expectativas (E1); Ficha de Fundação (E0); conhecimento acumulado no Dossiê Vivo, se existir.
- **Processamento:** método de hipóteses competitivas (ACH): listar teses mutuamente excludentes sobre as variáveis sensíveis; para cada uma, listar evidências que a **refutariam**; explicitar por que o mercado estaria errado e por que o erro persiste.
- **Saída:** **Registro de Hipóteses** — hipótese principal falsificável com horizonte, hipóteses concorrentes, evidências discriminantes pendentes. Formato no Anexo E2.

### E3 — Pesquisa ordenada por Valor da Informação (VOI)
- **Objetivo:** gastar horas de pesquisa apenas no que muda a decisão.
- **Entrada:** Registro de Hipóteses (E2).
- **Processamento:** para cada evidência pendente, estimar {impacto na decisão se resolvida} × {probabilidade de resolver} ÷ {custo em horas}; ordenar; executar do topo; atualizar hipóteses após cada item (loop E2↔E3).
- **Saída:** **Fila de Pesquisa** priorizada + **Notas de Pesquisa** padronizadas anexadas ao Dossiê. Formato no Anexo E3.

### E4 — Decisão e Sizing
- **Objetivo:** decidir {comprar | vender | manter | passar} com tamanho proporcional à confiança, e registrar o porquê.
- **Entrada:** Hipóteses atualizadas (E2/E3), Mapa de Expectativas (E1), restrições de portfólio.
- **Processamento:** decisão explícita; sizing como função da confiança e da assimetria; definição ex-ante de **critérios de kill** (que evidência encerra a posição) e de gatilhos de aumento.
- **Saída:** **Memo de Decisão** — decisão, racional, sizing, kill criteria, gatilhos, data de revisão. Formato no Anexo E4.

### E5 — Dossiê Vivo e Monitoramento
- **Objetivo:** transformar entendimento de fluxo em estoque; custo marginal de manutenção baixo.
- **Entrada:** todos os artefatos das etapas anteriores + fluxo de fatos novos (releases, fatos relevantes, transcripts, notícias, dados setoriais).
- **Processamento:** cada fato novo é classificado contra: (a) hipóteses ativas (confirma/refuta/neutro); (b) tripwires de fundação; (c) premissas do Mapa de Expectativas. Atualização incremental do dossiê; disparo de alerta quando tripwire ou kill criteria forem tocados.
- **Saída:** **Dossiê Vivo** atualizado (arquivo único por empresa, contendo/indexando: Ficha de Fundação, Mapa de Expectativas, Registro de Hipóteses, Notas de Pesquisa, Memos de Decisão, log de eventos com leitura). Formato no Anexo E5.
- **Nota:** o Dossiê Vivo é o banco de dados central do sistema. Todas as etapas leem dele e escrevem nele.

---

## 4. Arquitetura de dados e artefatos (MVP)

### 4.1 Princípios
- **Arquivos Markdown estruturados como banco de dados.** Legíveis por humanos e por LLMs, versionáveis, sem infraestrutura. Estruturas tabulares auxiliares em CSV quando necessário.
- **Formato é contrato.** Cada artefato tem template definido em anexo; ferramentas produzem exatamente o template. Isso é o que permite trocar de modelo (Opus↔Sonnet) sem perda.
- **Um diretório por empresa** contendo o Dossiê Vivo e artefatos. O documento mestre e anexos ficam na raiz do projeto.

### 4.2 Fontes de dados (MVP)
| Fonte | Uso | Status |
|---|---|---|
| CVM Dados Abertos / ENET (DFs, FRE, ITR, fatos relevantes, atas) | Base de E0, E1, E5 | Pública, disponível |
| Sites de RI (releases, apresentações, transcripts) | E1, E2, E5 | Pública, disponível |
| B3 (dados de mercado, aluguel de ações, segmentos de listagem) | E0, E1 | Pública, disponível |
| CADE (atos de concentração, decisões) | E0 Bloco B | Pública, disponível |
| Economatica / Comdinheiro | Séries financeiras estruturadas, consenso, múltiplos históricos | **Em trial — decisão pendente (Q-02)** |

### 4.3 Estrutura de diretórios (proposta)
```
/projeto-novo-loop/
  documento-mestre.md
  /anexos/
    anexo-E0-fundacao.md
    anexo-E1-expectativas.md
    ...
  /ferramentas/
    F-E0-01-ficha-fundacao.prompt.md
    F-E1-01-dcf-reverso.py
    ...
  /empresas/
    /TICKER/
      dossie-vivo.md
      /artefatos/  (fichas, mapas, memos, notas datadas)
      /fontes/     (documentos-fonte baixados, quando relevante)
```

---

## 5. Registro de Decisões (ADR — Architecture Decision Records)

Regras: ADRs são imutáveis. Para reverter, cria-se nova ADR com status "substitui ADR-xxx". Toda decisão estrutural do projeto entra aqui com racional.

| ID | Data | Decisão | Racional | Status |
|---|---|---|---|---|
| ADR-001 | 2026-08-06 | O loop adota **expectations investing** (partir do que o preço embute) em vez de valuation construído do zero | O único entendimento que gera retorno é o delta contra o consenso; inverter reduz drasticamente a superfície de entendimento necessária | Ativa |
| ADR-002 | 2026-08-06 | Entendimento tratado como **estoque mantido continuamente** (Dossiê Vivo), não fluxo episódico | LLMs derrubam o custo marginal de manutenção; elimina o retrabalho de reconstruir entendimento a cada evento | Ativa |
| ADR-003 | 2026-08-06 | Etapa de **Fundação (E0) como estágio 0 com gates desqualificadores**, antes de qualquer análise profunda | Casos observados de tese operacional correta sem retorno ao minoritário (falha de captura via governança) e de entrega de métricas com derating de múltiplo (falha de regime competitivo/CAP). Assimetria favorece gates rígidos: falso negativo custa pouco num universo amplo; falso positivo destrói capital e horas | Ativa |
| ADR-004 | 2026-08-06 | Fundação **não é one-time**: gera tripwires monitorados em E5 e pode ser reaberta | Governança se deteriora e regimes competitivos mudam durante a vida da tese | Ativa |
| ADR-005 | 2026-08-06 | Pesquisa ordenada por **Valor da Informação** (E3), não por completude de modelo | A maior parte da análise tradicional não muda a decisão; horas devem ir para o que discrimina hipóteses | Ativa |
| ADR-006 | 2026-08-06 | Documento mestre em **Markdown**, estilo documentação de software, com anexos separados por etapa | Precisa ser percorrido por outros modelos a custo baixo: texto plano, estruturado, versionável, sem dependência de ferramenta | Ativa |
| ADR-007 | 2026-08-06 | MVP **sem interfaces e sem automação de infraestrutura**; execução por sessões de chat + arquivos | Foco no essencial (dados de entrada, processamento, dados de saída); interface é otimização prematura antes de estabilizar formatos | Ativa |
| ADR-008 | 2026-08-06 | **Roteamento de modelos por natureza da tarefa**: Opus cria e julga; Sonnet opera e mantém (§8) | Controle de custo de execução mantendo qualidade onde julgamento importa | Ativa |
| ADR-009 | 2026-08-06 | Ferramentas replicáveis nascem como **prompt-padrão**; migram para **script** apenas quando o processamento for determinístico (cálculo, parsing) | Prompt é mais barato de iterar no MVP; script garante reprodutibilidade onde não há julgamento envolvido | Proposta — aguardando aprovação do sponsor |

---

## 6. Modelo de Anexos — como cada etapa será detalhada

### 6.1 Regras
1. Cada etapa do loop (E0–E5) terá **um anexo próprio**, em arquivo separado, criado sob demanda ao longo das interações.
2. Um anexo só passa a valer com status **Aprovado** pelo sponsor. Até lá, é Rascunho e o mestre indica isso no índice (§6.3).
3. Anexos seguem obrigatoriamente o template da §6.2. Seções podem ser adicionadas; nenhuma pode ser removida.
4. Toda ferramenta definida em anexo segue a especificação da §6.2.1 (Ferramenta Replicável). Uma ferramenta só é considerada pronta após passar nos próprios critérios de aceite ao menos uma vez em empresa real do piloto.
5. Mudança em anexo aprovado = nova versão do anexo + entrada no changelog do anexo + atualização do índice no mestre. Mudanças que alterem **entradas ou saídas** de uma etapa exigem ADR no mestre (afetam o contrato entre etapas).

### 6.2 Template de Anexo (obrigatório)
```markdown
# Anexo E{n} — {Nome da Etapa}
Versão | Data | Status: {Rascunho | Aprovado} | Depende de: {anexos/ADRs}

## 1. Objetivo da etapa
## 2. Entradas
   (artefatos e dados exigidos, com formato e origem)
## 3. Procedimento
   (passo a passo executável por um modelo sem contexto adicional)
## 4. Saídas
   (template exato de cada artefato produzido — formato é contrato)
## 5. Ferramentas Replicáveis
   (uma subseção por ferramenta, conforme §6.2.1 do mestre)
## 6. Roteamento de modelo
   (qual modelo executa cada passo, e quando escalar para Opus/humano)
## 7. Critérios de qualidade da etapa
   (como saber se a execução foi boa; erros comuns a evitar)
## 8. Casos de teste
   (empresas reais usadas para validar; resultado esperado vs obtido)
## 9. Changelog do anexo
```

### 6.2.1 Especificação de Ferramenta Replicável (obrigatória para cada ferramenta)
```markdown
### F-E{n}-{seq} — {nome da ferramenta}
- Tipo: {prompt-padrão | script python | planilha de cálculo}
- Objetivo: (uma frase)
- Entrada: (formato exato; de onde vem)
- Saída: (template exato; para onde vai)
- Modelo executor: {Sonnet | Opus | humano} + condição de escalonamento
- Procedimento/corpo: (o prompt completo, ou o script, ou as fórmulas)
- Critérios de aceite: (condições verificáveis de saída correta)
- Casos de teste: (input real → output validado)
- Custo estimado por execução: (ordem de grandeza: tokens/tempo)
- Versão e changelog
```

### 6.3 Índice de anexos (estado atual)
| Anexo | Etapa | Status | Versão |
|---|---|---|---|
| anexo-E0-fundacao.md | E0 Fundação | Não iniciado | — |
| anexo-E1-expectativas.md | E1 Expectativas do Preço | Não iniciado | — |
| anexo-E2-hipoteses.md | E2 Hipóteses / Variant Perception | Não iniciado | — |
| anexo-E3-voi.md | E3 Pesquisa por VOI | Não iniciado | — |
| anexo-E4-decisao.md | E4 Decisão e Sizing | Não iniciado | — |
| anexo-E5-dossie-vivo.md | E5 Dossiê Vivo / Monitoramento | Não iniciado | — |

---

## 7. Regras de atualização deste documento

1. **Fluxo:** qualquer sessão pode propor mudanças; o modelo apresenta o diff proposto (o que muda e por quê); **só o sponsor aprova**. Aprovado → nova versão do arquivo + entrada no Changelog (§10).
2. **Versionamento semântico simplificado:**
   - **Major (1.0, 2.0):** mudança na estrutura do loop (adicionar/remover/reordenar etapas) ou na definição de sucesso.
   - **Minor (0.2, 0.3):** novas ADRs, mudanças em regras, atualização do índice de anexos, escopo.
   - Correções de texto sem efeito normativo não geram versão; entram na próxima.
3. **Imutabilidade do histórico:** ADRs e Changelog nunca são editados retroativamente — apenas acrescidos.
4. **Sincronização:** ao final de qualquer sessão que altere o mestre ou anexos, o modelo entrega os arquivos atualizados. A versão mais recente entregue é a fonte da verdade da próxima sessão.
5. **Divergência:** se um modelo executor identificar contradição entre o mestre e um anexo, prevalece o mestre; a contradição deve ser reportada como pendência.
6. **Questões abertas (§9.2):** toda dúvida material vira item numerado (Q-xx) com dono da resposta (normalmente o sponsor). Resolvida → vira ADR ou texto normativo, e o item é marcado como resolvido (não apagado).

---

## 8. Roteamento de modelos e controle de custo

### 8.1 Princípio
**Opus cria e julga; Sonnet opera e mantém.** Toda ferramenta nova é executada pela primeira vez sob supervisão de Opus (ou com revisão de Opus); em regime, roda em Sonnet. Escalonamento para Opus (ou para o humano) é sempre definido por condição explícita na ferramenta, nunca por preferência.

### 8.2 Mapa de roteamento por etapa
| Etapa / tarefa | Modelo em regime | Racional |
|---|---|---|
| E0 — extração documental e preenchimento de gates | **Sonnet** | Trabalho de extração e checklist sobre documentos públicos; alto volume |
| E0 — veredito em casos AMBÍGUO; leitura de histórico do controlador sob conflito | **Opus** (+ humano) | Julgamento sobre intenção e padrão de comportamento; maior custo de erro |
| E1 — cálculo do DCF reverso e sensibilidades | **Sonnet** (script quando pronto) | Determinístico após padronização |
| E1 — interpretação do CAP implícito e escolha das variáveis sensíveis | **Opus** na 1ª vez por empresa; **Sonnet** nas atualizações | Enquadramento inicial exige julgamento; atualização é incremental |
| E2 — geração de hipóteses e variant perception | **Opus** | É o coração do edge; economizar aqui é falsa economia |
| E3 — montagem e reordenação da fila VOI | **Sonnet** com revisão do sponsor | Mecânico após E2 bem feito |
| E3 — execução de itens de pesquisa | **Sonnet**; **Opus** quando o item exigir síntese interpretativa | Maioria é coleta/extração |
| E4 — Memo de Decisão | **Opus** + decisão final humana | Documento de maior consequência do sistema |
| E5 — triagem diária/semanal de fatos novos contra hipóteses e tripwires | **Sonnet** | Alto volume, baixa ambiguidade por item |
| E5 — quando um fato toca tripwire ou kill criteria | Escala para **Opus** + alerta ao humano | Momento de julgamento |
| Criação e revisão de anexos e ferramentas | **Opus** | Erro de design se propaga para todas as execuções |

### 8.3 Regras de custo
- Sessões de manutenção (E5) devem operar apenas com: este mestre (bloco §0 + §3 + §8) + o anexo da etapa + o dossiê da empresa. Não carregar anexos de outras etapas.
- Dossiês Vivos têm seção de **sumário executivo** no topo (máx. 1 página) para que sessões de triagem não precisem ler o dossiê inteiro.
- Toda ferramenta declara custo estimado por execução (§6.2.1); ferramentas de alto volume são as primeiras candidatas a virar script.

---

## 9. Roadmap e questões abertas

### 9.1 Backlog ordenado (próximos passos)
1. Aprovação deste documento (v0.1 → v0.2 com ajustes do sponsor).
2. Resolver Q-01 a Q-04.
3. Criar **Anexo E0 — Fundação** (primeiro, por ser o gate de entrada e o mais automatizável) com suas ferramentas.
4. Rodar E0 no piloto (incluir deliberadamente ≥1 empresa com problema conhecido de governança, como teste negativo do gate).
5. Criar Anexo E1 e rodar nas aprovadas do piloto.
6. Criar Anexo E5 (estrutura do Dossiê Vivo) — necessário cedo, pois E0/E1 já produzem artefatos que precisam de casa.
7. Anexos E2, E3, E4, nessa ordem, acompanhando o avanço do piloto.
8. Retrospectiva do MVP: o que vira script, o que muda de formato, decisão sobre pós-MVP (automação de coleta).

### 9.2 Questões abertas
| ID | Questão | Dono | Status |
|---|---|---|---|
| Q-01 | Universo piloto: quais 3–5 empresas? (sugestão: 2 já em cobertura da gestora + 1 caso sabidamente problemático de governança + 1 caso de derating por concorrência, para testar E0 contra os dois modos de falha) | Sponsor | Aberta |
| Q-02 | Fonte paga de dados estruturados: Economatica, Comdinheiro, nenhuma no MVP? (trial em andamento) | Sponsor | Aberta |
| Q-03 | Onde os arquivos serão versionados entre sessões: diretório local do sponsor re-anexado a cada sessão, Projeto no Claude, ou repositório Git? | Sponsor | Aberta |
| Q-04 | Nome definitivo do projeto ("Novo Loop" é placeholder) | Sponsor | Aberta |
| Q-05 | Restrições de portfólio relevantes para E4 (limites de posição, liquidez mínima) — necessário antes do Anexo E4 | Sponsor | Aberta |

---

## 10. Changelog

| Versão | Data | Mudanças |
|---|---|---|
| 0.1 | 2026-08-06 | Criação do documento: visão, escopo do MVP, especificação do loop (E0–E5), arquitetura de dados, ADR-001 a ADR-009, modelo de anexos e de ferramentas replicáveis, regras de atualização, roteamento de modelos, backlog e questões abertas |
