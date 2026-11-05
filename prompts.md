# Registro de prompts usados com IA — Empresta Aí

**Aluno:** Mateus Pinheiro Silva Rodrigues  
**Atualização:** 05/11/2026
**Ferramentas:** Claude Code (interações 01–03 em 30/10/2026 e 06–07 em 05/11/2026), Codex/OpenAI (interações 04–05 em 03/11/2026, referida pelo aluno como ChatGPT Work).
**Status:** registro das interações conhecidas. A versão de 03/11/2026 (DEC01–DEC57 antigas) está preservada no commit `468ab41`; a revisão MVP de 05/11/2026 renumerou as decisões como DEC01–DEC27 em `spec.md`. O aplicativo não foi implementado.

## Proveniência e limitações do histórico

- As interações 00–03 foram obtidas de prompts.md do repositório original, commit `8ef694b55c90408202ef60c0f82abebeaafdfc2a`. Seus prompts e relatos foram preservados como histórico recebido, não como execução desta sessão.
- O cabeçalho anterior informava: Claude Code, extensão VS Code, modelo “Claude Fable 5.1” (`claude-fable-5-1`), com Superpowers/brainstorming. Essa identificação de modelo é informação do registro anterior e não foi verificada independentemente nesta sessão.
- As conversas externas de Claude, CLAUDE.md e DECISOES_PARA_APROVAR.md não estavam publicados no repositório consultado. Não foi possível recuperar ou transcrever prompts anteriores aos registrados; não se presume que não tenham existido. O PDF original foi fornecido separadamente pelo aluno e lido integralmente nesta revisão.
- O ambiente efetivamente usado nas interações 04–05 foi Codex/OpenAI. A expressão “ChatGPT Work” nos prompts do aluno foi mantida literalmente; não designa uma segunda sessão que tenha ocorrido fora deste registro.
- O nome e o status do cabeçalho foram atualizados. Afirmações como “ainda não foi executada” nas interações históricas descrevem o estado em 30/10/2026. A revisão posterior está registrada nas interações 04–05.
- Não se atribuem a Claude Code as correções, novas propostas DEC50–DEC57 ou auditorias executadas no Codex. A aprovação autoral das escolhas é do aluno, não da IA.
- Marcadores de preenchimento e referências a rascunho que aparecem dentro de prompts literais são citações históricas do pedido, não campos pendentes deste documento.
- As interações 06–07 (05/11/2026) ocorreram no Claude Code, extensão VS Code, modelo Claude Fable 5.1 (`claude-fable-5-1`), identificação informada pelo próprio ambiente. Nada delas é atribuído ao Codex, e nenhuma correção das interações 04–05 é atribuída ao Claude. Os números DEC citados nas interações 00–05 referem-se à numeração antiga (DEC01–DEC57); a spec MVP usa DEC01–DEC27 novas, com coluna de origem.

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

## Interação 04 — conferência das fontes e lote de decisões pendentes

**Data:** 03/11/2026  
**Ferramenta:** Codex/OpenAI; integração GitHub; leitura e renderização do PDF com ferramentas locais.  
**Objetivo:** verificar o repositório atual e o enunciado, distinguir regras do professor de escolhas de produto e apresentar somente pendências para aprovação conjunta.

**Prompt enviado (literal, conteúdo do texto colado pelo aluno):**

````text
# MISSÃO: FINALIZAR INTEGRALMENTE O TRABALHO ACADÊMICO SDD — EMPRESTA AÍ

Atue como analista sênior de requisitos, especialista em Specification-Driven Development (SDD), engenharia de software, documentação técnica, controle de qualidade e GitHub.

Sua responsabilidade é concluir um trabalho acadêmico já desenvolvido parcialmente com Claude Code, realizando as correções necessárias, concluindo os entregáveis e atualizando o repositório GitHub original.

**Prazo de entrega: 03/11/2026 — hoje.**

Quero o trabalho finalizado e pronto para encaminhar ao professor pelo WhatsApp.

## 1. CONTEXTO E FONTES

O trabalho é uma atividade individual denominada **"Empresta Aí — Especificação SDD"**.

Repositório GitHub original:

https://github.com/Mateuspsr/empresta-ai-sdd

Arquivos principais:
- https://github.com/Mateuspsr/empresta-ai-sdd/blob/main/spec.md
- https://github.com/Mateuspsr/empresta-ai-sdd/blob/main/prompts.md

O enunciado original chama-se `Trabalho SDD — App Empresta Aí.pdf` e possui seis páginas. Vou anexá-lo a esta conversa ou disponibilizá-lo entre os arquivos acessíveis.

**Ordem de prioridade das fontes:**

1. Enunciado original do professor.
2. Decisões de produto efetivamente aprovadas por mim.
3. Especificação e histórico existentes no GitHub.
4. Instruções deste prompt.

Não invente exigências do professor. Diferencie obrigações do enunciado de decisões de produto e de sugestões da IA.

Se o PDF não estiver acessível, informe essa limitação antes de afirmar que fez uma auditoria completa.

## 2. OBJETIVO ACADÊMICO

O professor solicitou a elaboração de uma especificação de software suficientemente precisa para permitir que outra pessoa implemente o aplicativo sem precisar perguntar nada ao autor.

**Não desenvolver o aplicativo.**

Não criar frontend, backend, banco de dados executável, interfaces funcionais ou código de produção.

A entrega oficial exige dois arquivos Markdown em um repositório Git:

- `spec.md`: especificação completa.
- `prompts.md`: registro fiel do uso da inteligência artificial.

