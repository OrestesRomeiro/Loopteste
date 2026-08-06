# Documento Mestre — Projeto Novo Loop (MVP)

| Campo | Valor |
|---|---|
| Versão | 0.3 |
| Data | 2026-08-06 |
| Status | Rascunho — aguardando aprovação do sponsor |
| Sponsor / decisor | Orestes |
| Executores | Claude (Opus = design e julgamento; Sonnet = operação e manutenção) |
| Fonte da verdade | Branch `main` do repositório `OrestesRomeiro/Loopteste`. Anexos são arquivos separados, indexados na §6.3 |

---

## 0. Instruções para o modelo executor

Este bloco é lido por qualquer modelo (Opus ou Sonnet) que receber este documento no início de uma sessão.

1. Este documento é a **fonte única da verdade** do projeto. Em conflito entre este documento e memória/contexto anterior, este documento prevalece.
2. Antes de executar qualquer tarefa, identifique: (a) qual etapa do loop (§3) a tarefa pertence; (b) se existe anexo aprovado para a etapa (§6.3); (c) se você é o modelo recomendado para a tarefa (§8). Se não for, avise o usuário antes de prosseguir.
3. Toda proposta de mudança neste documento segue as regras da §7. Nunca altere o Registro de Decisões (§5) — decisões só são revertidas por nova ADR.
4. Saídas de ferramentas devem seguir exatamente os formatos definidos nos anexos. Formato é contrato.
5. Em ambiguidade material, pergunte ao sponsor. Em ambiguidade trivial, decida, registre a decisão tomada na resposta e siga.
6. **Nenhum artefato é produzido sem o cabeçalho da §4.5 e sem a marcação de conteúdo da §4.4.** Artefato que descumpra qualquer das duas é inválido e deve ser rejeitado pelo próprio executor antes da entrega.
7. **Nenhum artefato deste sistema constitui recomendação de investimento a terceiros nem decisão formal da gestora.** Ver §10.

---

## 1. Visão e definição de sucesso

**Problema.** O processo fundamentalista tradicional (coleta → análise → decisão) tem como gargalo a construção de entendimento por ativo — cara, episódica e não escalável. Parte relevante das horas de analista é gasta em trabalho escalável (extração, cálculo, monitoramento) que não gera edge.

**Visão.** Reestruturar o loop para que: (i) o entendimento profundo seja gasto apenas onde é contestado e decisivo (variant perception sobre o que o preço embute); (ii) o entendimento deixe de ser fluxo episódico e vire **estoque mantido continuamente** (dossiês vivos); (iii) casos estruturalmente problemáticos sejam eliminados **antes** do investimento de horas (fundação).

**Definição de sucesso do projeto.** Construir, ao longo de diversas interações, um "software" operado por LLMs + ferramentas replicáveis que execute o loop da §3 de ponta a ponta para o universo de cobertura, tal que:

- S1. O estágio de Fundação rode de forma semi-automática sobre candidatos, com veredito e tripwires, **e discrimine demonstravelmente entre casos** (ver ADR-015).
- S2. Cada empresa em cobertura tenha um Dossiê Vivo atualizado a custo marginal baixo.
- S3. As horas humanas do analista se concentrem em: julgamento de casos ambíguos, variant perception, relacionamentos e decisão.
- S4. Toda decisão de compra/venda/manutenção tenha memo registrado com hipóteses falsificáveis e critérios de kill.

**Definição de sucesso do MVP.** O loop completo executado manualmente-assistido (sem automação de infraestrutura) sobre o caso principal do piloto, mais E0 executado sobre o controle negativo, com todas as ferramentas dos anexos testadas ao menos uma vez, formatos estabilizados conforme M3, e os critérios da §1.5 medidos.

### 1.5 Critérios mensuráveis de aceite do MVP

Os critérios S1–S4 são qualitativos. Os cinco abaixo são os que a retrospectiva (§9.1) mede.

