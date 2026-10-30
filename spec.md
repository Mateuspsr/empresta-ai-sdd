# Empresta Aí — Especificação v1

**Aluno:** [PREENCHER PELO ALUNO]  
**Data:** 30/10/2026  
**Status:** RASCUNHO v1 — DEC01 a DEC26 aprovadas pelo aluno em 30/10/2026; DEC27 a DEC49 são propostas da IA que aguardam aprovação expressa (ver seções 11 e 12). Não entregar antes de resolver a seção 12.

---

## 1. Visão

Todo mundo já emprestou um livro, uma furadeira ou um jogo e nunca mais viu. O Empresta Aí é um aplicativo web para uma pessoa física, o dono, registrar o que emprestou, para quem e até quando, e acompanhar devoluções, renovações, atrasos, danos e perdas. O tomador não usa o aplicativo: é um contato cadastrado pelo dono. O resultado esperado é que o dono saiba, a qualquer momento, onde está cada objeto, quais empréstimos vencem ou estão atrasados e qual o histórico de cada objeto e de cada amigo, sem cobrança, sem marketplace e sem chat.

## 2. Escopo

### 2.1 Dentro do escopo

1. Cadastrar objetos com nome, categoria, foto opcional e estado de conservação, inclusive conjuntos com componentes.
2. Cadastrar amigos ou contatos, com bloqueio, arquivamento e anonimização.
3. Registrar empréstimo com data de saída e data de vencimento.
4. Renovar empréstimo.
5. Registrar devolução integral ou parcial, com estado de conservação e nível de dano na volta, contestação e retificação.
6. Registrar perda ou não devolução e, se o objeto reaparecer, a recuperação.
7. Listar empréstimos ativos, atrasados e encerrados.
8. Avisar o dono, no painel do aplicativo, sobre prazos próximos, vencimento, atraso e possibilidade de perda.
9. Consultar o histórico de um objeto e de um amigo.
10. Cancelar empréstimo registrado por engano (condicionado à DEC37).

### 2.2 Fora do escopo

O aplicativo explicitamente **não faz**:

- Cobrança ou pagamento real de multas e indenizações; não calcula valores monetários.
- Marketplace público de objetos entre desconhecidos; não há busca de objetos de outras contas.
- Chat entre usuários; a contestação do tomador acontece fora do aplicativo e é apenas anotada pelo dono.
- Rastreamento por geolocalização; nenhuma coordenada é coletada.
- Conta ou acesso para o tomador; envio de avisos por e-mail, SMS ou push; pontuação de confiabilidade; bloqueio automático; funcionamento sem conexão.

## 3. Glossário

Cada termo tem um único significado em toda a especificação.

- **Dono:** pessoa física titular da conta; único usuário do aplicativo; empresta objetos.
- **Tomador:** amigo que recebe o objeto emprestado. Não usa o aplicativo; é representado por um contato.
- **Contato:** registro de um tomador na conta do dono (nome, telefone opcional, observação opcional).
- **Objeto:** bem físico individual cadastrado pelo dono. Dois itens idênticos são dois objetos.
- **Conjunto:** objeto cadastrado com dois ou mais componentes nomeados (exemplo: videogame com dois controles).
- **Componente:** parte nomeada de um conjunto; não pode ser emprestada separadamente.
- **Estado de conservação:** classificação do objeto em uma escala única: novo, bom, regular, ruim.
- **Status do objeto:** Disponível, Emprestado, Indisponível ou Arquivado (seção 7.2).
- **Empréstimo:** registro de que um objeto saiu com um tomador em uma data de saída e deve voltar até uma data de vencimento.
- **Data de saída:** data civil em que o objeto foi entregue ao tomador.
- **Data de vencimento:** último dia em que a devolução é considerada no prazo. Vence às 23:59:59,999 desse dia, no fuso America/Fortaleza.
- **Prazo:** número de dias corridos entre a data de saída e a data de vencimento.
- **Hoje:** data civil corrente no fuso America/Fortaleza, pelo relógio oficial do servidor.
- **Empréstimo ativo:** empréstimo no estado persistido Ativo: o objeto está com o tomador e não houve devolução integral, perda ou cancelamento.
- **Em dia:** classificação derivada de empréstimo ativo cujo vencimento é hoje ou posterior.
- **Atrasado:** classificação derivada de empréstimo ativo cujo vencimento é anterior a hoje. Não é estado persistido.
- **Dias de atraso:** hoje menos data de vencimento, em dias corridos, para empréstimo atrasado.
- **Renovação:** ação do dono que adia a data de vencimento de um empréstimo ativo em dia.
- **Devolução integral:** ação que registra a volta do objeto completo, com estado de conservação e nível de dano, e encerra o empréstimo no estado Devolvido.
- **Devolução parcial:** ação, exclusiva de conjuntos, que registra componentes recebidos enquanto outros continuam faltando; não encerra o empréstimo.
- **Devolvido:** estado persistido de empréstimo encerrado por devolução integral. Subclassifica-se em "no prazo" ou "com atraso".
- **Dano:** avaria constatada pelo dono na volta ou na recuperação, classificada em nenhum, leve, grave ou inutilizado.
- **Contestação:** nota registrada pelo dono, a pedido do tomador feito fora do aplicativo, discordando do dano registrado. Não altera o dano.
- **Retificação:** correção, pelo dono, do nível de dano registrado, preservando o valor anterior.
- **Perdido:** estado persistido de empréstimo encerrado porque o objeto não voltou, com motivo "perda comunicada" ou "não devolução".
- **Recuperado:** marca em empréstimo Perdido indicando que o objeto reapareceu e foi registrado de volta. Não reabre o empréstimo.
- **Cancelado:** estado persistido de empréstimo encerrado por registro indevido, sem que o objeto tenha saído (condicionado à DEC37).
- **Encerrado:** qualquer empréstimo nos estados Devolvido, Perdido ou Cancelado.
- **Indisponível:** status do objeto que impede novo empréstimo, por marcação manual do dono ou automática (dano grave ou inutilizado, perda, conjunto incompleto).
- **Arquivado:** status de objeto ou contato retirado de uso, preservado por ter histórico; não participa de novos empréstimos; reversível.
- **Bloqueado:** marca manual em contato que impede novos empréstimos para ele, sem afetar os já ativos.
- **Aviso:** registro gerado pelo sistema no painel do aplicativo, com tipo, data e hora, empréstimo e situação lido ou não lido. É o único canal de aviso.
- **Painel:** área do aplicativo que lista os avisos e exibe o contador de não lidos.
- **Evento:** registro imutável de uma ação aceita sobre um empréstimo, com data e hora, tipo e dados da ação; forma o histórico.
- **Anonimização:** substituição irreversível dos dados identificadores de um contato, preservando os eventos.

## 4. Atores e permissões

| Ator | Pode fazer | Não pode fazer |
|---|---|---|
| Dono (usuário autenticado) | Criar, editar, arquivar, desarquivar e excluir objetos e contatos (RN15 a RN19, RN24 a RN26); bloquear e desbloquear contatos (RN22); registrar, renovar, devolver (integral e parcial), marcar perda, registrar recuperação, cancelar e anotar observações em empréstimos (RN28 a RN83); registrar dano, contestação e retificação (RN61 a RN66); consultar listas, históricos e avisos; marcar avisos como lidos; anonimizar contato (RN93) | Ver dados de outra conta (RN02); editar ou excluir empréstimos e eventos (RN36, RN90); renovar empréstimo atrasado ou encerrado (RN54); marcar perda antes de 30 dias de atraso (RN73); reabrir empréstimo encerrado (RN77); alterar dano por outro meio que a retificação única (RN66); reverter anonimização (RN93) |
| Sistema (regras automáticas) | Calcular classificação em dia ou atrasado (RN39); gerar avisos às 09:00 (RN45); derivar status Emprestado e Indisponível automático (RN12, RN13); validar transições e recusar ações inválidas (RN82); gravar eventos (RN83) | Criar, renovar ou encerrar empréstimos por conta própria; bloquear contatos (RN22); alterar dano; enviar avisos por canal externo (RN44) |
| Tomador | Nada no aplicativo. Pede renovação, contesta dano ou pede remoção de dados por meio externo ao dono (RN51, RN65, RN93) | Acessar o aplicativo; ver ou alterar qualquer registro (RN03) |

## 5. Regras de negócio

Cada regra é atômica, verificável e cita a decisão de origem (seção 11). Datas e horas seguem o fuso America/Fortaleza.

### 5.1 Conta e acesso

- **RN01** — Cada conta pertence a exatamente um dono, identificado por e-mail único, e o acesso exige e-mail e senha com no mínimo 8 caracteres. (DEC01, DEC45)
- **RN02** — Objetos, contatos, empréstimos, eventos e avisos pertencem à conta que os criou e nunca são exibidos a outra conta. (DEC01, DEC45)
- **RN03** — O tomador não possui conta nem acesso; é representado exclusivamente por um contato. (DEC01)

### 5.2 Objetos

- **RN04** — O cadastro de objeto exige nome com 1 a 80 caracteres, categoria (RN05) e estado de conservação (RN06); a foto é opcional (RN07). (DEC03, DEC41)
- **RN05** — A categoria é uma destas oito, sem outras: Livros, Ferramentas, Jogos, Eletrônicos, Casa e cozinha, Esporte e lazer, Roupas e acessórios, Outros. (DEC03, DEC41)
- **RN06** — O estado de conservação usa a escala ordenada novo > bom > regular > ruim, no cadastro, na saída, na volta e na recuperação. (DEC15)
- **RN07** — A foto do objeto é um arquivo JPEG ou PNG de até 5 MB, no máximo uma por objeto, substituível pelo dono. (DEC41)
- **RN08** — O nome do objeto não precisa ser único; o sistema gera um identificador interno. (DEC41)
- **RN09** — Cada objeto representa uma única unidade física; itens idênticos são cadastrados como objetos distintos. (DEC04)
- **RN10** — Um objeto pode ser cadastrado como conjunto com 2 a 50 componentes, cada um com nome de 1 a 80 caracteres; componentes não podem ser emprestados separadamente. (DEC05, DEC41)
- **RN11** — O status do objeto é um destes: Disponível, Emprestado, Indisponível, Arquivado, com precedência Arquivado > Emprestado > Indisponível > Disponível. (DEC29)
- **RN12** — Um objeto é Emprestado enquanto existir um empréstimo Ativo dele; o status não é gravado, é derivado. (DEC29)
- **RN13** — Um objeto fica Indisponível por marcação manual do dono (motivo opcional de até 500 caracteres) ou automaticamente nos casos das RN63, RN71 e RN75. (DEC17, DEC29, DEC34, DEC36)
- **RN14** — Marcar ou desmarcar Indisponível só é permitido quando o objeto não está Emprestado. (DEC29)
- **RN15** — Nome, categoria e foto do objeto podem ser editados a qualquer momento; o estado de conservação só quando o objeto não está Emprestado. (DEC44)
- **RN16** — Objeto sem nenhum empréstimo registrado pode ser excluído fisicamente, junto com componentes e foto. (DEC42)
- **RN17** — Objeto com ao menos um empréstimo, em qualquer estado, não pode ser excluído; só arquivado. (DEC22, DEC42)
- **RN18** — Arquivar objeto exige que ele não tenha empréstimo Ativo. (DEC42)
- **RN19** — Objeto Arquivado não aceita novo empréstimo e pode ser desarquivado pelo dono a qualquer momento. (DEC22, DEC42)

### 5.3 Contatos

