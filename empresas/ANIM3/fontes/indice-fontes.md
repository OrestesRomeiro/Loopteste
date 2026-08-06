---
artefato: indice-fontes
empresa: ANIM3
classe_validade: C          # contêiner (ADR-016)
finalidade: analise
as_of: 2026-08-06
status: Vigente
depende_de: []
executor: {modelo: Claude Opus 4.5 (sessão de chat), revisor: pendente — sponsor}
---

# Índice de fontes — ANIM3

Existe porque a §4.4 (regra 3) define referência resolvível como "o sponsor consegue abrir o documento
e localizar a informação". Como a coleta foi feita por busca web e **não foi possível baixar os
arquivos para o repositório** (ver log do passe 1, achado F2), a resolubilidade depende deste índice:
ele guarda a URL exata, a data do documento e a data do acesso.

**Risco assumido:** URLs de site de RI mudam. Toda fonte marcada `[web]` deve ser substituída por
cópia local do PDF quando o sponsor puder baixá-la. Até lá, a referência é resolvível mas frágil.

## Documentos abertos nesta sessão

| ID | Documento | Data do doc. | Acesso | URL | Estado |
|---|---|---|---|---|---|
| S-01 | Estatuto Social consolidado | 2025-03-11 | 2026-08-06 | `ri.animaeducacao.com.br/Download.aspx?Arquivo=5U6MepDdxQ2oJlxXQdXbKw==` | **Lido integralmente** (19 pp.) |
| S-02 | Composição Acionária (página de RI) | atualizada Apr/26 | 2026-08-06 | `ri.animaeducacao.com.br/show.aspx?idCanal=Bs92ddTDFnbrw6vY42R3/g==` | Lido |
| S-03 | Formulário de Referência 2025 — Versão 6 (base 31/12/2025) | 2025-12-19 | 2026-08-06 | `ri.animaeducacao.com.br/Download.aspx?Arquivo=nRK7Nxr5Ui2CsIvQ1mOy9g==` | **Lido só até ~p. 24 de 313** — ver achado F3 |
| S-04 | Índice de Estatutos, Códigos e Políticas | — | 2026-08-06 | `ri.animaeducacao.com.br/list.aspx?idCanal=2AJ8uFGEhp0l489FyfEftg==` | Lido (navegação) |
| S-05 | Índice de Formulários de Referência | — | 2026-08-06 | `ri.animaeducacao.com.br/List.aspx?idCanal=6ql8C4wok4Z/nWa4YULT8w==&ano=2025` | Lido (navegação) |

**Nota sobre S-03.** A aba de ano do índice S-05 lista **2026**, o que indica existir FRE mais recente
que o lido. `[I]` A versão de 2026 não foi aberta nesta sessão. Nenhuma afirmação `[F]` deste dossiê
deve ser tratada como a informação mais atual do FRE até que essa versão seja verificada.

## Documentos identificados e ainda não abertos

| Documento | Onde | Por que importa em E0 |
|---|---|---|
| FRE mais recente (2026) | índice S-05, aba 2026 | Todos os itens abaixo |
| FRE item 1.13 — Acordos de acionistas | S-03, p. 80 | Bloco A — quem de fato controla |
| FRE itens 6.1/6.2, 6.3, 6.5 — posição acionária, distribuição de capital, organograma | S-03, pp. 180–190 | Bloco A — arquitetura de controle |
| FRE item 7.x — administração, relações familiares, subordinação | S-03, pp. 192–233 | Bloco A — independência do conselho |
| FRE item 8.x — remuneração, incl. 8.17 % partes relacionadas na remuneração | S-03, pp. 236–272 | Bloco A — incentivos da gestão |
| FRE itens 11.1–11.3 — transações com partes relacionadas | S-03, pp. 290–293 | Bloco A — gate A2 |
| Acordo de Acionistas | `ri.animaeducacao.com.br/list.aspx?idCanal=8DcPYUjIcwmrEe31bWQrMQ==` | Bloco A — direitos e amarras |
| DF anual + nota de partes relacionadas | Central de Resultados | Bloco A — números, não só regras |
| Decisões CADE (Laureate 2021 e anteriores) | CADE | Bloco B — estrutura competitiva |