| ID | Métrica | Como medir | Alvo |
|---|---|---|---|
| M1 | Horas humanas por empresa em E0 | Cronometrar o passe 1 (execução manual assistida) e a execução em regime | Regime ≤ 30% do passe 1 |
| M2 | Concordância de veredito entre Sonnet e Opus em E0 | Rodar a mesma ficha nos dois modelos e comparar gate a gate | ≥ 90% dos gates com mesma classificação; divergência no veredito final = falha de especificação do anexo, não do modelo |
| M3 | Estabilidade de formato | Nº de reexecuções causadas por alteração de template | Formato declarado estável após 2 execuções consecutivas sem alteração; ≤ 1 reexecução por artefato até lá |
| M4 | Cobertura de fonte | % de campos marcados como **Fato** (§4.4) com referência resolvível | 100%. Abaixo disso o artefato é rejeitado, não corrigido a posteriori |
| M5 | **Poder discriminante do gate E0** | Comparar o veredito produzido nos dois casos do piloto entre si, e cada um contra a expectativa selada do sponsor (ADR-015) | Vereditos diferentes entre os dois casos **e** justificativas por gate que expliquem a diferença. Veredito idêntico nos dois casos = gate não validado |

**Ressalva estatística.** Com piloto de duas empresas, M1 e M2 têm n≤2: são indicativos de ordem de grandeza, não medidas de confiabilidade. M5, por construção, exige exatamente os dois casos e é o critério que dá sentido a S1.

---

## 2. Escopo do MVP

### 2.1 Dentro do escopo
- Especificação e execução das 6 etapas do loop (§3) para o caso principal do piloto.
- Execução de **E0 apenas** sobre o controle negativo (ADR-015).
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
- **Etapas E1–E5 sobre o controle negativo.** Ele existe para testar o gate, não para ser analisado.

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
- **Saída:** **Ficha de Fundação** — veredito {GO | NO-GO | AMBÍGUO→julgamento humano}, justificativa por gate, e lista de **tripwires** no formato canônico da §4.6. Formato completo definido no Anexo E0.
- **Nota:** etapa mais automatizável do loop (baseada em documentos públicos). Fundação não é one-time: tripwires são monitorados em E5.

### E1 — Expectativas do Preço (engenharia reversa)
- **Objetivo:** ler o que o preço atual já embute, em vez de construir valuation do zero.
- **Entrada:** preço/múltiplos atuais; consenso (ADR-011 — input manual do sponsor no MVP); DFs históricas; custo de capital **nominal em BRL** (ADR-014).
- **Processamento:** DCF reverso simplificado; decomposição do preço em premissas (crescimento, margem, reinvestimento, **CAP — anos de retorno acima do custo de capital embutidos**); identificação das 2–3 variáveis às quais o valor é mais sensível.
- **Saída:** **Mapa de Expectativas** — o que o preço assume, CAP implícito, ranking de sensibilidade das variáveis. Artefato de **classe de validade P** (§4.5). Formato no Anexo E1.

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
- **Saída:** **Memo de Decisão** — decisão, racional, sizing, kill criteria, gatilhos, data de revisão. Artefato de **classe de validade D** (§4.5). Formato no Anexo E4. Ver §10 quanto ao estatuto deste documento perante o processo formal da gestora.

### E5 — Dossiê Vivo e Monitoramento
- **Objetivo:** transformar entendimento de fluxo em estoque; custo marginal de manutenção baixo.
- **Entrada:** todos os artefatos das etapas anteriores + fluxo de fatos novos (releases, fatos relevantes, transcripts, notícias, dados setoriais).
- **Processamento:** cada fato novo é classificado contra: (a) hipóteses ativas (confirma/refuta/neutro); (b) tripwires de fundação; (c) premissas do Mapa de Expectativas. Atualização incremental do dossiê; disparo de alerta quando tripwire ou kill criteria forem tocados. Toda sessão de E5 começa lendo a **Tabela de Estado dos Artefatos** (§4.5) e reporta o que venceu.
- **Saída:** **Dossiê Vivo** atualizado (arquivo único por empresa, contendo/indexando: sumário executivo, Tabela de Estado dos Artefatos, Ficha de Fundação, Mapa de Expectativas, Registro de Hipóteses, Notas de Pesquisa, Memos de Decisão, log de eventos com leitura). Formato no Anexo E5.
- **Nota:** o Dossiê Vivo é o banco de dados central do sistema. Todas as etapas leem dele e escrevem nele.

---

## 4. Arquitetura de dados e artefatos (MVP)