Arquivos auxiliares e versões para apresentação podem ser criados separadamente, mas não devem substituir os entregáveis oficiais.

## 3. HISTÓRICO DO TRABALHO JÁ REALIZADO

O projeto foi preparado no VS Code com Claude Code e o plugin Superpowers, utilizando a habilidade brainstorming para apoiar a análise de requisitos.

Foram produzidos e trabalhados os arquivos:
- `enunciado.pdf`
- `CLAUDE.md`
- `DECISOES_PARA_APROVAR.md`
- `spec.md`
- `prompts.md`

O documento de decisões inicial continha 26 propostas, posteriormente aprovadas pelo aluno em uma interação registrada no histórico.

O Claude Code realizou a análise do enunciado e identificou lacunas adicionais. Em seguida, elaborou a especificação completa em formato de rascunho.

**Estado verificado anteriormente no GitHub:**

- 96 regras de negócio (RN01–RN96).
- 17 histórias de usuário (HU01–HU17).
- 68 casos de borda, conforme o registro de prompts.
- 9 entidades de dados.
- 10 requisitos não funcionais.
- 49 decisões documentadas.
- 12 seções do template do professor.

As DEC01–DEC26 estão registradas como aprovadas. As DEC27–DEC49 permaneciam pendentes na última versão consultada.

O `spec.md` ainda continha o marcador `[PREENCHER PELO ALUNO]`, indicação de RASCUNHO e cinco questões em aberto.

O `prompts.md` indicava que a auditoria final como implementador cético ainda não havia sido realizada.

**Você deve verificar o estado ATUAL do GitHub antes de editar**, pois os arquivos podem ter mudado.

Não descarte a estrutura existente nem reescreva tudo desnecessariamente.

## 4. REGRAS E DECISÕES JÁ DOCUMENTADAS

A especificação existente estabelece, entre outras, as seguintes escolhas:

- O dono é o único usuário do aplicativo; o tomador é um contato cadastrado.
- O aplicativo é web e responsivo.
- Objetos possuem nome, categoria, estado de conservação e foto opcional.
- Um objeto físico não pode ter dois empréstimos ativos simultaneamente.
- Conjuntos de objetos possuem componentes identificados.
- Cada empréstimo possui data de saída e data de vencimento.
- O prazo inicial permitido vai de 1 a 365 dias, com sugestão de 7 dias.
- O vencimento ocorre no final do dia, no fuso America/Fortaleza.
- O atraso começa no dia seguinte ao vencimento, sem tolerância.
- Não existem multas, pontuação de confiabilidade ou bloqueio automático por atraso.
- Os avisos aparecem no painel do aplicativo.
- Somente o dono pode renovar um empréstimo.
- Cada empréstimo pode ser renovado no máximo duas vezes, adicionando de 1 a 7 dias ao vencimento anterior.
- Empréstimos atrasados não podem ser renovados.
- O estado de conservação é classificado como novo, bom, regular ou ruim.
- Os danos são classificados como nenhum, leve, grave ou inutilizado.
- Danos graves ou inutilização tornam o objeto indisponível.
- Devolução parcial de um conjunto não encerra automaticamente o empréstimo.
- Contatos podem ser bloqueados manualmente.
- Objetos e contatos com histórico preservam seus registros.
- Dados pessoais devem ser tratados com minimização e mecanismos de privacidade.

**Atenção:** o documento também inclui regras adicionais provenientes de DEC27–DEC49. Essas decisões não devem ser tratadas como aprovadas apenas porque aparecem incorporadas no rascunho.

Confirme a coerência delas e solicite minha aprovação expressa antes de consolidá-las.

Em especial, verifique:
- DEC37: possibilidade de cancelar um empréstimo registrado por engano.
- DEC49: tratamento do aviso de possível perda no 30º dia de atraso.
- Critério de perda e recuperação.
- Retificação de danos e contestação.
- Anonimização de contatos.
- Exclusão da conta do proprietário.

## 5. PRIMEIRA AÇÃO — RESOLVER PENDÊNCIAS

Leia o repositório completo e o enunciado.

Apresente todas as decisões ainda pendentes em uma tabela consolidada e objetiva, contendo:

- ID.
- Decisão proposta.
- Alternativas.
- Recomendação.
- Consequências.
- Justificativa.

Para acelerar, agrupe decisões relacionadas e permita que eu aprove todas em uma única resposta, alterando apenas aquelas com as quais discordar.

Não me faça responder dezenas de perguntas separadamente.

**Não considere nenhuma decisão aprovada sem minha manifestação expressa.**

Se houver dúvida quanto ao nome completo a usar no cabeçalho, confirme comigo.

Depois da minha resposta, avance diretamente para a conclusão, sem solicitar confirmações redundantes.

## 6. SEGUNDA AÇÃO — FINALIZAR SPEC.MD

Preserve as 12 seções obrigatórias:

1. Visão.
2. Escopo.
3. Glossário.
4. Atores e permissões.
5. Regras de negócio.
6. Histórias de usuário.
7. Estados do empréstimo.
8. Modelo de dados.
9. Casos de borda e erros.
10. Requisitos não funcionais.
11. Decisões e premissas.
12. Questões em aberto.

Todas as nove funcionalidades obrigatórias do enunciado precisam estar cobertas.

As quatro exclusões de escopo devem permanecer explícitas:
- Cobrança ou pagamento real de multas e indenizações.
- Marketplace público.
- Chat entre usuários.
- Rastreamento por geolocalização.

