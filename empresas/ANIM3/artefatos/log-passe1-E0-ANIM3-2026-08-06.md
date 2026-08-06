---
artefato: log-passe1
etapa: E0
empresa: ANIM3
classe_validade: C          # contêiner (ADR-016)
finalidade: analise
as_of: 2026-08-06
status: Vigente
depende_de: [indice-fontes.md]
executor: {modelo: Claude Opus 4.5 (sessão de chat), revisor: pendente — sponsor}
---

# Log do passe 1 — E0 sobre ANIM3 (sessão 1, parcial)

Registro exigido pela §6.4: quais documentos foram abertos, em que ordem, o que foi procurado, onde a
informação estava, **onde não estava**, e que julgamentos foram feitos. É o insumo do passe 2.

**Escopo desta sessão:** teste de viabilidade da coleta por busca web (alternativa 2 escolhida pelo
sponsor). Não é o passe 1 completo — **nenhuma Ficha de Fundação foi produzida** e nenhum gate foi
fechado, porque a fonte principal do Bloco A não pôde ser lida inteira.

---

## 1. Sequência executada

| # | Ação | O que eu procurava | Resultado |
|---|---|---|---|
| 1 | Busca web: "Ânima Educação RI estatuto social ANIM3" | Porta de entrada do RI | Achou RI + página de composição acionária |
| 2 | Abriu página de Composição Acionária (S-02) | Estrutura de controle, free float | **Achou.** Tabela de participações + free float |
| 3 | Abriu índice de FRE (S-05) | Localizar o FRE mais recente | Achou lista por ano; links de download diretos |
| 4 | Baixou FRE 2025 v6 (S-03) | Partes relacionadas, controle, remuneração | **Parcial** — só o começo do documento. Ver F3 |
| 5 | Testou download do FRE pelo container (`curl`) | Contornar o truncamento | **Bloqueado** — HTTP 403, ver F2 |
| 6 | Abriu índice de Estatutos (S-04) | Localizar o estatuto vigente | Achou |
| 7 | Baixou Estatuto Social 11/03/2025 (S-01) | Direitos do minoritário | **Achou, integral** (19 pp.) |

**Custo:** 8 chamadas de ferramenta. Tempo de execução das ferramentas na ordem de minutos.
`[J]` M1 não foi medida: exige o cronômetro do sponsor sobre a sessão inteira, incluindo o tempo de
leitura humana. O que este log mede é só o custo de coleta.

---

## 2. Achados operacionais (o que muda o desenho da ferramenta de E0)

**F1 — Sites de RI no padrão PRISMA/MZ são navegáveis por busca web.** Páginas HTML, índices por ano
e links `Download.aspx` funcionam. PDFs voltam com texto extraído. A coleta automatizada é viável
para esta classe de emissor.

**F2 — O container não tem rede para o site do emissor.** `curl` retorna `403 host_not_allowed`. Só a
busca web alcança o documento, e ela entrega texto, não arquivo. Consequências: (a) não dá para
guardar cópia local em `/fontes/`; (b) não dá para usar extração por intervalo de páginas; (c) a
resolubilidade das referências `[F]` fica ancorada em URL, que é frágil — daí o `indice-fontes.md`.

**F3 — Documento grande é truncado pelo começo, sem controle de trecho.** O FRE 2025 v6 tem 313
páginas; a leitura chegou à p. 24. **Todo o Bloco A vive depois disso:** acordos de acionistas
(p. 80), posição acionária e organograma (pp. 180–190), administração (pp. 192–233), remuneração
(pp. 236–272) e transações com partes relacionadas (pp. 290–293). Não há paginação na ferramenta, e
o F2 fecha a alternativa. **A coleta por web falha exatamente no documento mais importante de E0.**

**F4 — Documentos pequenos são lidos integralmente.** O Estatuto Social (19 pp.) veio completo e
sustenta sozinho boa parte do gate A4. O corte prático fica na casa de algumas dezenas de páginas.

**F5 — O índice do FRE é barato e é o mapa.** A primeira leitura truncada, que falhou em trazer o
conteúdo, trouxe o índice com **números de página por item**. Isso não é consolo: é o insumo de um
protocolo melhor, descrito abaixo.

---

## 3. Decorrência de desenho — protocolo híbrido para o Anexo E0