- **RN20** — O cadastro de contato exige nome com 1 a 80 caracteres; telefone (10 ou 11 dígitos numéricos) e observação (até 500 caracteres) são opcionais; nenhum outro dado pessoal é coletado. (DEC06, DEC41, DEC48)
- **RN21** — O nome do contato não precisa ser único; o sistema gera um identificador interno. (DEC41)
- **RN22** — O dono pode bloquear e desbloquear um contato manualmente, com motivo opcional de até 500 caracteres; não existe bloqueio automático. (DEC11, DEC23, DEC48)
- **RN23** — Contato Bloqueado não pode ser tomador de novo empréstimo; seus empréstimos Ativos continuam e podem ser renovados. (DEC23, DEC40)
- **RN24** — Nome, telefone e observação do contato podem ser editados a qualquer momento; o histórico exibe o nome atual. (DEC44)
- **RN25** — Contato sem nenhum empréstimo pode ser excluído fisicamente; com empréstimo só pode ser arquivado; arquivar exige nenhum empréstimo Ativo; arquivamento é reversível. (DEC22, DEC42)
- **RN26** — Contato Arquivado não pode ser tomador de novo empréstimo. (DEC22)
- **RN27** — Não há limite de empréstimos Ativos simultâneos por contato. (DEC43)

### 5.4 Registro de empréstimo e prazo

- **RN28** — Registrar empréstimo exige objeto Disponível, contato não Bloqueado e não Arquivado, data de saída e data de vencimento; o estado na saída pode ser ajustado e uma observação de até 300 caracteres é opcional. (DEC04, DEC23, DEC29, DEC30)
- **RN29** — Um objeto tem no máximo um empréstimo Ativo por vez. (DEC04)
- **RN30** — A data de saída deve ser menor ou igual a hoje; não há limite para datas passadas. (DEC09, DEC38)
- **RN31** — O prazo é informado como data de vencimento; o campo vem pré-preenchido com data de saída mais 7 dias e é editável. (DEC07, DEC48)
- **RN32** — A data de vencimento deve estar entre data de saída mais 1 dia e data de saída mais 365 dias corridos; não existe prazo padrão por categoria e não existe empréstimo sem prazo. (DEC07, DEC08)
- **RN33** — O vencimento ocorre às 23:59:59,999 da data de vencimento no fuso America/Fortaleza, para todas as contas, independentemente da localização do dono. (DEC09, DEC48)
- **RN34** — O estado na saída é preenchido com o estado de conservação atual do objeto e pode ser alterado pelo dono no ato; o valor gravado no empréstimo passa a ser o estado atual do objeto. (DEC30)
- **RN35** — Um registro válido cria o empréstimo no estado Ativo, grava o evento "Registro" e torna o objeto Emprestado. (DEC26, DEC29)
- **RN36** — Após o registro, nenhum campo do empréstimo pode ser editado e o empréstimo não pode ser excluído; correções se dão por renovação, devolução, perda ou cancelamento. (DEC37)
- **RN37** — Todo cálculo de data e hora usa o relógio oficial do servidor no fuso America/Fortaleza, nunca o relógio do dispositivo. (DEC25)
- **RN38** — "Hoje" é a data civil corrente em America/Fortaleza no instante da ação. (DEC09, DEC25)

### 5.5 Atraso

- **RN39** — Um empréstimo Ativo é atrasado a partir das 00:00:00 do dia seguinte à data de vencimento, sem tolerância; antes disso é em dia. (DEC10, DEC27)
- **RN40** — Atrasado é uma classificação calculada no momento da consulta, não um estado persistido. (DEC27)
- **RN41** — Dias de atraso = hoje menos data de vencimento, em dias corridos. (DEC10)
- **RN42** — Atraso não gera multa, pontuação, bloqueio automático do contato nem impedimento de novos empréstimos a ele. (DEC11)
- **RN43** — Empréstimo registrado com data de vencimento anterior a hoje nasce Ativo e já atrasado. (DEC38)

### 5.6 Avisos

- **RN44** — Aviso é um registro no painel com tipo, data e hora de geração, empréstimo e situação lido ou não lido; o painel é o único canal de aviso. (DEC12, DEC39)
- **RN45** — O sistema gera, às 09:00:00 America/Fortaleza: "Prazo próximo" no dia vencimento menos 2; "Vence hoje" no dia do vencimento; "Atrasado" em cada dia em que o empréstimo esteja atrasado; "Perda possível" no 30º dia de atraso, substituindo o aviso "Atrasado" daquele dia. (DEC12, DEC39, DEC49)
- **RN46** — Não se gera aviso cujo instante programado seja anterior ao instante de registro do empréstimo. (DEC12, DEC38)
- **RN47** — Gera-se no máximo um aviso por empréstimo por dia civil. (DEC39)
- **RN48** — A renovação descarta os avisos ainda não gerados e recalcula "Prazo próximo" e "Vence hoje" a partir do novo vencimento. (DEC39)
- **RN49** — Ao encerrar o empréstimo (Devolvido, Perdido ou Cancelado) nenhum aviso novo é gerado; os já gerados permanecem no painel até o dono marcá-los como lidos. (DEC12, DEC39)
- **RN50** — O painel exibe o contador de avisos não lidos em todas as telas e permite marcar como lido um aviso ou todos. (DEC39)

### 5.7 Renovação

- **RN51** — Somente o dono renova; pedido do tomador ocorre fora do aplicativo. (DEC13)
- **RN52** — Cada empréstimo admite no máximo 2 renovações. (DEC13)
- **RN53** — Cada renovação adiciona de 1 a 7 dias corridos à data de vencimento atual, nunca à data de hoje. (DEC13, DEC14, DEC40)
- **RN54** — A renovação só é permitida com o empréstimo Ativo e em dia, isto é, até 23:59:59,999 da data de vencimento. (DEC14, DEC40)
- **RN55** — A renovação é permitida mesmo com devolução parcial registrada ou contato Bloqueado. (DEC36, DEC40)
- **RN56** — Cada renovação grava o evento "Renovação" com vencimento anterior, novo vencimento e número da renovação (1 ou 2). (DEC40)
- **RN57** — O limite de 365 dias (RN32) aplica-se apenas ao registro; com renovações o vencimento pode chegar a saída mais 379 dias. (DEC08, DEC13)

### 5.8 Devolução integral e dano

- **RN58** — A devolução integral exige data de devolução (entre data de saída e hoje, pré-preenchida com hoje), estado de volta (RN06) e nível de dano (RN61); descrição de 1 a 500 caracteres é obrigatória se o dano for diferente de "nenhum"; uma foto JPEG ou PNG de até 5 MB é opcional. (DEC16, DEC31, DEC33, DEC41)
- **RN59** — A devolução integral leva o empréstimo de Ativo a Devolvido, grava o evento "Devolução", atualiza o estado de conservação do objeto para o estado de volta e devolve o objeto a Disponível, salvo RN63. (DEC17, DEC29, DEC30)
- **RN60** — Devolução com data menor ou igual ao vencimento é "no prazo"; com data posterior é "com atraso", com dias de atraso = data de devolução menos vencimento. (DEC33)
- **RN61** — O nível de dano é um destes: nenhum, leve, grave, inutilizado; quem define é exclusivamente o dono. (DEC16)
- **RN62** — Dano "inutilizado" exige estado de volta "ruim"; qualquer outra combinação de estado e dano é aceita. (DEC31)
- **RN63** — Dano grave ou inutilizado torna o objeto Indisponível automaticamente, com motivo "dano grave" ou "inutilizado"; o dono pode desmarcar depois (RN14). (DEC17, DEC29)
- **RN64** — O dano gera apenas registro; o aplicativo não calcula, cobra nem controla reparo, reposição ou indenização. (DEC17)
- **RN65** — O dono pode registrar uma única contestação por registro de dano, com 1 a 500 caracteres, a qualquer momento após a devolução ou recuperação; a contestação não altera o nível de dano. (DEC18, DEC31)
- **RN66** — O dono pode retificar o nível de dano uma única vez, até 7 dias corridos após a data em que a devolução ou recuperação foi registrada; o evento "Retificação" preserva o nível anterior e reaplica RN62 e RN63 ao novo nível; se o novo nível for nenhum ou leve e a indisponibilidade era automática por dano, o objeto volta a Disponível. (DEC32)

### 5.9 Devolução parcial

- **RN67** — A devolução parcial só existe para conjuntos: o dono marca cada componente como recebido ou faltante, com ao menos um recebido e ao menos um faltante; informa data (RN58) e observação opcional de até 300 caracteres. (DEC19, DEC36)
- **RN68** — A devolução parcial mantém o empréstimo Ativo; prazo, classificação de atraso, avisos e renovação continuam valendo. (DEC19, DEC36)
- **RN69** — Componente marcado recebido não pode voltar a faltante; devoluções parciais sucessivas são permitidas até que todos os componentes estejam recebidos. (DEC36)
- **RN70** — Quando o último componente faltante é marcado recebido, a ação segue as regras da devolução integral (RN58 a RN63), com estado de volta e dano informados nesse momento. (DEC36)
- **RN71** — Perda declarada em empréstimo com devolução parcial leva ao estado Perdido, grava quais componentes foram recebidos e torna o objeto Indisponível com motivo "incompleto". (DEC36)
- **RN72** — Objeto sem componentes só admite devolução integral. (DEC19)

### 5.10 Perda, não devolução e recuperação

- **RN73** — O dono só pode marcar o empréstimo como Perdido quando os dias de atraso forem maiores ou iguais a 30. (DEC20)
- **RN74** — A perda exige motivo "perda comunicada" ou "não devolução" e aceita observação opcional de até 500 caracteres. (DEC34)
- **RN75** — Marcar perda leva o empréstimo de Ativo a Perdido, grava o evento "Perda", torna o objeto Indisponível com motivo "perdido" (ou "incompleto", RN71) e interrompe avisos. (DEC29, DEC34)
- **RN76** — Perda comunicada pelo tomador antes do 30º dia de atraso não altera o estado: o empréstimo segue Ativo e atrasado e o dono registra o fato como observação (RN84). (DEC20, DEC49)
- **RN77** — Perdido é estado final: o empréstimo não volta a Ativo. (DEC21)
- **RN78** — Em empréstimo Perdido o dono pode registrar uma única recuperação com data (entre a data da perda e hoje), estado de conservação (RN06) e dano (RN61 a RN63, RN58 para descrição e foto); o empréstimo permanece Perdido com a marca "recuperado em dd/mm/aaaa" e o objeto volta a Disponível, ou fica Indisponível conforme RN63. (DEC21, DEC35)
- **RN79** — Após a recuperação, emprestar o objeto de novo exige um novo registro de empréstimo. (DEC21)

### 5.11 Cancelamento e observações

- **RN80** — O dono pode cancelar um empréstimo Ativo que não tenha renovação nem devolução parcial, informando motivo de 1 a 200 caracteres; o empréstimo passa a Cancelado, o objeto volta a Disponível com o estado de conservação inalterado e os avisos cessam. (DEC37)
- **RN81** — Empréstimo Cancelado aparece em Encerrados e nos históricos, mas não entra em contagens de atraso nem de devolução. (DEC37)
- **RN82** — Toda ação sobre empréstimo valida o estado atual no instante da gravação; se o estado não admitir a ação, ela é recusada sem alterar nenhum registro e com a mensagem específica da seção 7.3. (DEC26)
- **RN83** — Somente ações aceitas geram eventos; ações recusadas não deixam registro. (DEC26)
- **RN84** — O dono pode anexar a qualquer empréstimo, em qualquer estado, uma observação de 1 a 500 caracteres, gravada como evento "Observação". (DEC49)

### 5.12 Listagens e histórico

