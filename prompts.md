# Registro de prompts usados com IA — Empresta Aí

**Aluno:** [PREENCHER PELO ALUNO]  
**Ferramenta:** Claude Code (extensão VS Code), modelo Claude Fable 5.1 (`claude-fable-5-1`), com o plugin Superpowers (habilidade `brainstorming`, usada apenas para especificação).  
**Status:** interações registradas conforme ocorreram em 30/10/2026. Nenhuma interação anterior a esta sessão é conhecida pelo Claude; se o aluno tiver usado IA antes (por exemplo, para gerar `DECISOES_PARA_APROVAR.md`), deve acrescentar aqui o texto literal desses prompts.

> A página 6 do enunciado exige registrar os prompts utilizados e marcar, na seção 11 de `spec.md`, toda regra sugerida pela IA e aceita pelo aluno. Este arquivo transcreve literalmente cada prompt enviado nesta sessão e resume o resultado real.

---

## Interação 00 — comandos de ambiente (sem conteúdo de especificação)

**Data:** 30/10/2026  
**Comandos digitados:** `/plugin install superpowers@claude-plugins-official` (duas vezes).  
**Resultado:** o terminal respondeu "/plugin isn't available in this environment". O plugin já estava disponível na sessão; a habilidade `brainstorming` foi invocada pelo Claude na Interação 01.

---

## Interação 01 — análise do enunciado e das decisões propostas

**Data:** 30/10/2026  
**Ferramenta/modelo:** Claude Code / Claude Fable 5.1  
**Modo:** Plan (somente leitura)  
**Objetivo:** analisar o enunciado, conferir escopo e perguntas obrigatórias, avaliar DEC01–DEC26, identificar lacunas e preparar checklist de conformidade, sem editar arquivos.

**Prompt enviado (literal):**

```text
Atue como analista sênior de requisitos e especialista em Specification-Driven Development (SDD).

Estamos preparando minha entrega acadêmica individual "Empresta Aí", com prazo de 03/11/2026.

Leia integralmente os arquivos:
- @enunciado.pdf
- @CLAUDE.md
- @DECISOES_PARA_APROVAR.md
- @spec.md
- @prompts.md

Use a habilidade brainstorming do Superpowers, se disponível, somente para especificação de requisitos.

O objetivo é produzir a melhor especificação possível segundo a rubrica oficial do professor. NÃO é permitido implementar o aplicativo.

PRIMEIRA ETAPA:

1. Analise as seis páginas do enunciado oficial.
2. Confira as nove funcionalidades obrigatórias e as quatro exclusões de escopo.
3. Analise as 26 propostas em DECISOES_PARA_APROVAR.md.
4. Confira se elas resolvem todas as perguntas obrigatórias do professor.
5. Identifique ambiguidades, inconsistências e decisões adicionais indispensáveis.
6. Apresente uma tabela consolidada de decisões, alternativas e justificativas.
7. Priorize simplicidade, consistência, requisitos verificáveis e cobertura integral da rubrica.
8. Prepare um checklist de conformidade que usaremos na revisão final.

Não considere nenhuma decisão aprovada sem minha confirmação expressa.

Não crie código, não altere arquivos e não inicie a implementação.

Faça a análise completa agora e apresente as decisões para minha aprovação conjunta.
```

**Resposta e resultado:** o Claude extraiu o texto das 6 páginas do PDF (a renderização direta não estava disponível no ambiente), confirmou as 9 funcionalidades e 4 exclusões, confirmou que DEC01–DEC26 tocam os 7 blocos de perguntas do enunciado e listou 20 lacunas (L1–L20) que um implementador ainda teria de adivinhar, entre elas: "atrasado" como estado ou filtro; definição das listagens; ciclo de vida do objeto; origem do estado na saída; validações de dano, retificação e contestação; data de devolução retroativa; perda × não devolução e recuperação; devolução parcial × renovação × perda; cancelamento de registro indevido; data de saída retroativa; horário e definição de aviso; detalhes de renovação; limites de campos e categorias; exclusão × arquivamento; limite por contato; edição e histórico; autenticação do dono; procedimento LGPD; RNFs mensuráveis; rigidez da DEC20. Propôs DEC27–DEC48 (depois renumeradas DEC27–DEC49) como hipóteses, com alternativas e justificativas, e um checklist de conformidade C1–C10 baseado na rubrica. Nenhum arquivo foi alterado além do arquivo de plano interno do Claude Code.  
**Sugestões da IA aceitas:** nenhuma nesta interação (apenas análise).  
**Sugestões alteradas/rejeitadas:** nenhuma.  
**Decisões associadas:** DEC01–DEC26 (análise); DEC27–DEC49 (propostas).  
**Modificações em `spec.md`:** nenhuma.

---

## Interação 02 — aprovação do lote inicial de decisões

**Data:** 30/10/2026  
**Ferramenta/modelo:** Claude Code / Claude Fable 5.1  
**Modo:** Plan → aprovação do plano de trabalho  
**Objetivo:** aprovar DEC01–DEC26 e pedir que decisões adicionais fossem apresentadas antes da redação.

**Prompt enviado (literal):**