### 4.1 Princípios
- **Arquivos Markdown estruturados como banco de dados.** Legíveis por humanos e por LLMs, versionáveis, sem infraestrutura. Estruturas tabulares auxiliares em CSV quando necessário.
- **Formato é contrato.** Cada artefato tem template definido em anexo; ferramentas produzem exatamente o template. Isso é o que permite trocar de modelo (Opus↔Sonnet) sem perda.
- **Um diretório por empresa** contendo o Dossiê Vivo e artefatos. O documento mestre e anexos ficam na raiz do projeto.
- **Estoque só é ativo se for confiável.** Como o entendimento é acumulado (ADR-002), um erro de extração não é descartado ao fim da análise: ele persiste e contamina toda decisão derivada. Daí a §4.4 e a §4.5 serem normativas, não recomendações.

### 4.2 Fontes de dados (MVP)
| Fonte | Uso | Status |
|---|---|---|
| CVM Dados Abertos / ENET (DFs, FRE, ITR, fatos relevantes, atas) | Base de E0, E1, E5 | Pública, disponível |
| Sites de RI (releases, apresentações, transcripts) | E1, E2, E5 | Pública, disponível |
| B3 (dados de mercado, aluguel de ações, segmentos de listagem) | E0, E1 | Pública, disponível |
| CADE (atos de concentração, decisões) | E0 Bloco B | Pública, disponível |
| Consenso sell-side | E1, E2 | **Input manual do sponsor** (ADR-011) |
| Curva de juros nominal (título prefixado / DI futuro) | E1 — taxa livre de risco | Pública; fonte e data registradas no artefato (ADR-014) |
| Economatica / Comdinheiro | Séries financeiras estruturadas, consenso, múltiplos históricos | Em avaliação — Q-02, **não bloqueante** após ADR-011 |

### 4.3 Estrutura de diretórios
```
/Loopteste/                          (repositório GitHub, branch main)
  documento-mestre-novo-loop.md
  /anexos/
    anexo-E0-fundacao.md
    anexo-E1-expectativas.md
    ...
  /ferramentas/
    F-E0-01-ficha-fundacao.prompt.md
    F-E1-01-dcf-reverso.py
    ...
  /empresas/
    /ANIM3/                          (caso principal — loop completo)
      dossie-vivo.md
      /artefatos/
      /fontes/
    /PETR4/                          (controle negativo — E0 apenas, ADR-015)
      /artefatos/
        expectativa-selada-PETR4.md  (preenchida ANTES da execução)
        ficha-fundacao-PETR4-<data>.md
      /fontes/
```

### 4.4 Rastreabilidade e classificação de conteúdo (normativo)

Todo conteúdo de qualquer artefato é classificado em exatamente uma de três categorias, marcada explicitamente:

| Marca | Categoria | Regra |
|---|---|---|
| `[F]` | **Fato** | Afirmação verificável em documento. **Obrigatoriamente** acompanhada de referência resolvível |
| `[I]` | **Inferência** | Derivada de fatos citados no próprio artefato. Deve nomear de quais fatos deriva |
| `[J]` | **Julgamento** | Opinião do executor. Não precisa de fonte, mas precisa estar marcada como julgamento |

**Formato da referência:** `[doc:tipo | data do documento | seção/página]`
Exemplo: `[F] Transações com partes relacionadas somaram R$ X mn em 2025 [FRE 2026 | 2026-05-30 | item 16.2]`

**Regras de execução:**
1. Campo de Fato sem referência resolvível é **inválido**. A ferramenta deve emitir literalmente `SEM FONTE` no campo, nunca preencher com estimativa ou memória do modelo.
2. Nenhum executor pode citar conhecimento próprio como Fato. Se não veio de documento carregado na sessão, é `[J]` ou `SEM FONTE`. Isso vale com força redobrada para empresas de alta cobertura midiática, onde o modelo tem muito conteúdo memorizado e imprecisamente datado.
3. Referência resolvível = o sponsor consegue abrir o documento e localizar a informação a partir do que está escrito.
4. Todo critério de aceite de ferramenta (§6.2.1) inclui obrigatoriamente: *"100% dos campos `[F]` com referência resolvível"* (métrica M4).

**Racional.** O modo de falha dominante de um modelo extraindo documentos em volume não é errar o julgamento — é fundir silenciosamente fato, inferência e opinião num parágrafo fluente. A trinca `[F]/[I]/[J]` torna essa fusão visível na leitura, sem custo de execução.

### 4.5 Validade dos artefatos — política de "as of" (normativo)