- **RN85** — A lista Ativos contém todos os empréstimos Ativos, com os atrasados destacados, ordenados por data de vencimento crescente. (DEC28)
- **RN86** — A lista Atrasados contém os empréstimos Ativos com data de vencimento anterior a hoje, ordenados por dias de atraso decrescente. (DEC28)
- **RN87** — A lista Encerrados contém os empréstimos Devolvido, Perdido e Cancelado, ordenados por data de encerramento decrescente. (DEC28)
- **RN88** — O histórico de um objeto lista todos os seus empréstimos, inclusive de contatos arquivados ou anonimizados, ordenados por data de saída decrescente, cada um com seus eventos. (DEC22)
- **RN89** — O histórico de um contato lista todos os seus empréstimos, inclusive de objetos arquivados, ordenados por data de saída decrescente, cada um com seus eventos. (DEC22)
- **RN90** — Eventos são imutáveis: nunca são editados nem excluídos, nem pela anonimização. (DEC21, DEC26)
- **RN91** — Cada empréstimo no histórico exibe: objeto, contato, data de saída, vencimento atual, estado, classificação (em dia, atrasado com dias, no prazo, com atraso com dias), renovações, estado na saída e na volta, dano, contestação, retificação e recuperação, quando existirem. (DEC28, DEC33)

### 5.13 Dados pessoais e privacidade

- **RN92** — Os dados do contato (nome, telefone, observação, bloqueio e motivo, datas) são usados exclusivamente para identificar o tomador e acompanhar seus empréstimos; a finalidade é exibida na tela "Privacidade" do aplicativo. (DEC24, DEC46)
- **RN93** — O dono pode anonimizar um contato sem empréstimo Ativo: o nome vira "Contato removido", telefone, observação e motivo de bloqueio são apagados, o contato fica Arquivado e os eventos são preservados; a ação é irreversível e grava data e hora. (DEC24, DEC46)
- **RN94** — Anonimização de contato com empréstimo Ativo é recusada com a mensagem "Não é possível anonimizar: há empréstimo ativo". (DEC46)
- **RN95** — A tela "Privacidade" informa que o pedido de remoção é feito pelo tomador ao dono por meio externo e orienta o dono a atendê-lo em até 15 dias corridos. (DEC46)
- **RN96** — Somente o dono autenticado acessa os dados de seus contatos; nenhum dado de contato é exposto a terceiros, exportado ou compartilhado. (DEC24, DEC45)

## 6. Histórias de usuário

Convenções dos cenários: hoje é 30/10/2026 (quinta-feira), salvo indicação; horas no fuso America/Fortaleza; mensagens entre aspas são exibidas literalmente.

### HU01 — Cadastrar objeto

**Como** dono, **quero** cadastrar um objeto, **para** poder emprestá-lo depois.  
**Regras relacionadas:** RN04, RN05, RN06, RN07, RN08, RN09, RN11.

- **Cenário: cadastro válido**  
  Dado que estou na tela de novo objeto  
  Quando informo nome "Furadeira Bosch", categoria "Ferramentas", estado "bom" e nenhuma foto  
  Então o objeto é salvo com status "Disponível" e aparece na lista de objetos
- **Cenário: nome vazio**  
  Dado que estou na tela de novo objeto  
  Quando tento salvar com nome vazio, categoria "Livros" e estado "novo"  
  Então vejo "Informe o nome do objeto" e nada é salvo
- **Cenário: categoria não selecionada**  
  Dado que informei nome "Dom Casmurro" e estado "bom"  
  Quando tento salvar sem categoria  
  Então vejo "Selecione uma categoria" e nada é salvo
- **Cenário: estado não informado**  
  Dado que informei nome "Dom Casmurro" e categoria "Livros"  
  Quando tento salvar sem estado de conservação  
  Então vejo "Informe o estado de conservação" e nada é salvo
- **Cenário limite: nome com 80 caracteres**  
  Dado que informo um nome com exatamente 80 caracteres  
  Quando salvo  
  Então o objeto é salvo com o nome completo
- **Cenário limite: nome com 81 caracteres**  
  Dado que informo um nome com 81 caracteres  
  Quando tento salvar  
  Então vejo "O nome deve ter de 1 a 80 caracteres" e nada é salvo
- **Cenário: foto inválida**  
  Dado que anexo um arquivo PDF ou uma imagem JPEG de 6 MB  
  Quando tento salvar  
  Então vejo "A foto deve ser JPEG ou PNG com até 5 MB" e nada é salvo
- **Cenário: nomes repetidos**  
  Dado que já existe o objeto "Cadeira dobrável"  
  Quando cadastro outro objeto "Cadeira dobrável"  
  Então ambos são salvos, com identificadores distintos, e a lista mostra dois objetos

### HU02 — Cadastrar conjunto

**Como** dono, **quero** cadastrar um conjunto com componentes, **para** controlar devoluções incompletas.  
**Regras relacionadas:** RN10, RN04, RN67, RN72.

- **Cenário: conjunto válido**  
  Dado que estou na tela de novo objeto e marco "É um conjunto"  
  Quando informo nome "PlayStation 5", categoria "Eletrônicos", estado "bom" e componentes "Console", "Controle 1", "Controle 2"  
  Então o objeto é salvo como conjunto com 3 componentes e status "Disponível"
- **Cenário: um componente só**  
  Dado que marquei "É um conjunto"  
  Quando informo apenas o componente "Console" e tento salvar  
  Então vejo "Um conjunto precisa de 2 a 50 componentes" e nada é salvo
- **Cenário limite: 51 componentes**  
  Dado que informei 51 componentes  
  Quando tento salvar  
  Então vejo "Um conjunto precisa de 2 a 50 componentes" e nada é salvo
- **Cenário: componente sem nome**  
  Dado que um componente está com nome vazio  
  Quando tento salvar  
  Então vejo "Informe o nome de todos os componentes" e nada é salvo

### HU03 — Cadastrar contato

**Como** dono, **quero** cadastrar um amigo, **para** registrar empréstimos a ele.  
**Regras relacionadas:** RN20, RN21, RN92.

- **Cenário: cadastro mínimo**  
  Dado que estou na tela de novo contato  
  Quando informo nome "Ana Souza" e deixo telefone e observação vazios  
  Então o contato é salvo e aparece na lista como "Ana Souza"
- **Cenário: nome vazio**  
  Quando tento salvar com nome vazio  
  Então vejo "Informe o nome do contato" e nada é salvo
- **Cenário: telefone com 9 dígitos**  
  Dado que informo nome "Ana Souza" e telefone "859999999"  
  Quando tento salvar  
  Então vejo "Telefone deve ter 10 ou 11 dígitos" e nada é salvo
- **Cenário limite: telefone com 11 dígitos**  
  Dado que informo telefone "85999990000"  
  Quando salvo  
  Então o contato é salvo com o telefone
- **Cenário: tela de privacidade**  
  Dado que estou na tela de novo contato  
  Quando aciono "Privacidade"  
  Então vejo o texto da finalidade (RN92) e a orientação de atendimento em até 15 dias (RN95)

### HU04 — Bloquear e desbloquear contato

**Como** dono, **quero** bloquear um amigo, **para** impedir novos empréstimos a ele sem perder o histórico.  
**Regras relacionadas:** RN22, RN23, RN42, RN55.

- **Cenário: bloqueio com motivo**  
  Dado que "Ana Souza" não está bloqueada  
  Quando aciono "Bloquear" e informo motivo "Devolveu o livro rasgado"  
  Então o contato exibe a marca "Bloqueado" e o motivo
- **Cenário: bloqueio sem motivo**  
  Quando aciono "Bloquear" sem motivo  
  Então o contato é bloqueado e o motivo fica vazio
- **Cenário: empréstimo ativo continua**  
  Dado que "Ana Souza" tem o empréstimo da "Furadeira Bosch" Ativo com vencimento 06/11/2026  
  Quando bloqueio o contato  
  Então o empréstimo continua Ativo com vencimento 06/11/2026 e aparece em Ativos
- **Cenário: novo empréstimo recusado**  
  Dado que "Ana Souza" está bloqueada  
  Quando tento registrar empréstimo do "Dom Casmurro" para ela  
  Então vejo "Contato bloqueado não pode receber empréstimos" e nada é salvo
- **Cenário: desbloqueio**  
  Dado que "Ana Souza" está bloqueada  
  Quando aciono "Desbloquear"  
  Então a marca desaparece e ela volta a poder receber empréstimos
- **Cenário: atraso não bloqueia**  
  Dado que "Ana Souza" tem empréstimo atrasado há 45 dias  
  Quando consulto o contato  
  Então ele não está bloqueado e um novo empréstimo a ela é aceito

### HU05 — Registrar empréstimo

**Como** dono, **quero** registrar que emprestei um objeto, **para** acompanhar a devolução.  
**Regras relacionadas:** RN28 a RN35, RN43, RN46.

- **Cenário: registro válido com prazo sugerido**  
  Dado que "Furadeira Bosch" está Disponível com estado "bom" e "Ana Souza" não está bloqueada nem arquivada  
  Quando abro novo empréstimo, seleciono o objeto e o contato e aceito saída "30/10/2026" e vencimento pré-preenchido "06/11/2026"  
  Então o empréstimo é salvo como Ativo, em dia, estado na saída "bom", o objeto passa a "Emprestado" e o evento "Registro" aparece no histórico
- **Cenário: ajuste do estado na saída**  
  Dado que o objeto tem estado "bom"  
  Quando altero o estado na saída para "regular" com observação "Arranhão na lateral" e salvo  
  Então o empréstimo grava estado na saída "regular" e o objeto passa a ter estado "regular"
- **Cenário: saída futura**  
  Quando informo saída "31/10/2026"  
  Então vejo "A data de saída não pode ser futura" e nada é salvo
- **Cenário limite: vencimento igual à saída**  
  Quando informo saída "30/10/2026" e vencimento "30/10/2026"  
  Então vejo "O vencimento deve ser de 1 a 365 dias após a saída" e nada é salvo
- **Cenário limite: vencimento em 365 dias**  
  Quando informo saída "30/10/2026" e vencimento "30/10/2027"  
  Então o empréstimo é salvo
- **Cenário limite: vencimento em 366 dias**  
  Quando informo saída "30/10/2026" e vencimento "31/10/2027"  
  Então vejo "O vencimento deve ser de 1 a 365 dias após a saída" e nada é salvo
- **Cenário: objeto já emprestado**  
  Dado que "Furadeira Bosch" tem empréstimo Ativo  
  Quando tento registrar outro empréstimo dela  
  Então vejo "Objeto já emprestado" e nada é salvo
- **Cenário: objeto indisponível ou arquivado**  
  Dado que o objeto está Indisponível (ou Arquivado)  
  Quando tento registrar empréstimo  
  Então vejo "Objeto indisponível" (ou "Objeto arquivado") e nada é salvo
- **Cenário: registro retroativo já vencido**  
  Dado que hoje é 30/10/2026  
  Quando registro saída "01/09/2026" e vencimento "08/09/2026"  
  Então o empréstimo é salvo Ativo, aparece em Atrasados com 52 dias de atraso, e nenhum aviso de "Prazo próximo" ou "Vence hoje" é gerado
- **Cenário: dois empréstimos para o mesmo contato**  
  Dado que "Ana Souza" já tem a "Furadeira Bosch" emprestada  
  Quando registro o "Dom Casmurro" para ela  
  Então o segundo empréstimo é aceito

### HU06 — Renovar empréstimo

**Como** dono, **quero** renovar um empréstimo, **para** adiar o vencimento a pedido do amigo.  
**Regras relacionadas:** RN51 a RN57, RN48.

- **Cenário: primeira renovação**  
  Dado um empréstimo Ativo com vencimento 06/11/2026 e hoje 30/10/2026  
  Quando aciono "Renovar" com 7 dias  
  Então o vencimento passa a 13/11/2026, o contador exibe "1 de 2 renovações" e o evento "Renovação" registra 06/11/2026 → 13/11/2026
- **Cenário: renovação parte do vencimento, não de hoje**  
  Dado vencimento 06/11/2026 e hoje 02/11/2026  
  Quando renovo por 3 dias  
  Então o vencimento passa a 09/11/2026