```text
Li as decisões DEC01 a DEC26 do documento e aprovo as propostas e justificativas sem alterações.

Caso tenha identificado decisões adicionais indispensáveis, apresente-as para minha aprovação antes de elaborar a especificação.

Quero que todas as regras aprovadas fiquem registradas com justificativas e rastreabilidade, conforme o professor exige.
```

**Resposta e resultado:** o Claude registrou DEC01–DEC26 como aprovadas pelo aluno em 30/10/2026 e reapresentou, em tabela, as decisões adicionais DEC27–DEC49 (DEC48 agrupa esclarecimentos de redação às aprovadas; DEC49 complementa a DEC20 com o aviso "Perda possível" aos 30 dias e oferece as alternativas B e C). Pediu ainda: nome do aluno, confirmação de ausência de prompts anteriores e confirmação das justificativas.  
**Sugestões da IA aceitas:** DEC01–DEC26, com as justificativas do arquivo `DECISOES_PARA_APROVAR.md`, assumidas pelo aluno.  
**Sugestões alteradas/rejeitadas:** nenhuma.  
**Decisões associadas:** DEC01–DEC26 (aprovadas); DEC27–DEC49 (apresentadas, pendentes).  
**Modificações em `spec.md`:** nenhuma.

---

## Interação 03 — redação completa da especificação

**Data:** 30/10/2026  
**Ferramenta/modelo:** Claude Code / Claude Fable 5.1  
**Modo:** edição de arquivos (Accept Edits / auto)  
**Objetivo:** preencher integralmente `spec.md` e atualizar `prompts.md`.

**Prompt enviado (literal):**

```text
Agora produza a entrega acadêmica completa do Empresta Aí.

Utilize exclusivamente o enunciado oficial e as decisões de produto que aprovei expressamente.

Preencha integralmente spec.md, seguindo as 12 seções obrigatórias:

1. Visão
2. Escopo
3. Glossário
4. Atores e permissões
5. Regras de negócio
6. Histórias de usuário
7. Estados do empréstimo
8. Modelo de dados
9. Casos de borda e erros
10. Requisitos não funcionais
11. Decisões e premissas
12. Questões em aberto

CRITÉRIOS OBRIGATÓRIOS:

- Regras RNxx numeradas, atômicas e testáveis.
- Critérios Dado/Quando/Então para todas as histórias de usuário.
- Cenários concretos de sucesso, falha e situações-limite.
- Todas as funcionalidades e perguntas do PDF respondidas.
- Transições de estados válidas e completas.
- Modelo de dados coerente com as regras.
- Histórico de objetos e contatos corretamente especificado.
- Tratamento de danos, devoluções parciais e perdas.
- Requisitos de privacidade e LGPD.
- Decisões aprovadas, alternativas consideradas e justificativas.
- Rastreabilidade entre histórias, regras, critérios e casos de borda.

Atualize prompts.md com o registro fiel dos prompts efetivamente utilizados. Inclua os prompts desta sessão; não invente interações anteriores nem registre uma revisão como executada antes de realizá-la.

Revise o documento inteiro contra a rubrica oficial, especialmente os critérios que representam 25% cada: regras de negócio e critérios de aceite.

Corrija inconsistências editoriais e lacunas que possam ser resolvidas pelas decisões já aprovadas. Se faltar uma decisão de produto, pergunte antes de inventar uma regra.

O resultado deve permitir que outra pessoa implemente o aplicativo sem perguntar nada ao autor.

NÃO implemente o aplicativo. NÃO crie arquivos de código.

Ao terminar, informe quais arquivos foram modificados e quais requisitos foram verificados.
```

**Resposta e resultado:** o Claude preencheu as 12 seções de `spec.md` (96 regras RN01–RN96; 17 histórias HU01–HU17 com cenários Dado/Quando/Então de sucesso, erro e limite; tabela de transições do empréstimo e do status do objeto; modelo de dados com 9 entidades; 68 casos de borda; 10 RNFs; 49 decisões; 5 questões em aberto). Como apenas DEC01–DEC26 tinham aprovação expressa, DEC27–DEC49 foram marcadas "Aguarda aprovação" na seção 11 e listadas na seção 12; nas escolhas abertas o rascunho adotou DEC37-A e DEC49 (manter DEC20). O Claude executou, nesta mesma interação, uma verificação mecânica de rastreabilidade (toda RN e DEC citada existe; ausência de termos vagos e de placeholders fora do cabeçalho e da seção 12), cujo resultado consta da resposta ao aluno. A revisão de "implementador cético" sugerida na página 6 do enunciado **ainda não foi executada** e será registrada como Interação 04 quando ocorrer.  
**Sugestões da IA aceitas:** pendentes de confirmação do aluno (DEC27–DEC49).  
**Sugestões alteradas/rejeitadas:** nenhuma até o momento.  
**Decisões associadas:** DEC01–DEC49.  
**Modificações em `spec.md`:** redação integral das 12 seções a partir do template.

---

[AS PRÓXIMAS INTERAÇÕES — aprovação de DEC27–DEC49, revisão cética e correções — SERÃO ACRESCENTADAS QUANDO OCORREREM.]