Artefatos não envelhecem à mesma velocidade. Três classes:

| Classe | Artefatos | Expira por | Regra de reexecução |
|---|---|---|---|
| **P** — preço-dependente | Mapa de Expectativas | Tempo e movimento de preço | Reexecutar quando o preço se afastar **±15%** do `as_of_preco`, **ou** após **90 dias**, **ou** na divulgação de qualquer resultado trimestral — o que ocorrer primeiro |
| **F** — fundamento-dependente | Ficha de Fundação, Notas de Pesquisa | Evento, não tempo | Reexecutar quando: tripwire tocado, novo FRE anual, mudança de controle, alteração estatutária, ou mudança de segmento de listagem |
| **D** — decisão | Memo de Decisão | Data de revisão própria **ou** vencimento de qualquer artefato do qual dependa | Revisar na data definida no memo, ou imediatamente se um artefato em `depende_de` vencer |

**Cabeçalho obrigatório de todo artefato** (YAML no topo do arquivo):
```yaml
artefato: mapa-expectativas
empresa: ANIM3
classe_validade: P
as_of: 2026-08-06
as_of_preco: {preco: 0.00, fonte: B3, data: 2026-08-06}   # só classe P
valido_ate: 2026-11-04                                     # só classe P
status: Vigente          # Vigente | Vencido | Substituído por <arquivo>
depende_de: [ficha-fundacao-ANIM3-2026-08-06.md]
executor: {modelo: <nome+versão>, revisor: <humano|Opus>}
```

**Regras de execução:**
1. Artefato vencido **não é apagado nem sobrescrito**. Muda `status` para `Vencido` (ou `Substituído por`) e permanece no dossiê como histórico. Estoque preserva; sinalização evita que o vencido seja lido como vigente.
2. O Dossiê Vivo mantém uma **Tabela de Estado dos Artefatos** no sumário executivo: artefato, classe, `as_of`, status. É a primeira coisa lida por qualquer sessão de E5, e custa quase nada.
3. Nenhuma etapa consome artefato com `status` diferente de `Vigente`. Se precisar, reexecuta antes.

### 4.6 Formato canônico de tripwire (contrato E0 ↔ E5)

Tripwires são produzidos em E0 e consumidos em E5 — logo o formato pertence ao mestre, não a um anexo isolado (§6.1, regra 5).

| Campo | Regra |
|---|---|
| ID | `TW-<TICKER>-<seq>` |
| Condição observável | Valor observável + operador + limiar. Deve ser verificável **sem julgamento** |
| Fonte de verificação | Documento específico onde a condição é checada |
| Frequência | Trimestral, anual, ou por evento |
| Ação se violado | Reabrir E0 (bloco A ou B), alertar humano, ou revisar hipótese X |

Exemplo:
`TW-ANIM3-01 | Transações com partes relacionadas > 5% da receita líquida em qualquer exercício | DF anual, nota de partes relacionadas | anual | reabrir E0 Bloco A`