- **Cenário limite: no dia do vencimento**  
  Dado vencimento 06/11/2026 e agora 06/11/2026 às 23:30  
  Quando renovo por 7 dias  
  Então a renovação é aceita e o vencimento passa a 13/11/2026
- **Cenário: já atrasado**  
  Dado vencimento 06/11/2026 e agora 07/11/2026 às 00:05  
  Quando tento renovar  
  Então vejo "Empréstimo atrasado não pode ser renovado" e o vencimento continua 06/11/2026
- **Cenário limite: terceira renovação**  
  Dado um empréstimo com 2 renovações  
  Quando tento renovar  
  Então vejo "Limite de 2 renovações atingido" e nada muda
- **Cenário limite: 0 ou 8 dias**  
  Quando informo 0 dias (ou 8 dias)  
  Então vejo "A renovação deve ser de 1 a 7 dias" e nada muda
- **Cenário: encerrado**  
  Dado um empréstimo Devolvido  
  Quando tento renovar  
  Então vejo "Este empréstimo já está encerrado" e nada muda
- **Cenário: avisos recalculados**  
  Dado vencimento 06/11/2026 renovado em 30/10/2026 para 13/11/2026  
  Quando chega 04/11/2026 às 09:00  
  Então nenhum aviso é gerado; o aviso "Prazo próximo" é gerado em 11/11/2026 às 09:00

### HU07 — Registrar devolução integral

**Como** dono, **quero** registrar a volta do objeto com seu estado, **para** encerrar o empréstimo e comparar com a saída.  
**Regras relacionadas:** RN58 a RN64, RN49.

- **Cenário: devolução no prazo sem dano**  
  Dado empréstimo Ativo da "Furadeira Bosch" com saída "bom" e vencimento 06/11/2026, hoje 05/11/2026  
  Quando registro devolução com data "05/11/2026", estado "bom" e dano "nenhum"  
  Então o empréstimo fica Devolvido "no prazo", o objeto volta a "Disponível" com estado "bom" e aparece em Encerrados
- **Cenário: devolução com atraso**  
  Dado vencimento 06/11/2026 e hoje 10/11/2026  
  Quando registro devolução com data "10/11/2026"  
  Então o empréstimo fica Devolvido "com atraso (4 dias)"
- **Cenário: devolução retroativa**  
  Dado vencimento 06/11/2026 e hoje 10/11/2026  
  Quando registro devolução com data "06/11/2026"  
  Então o empréstimo fica Devolvido "no prazo"
- **Cenário: data anterior à saída**  
  Dado saída 30/10/2026  
  Quando informo data de devolução "29/10/2026" (ou "11/11/2026" quando hoje é 10/11/2026)  
  Então vejo "A data de devolução deve estar entre a saída e hoje" e nada é salvo
- **Cenário: dano sem descrição**  
  Quando informo dano "leve" e descrição vazia  
  Então vejo "Descreva o dano" e nada é salvo
- **Cenário: dano grave**  
  Quando registro estado "ruim", dano "grave" e descrição "Mandril travado"  
  Então o empréstimo fica Devolvido, o objeto fica "Indisponível" com motivo "dano grave" e estado "ruim"
- **Cenário: inutilizado com estado incompatível**  
  Quando informo dano "inutilizado" e estado "regular"  
  Então vejo "Objeto inutilizado deve ter estado 'ruim'" e nada é salvo
- **Cenário: devolução duplicada**  
  Dado empréstimo já Devolvido  
  Quando tento registrar devolução de novo  
  Então vejo "Este empréstimo já está encerrado" e nada muda
- **Cenário: avisos cessam**  
  Dado empréstimo atrasado devolvido em 10/11/2026 às 15:00  
  Quando chega 11/11/2026 às 09:00  
  Então nenhum aviso "Atrasado" é gerado para ele e o aviso de 10/11 continua no painel até ser lido

### HU08 — Registrar devolução parcial

**Como** dono, **quero** registrar que só parte de um conjunto voltou, **para** não dar o empréstimo como encerrado.  
**Regras relacionadas:** RN67 a RN72.

- **Cenário: parcial válida**  
  Dado empréstimo Ativo do "PlayStation 5" (Console, Controle 1, Controle 2), vencimento 06/11/2026  
  Quando registro devolução parcial em "03/11/2026" marcando "Console" e "Controle 1" recebidos e "Controle 2" faltante  
  Então o empréstimo continua Ativo com marca "Devolução parcial: falta Controle 2", o vencimento permanece 06/11/2026 e o evento "Devolução parcial" é gravado
- **Cenário: nada recebido**  
  Quando tento registrar parcial sem marcar componente recebido  
  Então vejo "Marque ao menos um componente recebido" e nada é salvo
- **Cenário: todos recebidos vira integral**  
  Dado parcial anterior com "Controle 2" faltante  
  Quando marco "Controle 2" recebido em "08/11/2026" com estado "bom" e dano "nenhum"  
  Então o empréstimo fica Devolvido "com atraso (2 dias)" e o objeto volta a "Disponível"
- **Cenário: renovação após parcial**  
  Dado parcial registrada e hoje 05/11/2026  
  Quando renovo por 5 dias  
  Então o vencimento passa a 11/11/2026
- **Cenário: objeto sem componentes**  
  Dado empréstimo da "Furadeira Bosch"  
  Quando abro a devolução  
  Então a opção de devolução parcial não é exibida
- **Cenário: perda após parcial**  
  Dado parcial com "Controle 2" faltante e 30 dias de atraso  
  Quando marco perda com motivo "não devolução"  
  Então o empréstimo fica Perdido com registro "recebidos: Console, Controle 1; faltante: Controle 2" e o objeto fica "Indisponível" com motivo "incompleto"

### HU09 — Registrar contestação e retificar dano

**Como** dono, **quero** anotar a discordância do amigo e corrigir um dano registrado por engano, **para** manter o histórico fiel.  
**Regras relacionadas:** RN65, RN66, RN90.

- **Cenário: contestação**  
  Dado devolução registrada em 10/11/2026 com dano "leve"  
  Quando registro contestação "Ana afirma que o risco já existia"  
  Então o evento "Contestação" aparece no histórico e o dano continua "leve"
- **Cenário: segunda contestação**  
  Dado que já existe contestação  
  Quando tento registrar outra  
  Então vejo "Contestação já registrada" e nada muda
- **Cenário: retificação dentro do prazo**  
  Dado devolução em 10/11/2026 com dano "grave" e objeto Indisponível, hoje 15/11/2026  
  Quando retifico para "leve"  
  Então o evento "Retificação: grave → leve" é gravado e o objeto volta a "Disponível"
- **Cenário limite: 8º dia**  
  Dado devolução em 10/11/2026 e hoje 18/11/2026  
  Quando tento retificar  
  Então vejo "Prazo de retificação encerrado" e nada muda
- **Cenário: segunda retificação**  
  Dado que já houve retificação  
  Quando tento retificar de novo  
  Então vejo "Dano já retificado" e nada muda
- **Cenário: retificar para inutilizado com estado "bom"**  
  Quando retifico para "inutilizado" com estado de volta registrado "bom"  
  Então vejo "Objeto inutilizado deve ter estado 'ruim'" e nada muda

### HU10 — Registrar perda ou não devolução

**Como** dono, **quero** marcar que o objeto não voltou, **para** encerrar o empréstimo e deixar o objeto indisponível.  
**Regras relacionadas:** RN73 a RN77, RN84.

- **Cenário: perda após 30 dias**  
  Dado vencimento 06/11/2026 e hoje 06/12/2026 (30 dias de atraso)  
  Quando marco perda com motivo "não devolução"  
  Então o empréstimo fica Perdido, o objeto fica "Indisponível" com motivo "perdido" e aparece em Encerrados
- **Cenário limite: 29 dias**  
  Dado vencimento 06/11/2026 e hoje 05/12/2026  
  Quando tento marcar perda  
  Então vejo "Perda só pode ser registrada após 30 dias de atraso" e nada muda
- **Cenário: perda comunicada cedo**  
  Dado vencimento 06/11/2026, hoje 08/11/2026 e o tomador avisou que perdeu o objeto  
  Quando registro observação "Ana comunicou perda em 08/11/2026"  
  Então o evento "Observação" é gravado, o empréstimo segue Ativo e atrasado, e a perda só pode ser marcada em 06/12/2026
- **Cenário: motivo ausente**  
  Quando tento marcar perda sem motivo  
  Então vejo "Informe o motivo da perda" e nada muda
- **Cenário: aviso "Perda possível"**  
  Dado vencimento 06/11/2026  
  Quando chega 06/12/2026 às 09:00  
  Então o painel recebe "Perda possível" e não recebe "Atrasado" nesse dia
- **Cenário: perda em empréstimo encerrado**  
  Dado empréstimo Devolvido  
  Quando tento marcar perda  
  Então vejo "Este empréstimo já está encerrado" e nada muda

### HU11 — Registrar recuperação de objeto perdido

**Como** dono, **quero** registrar que um objeto perdido reapareceu, **para** voltar a usá-lo sem apagar a perda.  
**Regras relacionadas:** RN77, RN78, RN79.

- **Cenário: recuperação sem dano**  
  Dado empréstimo Perdido em 06/12/2026 e hoje 20/12/2026  
  Quando registro recuperação com data "20/12/2026", estado "regular" e dano "nenhum"  
  Então o empréstimo exibe "Perdido — recuperado em 20/12/2026", o evento "Recuperado" é gravado e o objeto fica "Disponível" com estado "regular"
- **Cenário: recuperação com dano grave**  
  Quando registro dano "grave" com descrição "Tela trincada" e estado "ruim"  
  Então o objeto fica "Indisponível" com motivo "dano grave"
- **Cenário: data anterior à perda**  
  Quando informo data "05/12/2026"  
  Então vejo "A data de recuperação deve estar entre a perda e hoje" e nada muda
- **Cenário: segunda recuperação**  
  Dado que já há recuperação registrada  
  Quando tento registrar outra  
  Então vejo "Recuperação já registrada" e nada muda
- **Cenário: empréstimo não reabre**  
  Dado empréstimo recuperado  
  Quando consulto Ativos  
  Então ele não aparece; aparece em Encerrados como Perdido

### HU12 — Cancelar empréstimo registrado por engano (condicionado à DEC37)

**Como** dono, **quero** cancelar um registro feito por engano, **para** não falsear o histórico.  
**Regras relacionadas:** RN80, RN81.

- **Cenário: cancelamento válido**  
  Dado empréstimo Ativo sem renovação nem devolução parcial  
  Quando aciono "Cancelar" com motivo "Registrei no contato errado"  
  Então o empréstimo fica Cancelado, o objeto volta a "Disponível" e o evento "Cancelamento" é gravado
- **Cenário: com renovação**  
  Dado empréstimo com 1 renovação  
  Quando tento cancelar  
  Então vejo "Empréstimo com renovação ou devolução parcial não pode ser cancelado" e nada muda
- **Cenário: motivo vazio**  
  Quando tento cancelar sem motivo  
  Então vejo "Informe o motivo do cancelamento" e nada muda

### HU13 — Listar ativos, atrasados e encerrados

**Como** dono, **quero** ver meus empréstimos por situação, **para** saber o que cobrar.  
**Regras relacionadas:** RN39 a RN41, RN85 a RN87.

- **Cenário: lista Ativos**  
  Dado três empréstimos Ativos com vencimentos 06/11/2026, 02/11/2026 e 28/10/2026, hoje 30/10/2026  
  Quando abro Ativos  
  Então vejo os três, na ordem 28/10 (destacado "Atrasado 2 dias"), 02/11, 06/11