`[J]` Nem "sponsor anexa tudo" nem "modelo busca tudo". A divisão que os achados sugerem:

1. **Modelo, sozinho:** localiza o RI, identifica a versão vigente de cada documento, lê integralmente
   estatuto, acordo de acionistas, atas e fatos relevantes, e **lê o índice do FRE**.
2. **Modelo → sponsor:** devolve a lista exata de intervalos de páginas do FRE e da DF que precisa,
   já com os números de página do índice.
3. **Sponsor:** anexa só esses trechos.
4. **Modelo:** preenche os gates.

O passo 2 é o que torna o passo 3 barato: o sponsor extrai ~40 páginas indicadas em vez de anexar
um PDF de 313. Este protocolo deve virar o passo 1 do procedimento (§3) do Anexo E0, e a lista de
páginas do item 6 abaixo é o primeiro caso de teste dele.

**Segunda decorrência:** a ferramenta F-E0-01 precisa de um critério de aceite adicional —
*"nenhum gate é fechado com base em documento lido parcialmente; documento truncado é registrado
como leitura parcial e o gate correspondente fica PENDENTE"*. Sem isso, o modo de falha óbvio é
fechar o gate A2 com o que dá para ler das primeiras páginas do FRE, que é justamente onde está a
descrição institucional que a companhia escreve sobre si mesma.

---

## 4. Onde a informação estava — mapa por gate

| Gate | Fonte que resolve | Situação |
|---|---|---|
| A1 — Histórico do controlador sob conflito | FRE 1.13, 6.x; atas; fatos relevantes | Não coletado |
| A2 — Partes relacionadas | FRE 11.1–11.3 + nota da DF | Não coletado |
| A3 — Alocação de capital | FRE 1.1 (histórico de M&A) + DFs | **Parcialmente ao alcance** — FRE 1.1 foi lido |
| A4 — Direitos do minoritário | **Estatuto Social** + segmento B3 | **Coletado** — ver §5 |
| A5 — Incentivos da gestão | FRE 8.1–8.19 | Não coletado |
| A6 — Taxa base da classe de referência | Externa ao emissor | Não iniciado |
| B1–B4 — Defensibilidade | CADE, dados setoriais, MEC, concorrentes | Não iniciado |

`[J]` Observação para o passe 2: o gate A4 é resolvido por um documento pequeno, estável e público.
É o candidato natural a primeiro gate automatizado, e provavelmente roda em Sonnet sem supervisão.
Os gates A1/A2 são o oposto — dependem do documento mais caro de ler e de julgamento sobre padrão de
comportamento, o que confirma o roteamento já previsto na §8.2 do mestre.

---

## 5. Extração do gate A4 (única com material suficiente)

Fonte: S-01, Estatuto Social consolidado de 11/03/2025.

- `[F]` Companhia listada no **Novo Mercado** da B3, sujeitando companhia, controladores e
  administradores ao respectivo Regulamento `[Estatuto Social | 2025-03-11 | Art. 1º, par. único]`
- `[F]` Capital de R$ 2.569.624.313,76 dividido em 403.868.805 ações **ordinárias**, todas nominativas
  `[Estatuto Social | 2025-03-11 | Art. 5º]`
- `[F]` **Vedada** a emissão de ações preferenciais `[Estatuto Social | 2025-03-11 | Art. 5º, §3º]` e
  de partes beneficiárias `[Estatuto Social | 2025-03-11 | Art. 5º, §2º]`
- `[F]` Capital autorizado até R$ 4.000.000.000,00, por deliberação do Conselho
  `[Estatuto Social | 2025-03-11 | Art. 5º, §4º]`
- `[F]` **Tag along:** alienação direta ou indireta de controle exige OPA aos demais acionistas com
  tratamento igualitário ao do alienante `[Estatuto Social | 2025-03-11 | Art. 21]`
- `[F]` **Cláusula de dispersão ("poison pill"):** quem atingir 20% deve lançar OPA pela totalidade em
  até 60 dias, a preço não inferior ao maior entre valor econômico e o da última OPA dos 24 meses
  anteriores corrigida pelo IPCA `[Estatuto Social | 2025-03-11 | Art. 22 e §2º]`
- `[F]` A OPA do Art. 22 **pode ser dispensada** por AGE, por maioria absoluta dos presentes, sem
  computar as ações do adquirente `[Estatuto Social | 2025-03-11 | Art. 22, §4º]`