Exija:
- Regras RNxx atômicas, numeradas e testáveis.
- Histórias HUxx completas.
- Critérios de aceite em Dado/Quando/Então.
- Cenários positivos, negativos e limites.
- Valores concretos para validações.
- Transições de estados completas.
- Modelo de dados coerente.
- Rastreabilidade entre regras, histórias, estados e casos de borda.
- Decisões aprovadas com alternativas e justificativas.
- Requisitos não funcionais verificáveis.
- Coerência terminológica em todo o documento.

Verifique referências cruzadas e numeração depois de qualquer alteração.

Mantenha o estilo técnico, a organização e a terminologia do documento produzido no Claude Code, evitando mudanças estilísticas desnecessárias.

Não atribua falsamente ao Claude decisões ou alterações realizadas por este Work.

Remova os marcadores de rascunho apenas quando as pendências estiverem efetivamente resolvidas.

Não deixe placeholders como `[PREENCHER]` nos documentos finais.

## 7. TERCEIRA AÇÃO — REVISÃO CRÍTICA

Execute uma auditoria independente como um desenvolvedor que recebeu exclusivamente o `spec.md` e está proibido de fazer perguntas ao autor.

Procure qualquer comportamento que ainda precise ser adivinhado.

Para cada lacuna, identifique a seção, a regra e o problema.

Corrija falhas de redação, inconsistências e referências quebradas usando somente decisões já aprovadas.

Se uma correção exigir nova decisão de produto, obtenha minha aprovação.

Faça uma segunda auditoria usando a rubrica oficial:

- Regras de negócio: 25%.
- Critérios de aceite: 25%.
- Casos de borda: 15%.
- Estados e modelo de dados: 10%.
- Rastreabilidade e clareza: 15%.
- Decisões e uso de IA: 10%.

Verifique objetivamente os critérios de excelência de cada categoria.

Não invente uma nota oficial nem declare perfeição sem evidências.

## 8. QUARTA AÇÃO — FINALIZAR PROMPTS.MD

Preserve o histórico autêntico das interações com Claude Code.

Acrescente somente as interações que efetivamente ocorrerem no ChatGPT Work.

Para cada interação relevante, registre:
- Data real.
- Ferramenta utilizada.
- Objetivo.
- Prompt utilizado.
- Resultado obtido.
- Alterações realizadas.
- Decisões sugeridas, aprovadas ou rejeitadas.

Se um prompt anterior não estiver disponível integralmente, registre a limitação; não invente seu conteúdo.

Identifique claramente as contribuições de Claude Code e ChatGPT Work.

O arquivo deve demonstrar a utilização responsável da IA, conforme as orientações do professor.

## 9. QUINTA AÇÃO — ATUALIZAR O GITHUB

Use a integração GitHub disponível para acessar o repositório original:

https://github.com/Mateuspsr/empresta-ai-sdd

Trabalhe preferencialmente sobre a branch `main`.

Antes de alterar:
- Leia os arquivos atuais.
- Verifique o histórico e o último commit.
- Preserve o conteúdo válido.
- Evite sobrescrever alterações mais recentes.
- Verifique que não existem dados privados ou credenciais no material.

Depois que eu aprovar as decisões finais:
- Atualize `spec.md`.
- Atualize `prompts.md`.
- Revise os dois arquivos.
- Faça o commit das mudanças finais.
- Envie as alterações ao repositório original.

Não crie outro repositório desnecessariamente.

Se a integração não permitir fazer commit e push, produza os arquivos finais e os comandos exatos para atualizar o GitHub, esclarecendo que essa etapa não foi executada.

Não afirme que o GitHub está atualizado sem verificar o resultado.

## 10. SEXTA AÇÃO — ENTREGA PARA WHATSAPP

Prepare um pacote final para eu encaminhar ao professor.

Entregue:

A. `spec.md` final.

B. `prompts.md` final.

C. Um arquivo ZIP contendo os dois documentos oficiais.

D. O link público do repositório GitHub atualizado.

E. Se possível, uma versão PDF bem formatada da especificação para facilitar a leitura pelo professor, sem substituir os arquivos Markdown oficiais.

F. Uma mensagem curta, formal e natural para enviar pelo WhatsApp ao professor com o link e os anexos.

Verifique que os arquivos entregues correspondem à mesma versão publicada no GitHub.

## 11. SÉTIMA AÇÃO — PREPARAR MINHA APRESENTAÇÃO

Crie um guia de apresentação do trabalho, em português brasileiro.

Quero uma explicação simples, objetiva e tecnicamente correta, para que eu consiga demonstrar que entendi o projeto.

Entregue:

- Um resumo do problema que o Empresta Aí resolve.
- A explicação da metodologia SDD.
- O motivo de o trabalho ser documentação, e não programação.
- As principais funcionalidades.
- Exemplos concretos das regras de negócio.
- Explicação de histórias de usuário e Dado/Quando/Então.
- Explicação dos estados e do modelo de dados.
- Como foram tratados danos, atrasos, perdas e devoluções parciais.
- Como a IA foi utilizada e como as decisões foram registradas.
- Um roteiro oral de aproximadamente 2 minutos.
- Cinco perguntas difíceis que o professor poderia fazer, com respostas claras.
- Quais seções de `spec.md` devo abrir para demonstrar cada ponto.

Não quero apenas decorar uma apresentação: quero compreender as principais decisões que tomei.

## 12. CRITÉRIOS PARA ENCERRAR A MISSÃO

A tarefa somente estará concluída quando:

1. Todas as decisões de produto necessárias estiverem aprovadas.
2. As 12 seções do `spec.md` estiverem completas.
3. Não houver contradições relevantes ou lacunas de especificação conhecidas.
4. O `prompts.md` contiver um histórico fiel.
5. Os arquivos finais estiverem disponíveis para download.
6. O GitHub original estiver atualizado, ou houver indicação explícita e precisa do bloqueio.
7. A mensagem para WhatsApp estiver pronta.
8. O roteiro de apresentação estiver pronto.

Ao concluir, apresente um relatório de entrega com:

- Arquivos produzidos.
- Links diretos.
- Commit ou referência de atualização, quando realizado.
- Checklist da rubrica.
- Pendências remanescentes, caso existam.
- Instrução exata do que devo encaminhar ao professor.

**COMECE AGORA.**

Primeiro verifique o repositório e as fontes. Depois trate somente das decisões realmente pendentes. Assim que eu aprová-las, prossiga com todas as demais etapas da missão, dentro das permissões disponíveis, até produzir os entregáveis finais.
````

**Resultado efetivamente obtido:** foram lidas e conferidas visualmente as seis páginas do enunciado. A integração GitHub confirmou main no commit `8ef694b55c90408202ef60c0f82abebeaafdfc2a`, com apenas spec.md e prompts.md. Contagem do rascunho: 96 RNs, 17 HUs, 104 cenários, 68 casos de borda, nove entidades, dez RNFs, 49 decisões e doze seções. Foram encontrados 19 cenários sem Dado explícito. DEC01–DEC26 tinham aprovação transcrita; DEC27–DEC49 continuavam pendentes. Nome e exclusão da conta estavam em aberto.

**Sugestões:** consolidar DEC27–DEC49 com seus detalhes e acrescentar DEC50–DEC57 para exclusão da conta, preservação das causas de indisponibilidade, cronologia, composição dos conjuntos, repetição de operações, normalização de entradas, ordenação/avisos e saneamento de dados pessoais. O lote completo teve 31 linhas, com alternativas, recomendação, consequências e justificativa, além dos detalhamentos. A resposta pediu uma única aprovação e o nome completo. As escolhas finais e suas justificativas estão na seção 11 de spec.md.

**Conferências externas:** texto atualizado da LGPD na Câmara dos Deputados, para distinguir meta de resposta do produto de prazo legal geral de eliminação; W3C, para distinguir WCAG 2.1 AA da meta adicional de alvo de 44×44 pixels CSS. Fontes estão citadas em spec.md. Não houve declaração de conformidade jurídica ou teste de software.

**Alterações realizadas:** criado documento auxiliar de decisões para revisão e cópias locais de trabalho. Nenhum arquivo oficial ou commit remoto foi alterado nessa interação.

**Decisões aprovadas/rejeitadas nessa interação:** nenhuma nova; DEC27–DEC57 permaneceram pendentes até a resposta abaixo.

---

## Interação 05 — aprovação expressa, consolidação e auditoria final

**Data:** 03/11/2026  
**Ferramenta:** Codex/OpenAI; integração GitHub; scripts locais de edição, conferência e preparação documental.  
**Objetivo:** aplicar o lote aprovado, finalizar os entregáveis oficiais, revisar como implementador cético e pela rubrica, e preparar publicação e pacote de apresentação.

**Prompt enviado (literal):**

````text
Meu nome completo é **Mateus Pinheiro Silva Rodrigues**.

Aprovo as decisões **DEC27 a DEC57**, conforme as recomendações e os detalhamentos apresentados na tabela, incluindo:

- DEC37: opção A, permitindo cancelamento de registros indevidos nas condições estabelecidas.
- DEC49: manter os 30 dias e o aviso "Perda possível".
- DEC32/DEC51: permitir uma retificação até o fim do sétimo dia, sem afetar empréstimos posteriores.
- DEC35/DEC51: preservar o histórico de perda e exigir a recuperação completa dos conjuntos.
- DEC46/DEC57: proteger os dados pessoais também em observações e imagens, preservando os fatos necessários ao histórico.
- DEC50: permitir a exclusão integral da conta mediante autenticação e confirmação.
- DEC54: impedir operações duplicadas causadas por cliques repetidos.

Assumo as decisões aprovadas e suas justificativas como escolhas de produto da minha especificação.

**Alterações: nenhuma.**

Agora prossiga com todas as etapas restantes que já solicitei, sem interromper desnecessariamente o trabalho:

1. Finalize integralmente `spec.md`, resolvendo as questões em aberto.
2. Corrija os 19 cenários incompletos, referências, datas e inconsistências.
3. Execute a auditoria final usando todos os critérios oficiais do professor.
4. Atualize `prompts.md` com o histórico verdadeiro do Claude Code e do ChatGPT Work.
5. Atualize os dois documentos na branch `main` do repositório GitHub original e confirme o commit.
6. Entregue os dois arquivos Markdown finais e um ZIP pronto para enviar pelo WhatsApp.
7. Prepare uma mensagem curta para o professor e um roteiro oral de apresentação de aproximadamente dois minutos, com perguntas e respostas.

Mantenha a estrutura e o estilo técnico desenvolvidos no Claude, registrando corretamente as contribuições de cada ferramenta.

Não implemente o aplicativo, porque o trabalho é exclusivamente de especificação.

Ao terminar, forneça os links dos arquivos, o commit final, os resultados da auditoria e as instruções para a entrega.
````