Condição redigida de forma que exija interpretação ("governança piorar") é inválida e deve ser rejeitada na revisão do anexo E0.

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
| ADR-009 | 2026-08-06 | Ferramentas replicáveis nascem como **prompt-padrão**; migram para **script** apenas quando o processamento for determinístico (cálculo, parsing) | Prompt é mais barato de iterar no MVP; script garante reprodutibilidade onde não há julgamento envolvido | **Ativa** (aprovada pelo sponsor em 2026-08-06; resolve Q-07) |
| ADR-010 | 2026-08-06 | **Repositório GitHub `OrestesRomeiro/Loopteste`, branch `main`, é a fonte da verdade.** Escrita no repositório é feita pelo sponsor (upload/commit manual) ou por Claude Code operando localmente; sessões de chat **não** escrevem no repositório — entregam arquivos para commit | Resolve Q-03. Leitura anônima do repositório foi verificada e funciona; escrita a partir de sessão de chat é impossível por ausência de credencial e não deve ser contornada com token | Ativa |
| ADR-011 | 2026-08-06 | **Consenso sell-side é input manual do sponsor** até haver fonte confiável. Plano B registrado: se a fonte não se materializar, adota-se o **preço como consenso**, extraindo as premissas embutidas via DCF reverso de E1, e a variant perception passa a ser contra o preço em vez de contra a narrativa sell-side | ADR-001 define o edge como delta contra o consenso; sem fonte de consenso o método perderia âncora. Input manual destrava o MVP agora; o plano B é metodologicamente defensável e converte Q-02 de dependência crítica em decisão de conveniência | Ativa |
| ADR-012 | 2026-08-06 | **Rastreabilidade obrigatória**: toda afirmação em artefato é marcada `[F]` fato com fonte resolvível, `[I]` inferência, ou `[J]` julgamento (§4.4). Campo de fato sem fonte é inválido | Sob ADR-002 o erro de extração não é descartado: fica no estoque e contamina toda decisão derivada. A marcação torna visível a fusão silenciosa entre fato, inferência e opinião | Ativa |
| ADR-013 | 2026-08-06 | **Política de validade por classe de artefato** (P/F/D) com cabeçalho `as_of` obrigatório e Tabela de Estado dos Artefatos no dossiê (§4.5). Artefato vencido muda de status, nunca é apagado | Mapa de Expectativas depende de preço e envelhece em dias; Ficha de Fundação envelhece por evento. Sem carimbo de validade o Dossiê Vivo acumula vintages diferentes e o vencido é lido como vigente | Ativa |
| ADR-014 | 2026-08-06 | **Todo o valuation é nominal em BRL** (resolve Q-06). Fluxos projetados em reais nominais, descontados a custo de capital nominal. Detalhamento operacional na §4.7 | Elimina a variação entre sessões que tornaria os resultados não comparáveis entre empresas. Nominal também dispensa deflacionar as DFs históricas, que já são nominais | Ativa |
| ADR-015 | 2026-08-06 | **Petrobras (PETR4) entra no piloto como controle negativo do gate E0** (resolve Q-08), com escopo restrito a E0, sem qualquer intenção de investimento, e com **expectativa de veredito selada pelo sponsor antes da execução** | Um gate que nunca reprovou nada não foi validado, foi apenas executado. Petrobras é o caso canônico brasileiro de tensão entre controlador e minoritário, o que exercita o Bloco A de forma que ANIM3 provavelmente não exercita. A selagem prévia impede que o teste vire confirmação: se o veredito for produzido e só então comparado a uma expectativa formada depois, não se aprende nada | Ativa |

### 4.7 Convenção de valuation nominal (normativo — operacionaliza ADR-014)

1. **Fluxos de caixa** projetados em R$ nominais, com inflação embutida em crescimento de receita e na trajetória de margens.
2. **Taxa livre de risco** nominal em BRL: título público prefixado ou curva DI futuro de duration compatível com o horizonte da projeção. Fonte, vértice e data registrados no cabeçalho `as_of` do artefato.
3. **Regra de consistência — o erro mais comum.** Se a taxa de desconto é nominal, o crescimento e as margens têm de ser nominais. Descontar fluxo real com taxa nominal subavalia sistematicamente e produz um CAP implícito artificialmente curto. Toda execução de E1 verifica isso explicitamente antes de reportar.
4. **Crescimento na perpetuidade** nominal deve ser ≥ a inflação de longo prazo assumida, salvo tese explícita e justificada de encolhimento real do negócio.
5. **Toda premissa de inflação usada é declarada** no artefato e marcada `[F]` com fonte ou `[J]` como julgamento. Nunca implícita.
6. **Prêmio de risco de mercado, beta e ajustes específicos:** o método é fixado no Anexo E1 durante o passe 1 (§6.4) e registrado lá como normativo. Até que o Anexo E1 exista, nenhuma execução de E1 é considerada válida.

---

## 6. Modelo de Anexos — como cada etapa será detalhada