- **Cenário: lista Atrasados**  
  Dado os mesmos empréstimos  
  Quando abro Atrasados  
  Então vejo apenas o de 28/10/2026 com "2 dias de atraso"
- **Cenário limite: vira atrasado à meia-noite**  
  Dado vencimento 30/10/2026  
  Quando são 30/10/2026 23:59 e depois 31/10/2026 00:00  
  Então o empréstimo não está em Atrasados às 23:59 e está em Atrasados às 00:00
- **Cenário: Encerrados**  
  Dado um Devolvido em 10/11/2026, um Perdido em 06/12/2026 e um Cancelado em 01/11/2026  
  Quando abro Encerrados  
  Então vejo na ordem 06/12 (Perdido), 10/11 (Devolvido com atraso 4 dias), 01/11 (Cancelado)
- **Cenário: lista vazia**  
  Dado que não há empréstimos atrasados  
  Quando abro Atrasados  
  Então vejo "Nenhum empréstimo atrasado"

### HU14 — Receber avisos de prazo e atraso

**Como** dono, **quero** ser avisado no painel sobre prazos e atrasos, **para** agir a tempo.  
**Regras relacionadas:** RN44 a RN50.

- **Cenário: prazo próximo**  
  Dado empréstimo registrado em 30/10/2026 com vencimento 06/11/2026  
  Quando são 04/11/2026 09:00  
  Então o painel recebe "Prazo próximo: Furadeira Bosch vence em 06/11/2026" não lido e o contador mostra 1
- **Cenário: vence hoje**  
  Quando são 06/11/2026 09:00  
  Então o painel recebe "Vence hoje: Furadeira Bosch"
- **Cenário: atraso diário**  
  Quando são 07/11/2026 09:00 e 08/11/2026 09:00  
  Então o painel recebe um aviso "Atrasado: Furadeira Bosch, 1 dia" e depois "Atrasado: Furadeira Bosch, 2 dias"
- **Cenário: marcar como lido**  
  Dado dois avisos não lidos  
  Quando aciono "Marcar todos como lidos"  
  Então o contador mostra 0 e os avisos continuam listados como lidos
- **Cenário limite: prazo de 1 dia**  
  Dado registro em 30/10/2026 às 10:00 com vencimento 31/10/2026  
  Quando são 31/10/2026 09:00  
  Então o painel recebe "Vence hoje" e nunca recebe "Prazo próximo" para esse empréstimo
- **Cenário: registro após as 09:00 do dia do vencimento**  
  Dado que em 06/11/2026 às 14:00 registro um empréstimo com saída "05/11/2026" e vencimento "06/11/2026"  
  Quando o registro é salvo  
  Então nenhum aviso "Prazo próximo" nem "Vence hoje" é gerado, porque seus instantes (04/11 e 06/11 às 09:00) são anteriores ao registro; em 07/11/2026 às 09:00 é gerado "Atrasado: 1 dia"
- **Cenário: um aviso por dia**  
  Dado empréstimo atrasado  
  Quando são 07/11/2026 09:00 e, por indisponibilidade, o sistema só processa às 09:40  
  Então é gerado um único aviso "Atrasado" em 07/11/2026

### HU15 — Consultar histórico por objeto e por amigo

**Como** dono, **quero** ver tudo que aconteceu com um objeto ou com um amigo, **para** decidir se empresto de novo.  
**Regras relacionadas:** RN88 a RN91.

- **Cenário: histórico do objeto**  
  Dado "Furadeira Bosch" com um empréstimo Devolvido (Ana, saída 30/10/2026) e um Ativo (Bruno, saída 20/11/2026)  
  Quando abro o histórico do objeto  
  Então vejo primeiro o de Bruno (Ativo, em dia) e depois o de Ana (Devolvido no prazo), cada um com seus eventos
- **Cenário: histórico do contato com objeto arquivado**  
  Dado que "Ana Souza" devolveu a "Furadeira Bosch" e o objeto foi arquivado  
  Quando abro o histórico de Ana  
  Então o empréstimo da furadeira aparece com a marca "objeto arquivado"
- **Cenário: histórico com contato anonimizado**  
  Dado empréstimo de contato anonimizado  
  Quando abro o histórico do objeto  
  Então o tomador aparece como "Contato removido" e os eventos aparecem íntegros
- **Cenário: histórico mostra dano e retificação**  
  Dado devolução com dano "grave" retificado para "leve"  
  Quando abro o histórico  
  Então vejo "Dano: leve (retificado de grave em 15/11/2026)"
- **Cenário: sem histórico**  
  Dado objeto nunca emprestado  
  Quando abro o histórico  
  Então vejo "Nenhum empréstimo registrado"

### HU16 — Arquivar, desarquivar e excluir objeto ou contato

**Como** dono, **quero** tirar de uso objetos e amigos, **para** manter as listas limpas sem perder histórico.  
**Regras relacionadas:** RN16 a RN19, RN25, RN26.

- **Cenário: excluir sem histórico**  
  Dado objeto "Cadeira dobrável" nunca emprestado  
  Quando aciono "Excluir" e confirmo  
  Então o objeto desaparece definitivamente
- **Cenário: excluir com histórico**  
  Dado objeto com um empréstimo Devolvido  
  Quando aciono "Excluir"  
  Então vejo "Objeto com histórico só pode ser arquivado" e a opção "Arquivar" é oferecida
- **Cenário: arquivar com empréstimo ativo**  
  Dado objeto Emprestado  
  Quando aciono "Arquivar"  
  Então vejo "Não é possível arquivar: há empréstimo ativo" e nada muda
- **Cenário: arquivado não empresta**  
  Dado objeto Arquivado  
  Quando tento registrar empréstimo  
  Então vejo "Objeto arquivado" e nada é salvo
- **Cenário: desarquivar**  
  Dado objeto Arquivado  
  Quando aciono "Desarquivar"  
  Então o objeto volta a "Disponível" (ou "Indisponível", se a marca manual existia)
- **Cenário: contato arquivado**  
  Dado "Ana Souza" arquivada  
  Quando tento registrar empréstimo para ela  
  Então vejo "Contato arquivado não pode receber empréstimos"
- **Cenário: editar estado de objeto emprestado**  
  Dado objeto Emprestado  
  Quando tento alterar o estado de conservação  
  Então vejo "Objeto emprestado não pode ter o estado alterado" e o nome continua editável

### HU17 — Anonimizar contato (LGPD)

**Como** dono, **quero** atender ao pedido de remoção de dados de um amigo, **para** respeitar sua privacidade sem apagar o histórico de empréstimos.  
**Regras relacionadas:** RN92 a RN96.

- **Cenário: anonimização válida**  
  Dado "Ana Souza" com telefone "85999990000", observação "Vizinha", sem empréstimo Ativo  
  Quando aciono "Anonimizar contato" e confirmo  
  Então o contato passa a "Contato removido", telefone e observação ficam vazios, fica Arquivado, os eventos permanecem e o histórico registra data e hora da anonimização
- **Cenário: empréstimo ativo**  
  Dado "Ana Souza" com empréstimo Ativo  
  Quando aciono "Anonimizar contato"  
  Então vejo "Não é possível anonimizar: há empréstimo ativo" e nada muda
- **Cenário: irreversível**  
  Dado contato anonimizado  
  Quando abro o contato  
  Então não existe opção de editar, desarquivar ou restaurar dados
- **Cenário: finalidade visível**  
  Quando abro "Privacidade"  
  Então vejo quais dados são guardados (nome, telefone, observação, bloqueio e motivo, datas), a finalidade e o prazo de atendimento de 15 dias

## 7. Estados do empréstimo

### 7.1 Estados persistidos e classificações derivadas

Estados persistidos: **Ativo**, **Devolvido**, **Perdido**, **Cancelado** (RN35, RN59, RN75, RN80). Classificações derivadas, calculadas na consulta e nunca gravadas: **em dia** e **atrasado** (RN39, RN40) sobre Ativo; **no prazo** e **com atraso** (RN60) sobre Devolvido; **recuperado** (RN78) sobre Perdido; **devolução parcial** (RN68) sobre Ativo. "Atrasado" é filtro e não estado porque muda apenas com a passagem do tempo, sem ação do dono; um estado persistido exigiria rotina de transição e poderia divergir do relógio (DEC27).

### 7.2 Transições do empréstimo

| Estado atual | Evento | Condição | Próximo estado | Regra |
|---|---|---|---|---|
| (inexistente) | Registro | Objeto Disponível; contato não bloqueado e não arquivado; saída ≤ hoje; vencimento entre saída+1 e saída+365 | Ativo | RN28 a RN35 |
| Ativo (em dia) | Renovação | Renovações < 2; dias de 1 a 7; agora ≤ 23:59:59,999 do vencimento | Ativo (vencimento + dias) | RN52 a RN56 |
| Ativo (atrasado) | Renovação | — | Recusado: "Empréstimo atrasado não pode ser renovado" | RN54, RN82 |
| Ativo | Renovação | Já há 2 renovações | Recusado: "Limite de 2 renovações atingido" | RN52, RN82 |
| Ativo | Devolução integral | Data entre saída e hoje; estado e dano válidos | Devolvido (no prazo ou com atraso) | RN58 a RN63 |
| Ativo (conjunto) | Devolução parcial | ≥ 1 recebido e ≥ 1 faltante | Ativo (parcial) | RN67 a RN69 |
| Ativo (parcial) | Devolução parcial que completa | Todos recebidos | Devolvido | RN70 |
| Ativo (parcial) | Renovação | Mesmas condições de Ativo em dia | Ativo (parcial) | RN55 |
| Ativo (atrasado) | Perda | Dias de atraso ≥ 30; motivo informado | Perdido | RN73 a RN75 |
| Ativo | Perda | Dias de atraso < 30 | Recusado: "Perda só pode ser registrada após 30 dias de atraso" | RN73, RN82 |
| Ativo | Cancelamento | Sem renovação e sem parcial; motivo | Cancelado | RN80 |
| Ativo | Cancelamento | Há renovação ou parcial | Recusado: "Empréstimo com renovação ou devolução parcial não pode ser cancelado" | RN80, RN82 |
| Ativo | Observação | Texto 1–500 | Ativo (evento gravado) | RN84 |
| Ativo | Contestação, Retificação, Recuperação | — | Recusado: "Ação indisponível para empréstimo ativo" | RN65, RN66, RN78, RN82 |
| Devolvido | Contestação | Nenhuma contestação anterior | Devolvido (evento) | RN65 |
| Devolvido | Retificação | ≤ 7 dias da devolução; nenhuma retificação anterior; RN62 | Devolvido (dano alterado, evento) | RN66 |
| Devolvido | Observação | Texto 1–500 | Devolvido (evento) | RN84 |
| Devolvido | Renovação, Devolução, Perda, Cancelamento, Recuperação | — | Recusado: "Este empréstimo já está encerrado" | RN77, RN82 |
| Perdido | Recuperação | Nenhuma anterior; data entre perda e hoje | Perdido (recuperado) | RN78 |
| Perdido (recuperado) | Contestação / Retificação do dano da recuperação | Como em Devolvido | Perdido (evento) | RN65, RN66 |
| Perdido | Observação | Texto 1–500 | Perdido (evento) | RN84 |
| Perdido | Renovação, Devolução, Perda, Cancelamento | — | Recusado: "Este empréstimo já está encerrado" | RN77, RN82 |
| Perdido (recuperado) | Recuperação | — | Recusado: "Recuperação já registrada" | RN78, RN82 |
| Cancelado | Observação | Texto 1–500 | Cancelado (evento) | RN84 |
| Cancelado | Qualquer outro evento | — | Recusado: "Este empréstimo já está encerrado" | RN81, RN82 |

### 7.3 Mensagens de recusa