**Decisões sugeridas, aprovadas ou rejeitadas:** o aluno confirmou o nome completo e aprovou DEC27–DEC57 com todos os detalhamentos e justificativas, sem alterações. DEC37-A e DEC49 mantendo 30 dias foram explicitamente confirmadas. Nenhuma nova escolha de produto foi submetida como aprovação presumida; as correções de consistência foram feitas dentro desse lote.

**Alterações efetivamente realizadas:** preservadas as doze seções e os IDs RN01–RN96/HU01–HU17, atualizando as regras afetadas e acrescentando RN97–RN159 e HU18–HU20. Completados os 19 cenários sem Dado e corrigidas dependências de contexto e datas. Adicionados testes documentais de limites, erros, concorrência, retificação histórica, recuperação completa, privacidade e exclusão. Corrigidos transições, causas de disponibilidade, cronologia, campos condicionais e o evento de remoção de contato sem empréstimo. Acrescentado controle conceitual de confirmação de operação; não foi criado banco executável nem código do aplicativo. RNFs receberam protocolo mensurável. A seção 12 foi resolvida e o marcador de rascunho foi removido após a aprovação.

**Auditoria como implementador cético:** uma nova passagem pelo documento consolidado, realizada pelo próprio Codex, examinou comportamentos que exigiriam adivinhação. Não foi uma auditoria de terceiro nem de subagente. Nessa passagem foram corrigidos: permanência do aviso após leitura; data explícita do histórico de retificação; distinção entre data efetiva e gravação; apresentação da meta de privacidade; aplicação da causa de dano na retificação sem sobrescrever empréstimo posterior; vínculo do evento de remoção diretamente ao contato. As salvaguardas de perda, duplicidade e saneamento propostas na interação 04 foram conferidas após a implementação documental. Não foram identificadas decisões de produto ainda pendentes ao final dessa revisão.

**Verificações mecânicas concluídas:** 159 RNs, 20 HUs, 166 cenários com Dado/Quando/Então, 112 casos de borda, dez entidades conceituais, dez RNFs, 57 decisões aprovadas e doze seções. Numeração contínua e sem duplicatas; nenhuma referência RN/HU/DEC/RNF inexistente; todas as RNs têm decisão de origem e ao menos uma HU associada; todos os casos têm RN; tabelas Markdown com colunas consistentes; datas de limite conferidas; ausência de placeholders de preenchimento ou status de aprovação pendente em spec.md. Conferência por padrões não encontrou credenciais ou chaves privadas; nomes/telefones dos exemplos foram identificados como fictícios.

**Auditoria pela rubrica:** regras 25%, critérios 25%, bordas 15%, estados/modelo 10%, rastreabilidade/clareza 15% e decisões/IA 10% foram conferidos contra evidências documentais. O relatório auxiliar apresenta achados, correções e limites da verificação. Não foi atribuída nota oficial, nem executados testes de aplicação, porque não existe implementação nesta entrega.

**Publicação e apresentação:** esta entrada registra o trabalho editorial e as verificações anteriores à publicação. O commit final, a conferência de igualdade entre GitHub e arquivos locais, o ZIP, a versão PDF auxiliar e o guia de apresentação são identificados no relatório de entrega. Não se insere o hash do próprio commit dentro do arquivo que o constitui. A publicação na main está autorizada pelo pedido do aluno após a aprovação acima.

**Responsabilidade autoral:** Mateus Pinheiro Silva Rodrigues assumiu expressamente as escolhas de produto e suas justificativas. A IA auxiliou na análise, escrita, crítica e conferência. O produto acadêmico continua sendo documentação em Markdown.

---

## Interação 06 — análise da proposta do professor e plano de corte para MVP

**Data:** 05/11/2026
**Ferramenta/modelo:** Claude Code (extensão VS Code) / Claude Fable 5.1
**Modo:** Plan (somente leitura), com a habilidade `brainstorming` do Superpowers invocada pelo aluno (`/superpowers:brainstorming`) para análise de escopo, conforme o pedido.
**Objetivo:** ler o enunciado, o feedback do professor, o material sobre MVP e a spec atual; produzir matriz manter/simplificar/adiar/retirar, auditoria de rastreabilidade e um único bloco de escolhas para aprovação, sem editar arquivos.

**Contexto do pedido:** o aluno clonou `Mateuspsr/empresta-ai-sdd` na branch `revisao-mvp` (commit `468ab41`), extraiu o kit de revisão (`LEIA_PRIMEIRO.md`, `PROMPT_CLAUDE_MVP.md`, `CHECKLIST_ACEITE_MVP.md`, `referencias/`) e invocou a habilidade de brainstorming. O conteúdo do pedido é o arquivo `PROMPT_CLAUDE_MVP.md`, reproduzido literalmente:

````text
# PROMPT PARA CLAUDE CODE — REVISAR O EMPRESTA AÍ PARA MVP

Você é um analista sênior de requisitos e revisor independente especializado em SDD e produtos mínimos viáveis. Seu trabalho **não é programar** o Empresta Aí: é transformar a especificação acadêmica atual numa **versão MVP enxuta, completa, consistente e defensável perante o professor**.

## FONTES, ORDEM E AMBIENTE

Você está na raiz de um clone do repositório `Mateuspsr/empresta-ai-sdd` numa branch de revisão, inicialmente criado a partir do commit `468ab4185cda23c6c84373bb07f448b8e78dc458` (verifique o estado atual e não presuma que continua igual). Leia integralmente:

1. `referencias/Enunciado_original_Empresta_Ai.pdf` — **fonte obrigatória primária**, seis páginas;
2. `referencias/Feedback_Professor_MVP.md` — crítica, proposta e novo direcionamento do professor;
3. `referencias/A_importancia_do_MVP.pdf` — material de aula sobre valor, validação e ciclo construir–medir–aprender;
4. `spec.md` e `prompts.md` atuais;
5. `CLAUDE.md`, se existir e estiver consistente com as exigências anteriores (priorize o enunciado e este pedido em caso de conflito).

Se o PDF não puder ser lido, interrompa apenas o suficiente para informar o impedimento e peça o mínimo necessário; nunca finja que leu.

Use a habilidade de **brainstorming** do Superpowers, se instalada, apenas para analisar produto/escopo. Evite os fluxos de implementação de código ou a geração de planos de desenvolvimento.

## OBJETIVO

O professor elogiou a precisão, mas criticou a desproporção: a versão atual alcançou 159 RNs, 20 HUs, 112 bordas, 57 decisões e muitas decisões técnicas em regras de negócio. Ele solicitou uma nova SPEC ajustada ao **MVP**, com o menor conjunto suficiente de regras para as **9 funcionalidades obrigatórias**, e uma distinção clara entre núcleo v1, limites conhecidos e backlog v2.

O MVP não pode ser um protótipo malfeito: precisa entregar o ciclo essencial cadastrar → emprestar → acompanhar → renovar quando possível → registrar devolução/perda → consultar, com comportamento inequívoco. Explique brevemente qual hipótese de valor será testada quando o produto for construído e quais indicadores/feedback poderiam ser observados no futuro, sem inventar medições reais nesta atividade documental. Não implemente o sistema, não crie frontend/backend, API, SQL, testes executáveis ou aplicativo funcional.

## ETAPA A — LEITURA E PLANO DE CORTE (MODO PLAN)

Faça primeiro:
1. Compare o enunciado, a sugestão do professor e a SPEC atual; identifique os 9 itens obrigatórios, 4 exclusões de escopo e todas as perguntas que continuam exigindo resposta.
2. Apresente matriz resumida **manter / simplificar / adiar para v2 / retirar**, justificando valor, complexidade e impacto em requisito obrigatório.
3. Siga como linha-base as recomendações do professor em `Feedback_Professor_MVP.md`, mas sinalize trade-offs relevantes. Considere avisos **derivados no painel**, danos reduzidos a nenhum/leve/grave, perda após vencimento sem espera de 30 dias, remoção simples de dados com limitação explícita, sem contestação/retificação formal, sem bloqueio de contato, sem mecanismo sofisticado de idempotência, sem subsistema de avisos persistidos. Se qualquer corte inviabilizar requisito explícito do enunciado, proponha o menor ajuste para preservá-lo.
4. Aponte os erros confirmados de rastreabilidade: caso de borda 9 cita RN15 onde RN150 detalhava conservação; caso 39 omite RN154 sobre liberação. Faça auditoria completa, não corrija apenas dois pontos.
5. Identifique que a proposta fala em '11 HUs', mas enumera 12; a quantidade é ilustrativa. Meta orientativa: **aproximadamente 35–55 RN, 11–13 HU e 25–40 casos de borda**, se isso satisfizer realmente o escopo. Não force cotas, preserve os critérios de testabilidade e rastreabilidade.
6. Apresente no máximo um bloco concentrado de escolhas materiais que dependam de mim, com recomendação clara. Não reabra escolhas já definidas no enunciado nem faça dezenas de perguntas individuais.

**Neste primeiro momento:** não edite arquivos nem publique no Git. Retorne um plano de corte e as escolhas que preciso aprovar.

## ETAPA B — REDAÇÃO APÓS APROVAÇÃO (MODO ACCEPT EDITS)

Quando eu aprovar o plano, proceda até concluir os arquivos, sem ficar pedindo confirmação para cada detalhe editorial.

Reescreva `spec.md` integralmente para o **MVP**, preservando as 12 seções oficiais exatamente nesta ordem:
1. Visão;
2. Escopo (dentro, fora, limites e backlog v2 referido claramente);
3. Glossário;
4. Atores e permissões;
5. Regras de negócio;
6. Histórias de usuário e aceite Dado/Quando/Então;
7. Estados e transições;
8. Modelo de dados conceitual;
9. Casos de borda e erros;
10. Requisitos não funcionais verificáveis;
11. Decisões e premissas com alternativas e justificativas;
12. Questões em aberto (idealmente nenhuma, após minha aprovação).