### 6.1 Regras
1. Cada etapa do loop (E0–E5) terá **um anexo próprio**, em arquivo separado, criado sob demanda ao longo das interações.
2. Um anexo só passa a valer com status **Aprovado** pelo sponsor. Até lá, é Rascunho e o mestre indica isso no índice (§6.3).
3. Anexos seguem obrigatoriamente o template da §6.2. Seções podem ser adicionadas; nenhuma pode ser removida.
4. Toda ferramenta definida em anexo segue a especificação da §6.2.1 (Ferramenta Replicável). Uma ferramenta só é considerada pronta após passar nos próprios critérios de aceite ao menos uma vez em empresa real do piloto.
5. Mudança em anexo aprovado = nova versão do anexo + entrada no changelog do anexo + atualização do índice no mestre. Mudanças que alterem **entradas ou saídas** de uma etapa exigem ADR no mestre (afetam o contrato entre etapas).
6. **Anexos são escritos por destilação de execução real, não em abstrato** (§6.4).

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
   (template exato de cada artefato produzido — formato é contrato;
    inclui cabeçalho §4.5 e marcação §4.4)
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
- Modelo executor: {Sonnet | Opus | humano} + **versão exata usada na validação** + condição de escalonamento
- Procedimento/corpo: (o prompt completo, ou o script, ou as fórmulas)
- Critérios de aceite: (condições verificáveis; inclui obrigatoriamente
  "100% dos campos [F] com referência resolvível")
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

### 6.4 Método de criação de anexos — destilação em 3 passes (normativo)

Escrever um anexo em abstrato produz um procedimento plausível que quebra no primeiro contato com documentos reais. O anexo é derivado de uma execução, nesta ordem:

- **Passe 1 — Execução manual assistida (Opus + sponsor).** Rodar a etapa no caso principal do piloto sem procedimento pré-escrito, com os documentos reais. Registrar em log: quais documentos foram abertos, em que ordem, o que foi procurado em cada um, onde a informação estava, onde **não** estava, e que julgamentos foram feitos. O log é o insumo do passe 2. Cronometrar (métrica M1).
- **Passe 2 — Destilação.** Transformar o log em Anexo v0.1: a §3 (Procedimento) é a transcrição do que de fato funcionou; a §4 (Saídas) é o artefato produzido no passe 1, esvaziado, virando template. Formato nasce de um caso real.
- **Passe 3 — Validação cruzada.** Rodar a ferramenta em Sonnet sobre a **mesma** empresa e comparar com o resultado do passe 1. **Divergência é falha de especificação do anexo, não erro do modelo** — corrige-se o anexo, não o output. Gera a métrica M2. Repetir até convergir; então o anexo vai para aprovação do sponsor.
- **Passe 4 (só para E0) — Controle negativo.** Rodar a ferramenta já estabilizada sobre o controle negativo (ADR-015) e avaliar M5. Se o veredito for indistinguível do caso principal, o anexo E0 volta para revisão em vez de ser aprovado.

---

## 7. Regras de atualização deste documento

1. **Fluxo:** qualquer sessão pode propor mudanças; o modelo apresenta o diff proposto (o que muda e por quê); **só o sponsor aprova**. Aprovado → nova versão do arquivo + entrada no Changelog (§11) + commit no repositório pelo sponsor ou via Claude Code (ADR-010).
2. **Versionamento semântico simplificado:**
   - **Major (1.0, 2.0):** mudança na estrutura do loop (adicionar/remover/reordenar etapas) ou na definição de sucesso.
   - **Minor (0.2, 0.3):** novas ADRs, mudanças em regras, atualização do índice de anexos, escopo.
   - Correções de texto sem efeito normativo não geram versão; entram na próxima.
3. **Imutabilidade do histórico:** ADRs e Changelog nunca são editados retroativamente — apenas acrescidos. A única alteração admitida numa ADR é a mudança de status por decisão registrada do sponsor, anotada na própria linha.
4. **Sincronização:** a fonte da verdade é o arquivo na branch `main` do repositório (ADR-010). Sessões de chat leem o repositório e entregam arquivos atualizados; o commit é do sponsor ou do Claude Code. Ao final de qualquer sessão que altere o mestre ou anexos, o modelo entrega os arquivos e indica explicitamente o que precisa ser commitado.
5. **Divergência:** se um modelo executor identificar contradição entre o mestre e um anexo, prevalece o mestre; a contradição deve ser reportada como pendência.
6. **Questões abertas (§9.2):** toda dúvida material vira item numerado (Q-xx) com dono da resposta (normalmente o sponsor). Resolvida → vira ADR ou texto normativo, e o item é marcado como resolvido (não apagado).

---

## 8. Roteamento de modelos e controle de custo