Todas as recusas desta seção não alteram dados e não geram evento (RN82, RN83). Mensagens: "Objeto já emprestado"; "Objeto indisponível"; "Objeto arquivado"; "Contato bloqueado não pode receber empréstimos"; "Contato arquivado não pode receber empréstimos"; "A data de saída não pode ser futura"; "O vencimento deve ser de 1 a 365 dias após a saída"; "Empréstimo atrasado não pode ser renovado"; "Limite de 2 renovações atingido"; "A renovação deve ser de 1 a 7 dias"; "Este empréstimo já está encerrado"; "Descreva o dano"; "Objeto inutilizado deve ter estado 'ruim'"; "A data de devolução deve estar entre a saída e hoje"; "Marque ao menos um componente recebido"; "Perda só pode ser registrada após 30 dias de atraso"; "Informe o motivo da perda"; "Empréstimo com renovação ou devolução parcial não pode ser cancelado"; "Informe o motivo do cancelamento"; "Contestação já registrada"; "Prazo de retificação encerrado"; "Dano já retificado"; "Recuperação já registrada"; "A data de recuperação deve estar entre a perda e hoje"; "Ação indisponível para empréstimo ativo".

### 7.4 Status do objeto

| Status atual | Evento | Próximo status | Regra |
|---|---|---|---|
| Disponível | Registro de empréstimo | Emprestado | RN12, RN35 |
| Disponível | Marcar indisponível (manual) | Indisponível | RN13, RN14 |
| Disponível | Arquivar | Arquivado | RN17, RN18 |
| Disponível (sem histórico) | Excluir | (removido) | RN16 |
| Emprestado | Devolução integral com dano nenhum ou leve | Disponível | RN59 |
| Emprestado | Devolução integral com dano grave ou inutilizado | Indisponível (dano) | RN63 |
| Emprestado | Perda | Indisponível (perdido ou incompleto) | RN71, RN75 |
| Emprestado | Cancelamento | Disponível | RN80 |
| Emprestado | Marcar indisponível, arquivar, excluir, editar estado | Recusado | RN14, RN15, RN17, RN18 |
| Indisponível (dano) | Retificação para nenhum ou leve | Disponível | RN66 |
| Indisponível (perdido) | Recuperação sem dano grave | Disponível | RN78 |
| Indisponível (perdido) | Recuperação com dano grave ou inutilizado | Indisponível (dano) | RN78, RN63 |
| Indisponível | Desmarcar (manual) | Disponível | RN13, RN14 |
| Indisponível | Arquivar | Arquivado | RN18 |
| Arquivado | Desarquivar | Disponível, ou Indisponível se a marca manual persistir | RN19 |
| Arquivado | Registro de empréstimo | Recusado: "Objeto arquivado" | RN19 |

## 8. Modelo de dados

Modelo conceitual; tipos indicam o domínio do valor, não um esquema de banco. Todas as entidades carregam `id` (identificador único gerado pelo sistema), `criadoEm` e `atualizadoEm` (data e hora). Todas pertencem a uma `Conta` (RN02).

### 8.1 Entidades

**Conta** (dono) — RN01, RN02

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| email | texto | sim | único no sistema; formato de e-mail |
| senhaHash | texto | sim | derivado de senha com ≥ 8 caracteres; nunca armazenada em claro |

**Objeto** — RN04 a RN19

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| nome | texto | sim | 1–80 caracteres |
| categoria | enumeração | sim | uma das 8 de RN05 |
| estadoConservacao | enumeração | sim | novo, bom, regular, ruim |
| foto | arquivo | não | JPEG/PNG ≤ 5 MB; 0..1 |
| ehConjunto | booleano | sim | verdadeiro exige 2–50 Componentes |
| indisponivelManual | booleano | sim | padrão falso; só alterável quando não Emprestado |
| indisponivelMotivoManual | texto | não | ≤ 500 |
| indisponivelAutomatico | enumeração | não | nulo, dano grave, inutilizado, perdido, incompleto |
| arquivado | booleano | sim | padrão falso; verdadeiro exige nenhum Empréstimo Ativo |
| status (derivado) | enumeração | — | Arquivado > Emprestado > Indisponível > Disponível (RN11) |

**Componente** — RN10, RN67

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| objetoId | referência a Objeto | sim | objeto com ehConjunto verdadeiro |
| nome | texto | sim | 1–80 |
| ordem | inteiro | sim | 1..50, único por objeto |

**Contato** — RN20 a RN27, RN92 a RN96

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| nome | texto | sim | 1–80; "Contato removido" após anonimização |
| telefone | texto | não | 10 ou 11 dígitos; apagado na anonimização |
| observacao | texto | não | ≤ 500; apagada na anonimização |
| bloqueado | booleano | sim | padrão falso |
| bloqueioMotivo | texto | não | ≤ 500; apagado na anonimização |
| arquivado | booleano | sim | padrão falso; verdadeiro exige nenhum Empréstimo Ativo |
| anonimizadoEm | data e hora | não | preenchido torna o contato imutável e arquivado |

**Empréstimo** — RN28 a RN84

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| objetoId | referência a Objeto | sim | no máximo um Empréstimo com estado Ativo por objeto (RN29) |
| contatoId | referência a Contato | sim | no registro: não bloqueado, não arquivado |
| dataSaida | data | sim | ≤ hoje |
| dataVencimentoOriginal | data | sim | saída+1 ≤ valor ≤ saída+365 |
| dataVencimentoAtual | data | sim | = original + soma das renovações |
| estadoSaida | enumeração | sim | escala RN06 |
| observacaoSaida | texto | não | ≤ 300 |
| estado | enumeração | sim | Ativo, Devolvido, Perdido, Cancelado |
| quantidadeRenovacoes | inteiro | sim | 0..2 |
| dataDevolucao | data | condicional | obrigatória se Devolvido; saída ≤ valor ≤ hoje |
| estadoVolta | enumeração | condicional | obrigatória se Devolvido ou recuperado |
| danoNivel | enumeração | condicional | nenhum, leve, grave, inutilizado; obrigatória se Devolvido ou recuperado |
| danoDescricao | texto | condicional | 1–500 se danoNivel ≠ nenhum |
| danoFoto | arquivo | não | JPEG/PNG ≤ 5 MB |
| danoNivelAnterior | enumeração | não | preenchido só por retificação |
| retificadoEm | data e hora | não | ≤ 7 dias após registro da devolução/recuperação; único |
| contestacao | texto | não | 1–500; único |
| contestacaoEm | data e hora | não | — |
| perdaMotivo | enumeração | condicional | perda comunicada, não devolução; obrigatória se Perdido |
| perdaEm | data e hora | condicional | obrigatória se Perdido |
| perdaObservacao | texto | não | ≤ 500 |
| recuperadoEm | data | não | perda ≤ valor ≤ hoje; único |
| cancelamentoMotivo | texto | condicional | 1–200; obrigatório se Cancelado |
| encerradoEm | data e hora | condicional | obrigatório se estado ≠ Ativo |
| classificacao (derivada) | enumeração | — | em dia, atrasado, no prazo, com atraso (RN39, RN60) |
| diasAtraso (derivado) | inteiro | — | RN41 ou RN60 |

**Renovação** — RN52 a RN56

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| emprestimoId | referência a Empréstimo | sim | no máximo 2 por empréstimo |
| numero | inteiro | sim | 1 ou 2 |
| dias | inteiro | sim | 1..7 |
| vencimentoAnterior | data | sim | — |
| vencimentoNovo | data | sim | = anterior + dias |
| realizadaEm | data e hora | sim | ≤ 23:59:59,999 de vencimentoAnterior |

**DevoluçãoParcialItem** — RN67 a RN71

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| emprestimoId | referência a Empréstimo | sim | empréstimo de objeto conjunto |
| componenteId | referência a Componente | sim | único por empréstimo |
| recebidoEm | data | sim | saída ≤ valor ≤ hoje; irreversível |
| observacao | texto | não | ≤ 300 |

**Evento** — RN83, RN84, RN90

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| emprestimoId | referência a Empréstimo | sim | — |
| tipo | enumeração | sim | Registro, Renovação, Devolução parcial, Devolução, Perda, Recuperado, Cancelamento, Contestação, Retificação, Observação, Anonimização de contato |
| ocorridoEm | data e hora | sim | instante da gravação |
| dados | texto estruturado | sim | cópia dos valores da ação (ex.: vencimento anterior e novo) |
| imutável | — | — | nunca editado nem excluído |

**Aviso** — RN44 a RN50

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| emprestimoId | referência a Empréstimo | sim | — |
| tipo | enumeração | sim | Prazo próximo, Vence hoje, Atrasado, Perda possível |
| dataReferencia | data | sim | dia civil do aviso; único por (emprestimoId, dataReferencia) |
| geradoEm | data e hora | sim | ≥ 09:00:00 de dataReferencia |
| lido | booleano | sim | padrão falso |

### 8.2 Relacionamentos e cardinalidades

- Conta 1 — N Objeto; Conta 1 — N Contato; Conta 1 — N Empréstimo; Conta 1 — N Aviso.
- Objeto 1 — 0..50 Componente (2..50 se conjunto; 0 se não).
- Objeto 1 — N Empréstimo; no máximo 1 com estado Ativo (RN29).
- Contato 1 — N Empréstimo (sem limite de Ativos, RN27).
- Empréstimo 1 — 0..2 Renovação; 1 — 0..N DevoluçãoParcialItem (≤ número de componentes); 1 — 1..N Evento; 1 — 0..N Aviso.

### 8.3 Regras de integridade e privacidade do modelo

- Exclusão física só de Objeto ou Contato sem Empréstimo (RN16, RN25); caso contrário exclusão lógica por `arquivado` (RN17).
- Empréstimo e Evento nunca são excluídos nem editados (RN36, RN90); campos condicionais só são preenchidos pela ação correspondente.
- Anonimização (RN93) altera apenas Contato.nome, telefone, observacao, bloqueioMotivo e grava anonimizadoEm; Eventos permanecem e referenciam o contato pelo id.
- Fotos são acessíveis apenas pela conta proprietária (RN02, RN96).
- Nenhum atributo guarda geolocalização, valores monetários ou dados do dispositivo.

## 9. Casos de borda e erros