**Qualidade obrigatória:**
- Cada RN é curta, atômica, verificável e tem identificador sequencial novo RN01... sem 'buracos' nem números herdados. Não empilhe referências encadeadas como substituto de descrever o comportamento.
- Cada HU descreve objetivo e benefício; relaciona-se às suas RNs; tem critérios de aceite Dado/Quando/Então em cenário de sucesso, erro e limite quando pertinente.
- Cada caso de borda possui comportamento esperado e aponta a RN correta.
- Glossário com distinção entre estado persistido e classificação derivada, sem termos contraditórios.
- Estados, datas, permissões, dados, avisos e históricos consistentes. Não inventar envio de SMS/e-mail, chat, pagamentos, marketplace ou geolocalização.
- A solução de avisos derivados deve satisfazer efetivamente 'avisar o dono sobre prazos próximos e atrasos' sem jobs/agendamento/persistência desnecessários; definir o que aparece e quando.
- A exclusão/arquivamento de objetos e contatos com histórico e o pedido de remoção de dados precisam de comportamento explícito; **não afirmar que remoção parcial dos identificadores garante anonimização jurídica ou pleno atendimento à LGPD**. Minimize os campos livres e registre a limitação de textos históricos.
- Fuso America/Fortaleza é uma **premissa/limitação da v1**, explicitada em escopo, decisões e/ou RNF; explicar como dias e atraso são calculados sem sobrecarregar RNs com detalhes de infraestrutura.
- Concorrência básica, segurança, transações, hashes, validação técnica, limites técnicos e acessibilidade em RNF ou ADR opcional, evitando transformá-los em dezenas de RN.
- Evitar regras desnecessárias sobre janela de retificação, expurgo específico de backup, versionamento otimista, confirmação técnica detalhada e múltiplas causas automáticas de indisponibilidade.
- O modelo de dados deve comportar os requisitos obrigatórios e a devolução parcial. Não reduzir artificialmente a 6 entidades se isso impossibilitar o comportamento; justificar eventual diferença.
- Manter decisões justificadas com rastreabilidade; criar IDs novos e coerentes DEC-MVP01... ou DEC01... se todos forem renumerados corretamente. O histórico da versão anterior já existe no Git; não inventar que as decisões antigas foram todas mantidas na v1.
- Não marcar como 'implementado' nada que seja apenas especificado; não declarar testes executados num aplicativo inexistente.

Crie `backlog_v2.md` **curto**, com prioridade, valor, motivação e itens adiados: bloqueio; contestação/retificação; avisos persistidos; privacidade mais completa; idempotência avançada se necessária; fusos por conta. Pode incluir decisões técnicas futuras, sem transformá-las em promessas da v1.

Atualize `prompts.md` adicionando o(s) prompt(s) real(is) desta revisão, a decisão de cortes, as contribuições reais do Claude e minha aprovação efetiva. **Preserve o histórico verdadeiro de Claude Code e Codex/OpenAI**, sem inventar mensagens ou atribuir a uma ferramenta o trabalho de outra. O professor elogiou precisamente o registro honesto do uso de IA.

## ETAPA C — AUDITORIA FINAL OBRIGATÓRIA

Audite a nova versão pelos seis critérios e pesos do professor:
- 25% regras de negócio;
- 25% critérios de aceite;
- 15% casos de borda;
- 10% estados e modelo de dados;
- 15% rastreabilidade e clareza;
- 10% decisões e uso da IA.

Execute verificações documentais: contagem e sequência de IDs, referências RN/HU/DEC válidas, HUs ligadas a regras, casos ligados a RNs, ausência de placeholders, coerência do estado Ativo/Devolvido/Perdido/Cancelado, cobertura das nove funcionalidades e resposta explícita às perguntas do PDF.

Releia a nova SPEC como 'implementador cético': liste o que exigiria adivinhação. Corrija o que puder com base nas decisões aprovadas e registre como limitação ou questão aberta apenas aquilo que realmente exigir nova escolha minha.

Fique atento à crítica de legibilidade: a redução da spec não pode apenas converter 159 regras em poucas regras enormes. O texto precisa ficar **muito mais curto e fácil de seguir**.

## ETAPA D — CONTROLE DE VERSÃO E ENTREGA

Faça as edições na branch de revisão aberta, preservando o histórico da `main`. Ao concluir, apresente diff/resumo, contagens finais e um quadro 'antes → depois' com os cortes e seus motivos.

Não force-push. Não reescreva a história. Não altere a `main` nem abra PR sem minha autorização final. Depois de aprovado, crie um commit na branch e oriente como fazer merge/push com segurança, ou execute as operações que eu autorizar. Não declare GitHub publicado se não verificar o resultado.

Entregue o `spec.md` revisado, `prompts.md` atualizado, `backlog_v2.md` auxiliar e um roteiro curto para apresentar em **3–5 minutos**: problema, o que permaneceu, o que foi cortado, por que os avisos foram simplificados e como a nova SPEC continua testável.

## COMEÇO

Agora leia os cinco materiais, confira o estado do repositório e produza somente a **ETAPA A**. Quando eu aprovar o plano, siga automaticamente B, C e D, respeitando o controle de versão.
````

**Resposta e resultado:** a renderização dos PDFs não estava disponível no ambiente; o Claude extraiu o texto das seis páginas do enunciado e das cinco páginas do material de MVP com um script Python de biblioteca padrão, gravado fora do repositório, e leu integralmente `spec.md` (1.677 linhas) e `prompts.md`. Produziu, no arquivo de plano do Claude Code: confirmação das 9 funcionalidades, 4 exclusões e 7 blocos de perguntas do enunciado; matriz de corte por área (conta, objeto, contato, empréstimo, atraso, avisos, renovação, devolução, parcial, perda, cancelamento, listas, transversais) com justificativa; seis trade-offs da linha-base do professor; auditoria de rastreabilidade da spec de 03/11 (verificação mecânica: nenhum ID inexistente; verificação semântica: bordas 9, 13, 15, 18, 38 e 39 e a linha "editar conservação" da tabela 7.4 citavam RN que remetiam ao comportamento em vez de enunciá-lo; 31 RN citavam outras RN); estrutura proposta da nova spec (13 HU, cerca de 45–50 RN, 6 entidades); e a observação de que a proposta do professor fala em "11 HUs" mas enumera 12.
**Bloco de escolhas apresentado ao aluno (recomendação em primeiro lugar):** Q1 momento da perda (1º dia de atraso / qualquer momento Ativo / manter 30 dias); Q2 modelo com 6 entidades sem Evento ou 7 com Evento; Q3 campo único de indisponibilidade com liberação livre ou perdido liberado só por recuperação; Q4 conteúdo do commit (só entregáveis ou também referências).
**Sugestões da IA aceitas:** nenhuma nesta interação (apenas análise e plano).
**Modificações em `spec.md`:** nenhuma.