### 8.1 Princípio
**Opus cria e julga; Sonnet opera e mantém.** Toda ferramenta nova é executada pela primeira vez sob supervisão de Opus (ou com revisão de Opus); em regime, roda em Sonnet. Escalonamento para Opus (ou para o humano) é sempre definido por condição explícita na ferramenta, nunca por preferência. **A versão exata do modelo usada na validação de cada ferramenta é registrada (§6.2.1)** — ferramentas são calibradas contra um modelo específico e a reprodutibilidade depende disso.

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
- Sessões de manutenção (E5) devem operar apenas com: este mestre (bloco §0 + §3 + §4.4 + §4.5 + §8) + o anexo da etapa + o dossiê da empresa. Não carregar anexos de outras etapas.
- Dossiês Vivos têm seção de **sumário executivo** no topo (máx. 1 página, incluindo a Tabela de Estado dos Artefatos) para que sessões de triagem não precisem ler o dossiê inteiro.
- Toda ferramenta declara custo estimado por execução (§6.2.1); ferramentas de alto volume são as primeiras candidatas a virar script.

---

## 9. Roadmap e questões abertas

### 9.1 Backlog ordenado (próximos passos)
1. Aprovação deste documento (v0.3).
2. **Selar a expectativa de veredito de PETR4** (ADR-015). Feito pelo sponsor, antes de qualquer execução, em `expectativa-selada-PETR4.md`.
3. **Esqueleto mínimo do Dossiê Vivo** (estrutura de arquivo + sumário executivo + Tabela de Estado dos Artefatos). Precede E0 porque E0 já produz artefatos que precisam de casa.
4. Criar **Anexo E0 — Fundação** pelo método de destilação da §6.4, passes 1 e 2, sobre ANIM3.
5. Passe 3 (validação Sonnet sobre ANIM3) — mede M2.
6. Passe 4 (controle negativo PETR4) — mede M5. Se falhar, volta ao passo 4.
7. Aprovação do Anexo E0.
8. Criar Anexo E1 sobre ANIM3 (depende do consenso manual do sponsor e da fixação de prêmio de risco/beta, §4.7 item 6).
9. Anexo E5 completo, incorporando o que o uso real do esqueleto revelou.
10. Anexos E2, E3, E4, nessa ordem.
11. Retrospectiva do MVP: medir M1–M5; decidir o que vira script, o que muda de formato, e o pós-MVP.

### 9.2 Questões abertas
| ID | Questão | Dono | Status |
|---|---|---|---|
| Q-01 | Universo piloto | Sponsor | **Resolvida (2026-08-06): ANIM3 (loop completo) + PETR4 (E0 apenas, controle negativo)** |
| Q-02 | Fonte paga de dados estruturados: Economatica, Comdinheiro, nenhuma no MVP? | Sponsor | Aberta — **não bloqueante** após ADR-011 |
| Q-03 | Onde os arquivos serão versionados entre sessões | Sponsor | **Resolvida (2026-08-06) → ADR-010** |
| Q-04 | Nome definitivo do projeto ("Novo Loop" é placeholder) | Sponsor | Aberta — não bloqueante |
| Q-05 | Restrições de portfólio relevantes para E4 (limites de posição, liquidez mínima) | Sponsor | Aberta — necessária antes do Anexo E4 |
| Q-06 | Método de custo de capital em E1 | Sponsor | **Resolvida (2026-08-06): nominal → ADR-014 + §4.7** |
| Q-07 | Status da ADR-009 | Sponsor | **Resolvida (2026-08-06): aprovada, ADR-009 passa a Ativa** |
| Q-08 | Controle negativo do gate E0 | Sponsor | **Resolvida (2026-08-06): PETR4 → ADR-015** |
| Q-09 | Enquadramento do sistema no processo formal da gestora | Sponsor + compliance | **Explicitada na §10.** Escolha entre as três opções estruturais permanece aberta — necessária antes do Anexo E4 |

---

## 10. Enquadramento no processo da gestora (explicitação da Q-09)

Esta seção existe para que o estatuto dos artefatos não fique implícito. Ela **não** substitui avaliação do compliance da gestora, e nada aqui é orientação jurídica ou regulatória.

### 10.1 O que o sistema é
Uma camada de instrumentação do trabalho analítico: coleta, estrutura, mantém e torna auditável o entendimento sobre empresas. Produz papéis de trabalho.

### 10.2 O que o sistema não é
- Não é fonte de recomendação de investimento a terceiros.
- Não substitui o julgamento nem a responsabilidade do decisor humano. Nenhum artefato é decisão; a decisão é do sponsor, que a assume nominalmente.
- Não é, por si só, o registro formal de decisão de investimento da gestora — a menos que a opção C abaixo seja escolhida deliberadamente.

