# Roteiro de apresentação — Empresta Aí MVP (3 a 5 minutos)

**Aluno:** Mateus Pinheiro Silva Rodrigues · **Data:** 05/11/2026 · Material auxiliar; os entregáveis oficiais são `spec.md` e `prompts.md`.

## 1. O problema (30 s)

"A versão de 03/11 tinha 159 regras, 20 histórias e 112 casos de borda para um app de empréstimo entre amigos com nove funcionalidades. Estava precisa, mas desproporcional: cada regra custa desenvolvimento e teste, e detalhes técnicos como hash de senha e expurgo de backup estavam misturados com regras de produto. O professor pediu o menor conjunto suficiente, um MVP: a menor versão que entrega valor real e permite aprender com uso."

**Abrir:** `spec.md`, seção 1 (hipótese de valor) e 2.3 (limites conhecidos).

## 2. O que permaneceu (60 s)

"As nove funcionalidades obrigatórias e as quatro exclusões continuam, e as sete perguntas do enunciado seguem respondidas com valores concretos:"

- Prazo por data, 1 a 365 dias, sugestão de 7; vence ao fim do dia em Fortaleza.
- Atraso a partir do dia seguinte, sem tolerância, como classificação derivada.
- Renovação: só o dono, duas vezes, 1 a 7 dias sobre o vencimento, só em dia.
- Devolução com conservação na volta e dano nenhum/leve/grave; devolução parcial de conjuntos.
- Perda a partir do primeiro dia de atraso, com recuperação que não reabre.
- Um empréstimo ativo por objeto; conjunto é um objeto com componentes; com histórico só arquiva.
- Dados pessoais mínimos com remoção simples e limitação declarada.

**Números:** 49 RN, 13 HU, 51 casos de borda, 6 entidades, 27 decisões, 9 RNF, cerca de 890 linhas (antes: 1.677 linhas e 159 RN).

**Abrir:** seção 5 (regras curtas, sem referências encadeadas) e 9.1 (cobertura das nove funcionalidades).

## 3. O que foi cortado e por quê (60 s)

"Cortei em quatro categorias, registradas na seção 11 e no `backlog_v2.md`:"

- **Adiado para v2 por valor:** bloqueio de contato (arquivar resolve), contestação e retificação de dano (observação basta), avisos persistidos, higienização completa de textos, trilha de eventos.
- **Simplificado:** um só campo de indisponibilidade com motivo, no lugar de causas manual e automática com precedência; 'inutilizado' virou 'grave'; perda sem espera de 30 dias; cancelamento sem confirmação especial.
- **Movido para RNF:** HTTPS, hash, sessão, transação atômica, duplo clique, validação de entrada, acessibilidade.
- **Retirado:** idempotência elaborada, versionamento otimista, expurgo de backup, janela de retificação, cronologia entre empréstimos, desempates por ID.

"Cada corte tem a alternativa rejeitada e a justificativa; a versão anterior continua no Git."

**Abrir:** seção 11 (coluna Origem) e `backlog_v2.md`.

## 4. Por que os avisos ficaram derivados (45 s)

"A v1 anterior gerava avisos às 9h, gravava cada um, controlava lido/não lido, reexecução após falha e um aviso por dia. Eram 7 regras, uma entidade e 3 RNF. O enunciado pede apenas 'avisar o dono sobre prazos próximos e atrasos'. Agora o painel calcula, a cada abertura, três grupos a partir dos empréstimos ativos: vence hoje, vence em até 2 dias, atrasado há N dias, com um contador. Três regras, nenhuma gravação, nenhum job. O que se perde, lembrete fora do app, está no backlog."

**Abrir:** RN28–RN30 e HU11.

## 5. Como a nova spec continua testável (45 s)

"A redução não foi trocar 159 regras por 40 regras enormes. Cada RN continua atômica; cada HU tem cenários de sucesso, erro e limite com datas e mensagens literais; cada caso de borda aponta a regra que enuncia o comportamento, e não uma que remete a outra, o que corrige os desvios apontados nas bordas 9 e 39 da versão anterior. Estados persistidos e classificações derivadas estão separados no glossário. Fuso fixo é premissa declarada. A auditoria pela rubrica e a leitura como implementador cético estão em `prompts.md`."

**Abrir:** HU06 (cenários), seção 7.2 (transições com recusas) e seção 9.

## Perguntas prováveis

- **"Por que 49 RN e não 40?"** Nenhuma cota foi forçada; 49 é o mínimo que mantém cada regra atômica e as nove funcionalidades cobertas. DEC27 justifica.
- **"O dono pode liberar um objeto perdido sem recuperação?"** Sim, limitação declarada em 2.3 e DEC19; evita regras de precedência. A alternativa está no backlog.
- **"A remoção de dados atende à LGPD?"** A spec descreve o comportamento (apaga nome, telefone, observação) e declara que textos históricos não são alterados; não afirma conformidade jurídica absoluta.
- **"Como a IA foi usada?"** Claude Code analisou a proposta do professor, propôs a matriz de corte e quatro escolhas; eu decidi cada uma e assumi as justificativas. Tudo está em `prompts.md`, com o histórico anterior do Claude e do Codex preservado.