---

## Interação 07 — aprovação do plano de corte, redação da spec MVP, auditoria e commit

**Data:** 05/11/2026
**Ferramenta/modelo:** Claude Code (extensão VS Code) / Claude Fable 5.1
**Modo:** Plan → aprovação → edição de arquivos.
**Objetivo:** executar as Etapas B, C e D do pedido após a aprovação do aluno.

**Aprovação do aluno (por diálogo de escolhas do Claude Code e aprovação do plano):** Q1 = "1º dia de atraso (Recommended)"; Q2 = "6 entidades, sem Evento (Recommended)"; Q3 = "Sim, liberação livre (Recommended)"; Q4 = "Só os entregáveis (Recommended)". Em seguida o aluno aprovou o plano de corte completo. As quatro escolhas são decisões de produto do aluno, registradas em `spec.md` como DEC17, DEC26 e DEC19 e no escopo do commit; as alternativas rejeitadas constam da coluna "Alternativas consideradas".

**Alterações efetivamente realizadas pelo Claude, assumidas pelo aluno:**
- `spec.md` reescrito integralmente para o MVP, preservando as 12 seções oficiais na ordem: 49 RN (RN01–RN49, numeração nova, sem buracos), 13 HU com 78 cenários Dado/Quando/Então, tabela de transições com recusas, catálogo de mensagens, 6 entidades, 51 casos de borda, 9 RNF, 27 decisões (DEC01–DEC27 novas, com coluna "Origem" apontando as DEC antigas mantidas, simplificadas ou substituídas) e seção 12 sem pendências. 894 linhas (antes: 1.677). Hipótese de valor e indicadores futuros na seção 1, sem medições inventadas. Fuso America/Fortaleza declarado como premissa (2.3, DEC09, RNF05). Remoção de dados descrita com limitação explícita, sem promessa jurídica (RN19, DEC23).
- `backlog_v2.md` criado: 10 itens adiados com prioridade, valor, motivação e gatilho.
- `roteiro_apresentacao_mvp.md` criado: roteiro de 3–5 minutos com perguntas prováveis.
- `prompts.md`: cabeçalho atualizado e estas duas interações acrescentadas; interações 00–05 preservadas sem alteração de conteúdo.
- Arquivos do kit e `referencias/` não foram adicionados ao Git, conforme Q4.

**Verificações mecânicas executadas (script documental sobre `spec.md`):** RN01–RN49, HU01–HU13, DEC01–DEC27, RNF01–RNF09 e bordas 1–51 sequenciais; nenhuma referência RN/HU/DEC/RNF inexistente (as DEC28–DEC57 citadas na coluna "Origem" são as antigas, por design); toda RN ligada a pelo menos uma HU; toda borda com RN; lista de RN de cada HU igual à tabela 9.1; 78 cenários, todos com Dado, Quando e Então; mensagens entre aspas dos cenários presentes no catálogo 7.3; nenhum placeholder; 12 seções na ordem oficial.

**Leitura como implementador cético (feita pelo próprio Claude, não por terceiro):** itens que exigiriam adivinhação e foram corrigidos na própria redação: RN27 sem HU (ligada à HU10); mensagem de recuperação em empréstimo não perdido ausente do catálogo; critério de empate nas ordenações (declarado livre em DEC22); ordem dos itens no painel (RN28); edição de nome/telefone/observação do contato (RN15); substituição do motivo de indisponibilidade quando já havia um (RN12); cronologia entre devoluções parciais (RN39 e borda 51); formato de e-mail válido (RNF09). Permanecem como limitações declaradas, não como lacunas: liberação manual de objeto perdido (2.3, DEC19), textos históricos não saneados (RN19), fuso fixo (DEC09).

**Auditoria pela rubrica (evidência documental, sem nota):** regras de negócio 25% — 49 RN atômicas respondendo aos 7 blocos de perguntas (seção 5; cobertura em 9.1); critérios de aceite 25% — 78 cenários com datas, valores e mensagens literais, incluindo erro e limite em todas as HU (seção 6); casos de borda 15% — 51 casos com comportamento e RN que enuncia a regra (seção 9), corrigindo estruturalmente os desvios das bordas 9 e 39 da versão anterior; estados e modelo 10% — 4 estados persistidos, classificações derivadas separadas, transições com recusas e 6 entidades coerentes (seções 7 e 8); rastreabilidade e clareza 15% — glossário com estado × classificação, HU↔RN↔borda em 9.1, RN sem referências encadeadas como substituto de descrição; decisões e uso de IA 10% — 27 decisões com alternativas, justificativa e origem, e este registro. Nenhum teste de aplicativo foi executado.

**Controle de versão:** um commit na branch `revisao-mvp` com `spec.md`, `prompts.md`, `backlog_v2.md` e `roteiro_apresentacao_mvp.md`. Sem push, merge ou PR; a `main` permanece no commit `468ab41`. O hash do commit não é inserido neste arquivo porque o arquivo faz parte do próprio commit.

**Responsabilidade autoral:** Mateus Pinheiro Silva Rodrigues decidiu os cortes e as quatro escolhas e assumiu as justificativas. O Claude analisou, propôs, redigiu e conferiu. O produto continua sendo especificação em Markdown.