| # | Situação | Comportamento esperado | Regra |
|---|---|---|---|
| 1 | Nome de objeto com 81 caracteres | Recusa com "O nome deve ter de 1 a 80 caracteres" | RN04 |
| 2 | Dois objetos com o mesmo nome | Ambos salvos com ids distintos | RN08 |
| 3 | Três furadeiras idênticas | Três objetos; cada um com seu empréstimo | RN09 |
| 4 | Conjunto com 1 ou 51 componentes | Recusa com "Um conjunto precisa de 2 a 50 componentes" | RN10 |
| 5 | Tentar emprestar só um componente | Opção inexistente; empréstimo é sempre do objeto inteiro | RN10 |
| 6 | Foto PNG de exatamente 5 MB | Aceita | RN07 |
| 7 | Foto de 5 MB + 1 byte ou formato GIF | Recusa com "A foto deve ser JPEG ou PNG com até 5 MB" | RN07 |
| 8 | Marcar indisponível um objeto emprestado | Recusa; status continua Emprestado | RN14 |
| 9 | Editar estado de conservação de objeto emprestado | Recusa com "Objeto emprestado não pode ter o estado alterado" | RN15 |
| 10 | Excluir objeto com empréstimo Cancelado | Recusa; só arquivar (Cancelado conta como empréstimo) | RN17 |
| 11 | Arquivar objeto com empréstimo Ativo | Recusa com "Não é possível arquivar: há empréstimo ativo" | RN18 |
| 12 | Desarquivar objeto que tinha marca manual de indisponível | Volta a Indisponível, não a Disponível | RN19, RN13 |
| 13 | Telefone com letras ou 12 dígitos | Recusa com "Telefone deve ter 10 ou 11 dígitos" | RN20 |
| 14 | Bloquear contato com 3 empréstimos ativos | Bloqueio aceito; os 3 continuam Ativos e renováveis | RN23 |
| 15 | Contato bloqueado e arquivado ao mesmo tempo | Ambas as marcas coexistem; novo empréstimo recusado por "Contato arquivado..." (verificação de arquivamento precede) | RN23, RN26 |
| 16 | Mesmo contato com 10 empréstimos ativos | Aceito | RN27 |
| 17 | Registrar empréstimo de objeto Emprestado | Recusa com "Objeto já emprestado"; nenhum registro | RN29, RN82 |
| 18 | Dois registros simultâneos do mesmo objeto (duas abas) | O primeiro gravado vence; o segundo é recusado na gravação | RN29, RN82 |
| 19 | Saída em 29/02/2028 e vencimento em 28/02/2029 | 365 dias: aceito | RN32 |
| 20 | Saída em 30/10/2026 e vencimento em 30/10/2027 | 365 dias: aceito; 31/10/2027 recusado | RN32 |
| 21 | Saída com 3 anos no passado | Aceita; empréstimo nasce atrasado; nenhum aviso pretérito | RN30, RN43, RN46 |
| 22 | Dono em outro fuso (ex.: Lisboa) registra às 01:00 locais de 31/10 | "Hoje" é 30/10/2026 (America/Fortaleza); saída "31/10/2026" é recusada como futura | RN33, RN37, RN38 |
| 23 | Relógio do celular adiantado 2 dias | Ignorado; cálculo pelo servidor | RN37 |
| 24 | Vencimento 06/11/2026 às 23:59:59 | Em dia | RN39 |
| 25 | Vencimento 06/11/2026 às 00:00:00 de 07/11 | Atrasado, 1 dia | RN39, RN41 |
| 26 | Empréstimo atrasado há 200 dias | Continua Ativo e atrasado; nenhum bloqueio ou pontuação | RN42 |
| 27 | Renovar às 23:59:59,999 do vencimento | Aceita | RN54 |
| 28 | Renovar às 00:00:00,000 do dia seguinte | Recusa "Empréstimo atrasado não pode ser renovado" | RN54 |
| 29 | Renovar duas vezes no mesmo clique duplo | Segunda gravação valida quantidadeRenovacoes = 1 e grava a 2ª; terceira é recusada; confirmação explícita por renovação | RN52, RN82 |
| 30 | Renovação de 7 dias duas vezes em prazo de 365 | Vencimento = saída + 379 dias; aceito | RN57 |
| 31 | Renovar empréstimo com devolução parcial | Aceita se em dia | RN55 |
| 32 | Renovar empréstimo de contato bloqueado | Aceita | RN23, RN55 |
| 33 | Devolver com data igual à data de saída | Aceita; "no prazo" | RN58, RN60 |
| 34 | Devolver com data futura | Recusa "A data de devolução deve estar entre a saída e hoje" | RN58 |
| 35 | Devolver objeto que saiu "ruim" e volta "bom" | Aceito; estado do objeto passa a "bom" | RN59 |
| 36 | Dano "nenhum" com estado de volta pior que o da saída | Aceito; o dono decide o nível | RN61, RN62 |
| 37 | Dano "inutilizado" com estado "bom" | Recusa "Objeto inutilizado deve ter estado 'ruim'" | RN62 |
| 38 | Dano "leve" sem descrição | Recusa "Descreva o dano" | RN58 |
| 39 | Desmarcar Indisponível de objeto inutilizado | Permitido ao dono; registro de dano permanece no histórico | RN14, RN63 |
| 40 | Contestação em empréstimo Ativo | Recusa "Ação indisponível para empréstimo ativo" | RN65, RN82 |
| 41 | Retificação no 7º dia às 23:59 | Aceita; no 8º dia às 00:00 recusada | RN66 |
| 42 | Retificar "grave" para "nenhum" com objeto já desmarcado manualmente | Evento gravado; status não muda | RN66 |
| 43 | Parcial marcando todos os componentes recebidos | Tratada como devolução integral: exige estado e dano | RN70 |
| 44 | Parcial em objeto sem componentes | Opção inexistente | RN72 |
| 45 | Tentar desmarcar componente já recebido | Recusa; recebimento é irreversível | RN69 |
| 46 | Perda com 29 dias de atraso | Recusa "Perda só pode ser registrada após 30 dias de atraso" | RN73 |
| 47 | Perda em empréstimo em dia (tomador avisou que perdeu) | Recusa; dono registra Observação e aguarda o 30º dia de atraso | RN73, RN76, RN84 |
| 48 | Perda com devolução parcial | Perdido; componentes recebidos gravados; objeto "incompleto" | RN71 |
| 49 | Recuperação com data anterior à perda | Recusa "A data de recuperação deve estar entre a perda e hoje" | RN78 |
| 50 | Segunda recuperação | Recusa "Recuperação já registrada" | RN78 |
| 51 | Emprestar objeto recuperado | Novo empréstimo; o Perdido permanece no histórico | RN79 |
| 52 | Cancelar empréstimo renovado | Recusa | RN80 |
| 53 | Cancelar empréstimo atrasado sem renovação | Aceito; não conta como atraso | RN80, RN81 |
| 54 | Devolver duas vezes | Segunda recusada "Este empréstimo já está encerrado" | RN82 |
| 55 | Ação recusada | Nenhum evento gravado | RN83 |
| 56 | Lista Ativos com empréstimo atrasado | Aparece em Ativos (destacado) e em Atrasados | RN85, RN86 |
| 57 | Aviso "Prazo próximo" para prazo de 1 ou 2 dias | 1 dia: não gerado (instante anterior ao registro); 2 dias: gerado só se registrado antes das 09:00 do dia da saída | RN45, RN46 |
| 58 | Sistema indisponível às 09:00 | Aviso gerado quando o sistema voltar, no mesmo dia; único por dia | RN47 |
| 59 | Renovação às 08:00 do dia "vencimento − 2" | Aviso "Prazo próximo" daquele dia descartado; recalculado para novo vencimento − 2 | RN48 |
| 60 | Devolução às 08:59 do dia do vencimento | Aviso "Vence hoje" não é gerado | RN49 |
| 61 | Dia 30 de atraso | Gera "Perda possível" e não gera "Atrasado" | RN45, RN47 |
| 62 | Dia 31 de atraso em diante | Volta a gerar "Atrasado" diário até encerrar | RN45 |
| 63 | Anonimizar contato bloqueado sem empréstimo ativo | Aceito; motivo de bloqueio apagado; marca de bloqueio irrelevante pois arquivado | RN93 |
| 64 | Anonimizar contato com empréstimo Ativo | Recusa "Não é possível anonimizar: há empréstimo ativo" | RN94 |
| 65 | Histórico de objeto cujo contato foi anonimizado | Exibe "Contato removido"; eventos íntegros | RN88, RN93 |
| 66 | Editar nome do contato após empréstimos | Histórico mostra o novo nome | RN24 |
| 67 | Login com senha de 7 caracteres | Recusa no cadastro da conta | RN01 |
| 68 | Acesso a objeto de outra conta pela URL | Recusa; dado não exibido | RN02 |

## 10. Requisitos não funcionais

Os valores abaixo seguem DEC02, DEC25, DEC45 e DEC47 (esta última aguarda aprovação).

| ID | Requisito | Critério verificável |
|---|---|---|
| RNF01 | Plataforma: aplicativo web responsivo | Funciona nas 2 últimas versões estáveis de Chrome, Firefox, Safari e Edge, em largura de 360 px a 1920 px, sem rolagem horizontal |
| RNF02 | Desempenho | Listas Ativos, Atrasados e Encerrados com até 500 empréstimos carregam em até 2 s em conexão 4G (10 Mbps); registro de qualquer ação responde em até 1 s |
| RNF03 | Acessibilidade WCAG 2.1 nível AA | Contraste mínimo 4,5:1; toda ação executável por teclado; todo campo com rótulo; alvos de toque ≥ 44 × 44 px; mensagens de erro associadas ao campo |
| RNF04 | Idioma e formatos | Interface em português do Brasil; datas dd/mm/aaaa; horas hh:mm 24 h |
| RNF05 | Fuso e relógio | Toda data e hora calculada no servidor em America/Fortaleza; o cliente não envia "hoje" |
| RNF06 | Segurança | Tráfego exclusivamente HTTPS; senha armazenada com hash; sessão encerrada após 30 dias sem uso; acesso a dados sempre filtrado pela conta autenticada |
| RNF07 | Privacidade (LGPD) | Dados do contato limitados a RN20; finalidade exibida na tela "Privacidade"; anonimização disponível e irreversível (RN93); nenhuma coleta de geolocalização, contatos do telefone ou dados de dispositivo |
| RNF08 | Conectividade | O aplicativo exige conexão; sem conexão exibe "Sem conexão. Tente novamente." e não grava nada localmente além da sessão |
| RNF09 | Avisos | O processo de geração de avisos executa às 09:00:00 America/Fortaleza; se o sistema estiver indisponível nesse instante, executa na primeira oportunidade do mesmo dia (RN47) |
| RNF10 | Integridade | Gravações de transição de estado são atômicas: ou toda a ação (estado, evento, status do objeto) é gravada, ou nada é |

## 11. Decisões e premissas

Status: **Aprovada** = aprovada pelo aluno na conversa de 30/10/2026, sem alterações, com a justificativa abaixo assumida como sua. **Aguarda aprovação** = proposta pela IA (Claude) nesta sessão, ainda não assumida pelo aluno; permanece listada na seção 12. Todas as decisões foram sugeridas pela IA a partir do enunciado e revisadas pelo aluno, conforme exige a página 6 do enunciado.