### 10.3 As três opções estruturais (decisão pendente do sponsor + compliance)

| Opção | Descrição | Implicação |
|---|---|---|
| **A — Paralela** | Artefatos são papéis de trabalho do analista. O processo formal da gestora segue intocado e independente | Menor atrito. O sistema não herda nenhuma obrigação de guarda ou formato. Custo: o racional documentado não chega ao registro formal, e parte do valor de auditabilidade se perde |
| **B — Alimentadora** | O Memo de Decisão vira anexo/insumo do registro formal existente, sem substituí-lo | Provavelmente o melhor custo-benefício. Exige compatibilizar retenção e formato com a política da gestora, e deixar claro no memo que ele é insumo, não decisão |
| **C — Substitutiva** | O Memo de Decisão **é** o registro formal de decisão | Maior valor de processo e maior exigência: retenção, imutabilidade, trilha de revisão e aprovação passam a ter requisito regulatório. Só faz sentido com validação prévia do compliance |

### 10.4 Requisitos que valem em qualquer das opções
1. **Autoria e responsabilidade são humanas.** Todo Memo de Decisão identifica o decisor humano; o modelo é instrumento, e isso fica escrito no próprio artefato.
2. **Conteúdo gerado por modelo é identificável como tal.** Já garantido pela marcação `[F]`/`[I]`/`[J]` (§4.4) e pelo campo `executor` (§4.5).
3. **Reconstrutibilidade.** Qualquer conclusão tem de ser rastreável até o documento-fonte. Já garantido pela §4.4.
4. **Retenção.** Artefatos vencidos não são apagados (§4.5). Se a opção B ou C for escolhida, o prazo e o meio de guarda passam a seguir a política da gestora, não a conveniência do repositório.
5. **Ausência de recomendação a terceiros.** Os artefatos circulam internamente; qualquer uso externo exige avaliação prévia.

### 10.5 Ponto de atenção específico do controle negativo
PETR4 entra sob ADR-015 com escopo de teste de ferramenta e **sem intenção de investimento**. Todo artefato produzido sobre PETR4 carrega, no cabeçalho, a marcação `finalidade: teste-de-ferramenta`, para que uma Ficha de Fundação de teste nunca seja lida no futuro como avaliação de investimento — inclusive por uma sessão futura deste próprio sistema, que lê o dossiê sem contexto humano.

---

## 11. Changelog

| Versão | Data | Mudanças |
|---|---|---|
| 0.1 | 2026-08-06 | Criação do documento: visão, escopo do MVP, especificação do loop (E0–E5), arquitetura de dados, ADR-001 a ADR-009, modelo de anexos e de ferramentas replicáveis, regras de atualização, roteamento de modelos, backlog e questões abertas |
| 0.2 | 2026-08-06 | Q-01 resolvida (piloto = ANIM3). Q-03 resolvida → ADR-010 (repositório GitHub como fonte da verdade; sessões de chat não escrevem). ADR-011 (consenso como input manual; DCF reverso / preço-como-consenso como plano B). ADR-012 + §4.4 (rastreabilidade e marcação [F]/[I]/[J]). ADR-013 + §4.5 (política de validade P/F/D). §1.5 (critérios M1–M4). §4.6 (formato canônico de tripwire). §6.4 (destilação em 3 passes). §9.1 reordenado. §4.3 alinhado ao repositório. Novas questões Q-06 a Q-09 |
| 0.3 | 2026-08-06 | Q-06 resolvida → ADR-014 + §4.7 (valuation nominal em BRL, com regra de consistência e piso de crescimento na perpetuidade). Q-07 resolvida → ADR-009 passa a Ativa; §7.3 passa a admitir mudança de status registrada. Q-08 resolvida → ADR-015 (PETR4 como controle negativo, escopo E0, expectativa selada antes da execução) + métrica M5 + passe 4 na §6.4 + S1 reescrito. Q-09 explicitada na nova §10 (três opções estruturais, requisitos comuns, marcação `finalidade: teste-de-ferramenta`); escolha entre A/B/C segue aberta. §2.1/§2.2, §4.2, §4.3 e §9.1 atualizados para o piloto de dois casos. Changelog passa a §11 |