- `[F]` Negócios com administradores e/ou Acionista Controlador dependem de deliberação do Conselho e
  devem ser em condições estritamente comutativas
  `[Estatuto Social | 2025-03-11 | Art. 14, (xxiii)]`
- `[F]` Conselho de 5 a 9 membros, mínimo de 2 ou 20% independentes, mandato unificado de 2 anos
  `[Estatuto Social | 2025-03-11 | Art. 12 e §1º]`
- `[F]` **Vedada** a acumulação dos cargos de Presidente do Conselho e Diretor Presidente
  `[Estatuto Social | 2025-03-11 | Art. 10, par. único]`
- `[F]` Conselho Fiscal **não permanente** `[Estatuto Social | 2025-03-11 | Art. 19]`
- `[F]` Dividendo obrigatório de 25% do lucro líquido ajustado
  `[Estatuto Social | 2025-03-11 | Art. 26, (iv)]`
- `[F]` Arbitragem obrigatória na Câmara de Arbitragem do Mercado
  `[Estatuto Social | 2025-03-11 | Art. 28]`

**Composição acionária** — fonte S-02, atualizada Apr/26:

- `[F]` Acionistas Originais 29,7%; Perea Capital 7,7%; Organon Capital 5,0%; Outros 51,1%;
  Tesouraria 6,4% `[página de RI — Composição Acionária | atualizada Apr/26 | tabela]`
- `[F]` Em 17/04/2026, 256.720.748 ações ordinárias em circulação, ~63,6% do capital
  `[página de RI — Composição Acionária | 2026-04-17 | nota (1)]`

### Tensão que precisa ser resolvida antes de fechar A4

`[I]` O estatuto legisla repetidamente sobre "Acionista Controlador" (Arts. 21, 22 §9º (vii) e (viii),
14 (xxiii)), mas o bloco de Acionistas Originais aparece com 29,7% — abaixo de maioria. Deriva de
S-01 e S-02 combinados. As duas leituras possíveis são materialmente diferentes para o Bloco A:
controle exercido por acordo de acionistas sobre base dispersa, ou ausência de controlador definido
com o estatuto apenas prevendo a hipótese. **Não decido isso sem o Acordo de Acionistas e o FRE 6.1/6.5.**

`[J]` Este é o achado mais relevante da sessão para o desenho do gate: A4 não se fecha só com o
estatuto, apesar de o estatuto trazer quase todos os campos. O gate precisa de uma regra explícita de
que a arquitetura de direitos só é avaliável depois de identificado **quem** exerce controle — caso
contrário, o modelo lê "Novo Mercado + tag along + só ON + poison pill" e classifica como boa
arquitetura, sem ter estabelecido contra quem essa arquitetura protege.

---

## 6. Lista para o sponsor — trechos a anexar na próxima sessão

Do **FRE mais recente** (verificar antes se há versão 2026; a paginação abaixo é da versão 2025 v6):

| Item | Páginas | Para |
|---|---|---|
| 1.13 Acordos de acionistas | 80–81 | A1, A4 |
| 6.1/6.2 Posição acionária | 180–185 | A1, A4 |
| 6.3 Distribuição de capital | 186 | A4 |
| 6.5 Organograma dos acionistas e do grupo econômico | 190 | A1 |
| 7.5 Relações familiares | 230 | A1 |
| 7.6 Relações de subordinação, prestação de serviço ou controle | 231 | A1 |
| 8.17 Percentual de partes relacionadas na remuneração | 267 | A2, A5 |
| 8.19 Remuneração reconhecida do controlador/controlada | 270–271 | A2, A5 |
| 11.1 Regras, políticas e práticas | 290 | A2 |
| 11.2 Transações com partes relacionadas | 291–292 | A2 |

Mais: **Acordo de Acionistas** (link no `indice-fontes.md`) e a **nota de partes relacionadas** da DF
anual mais recente. Total estimado: ~40 páginas.

---

## 7. Changelog

| Versão | Data | Mudanças |
|---|---|---|
| 0.1 | 2026-08-06 | Sessão 1 do passe 1: teste de viabilidade da coleta por web. Achados F1–F5, protocolo híbrido proposto, gate A4 extraído parcialmente, lista de trechos para a próxima sessão |