| ID | Decisão | Alternativas consideradas | Justificativa | Status |
|---|---|---|---|---|
| DEC01 | Só o dono usa a conta; o tomador é um contato cadastrado e não precisa instalar o app | Ambos com contas; tomador convidado | Escopo mais simples, sem autenticação de terceiros | Aprovada |
| DEC02 | Aplicativo web responsivo acessível em celular e computador | Apenas Android/iOS; somente desktop | Documenta uma plataforma sem exigir implementação | Aprovada |
| DEC03 | Cadastro do objeto exige nome, categoria e estado; foto opcional; categorias pré-definidas com opção "Outros" | Categorias livremente cadastráveis | Reduz ambiguidades no cadastro | Aprovada |
| DEC04 | Cada objeto físico tem identificador individual e não pode ter dois empréstimos abertos simultaneamente | Unidades indistintas por quantidade | Evita empréstimos conflitantes | Aprovada |
| DEC05 | Conjuntos são um objeto com componentes listados; devolução só é completa com todos os componentes | Cada peça como objeto separado | Preserva histórico do conjunto e define a devolução parcial | Aprovada |
| DEC06 | Cada contato exige nome e aceita telefone opcional; nenhum dado de tomador é necessário para login | Exigir telefone/e-mail | Minimização de dados pessoais | Aprovada |
| DEC07 | Prazo informado por data de devolução no calendário, sem prazo padrão por categoria; sugestão inicial de 7 dias | Duração numérica; prazo obrigatório por categoria | Facilita previsão sem impedir personalização | Aprovada |
| DEC08 | Prazo obrigatório, de 1 a 365 dias corridos após a data de saída | Mesmo dia; sem prazo; ilimitado | Define limites de validação | Aprovada |
| DEC09 | Data de saída não pode ser futura; vencimento às 23:59:59,999 no fuso America/Fortaleza | UTC; fuso do dispositivo; começo do dia | Evita interpretação contraditória do vencimento | Aprovada |
| DEC10 | Atraso começa às 00:00 do dia seguinte ao vencimento, sem tolerância | Tolerância de 24/48 horas | Definição determinística | Aprovada |
| DEC11 | Atraso gera alerta; não cria multa, pontuação nem bloqueio automático | Bloqueio/score automáticos | Não amplia o escopo desnecessariamente | Aprovada |
| DEC12 | Avisos ao dono no painel: 2 dias antes, no vencimento e diariamente às 9h enquanto atrasado; encerram após devolução/perda; sem alertas de datas que já passaram | E-mail, push, aviso semanal | Frequência definida e simples de verificar | Aprovada |
| DEC13 | Somente o dono renova, no máximo 2 vezes por empréstimo, com até 7 dias corridos por renovação | Tomador solicita; renovações ilimitadas | Impede renovações indefinidas | Aprovada |
| DEC14 | Renovação soma dias à data atual de vencimento; empréstimos atrasados não podem ser renovados | Contar a partir de hoje; permitir atraso | Não apaga atraso nem encurta prazo | Aprovada |
| DEC15 | Estado na saída e no retorno na mesma escala: novo, bom, regular, ruim | Texto livre; duas escalas | Permite comparação objetiva | Aprovada |
| DEC16 | Dano classificado pelo dono como nenhum, leve, grave ou inutilizado, com descrição e foto opcional | Classificação automática ou pelo tomador | Autoridade e níveis explícitos | Aprovada |
| DEC17 | Dano gera só registro e indisponibilidade se grave/inutilizado; sem cobrança, reparo ou reposição no app | Acordos e indenizações no sistema | Respeita as exclusões oficiais | Aprovada |
| DEC18 | Tomador pede retificação por meio externo; dono registra nota de contestação no histórico, sem chat nem alteração automática | Contestação no app com conta de tomador | Registra contestação sem expandir para chat | Aprovada |
| DEC19 | Devolução parcial não encerra o empréstimo; registra recebidos e faltantes até devolução integral ou perda | Encerrar com pendência | Evita declarar devolvido um conjunto incompleto | Aprovada |
| DEC20 | Dono pode marcar perda/não devolução manualmente após 30 dias completos de atraso; antes disso o empréstimo fica atrasado | Marcação automática; qualquer momento | Garante critério objetivo | Aprovada |
| DEC21 | Objeto perdido que reaparece: registrar recuperação no histórico; não reabrir empréstimo; preservar o registro de perda | Reabrir o empréstimo perdido | Histórico sem apagamento de eventos | Aprovada |
| DEC22 | Contatos e objetos com histórico não são excluídos fisicamente, apenas arquivados; arquivados não entram em novos empréstimos | Exclusão definitiva com cascata | Mantém rastreabilidade dos empréstimos | Aprovada |
| DEC23 | Dono pode bloquear/desbloquear manualmente um amigo; bloqueado não recebe novos empréstimos, mas históricos e empréstimos abertos permanecem | Bloqueio automático por atraso | Critério simples e controle do dono | Aprovada |
| DEC24 | Dados do contato usados só para identificação e acompanhamento, com acesso apenas do dono; remoção apaga identificadores preservando histórico não identificável | Reter indefinidamente; excluir todos os registros | Minimização, finalidade, direitos LGPD e preservação do histórico | Aprovada |
| DEC25 | Vencimento, prazos e avisos calculados pela data/hora oficial no fuso escolhido, sem depender do relógio local | Usar só horário do dispositivo | Consistência da regra temporal | Aprovada |
| DEC26 | Ação inválida é recusada sem alterar registros, com mensagem específica; histórico só para ações aceitas | Aceitar sobreposição; duplicar eventos | Transições testáveis e integridade | Aprovada |
| DEC27 | "Atrasado" é classificação derivada de empréstimo Ativo com hoje posterior ao vencimento; estados persistidos: Ativo, Devolvido, Perdido, Cancelado | Estado "Atrasado" persistido por rotina diária | Nenhuma rotina de transição; filtro nunca defasado | Aguarda aprovação |
| DEC28 | Ativos = todos os Ativos com atrasados destacados, por vencimento crescente; Atrasados = Ativos com vencimento anterior a hoje, por dias de atraso decrescente; Encerrados = Devolvido, Perdido, Cancelado, por encerramento decrescente | Ativos excluem atrasados | Listas com definição única e ordenação testável | Aguarda aprovação |
| DEC29 | Status do objeto: Disponível, Emprestado (derivado), Indisponível (manual ou automático), Arquivado; precedência Arquivado > Emprestado > Indisponível > Disponível; só Disponível aceita empréstimo; marcar/desmarcar Indisponível só quando não emprestado | Inutilizado arquiva automaticamente | Dá à DEC17 um ciclo de vida fechado | Aguarda aprovação |
| DEC30 | Estado na saída copiado do estado atual do objeto, ajustável no ato, com observação opcional de até 300 caracteres; estado de volta obrigatório e passa a ser o estado atual do objeto | Estado fixo no cadastro | Garante a comparação saída/volta pedida no enunciado | Aguarda aprovação |
| DEC31 | Dano ≠ nenhum exige descrição de 1 a 500 caracteres; inutilizado exige estado "ruim"; contestação única de 1 a 500 caracteres por registro de dano, a qualquer momento | Campos independentes | Evita combinações contraditórias | Aguarda aprovação |
| DEC32 | Retificação do nível de dano uma única vez, até 7 dias após a devolução ou recuperação, preservando o anterior em evento | Dano imutável | Corrige erro sem apagar histórico | Aguarda aprovação |
| DEC33 | Data de devolução informada pelo dono, entre saída e hoje, padrão hoje; "no prazo" se ≤ vencimento, senão "com atraso" com dias calculados | Sempre a data de hoje | Permite registro tardio sem falsear atraso | Aguarda aprovação |
| DEC34 | Estado único "Perdido" com motivo "perda comunicada" ou "não devolução"; observação opcional; objeto Indisponível com motivo "perdido" | Dois estados distintos | Um estado, dois motivos rastreáveis | Aguarda aprovação |
| DEC35 | Recuperação: evento "Recuperado" com data, estado e dano; empréstimo segue Perdido com marca "recuperado em"; objeto volta a Disponível ou fica Indisponível por dano | Reabrir o empréstimo | Detalha DEC21 sem contradizer DEC29 | Aguarda aprovação |
| DEC36 | Devolução parcial por componente com data; empréstimo segue Ativo com prazo e avisos; renovação permitida se em dia; parciais sucessivas até integral; perda gera Perdido com recebidos e objeto "incompleto" | Encerrar e abrir novo empréstimo | Resolve o cruzamento DEC19 × DEC14 × DEC20 | Aguarda aprovação |
| DEC37 | Opção A adotada no rascunho: empréstimo Ativo sem renovação nem parcial pode ser cancelado com motivo de 1 a 200 caracteres; estado Cancelado em Encerrados, fora das contagens de atraso; nenhum empréstimo é editado ou excluído | Opção B: sem cancelamento; engano resolvido com devolução e observação | Corrige engano sem falsear histórico | Aguarda aprovação (A ou B) |
| DEC38 | Data de saída ≤ hoje sem limite retroativo; empréstimo registrado já vencido nasce Ativo e atrasado, com avisos diários a partir das próximas 09:00 e sem avisos pretéritos | Limitar a 30 dias no passado | Permite registrar empréstimos antigos | Aguarda aprovação |
| DEC39 | Aviso = registro no painel com tipo, data/hora, empréstimo, lido/não lido; todos às 09:00 America/Fortaleza; máximo um por empréstimo por dia; renovação recalcula; após encerramento nenhum novo, existentes permanecem até lidos; contador de não lidos | Apagar avisos ao encerrar | Define "aviso" e sua cessação | Aguarda aprovação |
| DEC40 | Renovação de 1 a 7 dias; permitida até 23:59:59,999 do vencimento, com contato bloqueado e com devolução parcial; recusada se atrasado, encerrado ou com 2 renovações; grava data anterior e nova | Bloquear no dia do vencimento | Coerente com DEC10, DEC13, DEC14 e DEC23 | Aguarda aprovação |
| DEC41 | Nomes de 1 a 80 caracteres, não únicos; 8 categorias fixas; textos livres até 500; telefone 10 ou 11 dígitos; foto JPEG/PNG até 5 MB, uma por objeto e uma por dano; conjunto com 2 a 50 componentes | Nome único | Limites testáveis em todo cenário | Aguarda aprovação |
| DEC42 | Objeto ou contato nunca emprestado pode ser excluído fisicamente; com histórico só arquiva; arquivar exige nenhum Ativo; arquivamento reversível | Nunca excluir | Rastreabilidade sem registros inúteis | Aguarda aprovação |
| DEC43 | Sem limite de empréstimos simultâneos por contato; limite só por objeto | Máximo por contato | Não amplia regras | Aguarda aprovação |
| DEC44 | Objeto editável (nome, categoria, foto) sempre; estado só quando não emprestado; contato editável sempre; histórico exibe nome atual | Congelar nome no histórico | Simplicidade | Aguarda aprovação |
| DEC45 | Conta do dono com e-mail e senha de no mínimo 8 caracteres; dados isolados por conta; sem perfil de tomador | Login social | Autenticação mínima para DEC01 | Aguarda aprovação |
| DEC46 | Dados do contato: nome, telefone, observação, bloqueio, datas; finalidade em tela de privacidade; pedido de remoção por meio externo; ação "Anonimizar contato" quando sem Ativo, imediata e irreversível (nome "Contato removido", telefone e observação apagados, eventos preservados); orientação de atendimento em até 15 dias; nota de que a LGPD (art. 4º, I) não se aplica a uso pessoal não econômico, adotada por boa prática | Exclusão total dos registros | Procedimento que DEC24 deixou em aberto | Aguarda aprovação |
| DEC47 | RNFs: listas de 500 empréstimos em até 2 s em 4G; WCAG 2.1 AA; pt-BR; dd/mm/aaaa; HTTPS; hash de senha; exige conexão; 2 últimas versões de Chrome, Firefox, Safari, Edge; largura mínima 360 px | Sem metas numéricas | Verificáveis, como exige a rubrica | Aguarda aprovação |
| DEC48 | Esclarecimentos às aprovadas: DEC06 ganha observação opcional; DEC07 pré-preenche saída + 7 editável; DEC09 vale para todos os usuários; DEC23 aceita motivo opcional; DEC26 valida no momento da gravação | Manter texto original | Remove ambiguidade sem mudar o sentido aprovado | Aguarda aprovação |
| DEC49 | Complemento à DEC20: aviso "Perda possível" no 30º dia de atraso; perda comunicada antes disso é registrada como observação e o empréstimo segue Ativo e atrasado | B: perda a partir do 1º dia de atraso; C: a qualquer momento enquanto Ativo | Mantém DEC20 e torna o 30º dia observável | Aguarda aprovação (manter, B ou C) |

## 12. Questões em aberto

1. **Aprovação de DEC27 a DEC49** pelo aluno, por ID. Enquanto não aprovadas, as RNs que as citam são propostas da IA. Sem elas, as lacunas L1 a L20 identificadas na análise de 30/10/2026 permanecem.
2. **DEC37:** escolher opção A (cancelamento permitido, adotada no rascunho: RN80, RN81, HU12) ou B (sem cancelamento; remover RN80, RN81, HU12 e o estado Cancelado).
3. **DEC49:** manter DEC20 com aviso aos 30 dias (adotado no rascunho), ou flexibilizar para B ou C (altera RN73, RN76, HU10).
4. **Nome do aluno** no cabeçalho de `spec.md` e `prompts.md`.
5. **Exclusão da conta do dono** (direito do próprio dono sobre seus dados): não coberta por nenhuma decisão. Proposta a avaliar: ação "Excluir conta" que apaga todos os dados da conta de forma irreversível após confirmação por senha.
