# Empresta Aí — Especificação v1

**Aluno:** Mateus Pinheiro Silva Rodrigues  
**Data:** 03/11/2026  
**Status:** FINAL v1 — DEC01–DEC26 aprovadas em 30/10/2026; DEC27–DEC57 aprovadas expressamente em 03/11/2026. Especificação acadêmica; aplicativo não implementado.

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
10. Cancelar empréstimo registrado por engano.
11. Cadastrar e autenticar a conta do dono, encerrar a sessão e excluir integralmente a conta.

Os itens 1–9 correspondem às funcionalidades obrigatórias do enunciado; os itens 10–11 e os detalhes de funcionamento são decisões de produto aprovadas pelo aluno.

### 2.2 Fora do escopo

O aplicativo explicitamente **não faz**:

- Cobrança ou pagamento real de multas e indenizações; não calcula valores monetários.
- Marketplace público de objetos entre desconhecidos; não há busca de objetos de outras contas.
- Chat entre usuários; a contestação do tomador acontece fora do aplicativo e é apenas anotada pelo dono.
- Rastreamento por geolocalização; nenhuma coordenada é coletada.
- Conta ou acesso para o tomador; envio de avisos por e-mail, SMS ou push; pontuação de confiabilidade; bloqueio automático; funcionamento sem conexão.
- Recuperação ou troca de senha, troca de e-mail e login social nesta v1. A tela de cadastro informa que perder a senha impede recuperar o acesso nesta versão.
- Avaliação de dano por componente e recuperação formal incompleta de conjunto perdido; o dano é avaliado no conjunto completo.

## 3. Glossário

Cada termo tem um único significado em toda a especificação.

- **Dono:** pessoa física titular da conta; único usuário do aplicativo; empresta objetos.
- **Tomador:** amigo que recebe o objeto emprestado. Não usa o aplicativo; é representado por um contato.
- **Contato:** registro de um tomador na conta do dono (nome, telefone opcional, observação opcional).
- **Objeto:** bem físico individual cadastrado pelo dono. Dois itens idênticos são dois objetos.
- **Conjunto:** objeto cadastrado com dois ou mais componentes nomeados (exemplo: videogame com dois controles).
- **Componente:** parte nomeada de um conjunto; não pode ser emprestada separadamente.
- **Estado de conservação:** classificação do objeto em uma escala única: novo, bom, regular, ruim.
- **Status do objeto:** Disponível, Emprestado, Indisponível ou Arquivado (seção 7.4).
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
- **Cancelado:** estado persistido de empréstimo encerrado por registro indevido, sem que o objeto tenha saído.
- **Encerrado:** qualquer empréstimo nos estados Devolvido, Perdido ou Cancelado.
- **Indisponível:** status do objeto que impede novo empréstimo, por marcação manual do dono ou automática (dano grave ou inutilizado, perda, conjunto incompleto).
- **Arquivado:** status de objeto ou contato retirado de uso, preservado por ter histórico; não participa de novos empréstimos; reversível.
- **Bloqueado:** marca manual em contato que impede novos empréstimos para ele, sem afetar os já ativos.
- **Aviso:** registro gerado pelo sistema no painel do aplicativo, com tipo, data e hora, empréstimo e situação lido ou não lido. É o único canal de aviso.
- **Painel:** área do aplicativo que lista os avisos e exibe o contador de não lidos.
- **Evento:** registro de uma ação aceita sobre um empréstimo ou de remoção de dados de um contato, com instante e fatos estruturados. Não admite edição livre; conteúdo pessoal pode ser saneado por RN135 e a exclusão da conta remove todo o histórico (RN141).
- **Anonimização operacional:** nome usado nesta spec para a remoção irreversível de identificadores do contato e saneamento dos textos e imagens vinculados (RN133–RN139). Na interface, a ação se chama "Remover dados pessoais". Preserva fatos estruturados; não significa garantia jurídica de anonimização absoluta ou impossibilidade de reidentificação.
- **Avaliação de dano:** conservação, nível, descrição e foto registrados na devolução integral ou recuperação; há no máximo uma avaliação por empréstimo.
- **Data efetiva:** dia civil em que ocorreu saída, recebimento ou recuperação; distinto do instante em que o dono registra a ação.
- **Instante de gravação:** data/hora oficial em que a ação é confirmada no servidor, com precisão de milissegundos.
- **Confirmação:** envio deliberado de uma ação; reenvios da mesma confirmação não repetem seus efeitos.
- **Saneamento:** remoção de conteúdo pessoal, com marcador de privacidade, preservando a ocorrência e os fatos estruturados do evento.
- **Arquivamento de contato anonimizado:** permanente; a reversibilidade do arquivamento comum não se aplica após a remoção de dados pessoais.

## 4. Atores e permissões

| Ator | Pode fazer | Não pode fazer |
|---|---|---|
| Dono (usuário autenticado) | Criar, editar, arquivar, desarquivar e excluir objetos e contatos (RN15 a RN19, RN24 a RN26); bloquear e desbloquear contatos (RN22); registrar, renovar, devolver (integral e parcial), marcar perda, registrar recuperação, cancelar e anotar observações em empréstimos (RN28 a RN83); registrar dano, contestação e retificação (RN61 a RN66); consultar listas, históricos e avisos; marcar avisos como lidos; remover dados pessoais do contato (RN93); excluir sua conta (RN140–RN142); sair (RN103) | Ver dados de outra conta (RN02); editar livremente ou excluir isoladamente empréstimos e eventos (RN36, RN90); renovar empréstimo atrasado ou encerrado (RN54); marcar perda antes de 30 dias de atraso (RN73); reabrir empréstimo encerrado (RN77); alterar dano por outro meio que a retificação única (RN66); reverter anonimização (RN93) |
| Sistema (regras automáticas) | Calcular classificação em dia ou atrasado (RN39); gerar avisos às 09:00 (RN45); derivar status Emprestado e Indisponível automático (RN12, RN13); validar transições e recusar ações inválidas (RN82); gravar eventos (RN83) | Criar, renovar ou encerrar empréstimos por conta própria; bloquear contatos (RN22); alterar dano; enviar avisos por canal externo (RN44) |
| Tomador | Nada no aplicativo. Pede renovação, contesta dano ou pede remoção de dados por meio externo ao dono (RN51, RN65, RN93) | Acessar o aplicativo; ver ou alterar qualquer registro (RN03) |

## 5. Regras de negócio

Cada RN expressa uma validação, permissão ou efeito verificável e cita a decisão de origem (seção 11). Os efeitos de uma ação são gravados juntos conforme RN116. Datas e horas seguem America/Fortaleza. RN01–RN96 conservam os IDs do rascunho; RN97 em diante detalham as decisões aprovadas nesta revisão. Os domínios e mensagens gerais de validação estão em RN97–RN111 e na seção 7.3.

### 5.1 Conta e acesso

- **RN01** — Cada conta pertence a exatamente um dono, identificado por e-mail único conforme RN97 e RN98. (DEC01, DEC45)
- **RN02** — Objetos, contatos, empréstimos, eventos e avisos pertencem à conta que os criou e nunca são exibidos a outra conta. (DEC01, DEC45)
- **RN03** — O tomador não possui conta nem acesso; é representado exclusivamente por um contato. (DEC01)

### 5.2 Objetos

- **RN04** — O cadastro de objeto exige nome com 1 a 80 caracteres, categoria (RN05) e estado de conservação (RN06); a foto é opcional (RN07). (DEC03, DEC41)
- **RN05** — A categoria é uma destas oito, sem outras: Livros, Ferramentas, Jogos, Eletrônicos, Casa e cozinha, Esporte e lazer, Roupas e acessórios, Outros. (DEC03, DEC41)
- **RN06** — O estado de conservação usa a escala ordenada novo > bom > regular > ruim, no cadastro, na saída, na volta e na recuperação. (DEC15)
- **RN07** — A foto do objeto é opcional e admite no máximo um arquivo JPEG ou PNG de até 5.000.000 bytes, validado por RN108. (DEC41, DEC55)
- **RN08** — O nome do objeto não precisa ser único; o sistema gera um identificador interno. (DEC41)
- **RN09** — Cada objeto representa uma única unidade física; itens idênticos são cadastrados como objetos distintos. (DEC04)
- **RN10** — Um objeto pode ser cadastrado como conjunto com 2 a 50 componentes, cada um com nome de 1 a 80 caracteres; componentes não podem ser emprestados separadamente. (DEC05, DEC41)
- **RN11** — O status do objeto é um destes: Disponível, Emprestado, Indisponível, Arquivado, com precedência Arquivado > Emprestado > Indisponível > Disponível. (DEC29)
- **RN12** — Um objeto é Emprestado enquanto existir um empréstimo Ativo dele; o status não é gravado, é derivado. (DEC29)
- **RN13** — Um objeto fica Indisponível por marcação manual do dono (motivo opcional de até 500 caracteres) ou automaticamente nos casos das RN63, RN71 e RN75. (DEC17, DEC29, DEC34, DEC36)
- **RN14** — Marcar indisponibilidade manual ou liberar uma causa liberável exige que não exista empréstimo Ativo do objeto; perda e incompletude obedecem à restrição RN126. (DEC29, DEC51)
- **RN15** — Nome, categoria e foto do objeto podem ser editados a qualquer momento, inclusive quando arquivado ou emprestado; foto pode ser substituída ou removida. (DEC44)
- **RN16** — Objeto sem nenhum empréstimo registrado pode ser excluído fisicamente, junto com componentes e foto. (DEC42)
- **RN17** — Objeto com ao menos um empréstimo, em qualquer estado, não admite exclusão isolada; preserva-se por arquivamento. A exclusão integral da conta é a exceção RN141. (DEC22, DEC42, DEC50)
- **RN18** — Arquivar objeto exige que ele não tenha empréstimo Ativo. (DEC42)
- **RN19** — Objeto Arquivado não aceita novo empréstimo. (DEC22, DEC42)

### 5.3 Contatos

- **RN20** — O cadastro do contato exige nome de 1 a 80 caracteres e aceita somente telefone opcional (RN109) e observação opcional de até 500 caracteres como campos pessoais informados pelo dono. Marcas e datas são produzidas pelas ações previstas. (DEC06, DEC41, DEC48)
- **RN21** — O nome do contato não precisa ser único; o sistema gera um identificador interno. (DEC41)
- **RN22** — O dono pode bloquear ou desbloquear contato não anonimizado, com motivo opcional de até 500 caracteres no bloqueio; ao desbloquear, a marca e o motivo são removidos. (DEC11, DEC23, DEC48, DEC57)
- **RN23** — Contato Bloqueado não pode ser tomador de novo empréstimo; seus empréstimos Ativos continuam e podem ser renovados. (DEC23, DEC40)
- **RN24** — Nome, telefone e observação do contato podem ser editados enquanto o contato não estiver anonimizado. (DEC44, DEC57)
- **RN25** — Contato sem nenhum empréstimo pode ser excluído fisicamente, salvo o registro anonimizado preservado por RN133; com empréstimo não admite exclusão isolada. (DEC22, DEC42, DEC57)
- **RN26** — Contato Arquivado não pode ser tomador de novo empréstimo. (DEC22)
- **RN27** — Não há limite de empréstimos Ativos simultâneos por contato. (DEC43)

### 5.4 Registro de empréstimo e prazo

- **RN28** — Registrar empréstimo exige objeto Disponível, contato não Bloqueado e não Arquivado, data de saída e data de vencimento; o estado na saída pode ser ajustado e uma observação de até 300 caracteres é opcional. (DEC04, DEC23, DEC29, DEC30)
- **RN29** — Um objeto tem no máximo um empréstimo Ativo por vez. (DEC04)
- **RN30** — A data de saída deve ser menor ou igual a hoje, sem limite de antiguidade além do domínio RN107 e da cronologia RN119. (DEC09, DEC38, DEC52, DEC55)
- **RN31** — O prazo é informado como data de vencimento; o campo vem pré-preenchido com data de saída mais 7 dias e é editável. (DEC07, DEC48)
- **RN32** — A data de vencimento obrigatória deve estar entre saída + 1 e saída + 365 dias corridos, para qualquer categoria. (DEC07, DEC08)
- **RN33** — O vencimento ocorre às 23:59:59,999 da data de vencimento no fuso America/Fortaleza, para todas as contas, independentemente da localização do dono. (DEC09, DEC48)
- **RN34** — O estado na saída é preenchido com o estado de conservação atual do objeto e pode ser alterado pelo dono no ato; o valor gravado no empréstimo passa a ser o estado atual do objeto. (DEC30)
- **RN35** — Um registro válido cria o empréstimo no estado Ativo. O evento é gerado por RN117 e a disponibilidade é derivada por RN12. (DEC26, DEC29)
- **RN36** — Empréstimo não admite edição livre nem exclusão isolada; seus campos só mudam pelas ações previstas nesta spec, inclusive saneamento, e sua remoção só ocorre com a exclusão integral da conta. (DEC37, DEC50, DEC57)
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
- **RN49** — Ao encerrar o empréstimo (Devolvido, Perdido ou Cancelado), nenhum aviso novo é gerado; os já gerados continuam no painel, inclusive depois de lidos. (DEC12, DEC39)
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

- **RN58** — A devolução integral exige data efetiva entre saída e hoje (padrão hoje), conservação da volta e nível de dano; as validações da avaliação estão em RN62 e RN157–RN159, e a cronologia em RN118. (DEC16, DEC31, DEC33, DEC41, DEC52)
- **RN59** — A devolução integral válida leva o empréstimo de Ativo a Devolvido; conservação atual do objeto recebe o estado de volta e o status é recalculado pelas causas existentes (RN63, RN127). (DEC17, DEC29, DEC30, DEC51)
- **RN60** — Devolução com data menor ou igual ao vencimento é "no prazo"; com data posterior é "com atraso", com dias de atraso = data de devolução menos vencimento. (DEC33)
- **RN61** — O nível de dano é um destes: nenhum, leve, grave, inutilizado; quem define é exclusivamente o dono. (DEC16)
- **RN62** — Dano "inutilizado" exige estado de volta "ruim"; qualquer outra combinação de estado e dano é aceita. (DEC31)
- **RN63** — Dano grave ou inutilizado define causa automática "dano grave" ou "inutilizado" no objeto; sem precedência de Arquivado/Emprestado (RN11), ele fica Indisponível. A liberação posterior segue RN14 e RN154. (DEC17, DEC29, DEC51)
- **RN64** — O dano gera apenas registro; o aplicativo não calcula, cobra nem controla reparo, reposição ou indenização. (DEC17)
- **RN65** — O dono pode registrar uma única contestação por avaliação elegível (RN132), com texto de 1 a 500 caracteres, a qualquer momento após a devolução integral ou recuperação; ela não altera o nível de dano. (DEC18, DEC31)
- **RN66** — Uma avaliação admite no máximo uma retificação de nível de dano, até 23:59:59,999 da data civil de sua gravação + 7 dias; RN62, RN129–RN131 e RN137 se aplicam. (DEC32, DEC51, DEC57)

### 5.9 Devolução parcial

- **RN67** — A parcial exige conjunto Ativo, data válida por RN118 e RN58, pelo menos um novo componente recebido e pelo menos um ainda faltante; observação de até 300 caracteres é opcional. (DEC19, DEC36, DEC52)
- **RN68** — A devolução parcial mantém o empréstimo Ativo; prazo, classificação de atraso, avisos e renovação continuam valendo. (DEC19, DEC36)
- **RN69** — Componente marcado recebido não pode voltar a faltante; devoluções parciais sucessivas são permitidas até que todos os componentes estejam recebidos. (DEC36)
- **RN70** — Receber o último componente faltante segue as validações e o encerramento da devolução integral; todos os recebimentos ficam registrados conforme RN124. (DEC36)
- **RN71** — Perda declarada em empréstimo com devolução parcial leva ao estado Perdido, grava quais componentes foram recebidos e torna o objeto Indisponível com motivo "incompleto". (DEC36)
- **RN72** — Objeto sem componentes só admite devolução integral. (DEC19)

### 5.10 Perda, não devolução e recuperação

- **RN73** — O dono só pode marcar empréstimo Ativo como Perdido quando hoje menos vencimento atual for pelo menos 30 dias civis; a permissão começa às 00:00 do 30º dia de atraso. (DEC20, DEC49)
- **RN74** — A perda exige motivo "perda comunicada" ou "não devolução" e aceita observação opcional de até 500 caracteres. (DEC34)
- **RN75** — Marcar perda leva o empréstimo de Ativo a Perdido, grava o evento "Perda", torna o objeto Indisponível com motivo "perdido" (ou "incompleto", RN71) e interrompe avisos. (DEC29, DEC34)
- **RN76** — Perda comunicada antes do 30º dia de atraso pode ser anotada por RN84; o empréstimo continua Ativo, em dia se ainda não venceu e atrasado caso contrário. (DEC20, DEC49)
- **RN77** — Perdido é estado final: o empréstimo não volta a Ativo. (DEC21)
- **RN78** — Uma recuperação é permitida em empréstimo Perdido ainda não recuperado, com data entre a data civil da perda e hoje, conservação e avaliação de dano válidas; o empréstimo permanece Perdido, com marca "recuperado em dd/mm/aaaa". O efeito no objeto segue RN125 e RN127. (DEC21, DEC35, DEC51)
- **RN79** — Após a recuperação, emprestar o objeto de novo exige um novo registro de empréstimo. (DEC21)

### 5.11 Cancelamento e observações

- **RN80** — O dono pode cancelar Ativo sem renovação nem parcial, com motivo de 1 a 200 caracteres e confirmação de que não houve entrega real; o estado passa a Cancelado, conservando o estado de conservação do objeto e recalculando sua disponibilidade. (DEC37)
- **RN81** — Empréstimo Cancelado aparece em Encerrados e nos históricos, mas não entra em contagens de atraso nem de devolução. (DEC37)
- **RN82** — Toda ação sobre empréstimo valida o estado atual no instante da gravação; se o estado não admitir a ação, ela é recusada sem alterar nenhum registro e com a mensagem específica da seção 7.3. (DEC26)
- **RN83** — Ações recusadas não geram evento de domínio nem alteram os registros do negócio; metadados técnicos de confirmação não são histórico do empréstimo. (DEC26, DEC54)
- **RN84** — O dono pode anexar observação de 1 a 500 caracteres a empréstimo em qualquer estado, exceto se o contato estiver anonimizado (RN137); cada observação aceita é um evento. (DEC49, DEC57)

### 5.12 Listagens e histórico

- **RN85** — A lista Ativos contém todos os empréstimos Ativos, com os atrasados destacados, ordenados por data de vencimento crescente. (DEC28)
- **RN86** — A lista Atrasados contém os empréstimos Ativos com data de vencimento anterior a hoje, ordenados por dias de atraso decrescente. (DEC28)
- **RN87** — A lista Encerrados contém os empréstimos Devolvido, Perdido e Cancelado, ordenados por data de encerramento decrescente. (DEC28)
- **RN88** — O histórico do objeto lista todos os seus empréstimos, inclusive de contatos arquivados ou anonimizados, por saída decrescente e desempate RN143, com os eventos disponíveis após saneamento. (DEC22, DEC56, DEC57)
- **RN89** — O histórico do contato lista todos os seus empréstimos, inclusive de objetos arquivados, por saída decrescente e desempate RN143, e mostra separadamente seu evento de remoção de dados se existir. (DEC22, DEC56)
- **RN90** — Eventos não admitem edição livre nem exclusão isolada; RN135 autoriza apenas saneamento de conteúdo pessoal e RN141 autoriza remoção integral com a conta. (DEC21, DEC26, DEC50, DEC57)
- **RN91** — Cada empréstimo no histórico exibe: objeto, contato, data de saída, vencimento atual, estado, classificação (em dia, atrasado com dias, no prazo, com atraso com dias), renovações, estado na saída e na volta, dano, contestação, retificação e recuperação, quando existirem. (DEC28, DEC33)

### 5.13 Dados pessoais e privacidade

- **RN92** — Os dados do contato (nome, telefone, observação, bloqueio e motivo, datas) são usados exclusivamente para identificar o tomador e acompanhar seus empréstimos; a finalidade é exibida na tela "Privacidade" do aplicativo. (DEC24, DEC46)
- **RN93** — Remover dados pessoais exige contato sem empréstimo Ativo e confirmação do dono; substitui o nome por "Contato removido", apaga telefone, observação e motivo do bloqueio e arquiva permanentemente o contato, aplicando RN133–RN139. (DEC24, DEC46, DEC57)
- **RN94** — Remoção de dados pessoais com empréstimo Ativo é recusada: "Não é possível remover dados pessoais: há empréstimo ativo". O dono responde ao pedido externo explicando a pendência, sem encerramento automático. (DEC46)
- **RN95** — A tela "Privacidade" orienta que pedidos sejam feitos externamente ao dono e que ele responda em até 15 dias corridos, como meta do produto; não apresenta esse prazo como obrigação legal universal de eliminação. (DEC46)
- **RN96** — Dados dos contatos só são consultados pelo dono autenticado; o aplicativo não oferece exportação, publicação nem compartilhamento desses dados com outros usuários. (DEC24, DEC45)

### 5.14 Conta, validações comuns e gravação

- **RN97** — E-mail é normalizado removendo espaços externos e convertendo para minúsculas; deve ter até 254 caracteres, exatamente uma arroba, partes não vazias, domínio com ponto e nenhum espaço. (DEC45)
- **RN98** — Cadastrar conta exige e-mail normalizado ainda não usado; duplicidade é recusada com "E-mail já cadastrado". (DEC45)
- **RN99** — Senha de cadastro deve ter de 8 a 128 pontos de código, sem remoção de espaços nem normalização. (DEC45, DEC55)
- **RN100** — Login válido por e-mail normalizado e senha exata inicia sessão somente da conta correspondente. (DEC45)
- **RN101** — Login com e-mail inexistente ou senha incorreta é recusado pela mesma mensagem "E-mail ou senha inválidos". (DEC45)
- **RN102** — A sessão expira ao completar 30 dias de 24 horas sem requisição autenticada aceita; depois disso, o acesso exige novo login. (DEC45)
- **RN103** — A ação "Sair" revoga a sessão atual e retorna ao login; dados da conta permanecem. (DEC45)
- **RN104** — A v1 não oferece recuperação/troca de senha, troca de e-mail ou login social, informando essa limitação no cadastro. (DEC45)
- **RN105** — Textos informados pelo dono, exceto senha, são normalizados em Unicode NFC e têm espaços em branco externos removidos antes da contagem de pontos de código. (DEC55)
- **RN106** — Campo obrigatório vazio após normalização é recusado; campo opcional vazio é armazenado como nulo. (DEC55)
- **RN107** — Datas devem existir no calendário gregoriano entre 01/01/0001 e 31/12/9999; quantidades de dias e contagens aceitam apenas inteiros; cálculo fora do domínio é recusado. (DEC38, DEC55)
- **RN108** — Foto deve ser imagem decodificável JPEG ou PNG com no máximo 5.000.000 bytes; renomear extensão não muda a validação de conteúdo. (DEC41, DEC55)
- **RN109** — Telefone, quando preenchido, deve conter exatamente 10 ou 11 dígitos ASCII, sem espaços internos, letras, prefixo + ou máscara. (DEC41, DEC55)
- **RN110** — Validações seguem autenticação/pertencimento, estado/versão, arquivamento, bloqueio/disponibilidade e campos; objeto prioriza Arquivado > Emprestado > Indisponível e contato Arquivado > Bloqueado. (DEC55)
- **RN111** — Havendo vários campos inválidos, exibir todas as mensagens junto aos campos e não gravar a ação; acesso a registro inexistente ou de outra conta usa "Registro não encontrado". (DEC26, DEC55)
- **RN112** — Reenvio da mesma confirmação, pelo mesmo dono e com os mesmos dados, retorna o resultado já obtido, sem repetir efeitos ou eventos; a consulta desse resultado respeita autenticação e remoção de dados. (DEC54, DEC57)
- **RN113** — Reutilizar identificador de confirmação com dados diferentes é recusado com "Confirmação já utilizada com outros dados". (DEC54)
- **RN114** — Duas confirmações diferentes baseadas na mesma versão de registro não podem sobrescrever uma à outra: só a primeira grava e a segunda recebe "Registro atualizado. Recarregue antes de confirmar". (DEC54)
- **RN115** — Se uma resposta de gravação se perder, mostrar "Resultado não confirmado. Consulte o registro antes de tentar novamente"; reenviar usa a mesma confirmação, sem fila offline nem promessa de que nada foi salvo. (DEC54)
- **RN116** — Cada mutação é indivisível: validação, atualização dos registros afetados, evento aplicável e resultado de confirmação são confirmados juntos ou nenhum efeito de negócio é gravado. (DEC26, DEC47, DEC54)
- **RN117** — Cada ação aceita sobre empréstimo gera um único evento de seu tipo; anonimização gera um evento ligado ao contato; cadastros, edição de objeto/contato, login e leitura de avisos não criam eventos de empréstimo. (DEC26, DEC57)

### 5.15 Cronologia, componentes e causas de indisponibilidade

- **RN118** — Data efetiva de parcial ou devolução integral deve ser igual ou posterior à saída, a todos os recebimentos já registrados no empréstimo e à data civil da última renovação; nunca posterior a hoje. (DEC33, DEC36, DEC52)
- **RN119** — Novo empréstimo do mesmo objeto só aceita saída igual ou posterior ao último retorno integral ou recuperação de empréstimo não Cancelado; não se insere registro antigo entre empréstimos já registrados. (DEC38, DEC52)
- **RN120** — encerradoEm é o instante da gravação da devolução, perda ou cancelamento, não a data efetiva informada; recuperação, retificação e saneamento não alteram esse instante. (DEC28, DEC52)
- **RN121** — Tipo simples/conjunto, nomes, ordem e composição dos componentes só podem mudar antes do primeiro empréstimo do objeto, inclusive se esse primeiro empréstimo for Cancelado. (DEC53)
- **RN122** — Componentes podem ter nomes iguais, mas cada um tem ID próprio e posição única; reposição física mantém a identidade lógica da peça. (DEC53)
- **RN123** — Uma parcial que não acrescente componente ainda faltante é recusada com "Marque ao menos um novo componente recebido". (DEC36)
- **RN124** — Na devolução integral de conjunto, cada componente ainda faltante recebe a data efetiva da devolução; componentes já recebidos conservam suas datas. (DEC36)
- **RN125** — Recuperação formal de conjunto Perdido exige confirmação de que todas as peças faltantes voltaram; grava seus recebimentos na data da recuperação e substitui a causa perdido/incompleto pela causa de dano aplicável; retorno incompleto admite somente observação quando RN137 permitir. (DEC35, DEC51, DEC57)
- **RN126** — Causa automática perdido ou incompleto não pode ser liberada manualmente; só recuperação completa a remove. (DEC51)
- **RN127** — Arquivar/desarquivar preserva causas manuais e automáticas; recuperação ou retificação remove apenas a causa correspondente, sem apagar motivo manual independente nem retirar arquivamento. (DEC51)
- **RN128** — Causa automática de dano referencia o empréstimo que a originou; motivo manual é independente dessa causa. (DEC51)
- **RN129** — Retificação só repercute na causa automática do objeto se a avaliação pertencer ao empréstimo mais recentemente registrado desse objeto; existindo qualquer empréstimo posterior, altera apenas o histórico. Sem empréstimo posterior, novo nível grave/inutilizado define a causa correspondente; novo nível nenhum/leve remove somente causa de dano desse empréstimo. Nunca altera conservação atual ou estadoVolta. (DEC32, DEC51)
- **RN130** — Retificação para dano diferente de nenhum exige descrição atual de 1–500 caracteres; para nenhum, limpa descrição atual; preserva nível e descrição anteriores no evento, salvo RN135. Foto não é editável pela retificação. (DEC32, DEC57)
- **RN131** — Retificar para o nível já vigente é recusado com "Informe um nível de dano diferente" e não consome a única retificação. (DEC32)
- **RN132** — Contestação só é permitida após avaliação cujo nível original ou atual seja diferente de nenhum; contato anonimizado não admite contestação textual. (DEC31, DEC57)

### 5.16 Remoção de dados pessoais e exclusão de conta

- **RN133** — Contato anonimizado fica imutável, permanentemente arquivado e não pode ser editado, desarquivado, bloqueado/desbloqueado, excluído isoladamente ou reutilizado; repetir a remoção não gera outro evento. (DEC44, DEC46, DEC57)
- **RN134** — Remoção de dados pessoais saneia observações de saída e parciais, descrição/foto de dano, contestação, observação de perda, motivo de cancelamento e observações livres de todos os empréstimos desse contato, incluindo cópias nos eventos e resultados de confirmação. (DEC57)
- **RN135** — Saneamento apaga conteúdo pessoal de forma irreversível, preservando ID, tipo, datas, níveis, estados e contagens; texto histórico obrigatório é substituído por "Conteúdo removido por privacidade" com sinalizador de saneamento. (DEC57)
- **RN136** — Antes de concluir a remoção, mostrar nomes e fotos dos objetos relacionados para o dono remover identificadores indevidos pela edição normal e confirmar revisão; o app não identifica automaticamente pessoas em textos ou fotos. (DEC57)
- **RN137** — Após anonimização do contato, seus empréstimos permitem recuperação e retificação apenas com fatos estruturados; textos/fotos ficam desabilitados, descrição obrigatória usa marcador de privacidade, e observações/contestações textuais são recusadas. (DEC57)
- **RN138** — A remoção grava data/hora e evento ligado ao contato, inclusive se ele não tiver empréstimos; o evento registra a ocorrência do saneamento, sem copiar conteúdo eliminado. (DEC57)
- **RN139** — Exclusões e saneamentos tornam os dados indisponíveis na base ativa imediatamente e expurgam cópias de backup em até 30 dias; qualquer restauração deve reaplicar remoções antes de disponibilizar dados. (DEC50, DEC57)
- **RN140** — Excluir conta exige sessão autenticada, senha atual correta e confirmação explícita após exibir a quantidade de empréstimos Ativos que serão eliminados. (DEC50)
- **RN141** — Exclusão confirmada da conta remove conta, objetos, componentes, contatos, empréstimos, renovações, recebimentos, eventos, avisos, arquivos e confirmações técnicas, e revoga todas as sessões; Ativos não impedem a ação. (DEC50)
- **RN142** — Senha incorreta, ausência/cancelamento de confirmação ou falha antes da confirmação da exclusão mantém a conta e todos os registros; sucesso encerra a sessão com "Conta excluída" e não permite restauração pelo dono. (DEC50, DEC54)

### 5.17 Ordenação, avisos e cadastros complementares

- **RN143** — Empates nas ordenações de RN85–RN89 são resolvidos por ID crescente, em comparação ordinal. (DEC28, DEC44, DEC56)
- **RN144** — Eventos são apresentados por instante de gravação crescente e ID crescente no empate; data efetiva, quando existir, aparece separadamente. (DEC56)
- **RN145** — Painel ordena avisos por dataReferencia decrescente e ID crescente no empate; marcar um ou todos como lidos altera somente lido, preservando todos os avisos. (DEC39, DEC56)
- **RN146** — Falha às 09:00 admite geração na primeira oportunidade até o fim do mesmo dia; depois da meia-noite, avisos do dia anterior são descartados, sem reposição. (DEC56)
- **RN147** — Ao gerar aviso, revalidar Ativo, vencimento atual, elegibilidade do horário e chave empréstimo+dia; não reconstruir situação passada nem alterar tipo, vencimentoNaGeracao ou diasAtrasoNaGeracao de aviso já gerado. Registro confirmado até as 09:00:00,000 é elegível nesse dia. (DEC39, DEC56)
- **RN148** — Tela aberta reconsulta o servidor na próxima meia-noite de Fortaleza e ao voltar ao primeiro plano, atualizando classificações derivadas; sem rede, aplica RN115/RNF08 sem inventar data oficial. (DEC25, DEC56)
- **RN149** — Contagens de atraso incluem somente Ativos atrasados e Devolvidos com atraso, apresentados separadamente; Perdidos e Cancelados têm suas próprias categorias e não recebem classificação no prazo/com atraso. (DEC28, DEC33, DEC37)
- **RN150** — Conservação do objeto só pode ser editada quando ele não tiver empréstimo Ativo; essa edição não altera estados registrados nos empréstimos. (DEC44)
- **RN151** — Históricos e avisos resolvem nomes atuais por referência ao objeto/contato; não copiam nomes de contatos nos dados estruturados dos eventos. Avisos não exibem nome de contato. (DEC44, DEC56, DEC57)
- **RN152** — Arquivar contato exige ausência de empréstimos Ativos e preserva bloqueio e histórico; arquivado não aceita novo empréstimo. (DEC22, DEC42)
- **RN153** — Desarquivar objeto ou contato comum preserva os demais impedimentos; contato anonimizado não admite desarquivamento. (DEC42, DEC51, DEC57)
- **RN154** — A ação manual de liberação pode remover causa de dano grave/inutilizado, mantendo o dano no histórico; se o dono também desmarcar o motivo manual, remove esse motivo. Perda/incompletude nunca é liberada por essa ação. (DEC17, DEC29, DEC51)
- **RN155** — Novo objeto inicia sem arquivamento ou indisponibilidade; novo contato inicia sem bloqueio, arquivamento ou anonimização; novo empréstimo inicia com zero renovações; novo aviso inicia não lido. (DEC29, DEC39, DEC42)
- **RN156** — Para Ativo em dia e Devolvido no prazo, diasAtraso vale 0; para Perdido e Cancelado, classificacao e diasAtraso não se aplicam (nulos). (DEC27, DEC28, DEC33)
- **RN157** — Avaliação de dano diferente de nenhum exige descrição de 1–500 caracteres; dano nenhum dispensa descrição e guarda nulo no campo atual; RN137 substitui texto por marcador após anonimização. (DEC31, DEC32, DEC57)
- **RN158** — Cada avaliação aceita no máximo uma foto opcional JPEG/PNG de até 5.000.000 bytes, exceto após anonimização, quando nenhuma foto é aceita. (DEC41, DEC57)
- **RN159** — A foto da avaliação não pode ser substituída nem removida por edição livre; saneamento e exclusão da conta são exceções. (DEC32, DEC57)

## 6. Histórias de usuário

Convenções dos cenários: cada cenário é independente e começa com o dono autenticado, registros da sua conta e versão atual, salvo testes de acesso. Não há ações prévias além das declaradas em seu Dado. As datas são dados fictícios de teste, não a data da entrega. Hoje é 30/10/2026 (sexta-feira), salvo data explicitada no cenário. Horas usam America/Fortaleza. Demais campos obrigatórios válidos do contexto: objeto "Furadeira Bosch", categoria "Ferramentas", conservação "bom", contato "Ana Souza" não bloqueado/arquivado, saída 30/10/2026, vencimento 06/11/2026; quando houver devolução sem outros valores, conservação "bom" e dano "nenhum". Na HU09, a avaliação padrão foi gravada em 10/11/2026, sem contestação/retificação, e o instante atual é 15/11/2026. Na HU11, perda padrão gravada em 06/12/2026 e hoje é 20/12/2026. Os nomes e telefones dos exemplos são fictícios. Mensagens entre aspas são literais.

### HU01 — Cadastrar objeto

**Como** dono, **quero** cadastrar um objeto, **para** poder emprestá-lo depois.  
**Regras relacionadas:** RN04, RN05, RN06, RN07, RN08, RN09, RN11, RN105, RN106, RN108, RN111, RN155.

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

- **Cenário: normalização do nome**  
  Dado que nome contém apenas três espaços e categoria Livros e estado bom estão selecionados  
  Quando tento salvar  
  Então vejo "Informe o nome do objeto" e nada é salvo
- **Cenário: foto exatamente no limite**  
  Dado que informo objeto válido e uma imagem PNG decodificável de 5.000.000 bytes  
  Quando salvo o objeto  
  Então a foto é aceita; a mesma entrada com 5.000.001 bytes é recusada por tamanho

### HU02 — Cadastrar conjunto

**Como** dono, **quero** cadastrar um conjunto com componentes, **para** controlar devoluções incompletas.  
**Regras relacionadas:** RN04, RN10, RN67, RN72, RN121, RN122, RN123, RN124.

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

- **Cenário: limites válidos de composição**  
  Dado que preenchi objeto válido como conjunto  
  Quando salvo com 2 componentes nomeados e, em cenário independente, com 50  
  Então ambos os cadastros são aceitos
- **Cenário: composição congelada pelo histórico**  
  Dado que conjunto de 3 componentes tem um empréstimo Cancelado  
  Quando tento retirar um componente  
  Então vejo "Composição não pode ser alterada após o primeiro empréstimo" e a composição permanece igual
- **Cenário: componentes homônimos**  
  Dado que um conjunto tem componentes Console, Controle e Controle, com IDs distintos  
  Quando salvo o cadastro  
  Então os três componentes são aceitos e suas posições distinguem os dois controles

### HU03 — Cadastrar contato

**Como** dono, **quero** cadastrar um amigo, **para** registrar empréstimos a ele.  
**Regras relacionadas:** RN20, RN21, RN92, RN95, RN105, RN106, RN109, RN111, RN155.

- **Cenário: cadastro mínimo**  
  Dado que estou na tela de novo contato  
  Quando informo nome "Ana Souza" e deixo telefone e observação vazios  
  Então o contato é salvo e aparece na lista como "Ana Souza"
- **Cenário: nome vazio**  
  Dado que estou na tela de novo contato e o nome está vazio  
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
  Então vejo o texto da finalidade (RN92) e a orientação da meta de resposta em até 15 dias (RN95)

- **Cenário: limite de observação**  
  Dado que nome é Ana Souza e telefone está vazio  
  Quando tento salvar observação com 501 caracteres  
  Então vejo "Observação deve ter no máximo 500 caracteres" e nada é salvo
- **Cenário: contatos homônimos**  
  Dado que já existe Ana Souza  
  Quando cadastro outro contato com o mesmo nome  
  Então ambos permanecem com IDs distintos

### HU04 — Bloquear e desbloquear contato

**Como** dono, **quero** bloquear um amigo, **para** impedir novos empréstimos a ele sem perder o histórico.  
**Regras relacionadas:** RN22, RN23, RN42, RN55, RN133.

- **Cenário: bloqueio com motivo**  
  Dado que "Ana Souza" não está bloqueada  
  Quando aciono "Bloquear" e informo motivo "Devolveu o livro rasgado"  
  Então o contato exibe a marca "Bloqueado" e o motivo
- **Cenário: bloqueio sem motivo**  
  Dado que "Ana Souza" não está bloqueada nem anonimizada  
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
**Regras relacionadas:** RN27, RN28, RN29, RN30, RN31, RN32, RN33, RN34, RN35, RN36, RN37, RN38, RN43, RN46, RN97, RN107, RN110, RN111, RN112, RN113, RN114, RN116, RN117, RN119, RN155.

- **Cenário: registro válido com prazo sugerido**  
  Dado que "Furadeira Bosch" está Disponível com estado "bom" e "Ana Souza" não está bloqueada nem arquivada  
  Quando abro novo empréstimo, seleciono o objeto e o contato e aceito saída "30/10/2026" e vencimento pré-preenchido "06/11/2026"  
  Então o empréstimo é salvo como Ativo, em dia, estado na saída "bom", o objeto passa a "Emprestado" e o evento "Registro" aparece no histórico
- **Cenário: ajuste do estado na saída**  
  Dado que o objeto tem estado "bom"  
  Quando altero o estado na saída para "regular" com observação "Arranhão na lateral" e salvo  
  Então o empréstimo grava estado na saída "regular" e o objeto passa a ter estado "regular"
- **Cenário: saída futura**  
  Dado que hoje é 30/10/2026 e objeto e contato estão aptos para novo empréstimo  
  Quando informo saída "31/10/2026"  
  Então vejo "A data de saída não pode ser futura" e nada é salvo
- **Cenário limite: vencimento igual à saída**  
  Dado que hoje é 30/10/2026 e objeto e contato estão aptos para novo empréstimo  
  Quando informo saída "30/10/2026" e vencimento "30/10/2026"  
  Então vejo "O vencimento deve ser de 1 a 365 dias após a saída" e nada é salvo
- **Cenário limite: vencimento em 365 dias**  
  Dado que hoje é 30/10/2026 e objeto e contato estão aptos para novo empréstimo  
  Quando informo saída "30/10/2026" e vencimento "30/10/2027"  
  Então o empréstimo é salvo
- **Cenário limite: vencimento em 366 dias**  
  Dado que hoje é 30/10/2026 e objeto e contato estão aptos para novo empréstimo  
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

- **Cenário: cronologia do mesmo objeto**  
  Dado que o último empréstimo não Cancelado da furadeira foi devolvido em 10/11/2026 e hoje é 12/11/2026  
  Quando tento registrar nova saída em 09/11/2026 e vencimento em 16/11/2026  
  Então vejo "Saída anterior ao último retorno do objeto" e nada é salvo
- **Cenário: reutilização no mesmo dia**  
  Dado que a furadeira foi devolvida em 10/11/2026, está Disponível e hoje é 10/11/2026  
  Quando registro nova saída em 10/11/2026 e vencimento em 17/11/2026  
  Então o novo empréstimo é aceito e o anterior permanece no histórico
- **Cenário: data inexistente**  
  Dado que objeto e contato estão aptos e hoje é 03/03/2027  
  Quando informo saída 30/02/2027  
  Então vejo "Informe uma data válida" e nada é salvo
- **Cenário: vencimento ausente**  
  Dado que objeto e contato estão aptos e a saída é 30/10/2026  
  Quando apago o vencimento sugerido e salvo  
  Então vejo "Informe a data de vencimento" e nada é salvo

### HU06 — Renovar empréstimo

**Como** dono, **quero** renovar um empréstimo, **para** adiar o vencimento a pedido do amigo.  
**Regras relacionadas:** RN48, RN51, RN52, RN53, RN54, RN55, RN56, RN57, RN112, RN113, RN114, RN115, RN116, RN117.

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
  Dado empréstimo Ativo em dia, com vencimento 06/11/2026 e zero renovações  
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

- **Cenário: clique duplo na mesma confirmação**  
  Dado que empréstimo Ativo em dia vence em 06/11/2026 e tem zero renovações  
  Quando envio duas vezes a mesma confirmação de renovar por 7 dias  
  Então o vencimento fica em 13/11/2026, há uma renovação e um evento, e o segundo envio retorna o mesmo resultado
- **Cenário: duas abas com versão antiga**  
  Dado que duas abas mostram o mesmo empréstimo em dia com zero renovações  
  Quando a primeira renova por 3 dias e a segunda envia outra confirmação baseada na versão anterior  
  Então só a primeira grava e a segunda recebe "Registro atualizado. Recarregue antes de confirmar"
- **Cenário: máximo total do prazo**  
  Dado que saída foi 30/10/2026, vencimento original é 30/10/2027, hoje é 01/11/2026 e não houve renovação  
  Quando confirmo duas renovações distintas de 7 dias, recarregando o estado entre elas  
  Então o vencimento final é 13/11/2027, equivalente a saída + 379 dias
- **Cenário: reenvio alterado**  
  Dado que uma confirmação já renovou por 3 dias  
  Quando reutilizo seu identificador para 7 dias  
  Então vejo "Confirmação já utilizada com outros dados" e não ocorre nova renovação

### HU07 — Registrar devolução integral

**Como** dono, **quero** registrar a volta do objeto com seu estado, **para** encerrar o empréstimo e comparar com a saída.  
**Regras relacionadas:** RN49, RN58, RN59, RN60, RN61, RN62, RN63, RN64, RN82, RN83, RN112, RN115, RN116, RN117, RN118, RN120, RN124, RN157, RN158, RN159.

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
  Dado empréstimo Ativo com saída 30/10/2026 e hoje 05/11/2026  
  Quando informo dano "leve" e descrição vazia  
  Então vejo "Descreva o dano" e nada é salvo
- **Cenário: dano grave**  
  Dado empréstimo Ativo com saída 30/10/2026 e hoje 05/11/2026  
  Quando registro estado "ruim", dano "grave" e descrição "Mandril travado"  
  Então o empréstimo fica Devolvido, o objeto fica "Indisponível" com motivo "dano grave" e estado "ruim"
- **Cenário: inutilizado com estado incompatível**  
  Dado empréstimo Ativo com saída 30/10/2026 e hoje 05/11/2026  
  Quando informo dano "inutilizado" e estado "regular"  
  Então vejo "Objeto inutilizado deve ter estado 'ruim'" e nada é salvo
- **Cenário: devolução duplicada**  
  Dado empréstimo já Devolvido  
  Quando tento registrar devolução de novo  
  Então vejo "Este empréstimo já está encerrado" e nada muda
- **Cenário: avisos cessam**  
  Dado empréstimo atrasado devolvido em 10/11/2026 às 15:00  
  Quando chega 11/11/2026 às 09:00  
  Então nenhum aviso "Atrasado" é gerado para ele e o aviso de 10/11 continua no painel, inclusive depois de lido

- **Cenário: devolução anterior à renovação**  
  Dado que empréstimo saiu em 30/10/2026, foi renovado em 05/11/2026 e hoje é 07/11/2026  
  Quando informo devolução efetiva em 04/11/2026 com estado bom e dano nenhum  
  Então vejo "Data anterior ao último recebimento ou renovação" e nada é salvo
- **Cenário: resposta de rede perdida**  
  Dado que uma devolução válida foi confirmada no servidor, mas a resposta não chegou  
  Quando consulto o registro e reenvio a mesma confirmação  
  Então vejo o empréstimo Devolvido e somente um evento; antes da consulta, a interface informa que o resultado não foi confirmado

### HU08 — Registrar devolução parcial

**Como** dono, **quero** registrar que só parte de um conjunto voltou, **para** não dar o empréstimo como encerrado.  
**Regras relacionadas:** RN55, RN67, RN68, RN69, RN70, RN71, RN72, RN118, RN123, RN124.

- **Cenário: parcial válida**  
  Dado hoje 03/11/2026, empréstimo Ativo do "PlayStation 5" (Console, Controle 1, Controle 2), saída 30/10/2026, vencimento 06/11/2026  
  Quando registro devolução parcial em "03/11/2026" marcando "Console" e "Controle 1" recebidos e "Controle 2" faltante  
  Então o empréstimo continua Ativo com marca "Devolução parcial: falta Controle 2", o vencimento permanece 06/11/2026 e o evento "Devolução parcial" é gravado
- **Cenário: nada recebido**  
  Dado conjunto Ativo de três componentes, todos faltantes, hoje 03/11/2026  
  Quando tento registrar parcial sem marcar componente recebido  
  Então vejo "Marque ao menos um novo componente recebido" e nada é salvo
- **Cenário: todos recebidos vira integral**  
  Dado empréstimo Ativo do PlayStation 5, vencimento 06/11/2026, parcial de Console e Controle 1 em 03/11/2026, Controle 2 faltante e hoje 08/11/2026  
  Quando marco "Controle 2" recebido em "08/11/2026" com estado "bom" e dano "nenhum"  
  Então o empréstimo fica Devolvido "com atraso (2 dias)" e o objeto volta a "Disponível"
- **Cenário: renovação após parcial**  
  Dado empréstimo Ativo de conjunto com vencimento 06/11/2026, zero renovações, parcial em 03/11/2026 e hoje 05/11/2026  
  Quando renovo por 5 dias  
  Então o vencimento passa a 11/11/2026
- **Cenário: objeto sem componentes**  
  Dado empréstimo da "Furadeira Bosch"  
  Quando abro a devolução  
  Então a opção de devolução parcial não é exibida
- **Cenário: perda após parcial**  
  Dado empréstimo Ativo de Console, Controle 1 e Controle 2 com vencimento 06/11/2026, os dois primeiros recebidos em 03/11/2026, Controle 2 faltante e hoje 06/12/2026  
  Quando marco perda com motivo "não devolução"  
  Então o empréstimo fica Perdido com registro "recebidos: Console, Controle 1; faltante: Controle 2" e o objeto fica "Indisponível" com motivo "incompleto"

- **Cenário: parcial sem novo recebimento**  
  Dado que Console foi recebido em 03/11/2026, Controle 1 e Controle 2 faltam e hoje é 04/11/2026  
  Quando envio outra parcial marcando apenas Console  
  Então vejo "Marque ao menos um novo componente recebido" e nenhum recebimento é duplicado
- **Cenário: parcial com data regressiva**  
  Dado que Console foi recebido em 03/11/2026 e hoje é 04/11/2026  
  Quando tento receber Controle 1 com data 02/11/2026  
  Então vejo "Data anterior ao último recebimento ou renovação" e nada é salvo
- **Cenário: integral registra as peças restantes**  
  Dado que Console foi recebido em 03/11/2026 e dois controles faltam, com vencimento 06/11/2026 e hoje 05/11/2026  
  Quando registro devolução integral em 05/11/2026, estado bom e dano nenhum  
  Então os controles recebem data 05/11/2026, Console mantém 03/11/2026 e o empréstimo fica Devolvido no prazo

### HU09 — Registrar contestação e retificar dano

**Como** dono, **quero** anotar a discordância do amigo e corrigir um dano registrado por engano, **para** manter o histórico fiel.  
**Regras relacionadas:** RN65, RN66, RN90, RN128, RN129, RN130, RN131, RN132, RN137, RN157, RN159.

- **Cenário: contestação**  
  Dado devolução registrada em 10/11/2026 com dano "leve"  
  Quando registro contestação "Ana afirma que o risco já existia"  
  Então o evento "Contestação" aparece no histórico e o dano continua "leve"
- **Cenário: segunda contestação**  
  Dado avaliação de 10/11/2026 com dano leve e uma contestação já registrada, hoje 15/11/2026  
  Quando tento registrar outra  
  Então vejo "Contestação já registrada" e nada muda
- **Cenário: retificação dentro do prazo**  
  Dado devolução registrada em 10/11/2026 com dano "grave", descrição "Mandril travado", estado ruim, sem empréstimo posterior nem causa manual, objeto Indisponível e hoje 15/11/2026  
  Quando retifico para "leve" com descrição "Risco superficial"  
  Então o evento "Retificação: grave → leve" é gravado e o objeto volta a "Disponível"
- **Cenário limite: 8º dia**  
  Dado devolução registrada em 10/11/2026 e hoje 18/11/2026  
  Quando tento retificar  
  Então vejo "Prazo de retificação encerrado" e nada muda
- **Cenário: segunda retificação**  
  Dado avaliação de 10/11/2026 retificada uma vez em 12/11/2026, hoje 15/11/2026  
  Quando tento retificar de novo  
  Então vejo "Dano já retificado" e nada muda
- **Cenário: retificar para inutilizado com estado "bom"**  
  Dado avaliação com dano leve e estadoVolta bom, gravada em 10/11/2026, ainda não retificada, hoje 15/11/2026  
  Quando retifico para "inutilizado" com descrição "Objeto sem uso" e estado de volta registrado "bom"  
  Então vejo "Objeto inutilizado deve ter estado 'ruim'" e nada muda

- **Cenário: limite inclusivo do sétimo dia**  
  Dado que avaliação foi gravada em 10/11/2026 às 15:00 com dano grave e estado ruim e não houve retificação  
  Quando retifico para leve com descrição Risco às 23:59:59,999 de 17/11/2026  
  Então a correção é aceita; em cenário independente às 00:00 de 18/11/2026 seria recusada
- **Cenário: retificação de empréstimo antigo**  
  Dado que o empréstimo E1 foi devolvido em 10/11/2026 com dano grave, o dono liberou o objeto e registrou E2 em 11/11/2026, ainda Ativo, e hoje é 12/11/2026  
  Quando retifico E1 para leve com descrição Risco  
  Então E1 mantém evento da correção e E2 permanece Ativo; o objeto continua Emprestado
- **Cenário: preservação de motivo manual**  
  Dado que avaliação grave de 10/11/2026 é a mais recente e há motivo manual "Revisão técnica", hoje 12/11/2026  
  Quando retifico para nenhum  
  Então a causa automática de dano é removida, a descrição atual fica nula e o objeto permanece Indisponível pelo motivo manual
- **Cenário: retificação sem mudança**  
  Dado que dano vigente é leve e ainda não houve correção  
  Quando tento retificar para leve  
  Então vejo "Informe um nível de dano diferente" e continuo com direito à única retificação
- **Cenário: contestação sem dano**  
  Dado que a avaliação original e atual tem nível nenhum e não foi saneada  
  Quando tento registrar contestação  
  Então vejo "Não há dano para contestar" e nada é salvo

### HU10 — Registrar perda ou não devolução

**Como** dono, **quero** marcar que o objeto não voltou, **para** encerrar o empréstimo e deixar o objeto indisponível.  
**Regras relacionadas:** RN71, RN73, RN74, RN75, RN76, RN77, RN84, RN120, RN126.

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
  Dado empréstimo Ativo com vencimento 06/11/2026 e hoje 06/12/2026  
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

- **Cenário: perda comunicada ainda em dia**  
  Dado que hoje é 02/11/2026, o empréstimo vence em 06/11/2026 e o amigo comunicou a perda  
  Quando registro observação sobre a comunicação  
  Então o empréstimo continua Ativo em dia; a marcação de perda permanece impedida até 06/12/2026
- **Cenário: limite de perda à meia-noite**  
  Dado que vencimento é 06/11/2026 e empréstimo continua Ativo  
  Quando confirmo perda às 00:00 de 06/12/2026  
  Então a ação é aceita com 30 dias civis de atraso, mesmo antes do aviso das 09:00

### HU11 — Registrar recuperação de objeto perdido

**Como** dono, **quero** registrar que um objeto perdido reapareceu, **para** voltar a usá-lo sem apagar a perda.  
**Regras relacionadas:** RN62, RN63, RN77, RN78, RN79, RN125, RN127, RN137, RN157, RN158.

- **Cenário: recuperação sem dano**  
  Dado empréstimo Perdido em 06/12/2026 e hoje 20/12/2026  
  Quando registro recuperação com data "20/12/2026", estado "regular" e dano "nenhum"  
  Então o empréstimo exibe "Perdido — recuperado em 20/12/2026", o evento "Recuperado" é gravado e o objeto fica "Disponível" com estado "regular"
- **Cenário: recuperação com dano grave**  
  Dado empréstimo Perdido em 06/12/2026, ainda não recuperado, hoje 20/12/2026  
  Quando registro dano "grave" com descrição "Tela trincada" e estado "ruim"  
  Então o objeto fica "Indisponível" com motivo "dano grave"
- **Cenário: data anterior à perda**  
  Dado empréstimo Perdido em 06/12/2026, ainda não recuperado, hoje 20/12/2026  
  Quando informo data "05/12/2026"  
  Então vejo "A data de recuperação deve estar entre a perda e hoje" e nada muda
- **Cenário: segunda recuperação**  
  Dado empréstimo Perdido em 06/12/2026 com recuperação registrada em 15/12/2026, hoje 20/12/2026  
  Quando tento registrar outra  
  Então vejo "Recuperação já registrada" e nada muda
- **Cenário: empréstimo não reabre**  
  Dado empréstimo Perdido em 06/12/2026 e recuperado em 15/12/2026, hoje 20/12/2026  
  Quando consulto Ativos  
  Então ele não aparece; aparece em Encerrados como Perdido

- **Cenário: conjunto ainda incompleto**  
  Dado que conjunto foi marcado Perdido em 06/12/2026 com dois controles faltantes e hoje é 20/12/2026  
  Quando tento registrar recuperação confirmando retorno de apenas um controle  
  Então vejo "Recuperação exige o conjunto completo"; nenhum recebimento formal é alterado e posso anotar observação se o contato não estiver anonimizado
- **Cenário: recuperação completa em objeto arquivado**  
  Dado que conjunto Perdido em 06/12/2026 está Arquivado e hoje é 20/12/2026  
  Quando confirmo retorno de todas as peças em 20/12/2026, conservação bom e dano nenhum  
  Então a causa incompleto é removida, todas as peças ficam recebidas, o objeto continua Arquivado e o empréstimo segue Perdido recuperado
- **Cenário: recuperação após remoção de dados**  
  Dado que contato foi anonimizado, objeto está Perdido desde 06/12/2026 e hoje é 20/12/2026  
  Quando registro recuperação completa com estado ruim e dano grave  
  Então os fatos estruturados são gravados, texto/foto não são aceitos e a descrição usa o marcador de privacidade

### HU12 — Cancelar empréstimo registrado por engano

**Como** dono, **quero** cancelar um registro feito por engano, **para** não falsear o histórico.  
**Regras relacionadas:** RN36, RN49, RN80, RN81, RN82, RN83, RN120.

- **Cenário: cancelamento válido**  
  Dado empréstimo Ativo sem renovação nem devolução parcial, sem entrega real do objeto  
  Quando aciono "Cancelar" com motivo "Registro duplicado por engano" e confirmo que não houve entrega real  
  Então o empréstimo fica Cancelado, o objeto volta a "Disponível" e o evento "Cancelamento" é gravado
- **Cenário: com renovação**  
  Dado empréstimo com 1 renovação  
  Quando tento cancelar  
  Então vejo "Empréstimo com renovação ou devolução parcial não pode ser cancelado" e nada muda
- **Cenário: motivo vazio**  
  Dado empréstimo Ativo sem renovação nem parcial e sem entrega real  
  Quando tento cancelar sem motivo  
  Então vejo "Informe o motivo do cancelamento" e nada muda

- **Cenário: entrega efetiva impede cancelamento**  
  Dado que empréstimo Ativo não tem renovação nem parcial, mas houve entrega real  
  Quando tento cancelar sem confirmar ausência de entrega  
  Então vejo "Cancelamento exige confirmação de que não houve entrega real" e nada muda
- **Cenário: parcial impede cancelamento**  
  Dado que um conjunto Ativo já teve parcial  
  Quando tento cancelar  
  Então vejo "Empréstimo com renovação ou devolução parcial não pode ser cancelado" e nada muda

### HU13 — Listar ativos, atrasados e encerrados

**Como** dono, **quero** ver meus empréstimos por situação, **para** saber o que cobrar.  
**Regras relacionadas:** RN39, RN40, RN41, RN85, RN86, RN87, RN91, RN143, RN148, RN149, RN156.

- **Cenário: lista Ativos**  
  Dado três empréstimos Ativos com vencimentos 06/11/2026, 02/11/2026 e 28/10/2026, hoje 30/10/2026  
  Quando abro Ativos  
  Então vejo os três, na ordem 28/10 (destacado "Atrasado 2 dias"), 02/11, 06/11
- **Cenário: lista Atrasados**  
  Dado três empréstimos Ativos com vencimentos 06/11/2026, 02/11/2026 e 28/10/2026, hoje 30/10/2026  
  Quando abro Atrasados  
  Então vejo apenas o de 28/10/2026 com "2 dias de atraso"
- **Cenário limite: vira atrasado à meia-noite**  
  Dado vencimento 30/10/2026  
  Quando são 30/10/2026 23:59 e depois 31/10/2026 00:00  
  Então o empréstimo não está em Atrasados às 23:59 e está em Atrasados às 00:00
- **Cenário: Encerrados**  
  Dado hoje 07/12/2026, um Devolvido registrado em 10/11/2026 com retorno nessa data e vencimento 06/11/2026, um Perdido registrado em 06/12/2026 e um Cancelado registrado em 01/11/2026  
  Quando abro Encerrados  
  Então vejo na ordem 06/12 (Perdido), 10/11 (Devolvido com atraso 4 dias), 01/11 (Cancelado)
- **Cenário: lista vazia**  
  Dado que não há empréstimos atrasados  
  Quando abro Atrasados  
  Então vejo "Nenhum empréstimo atrasado"

- **Cenário: desempate determinístico**  
  Dado que E001 e E002 têm vencimento 06/11/2026 e estão Ativos  
  Quando abro Ativos  
  Então E001 aparece antes de E002 por ID crescente
- **Cenário: perda sem falsa classificação**  
  Dado que E001 está Perdido e E002 Cancelado  
  Quando abro Encerrados  
  Então ambos aparecem por estado, sem classificação no prazo/com atraso nem diasAtraso

### HU14 — Receber avisos de prazo e atraso

**Como** dono, **quero** ser avisado no painel sobre prazos e atrasos, **para** agir a tempo.  
**Regras relacionadas:** RN44, RN45, RN46, RN47, RN48, RN49, RN50, RN112, RN114, RN145, RN146, RN147, RN148, RN151, RN155.

- **Cenário: prazo próximo**  
  Dado empréstimo registrado em 30/10/2026 com vencimento 06/11/2026  
  Quando são 04/11/2026 09:00  
  Então o painel recebe "Prazo próximo: Furadeira Bosch vence em 06/11/2026" não lido e o contador mostra 1
- **Cenário: vence hoje**  
  Dado empréstimo Ativo registrado em 30/10/2026 com vencimento 06/11/2026, sem aviso desse dia  
  Quando são 06/11/2026 09:00  
  Então o painel recebe "Vence hoje: Furadeira Bosch"
- **Cenário: atraso diário**  
  Dado empréstimo Ativo registrado em 30/10/2026 com vencimento 06/11/2026, sem avisos dos dias testados  
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

- **Cenário: falha atravessa a meia-noite**  
  Dado que o sistema ficou indisponível em 07/11/2026 às 09:00 e voltou em 08/11/2026 às 10:00, com empréstimo vencido em 06/11/2026 ainda Ativo  
  Quando o gerador retoma  
  Então gera somente Atrasado de 08/11/2026, com 2 dias; não repõe o aviso de 07/11/2026
- **Cenário: encerramento antes da recuperação do serviço**  
  Dado que aviso de 07/11/2026 às 09:00 não foi gerado, o empréstimo foi devolvido às 09:20 e o gerador voltou às 09:40  
  Quando o gerador processa o dia  
  Então nenhum aviso é criado para o empréstimo encerrado
- **Cenário: registro exatamente às nove**  
  Dado que empréstimo com saída 28/10/2026 e vencimento 30/10/2026 é confirmado em 30/10/2026 às 09:00:00,000  
  Quando o gerador processa esse dia depois da confirmação  
  Então gera um aviso Vence hoje, respeitando a chave empréstimo+dia

### HU15 — Consultar histórico por objeto e por amigo

**Como** dono, **quero** ver tudo que aconteceu com um objeto ou com um amigo, **para** decidir se empresto de novo.  
**Regras relacionadas:** RN88, RN89, RN90, RN91, RN138, RN143, RN144, RN151.

- **Cenário: histórico do objeto**  
  Dado hoje 21/11/2026, "Furadeira Bosch" com empréstimo Devolvido (Ana, saída 30/10/2026, vencimento 06/11/2026 e retorno 05/11/2026) e outro Ativo (Bruno, saída 20/11/2026, vencimento 27/11/2026)  
  Quando abro o histórico do objeto  
  Então vejo primeiro o de Bruno (Ativo, em dia) e depois o de Ana (Devolvido no prazo), cada um com seus eventos
- **Cenário: histórico do contato com objeto arquivado**  
  Dado que "Ana Souza" devolveu a "Furadeira Bosch" e o objeto foi arquivado  
  Quando abro o histórico de Ana  
  Então o empréstimo da furadeira aparece com a marca "objeto arquivado"
- **Cenário: histórico com contato anonimizado**  
  Dado empréstimo de contato anonimizado  
  Quando abro o histórico do objeto  
  Então o tomador aparece como "Contato removido" e os fatos estruturados dos eventos permanecem e os textos pessoais aparecem saneados
- **Cenário: histórico mostra dano e retificação**  
  Dado hoje 16/11/2026, devolução gravada em 10/11/2026 com dano "grave", retificada para "leve" em 15/11/2026  
  Quando abro o histórico  
  Então vejo "Dano: leve (retificado de grave em 15/11/2026)"
- **Cenário: sem histórico**  
  Dado objeto nunca emprestado  
  Quando abro o histórico  
  Então vejo "Nenhum empréstimo registrado"

- **Cenário: ordem dos eventos**  
  Dado que dois eventos E001 e E002 têm o mesmo instante de gravação e um terceiro E003 foi gravado depois  
  Quando abro o histórico  
  Então vejo E001, E002 e E003 nessa ordem, com datas efetivas exibidas separadamente
- **Cenário: contato sem empréstimos removido**  
  Dado que um contato nunca foi tomador e teve dados removidos  
  Quando abro seu histórico  
  Então vejo o evento de remoção ligado ao contato e nenhum empréstimo fictício

### HU16 — Arquivar, desarquivar e excluir objeto ou contato

**Como** dono, **quero** tirar de uso objetos e amigos, **para** manter as listas limpas sem perder histórico.  
**Regras relacionadas:** RN16, RN17, RN18, RN19, RN25, RN26, RN127, RN133, RN150, RN152, RN153.

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
  Então o objeto volta a "Disponível" se não houver impedimentos, ou "Indisponível" se houver causa manual ou automática preservada
- **Cenário: contato arquivado**  
  Dado "Ana Souza" arquivada  
  Quando tento registrar empréstimo para ela  
  Então vejo "Contato arquivado não pode receber empréstimos"
- **Cenário: editar estado de objeto emprestado**  
  Dado objeto Emprestado  
  Quando tento alterar o estado de conservação  
  Então vejo "Objeto emprestado não pode ter o estado alterado" e o nome continua editável

- **Cenário: contato com histórico**  
  Dado que contato possui um empréstimo Cancelado e nenhum Ativo  
  Quando tento excluir e depois escolho arquivar  
  Então a exclusão é recusada com "Contato com histórico só pode ser arquivado" e o arquivamento é aceito
- **Cenário: impedimento automático preservado**  
  Dado que objeto com dano grave foi arquivado sem liberação da causa  
  Quando desarquivo o objeto  
  Então ele fica Indisponível por dano grave

### HU17 — Remover dados pessoais do contato

**Como** dono, **quero** atender ao pedido de remoção de dados de um amigo, **para** respeitar sua privacidade sem apagar o histórico de empréstimos.  
**Regras relacionadas:** RN24, RN90, RN92, RN93, RN94, RN95, RN96, RN133, RN134, RN135, RN136, RN137, RN138, RN139.

- **Cenário: anonimização válida**  
  Dado "Ana Souza" com telefone "85999990000", observação "Vizinha", sem empréstimo Ativo  
  Quando aciono "Remover dados pessoais", reviso os nomes e fotos dos objetos associados e confirmo  
  Então o contato passa a "Contato removido", telefone e observação ficam vazios, fica Arquivado, os fatos estruturados permanecem, textos e fotos pessoais são saneados e o histórico do contato registra data e hora da remoção
- **Cenário: empréstimo ativo**  
  Dado "Ana Souza" com empréstimo Ativo  
  Quando aciono "Remover dados pessoais"  
  Então vejo "Não é possível remover dados pessoais: há empréstimo ativo" e nada muda
- **Cenário: irreversível**  
  Dado contato anonimizado  
  Quando abro o contato  
  Então não existe opção de editar, desarquivar ou restaurar dados
- **Cenário: finalidade visível**  
  Dado que estou autenticado na conta do dono  
  Quando abro "Privacidade"  
  Então vejo quais dados são guardados (nome, telefone, observação, bloqueio e motivo, datas), a finalidade e a meta do produto de resposta em até 15 dias, sem apresentá-la como prazo legal universal de eliminação

- **Cenário: dados pessoais nos eventos**  
  Dado que contato sem Ativos tem observação de empréstimo "Ana mora na Rua Exemplo" e foto de dano, com cópias históricas  
  Quando confirmo a remoção após revisar os objetos relacionados  
  Então nome vira Contato removido, textos e fotos são saneados inclusive nas cópias, níveis e datas permanecem e um único evento do contato registra a remoção
- **Cenário: tentativa de reidentificação**  
  Dado que um contato foi anonimizado  
  Quando tento editar seu nome, desarquivá-lo ou anexar observação ao empréstimo  
  Então as ações são recusadas com "Dados pessoais removidos: ação textual ou alteração de contato indisponível"
- **Cenário: remoção repetida**  
  Dado que contato já está anonimizado  
  Quando reenvio a confirmação original da remoção  
  Então recebo o resultado anterior sem evento adicional nem restauração de conteúdo

### HU18 — Cadastrar conta, entrar e sair

**Como** dono, **quero** acessar uma conta individual, **para** manter meus empréstimos separados dos de outras pessoas.  
**Regras relacionadas:** RN01, RN02, RN03, RN97, RN98, RN99, RN100, RN101, RN102, RN103, RN104, RN110, RN111, RN112, RN113, RN114, RN115, RN116.

- **Cenário: cadastro mínimo válido**  
  Dado que não existe a conta dono@example.test e estou na tela de cadastro  
  Quando informo " Dono@example.test ", senha "Exemplo123"  
  Então a conta é criada como dono@example.test, sem objetos ou contatos, e a tela de login permite entrar com essas credenciais
- **Cenário: e-mail duplicado após normalização**  
  Dado que dono@example.test já existe  
  Quando tento cadastrar "DONO@example.test" com senha válida  
  Então vejo "E-mail já cadastrado" e nenhuma segunda conta é criada
- **Cenário: limites de senha**  
  Dado um e-mail novo válido  
  Quando cadastro senha com 8 ou 128 caracteres, em cenários independentes  
  Então ambos são aceitos; senhas com 7 ou 129 são recusadas com "Senha deve ter de 8 a 128 caracteres"
- **Cenário: login e isolamento**  
  Dado contas A e B e um objeto pertencente a B  
  Quando entro corretamente em A e tento consultar ou alterar o ID do objeto de B  
  Então vejo "Registro não encontrado" e não recebo os dados de B nem altero registros
- **Cenário: credenciais inválidas**  
  Dado a tela de login  
  Quando uso e-mail inexistente ou senha incorreta, em cenários independentes  
  Então ambos recebem "E-mail ou senha inválidos" e nenhuma sessão autenticada é criada
- **Cenário: sessão vencida**  
  Dado que a última requisição autenticada aceita ocorreu em 01/11/2026 às 10:00  
  Quando tento acessar em 01/12/2026 às 10:00, sem uso intermediário  
  Então a sessão expirou e vejo "Entre para continuar"
- **Cenário: sair**  
  Dado uma sessão válida  
  Quando aciono Sair e tento reutilizar aquela sessão  
  Então volto ao login, o acesso autenticado é recusado e os registros da conta permanecem

### HU19 — Excluir a conta do dono

**Como** dono, **quero** excluir minha conta e todos os seus dados, **para** encerrar seu uso de forma irreversível.  
**Regras relacionadas:** RN90, RN112, RN115, RN116, RN139, RN140, RN141, RN142.

- **Cenário: exclusão com empréstimos ativos**  
  Dado conta autenticada com 2 empréstimos Ativos, objetos, contatos, fotos e histórico  
  Quando aciono Excluir conta, leio o aviso sobre os 2 Ativos, informo a senha atual correta e confirmo  
  Então todos os dados da base ativa e arquivos da conta são removidos, todas as sessões são revogadas e vejo "Conta excluída"; backups seguem RN139
- **Cenário: senha incorreta**  
  Dado conta autenticada  
  Quando confirmo exclusão com senha errada  
  Então vejo "Senha atual incorreta" e nenhum dado é removido
- **Cenário: desistência**  
  Dado a tela de confirmação de exclusão e senha correta  
  Quando aciono Voltar sem confirmar  
  Então conta, sessões e registros permanecem
- **Cenário: falha antes de confirmar a transação**  
  Dado um pedido válido de exclusão e falha de gravação antes da confirmação no servidor  
  Quando a operação falha  
  Então nenhum dado de negócio é parcialmente removido e vejo "Não foi possível concluir. Tente novamente"
- **Cenário: backup**  
  Dado exclusão confirmada em 03/11/2026 às 12:00 e cópia anterior em backup  
  Quando se verifica o armazenamento em 03/12/2026 às 12:00 ou se restaura cópia antes disso  
  Então a cópia foi expurgada até esse limite e nenhuma restauração torna os dados excluídos acessíveis

### HU20 — Editar cadastros e controlar disponibilidade

**Como** dono, **quero** atualizar meus cadastros e sua disponibilidade, **para** emprestar somente o que posso entregar.  
**Regras relacionadas:** RN07, RN11, RN12, RN13, RN14, RN15, RN20, RN21, RN22, RN23, RN24, RN27, RN121, RN122, RN126, RN127, RN128, RN129, RN150, RN151, RN152, RN153, RN154, RN155, RN159.

- **Cenário: edição do nome preserva referências**  
  Dado furadeira Emprestada e contato Ana Souza com histórico  
  Quando altero o nome do objeto para Furadeira azul e do contato para Ana S.  
  Então os históricos exibem os nomes atuais pelos mesmos IDs e nenhum empréstimo é reatribuído
- **Cenário: conservação bloqueada durante empréstimo**  
  Dado objeto Emprestado com conservação bom  
  Quando tento editar a conservação para regular  
  Então vejo "Objeto emprestado não pode ter o estado alterado" e o valor continua bom
- **Cenário: indisponibilidade manual**  
  Dado objeto Disponível sem empréstimo Ativo  
  Quando marco Indisponível com motivo Revisão técnica  
  Então novos empréstimos são recusados com "Objeto indisponível"; desmarcar o motivo o torna Disponível se não houver outra causa
- **Cenário: liberação de dano**  
  Dado objeto Indisponível somente por dano inutilizado em empréstimo encerrado  
  Quando confirmo a liberação dessa causa  
  Então o objeto fica Disponível e o dano continua no histórico
- **Cenário: perda não pode ser liberada manualmente**  
  Dado objeto Indisponível por perdido ou incompleto  
  Quando tento liberar a causa manualmente  
  Então vejo "Registre a recuperação completa para liberar o objeto" e ele continua Indisponível
- **Cenário: edição do contato bloqueado**  
  Dado contato Bloqueado, não anonimizado, com telefone antigo  
  Quando salvo telefone válido de 11 dígitos  
  Então o telefone muda e o bloqueio permanece

## 7. Estados do empréstimo

### 7.1 Estados persistidos e classificações derivadas

Estados persistidos: **Ativo**, **Devolvido**, **Perdido**, **Cancelado**. Em Ativo, **em dia/atrasado** depende do relógio; **parcial** depende de recebimentos existentes. Em Devolvido, **no prazo/com atraso** depende da data efetiva da devolução e vencimento atual. Em Perdido, **recuperado** indica a existência de recuperação; o estado não muda. Cancelado não tem classificação de atraso. Não existe reabertura. RN35, RN39–RN41, RN59, RN60, RN68, RN75, RN77, RN78, RN80, RN149 e RN156 definem esses resultados.

### 7.2 Transições do empréstimo

Todas as ações exigem conta correta, versão válida, campos válidos e confirmação não duplicada (RN110–RN116). Quando houver várias restrições, vale a ordem de RN110. A tabela fornece as permissões de estado; as guardas adicionais continuam obrigatórias. Recusa conserva o estado anterior e não gera evento. "Recuperado" abaixo é qualificação de Perdido, não novo estado.

| Estado atual | Evento | Condição adicional | Próximo estado / resultado | Regra |
|---|---|---|---|---|
| Inexistente | Registro | Objeto Disponível, contato apto, datas válidas, cronologia | Ativo | RN28–RN35, RN119 |
| Ativo em dia | Renovação | Menos de 2 renovações; 1–7 dias; admite parcial e contato bloqueado | Ativo com novo vencimento | RN51–RN57 |
| Ativo atrasado | Renovação | Nenhuma | Recusa: Empréstimo atrasado não pode ser renovado | RN54 |
| Ativo em dia | Renovação | Já houve 2 renovações | Recusa: Limite de 2 renovações atingido | RN52 |
| Ativo | Devolução integral | Data, conservação e avaliação válidas; todos componentes recebidos ao concluir | Devolvido | RN58–RN64, RN118, RN124 |
| Ativo de conjunto | Devolução parcial | Pelo menos um novo recebido e um faltante | Ativo parcial | RN67–RN69, RN118, RN123 |
| Ativo de conjunto | Recebimento final | Completa o conjunto e satisfaz validações da integral | Devolvido | RN70, RN124 |
| Ativo | Perda | Pelo menos 30 dias civis de atraso; motivo válido | Perdido | RN71, RN73–RN75 |
| Ativo | Perda | Menos de 30 dias de atraso, inclusive em dia | Recusa: Perda só pode ser registrada após 30 dias de atraso | RN73, RN76 |
| Ativo | Cancelamento | Sem renovação/parcial; motivo; confirmação de ausência de entrega real | Cancelado | RN80, RN81 |
| Ativo | Cancelamento | Com renovação ou parcial | Recusa: Empréstimo com renovação ou devolução parcial não pode ser cancelado | RN80 |
| Ativo | Recuperação, Contestação ou Retificação | Nenhuma | Recusa: Ação indisponível para empréstimo ativo | RN65, RN66, RN78 |
| Devolvido | Contestação | Avaliação elegível; nenhuma contestação; contato não anonimizado | Devolvido, novo evento | RN65, RN132 |
| Devolvido | Retificação | Prazo inclusivo de 7 dias; nenhuma anterior; novo nível distinto e válido | Devolvido, avaliação corrigida | RN66, RN129–RN131, RN137 |
| Devolvido | Recuperação, Renovação, Devolução, Perda ou Cancelamento | Nenhuma | Recusa: Este empréstimo já está encerrado | RN77, RN82 |
| Perdido não recuperado | Recuperação | Data válida; objeto/conjunto completo; conservação e dano válidos | Perdido recuperado | RN78, RN125, RN127, RN137 |
| Perdido não recuperado | Contestação ou Retificação | Não há avaliação | Recusa: Não há avaliação de dano registrada | RN65, RN66, RN132 |
| Perdido recuperado | Contestação | Como Devolvido; contato não anonimizado | Perdido recuperado, novo evento | RN65, RN132 |
| Perdido recuperado | Retificação | Como Devolvido, contando da gravação da recuperação | Perdido recuperado, avaliação corrigida | RN66, RN129–RN131, RN137 |
| Perdido recuperado | Recuperação | Já há uma recuperação | Recusa: Recuperação já registrada | RN78 |
| Perdido | Renovação, Devolução integral/parcial, Perda ou Cancelamento | Nenhuma | Recusa: Este empréstimo já está encerrado | RN77, RN82 |
| Cancelado | Qualquer ação de empréstimo, exceto Observação | Nenhuma | Recusa: Este empréstimo já está encerrado | RN81, RN82 |
| Qualquer existente | Observação | Texto válido; contato não anonimizado | Mesmo estado, novo evento | RN84, RN137 |
| Qualquer existente | Saneamento por remoção do contato | Contato sem Ativos e revisão confirmada | Mesmo estado, conteúdo pessoal removido; evento no contato | RN93, RN134–RN138 |
| Qualquer existente | Exclusão da conta | Senha e confirmação corretas | Registro removido com a conta | RN140, RN141 |

Parcial em objeto simples é recusada com "Objeto sem componentes não admite devolução parcial" mesmo se uma chamada for enviada sem opção na interface (RN72). Toda combinação de ação e estado não autorizada nesta tabela é recusada: para Ativo, "Ação indisponível para empréstimo ativo"; para encerrado, "Este empréstimo já está encerrado". Permissões de retificação/contestação/recuperação são as exceções explícitas. Uma confirmação já processada retorna seu resultado antes de ser tratada como nova transição (RN112).

### 7.3 Validações e mensagens de recusa

As mensagens dos cenários são literais. Para cada validação sem mensagem específica no cenário, aplicar o catálogo abaixo. A interface identifica o campo usando seu rótulo; no padrão com chaves, substituir apenas pelo rótulo e limite declarados. Não gravar nem truncar valor inválido.

| Validação | Mensagem |
|---|---|
| Nome de objeto/contato ausente | Informe o nome do objeto / Informe o nome do contato |
| Nome fora de 1–80 | O nome deve ter de 1 a 80 caracteres |
| Categoria ausente ou fora da enumeração | Selecione uma categoria |
| Conservação ausente ou inválida | Informe o estado de conservação |
| Conjunto fora de 2–50 | Um conjunto precisa de 2 a 50 componentes |
| Componente sem nome | Informe o nome de todos os componentes |
| Telefone inválido | Telefone deve ter 10 ou 11 dígitos |
| Foto inválida | A foto deve ser JPEG ou PNG com até 5 MB |
| Campo obrigatório genérico | Informe {rótulo do campo} |
| Texto maior que limite | {Rótulo do campo} deve ter no máximo {limite} caracteres |
| Data inexistente, fora do domínio ou cálculo excedente | Informe uma data válida |
| Saída futura | A data de saída não pode ser futura |
| Vencimento fora do intervalo | O vencimento deve ser de 1 a 365 dias após a saída |
| Renovação não inteira ou fora de 1–7 | A renovação deve ser de 1 a 7 dias |
| Devolução anterior à saída ou futura | A data de devolução deve estar entre a saída e hoje |
| Devolução/parcial antes de recebimento ou renovação | Data anterior ao último recebimento ou renovação |
| Saída sobrepõe histórico | Saída anterior ao último retorno do objeto |
| Dano ausente ou fora da enumeração | Informe o nível de dano |
| Dano não nenhum sem descrição | Descreva o dano |
| Inutilizado com conservação diferente de ruim | Objeto inutilizado deve ter estado 'ruim' |
| Contestação já feita | Contestação já registrada |
| Sem dano original/atual | Não há dano para contestar |
| Retificação após limite | Prazo de retificação encerrado |
| Retificação já feita | Dano já retificado |
| Mesmo nível na retificação | Informe um nível de dano diferente |
| Parcial sem novo componente | Marque ao menos um novo componente recebido |
| Desfazer recebido | Componente recebido não pode voltar a faltante |
| Perda sem motivo válido | Informe o motivo da perda |
| Recuperação fora de perda..hoje | A data de recuperação deve estar entre a perda e hoje |
| Recuperação incompleta | Recuperação exige o conjunto completo |
| Cancelamento sem motivo | Informe o motivo do cancelamento |
| Ausência da confirmação de não entrega | Cancelamento exige confirmação de que não houve entrega real |
| Objeto Emprestado / Indisponível / Arquivado | Objeto já emprestado / Objeto indisponível / Objeto arquivado |
| Contato Arquivado / Bloqueado | Contato arquivado não pode receber empréstimos / Contato bloqueado não pode receber empréstimos |
| Arquivar com Ativo | Não é possível arquivar: há empréstimo ativo |
| Excluir objeto/contato com histórico | Objeto com histórico só pode ser arquivado / Contato com histórico só pode ser arquivado |
| Alterar conservação de Emprestado | Objeto emprestado não pode ter o estado alterado |
| Marcar/liberar indisponibilidade de Emprestado | Objeto emprestado não pode ter a disponibilidade alterada |
| Composição já usada | Composição não pode ser alterada após o primeiro empréstimo |
| Liberar perdido/incompleto manualmente | Registre a recuperação completa para liberar o objeto |
| Remover dados com Ativo | Não é possível remover dados pessoais: há empréstimo ativo |
| Editar/reutilizar contato anonimizado ou inserir texto associado | Dados pessoais removidos: ação textual ou alteração de contato indisponível |
| Revisão de objetos não confirmada na remoção | Revise os nomes e fotos dos objetos antes de confirmar |
| E-mail em formato inválido | Informe um e-mail válido |
| E-mail duplicado no cadastro | E-mail já cadastrado |
| Senha de cadastro fora de 8–128 | Senha deve ter de 8 a 128 caracteres |
| Login incorreto | E-mail ou senha inválidos |
| Sem sessão válida | Entre para continuar |
| Senha incorreta na exclusão | Senha atual incorreta |
| Versão antiga | Registro atualizado. Recarregue antes de confirmar |
| Mesma confirmação com outros dados | Confirmação já utilizada com outros dados |
| Recurso inexistente/de outra conta | Registro não encontrado |
| Falha de gravação com certeza de não confirmação | Não foi possível concluir. Tente novamente |
| Falta de resposta após envio | Resultado não confirmado. Consulte o registro antes de tentar novamente |

### 7.4 Status do objeto

Calcular, nessa ordem: se arquivado, **Arquivado**; senão, se existe Ativo, **Emprestado**; senão, se existe causa manual ou automática, **Indisponível**; senão, **Disponível**. O estado de conservação não é causa automática por si só: "ruim" pode estar Disponível se o dono liberou dano ou se não houve dano grave. Nenhuma operação de arquivamento com Ativo é válida.

| Status/contexto | Evento | Resultado | Regra |
|---|---|---|---|
| Disponível | Registrar empréstimo | Emprestado | RN12, RN35 |
| Disponível | Marcar motivo manual | Indisponível | RN13, RN14 |
| Emprestado | Parcial / Renovação | Emprestado | RN12, RN68 |
| Emprestado | Integral | Deixa de haver Ativo; conservação recebe volta; aplica causa de dano e fórmula | RN59, RN63 |
| Emprestado | Perda | Indisponível por perdido ou incompleto | RN71, RN75 |
| Emprestado | Cancelamento válido | Sem Ativo; conservação não muda; recalcula status | RN80 |
| Emprestado | Arquivar / Excluir / editar conservação / marcar ou liberar causa | Recusa, sem mudança | RN14–RN18 |
| Sem Ativo | Arquivar / Desarquivar | Marca muda; todas as causas são preservadas | RN127, RN153 |
| Causa dano, sem Ativo | Liberação manual | Remove causa escolhida; histórico não muda; motivo manual independente continua | RN154 |
| Causa perdido/incompleto | Liberação manual | Recusa | RN126 |
| Perdido/incompleto, inclusive Arquivado | Recuperação completa | Remove causa de perda; atualiza conservação e eventual causa de dano; recalcula preservando arquivamento e motivo manual | RN78, RN125, RN127 |
| Avaliação do empréstimo mais recente | Retificação | Atualiza somente sua causa de dano, preservando motivo manual e arquivamento; não muda conservação | RN127–RN130 |
| Avaliação com empréstimo posterior | Retificação | Nenhum efeito no objeto atual | RN129 |
| Sem qualquer empréstimo | Excluir objeto | Remove objeto, componentes e foto | RN16 |

**Conteúdo dos avisos:** Prazo próximo: "Prazo próximo: {nome atual do objeto} vence em {vencimentoNaGeracao em dd/mm/aaaa}"; Vence hoje: "Vence hoje: {nome atual do objeto}"; Atrasado: "Atrasado: {nome atual do objeto}, {diasAtrasoNaGeracao} dia(s)", usando singular apenas para 1; Perda possível: "Perda possível: {nome atual do objeto}, 30 dias de atraso". Nomes atuais podem mudar por edição; vencimento e dias do fato que gerou o aviso permanecem fixos (RN151). Avisos não têm links de ação externos nem enviam mensagens ao tomador.

### 7.5 Fluxos auxiliares

Contato comum pode ser bloqueado e/ou arquivado; são marcas independentes. Arquivar exige nenhum Ativo. Desarquivar não desbloqueia. Remover dados pessoais torna o arquivamento permanente e elimina edições, sem apagar fatos estruturados. A exclusão da conta prevalece sobre toda retenção de histórico e elimina também contatos anonimizados. Conta inexistente pode ser cadastrada; conta existente pode iniciar sessão válida, encerrar uma sessão, ter sessões expiradas ou ser excluída. Após exclusão, o e-mail deixa de estar ocupado, mas um novo cadastro é uma nova conta vazia, nunca restauração.

## 8. Modelo de dados

Modelo conceitual; tipos indicam domínio, não esquema executável. Todas as entidades possuem `id` único e imutável, `criadoEm` e `atualizadoEm` (instantes do servidor). Exceto Conta, possuem `contaId` obrigatório; todas as referências devem apontar a registros da mesma conta. Registros mutáveis têm `versao` inteiro crescente, iniciando em 1 e incrementado em cada mutação aceita; eventos preservam fatos, com atualização de metadados somente no saneamento. Campos opcionais começam nulos, salvo padrão declarado. Datas efetivas têm tipo data; instantes têm data/hora com milissegundos. IDs têm representação textual canônica, ordenável de forma ordinal. A referência de causa automática liga um dano ao empréstimo correspondente; estado e status não são sinônimos.

### 8.1 Entidades

**Conta** (dono) — RN01, RN02

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| email | texto | sim | normalizado e único no sistema; validação RN97 |
| senhaHash | texto | sim | hash adaptativo com salt da senha de 8–128 caracteres; nunca senha em claro |
| sessoes | coleção técnica | sim | 0..N sessões, cada uma com identificador opaco protegido e ultimoUsoEm; revogáveis; sem conteúdo de empréstimo |

**Objeto** — RN04 a RN19

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| nome | texto | sim | 1–80 caracteres |
| categoria | enumeração | sim | uma das 8 de RN05 |
| estadoConservacao | enumeração | sim | novo, bom, regular, ruim |
| foto | arquivo | não | JPEG/PNG ≤ 5 MB; 0..1 |
| ehConjunto | booleano | sim | verdadeiro exige 2–50 Componentes; falso exige 0; estrutura imutável após primeiro empréstimo |
| indisponivelManual | booleano | sim | padrão falso; só alterável quando não Emprestado |
| indisponivelMotivoManual | texto | não | ≤ 500 |
| indisponivelAutomatico | enumeração | não | nulo, dano grave, inutilizado, perdido, incompleto |
| causaEmprestimoId | referência a Empréstimo | condicional | obrigatório se causa automática não nula; causa originada nesse empréstimo do objeto; limpo ao liberar |
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
| estadoVolta | enumeração | condicional | obrigatória se Devolvido ou recuperado; imutável após avaliação |
| avaliacaoRegistradaEm | data e hora | condicional | instante da gravação da devolução/recuperação; base do prazo da retificação |
| danoNivelOriginal | enumeração | condicional | nível informado na avaliação original, preservado para elegibilidade de contestação |
| danoNivel | enumeração | condicional | nenhum, leve, grave, inutilizado; obrigatória se Devolvido ou recuperado |
| danoDescricao | texto | condicional | 1–500 se danoNivel ≠ nenhum; nulo se nenhum; marcador se saneado |
| danoFoto | arquivo | não | JPEG/PNG ≤ 5 MB |
| danoNivelAnterior | enumeração | não | preenchido só por retificação |
| retificadoEm | data e hora | não | até fim da data civil de avaliacaoRegistradaEm + 7; no máximo um |
| contestacao | texto | não | 1–500; único |
| contestacaoEm | data e hora | não | — |
| perdaMotivo | enumeração | condicional | perda comunicada, não devolução; obrigatória se Perdido |
| perdaEm | data e hora | condicional | obrigatória se Perdido |
| perdaObservacao | texto | não | ≤ 500 |
| recuperadoEm | data | não | data civil de perdaEm ≤ valor ≤ hoje; único; retorno completo |
| recuperacaoRegistradaEm | data e hora | condicional | preenchido junto de recuperadoEm; instante da ação |
| dadosPessoaisRemovidosEm | data e hora | não | marca saneamento; impede novos textos/fotos, preserva fatos |
| cancelamentoMotivo | texto | condicional | 1–200; obrigatório se Cancelado |
| encerradoEm | data e hora | condicional | obrigatório se estado ≠ Ativo |
| classificacao (derivada) | enumeração | — | em dia/atrasado para Ativo; no prazo/com atraso para Devolvido; nula nos outros (RN156) |
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

**DevoluçãoParcialItem** — RN67 a RN71, RN124, RN125

O nome da entidade do rascunho é preservado; ela representa qualquer recebimento de componente, inclusive os registrados na integral e recuperação. Componente sem linha nessa relação permanece faltante.

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| emprestimoId | referência a Empréstimo | sim | empréstimo de objeto conjunto |
| componenteId | referência a Componente | sim | único por empréstimo |
| recebidoEm | data | sim | saída ≤ valor ≤ hoje, RN118; irreversível; inclusive integral e recuperação |
| origemEventoId | referência a Evento | sim | evento de parcial, devolução integral ou recuperação que recebeu a peça |
| observacao | texto | não | ≤ 300 |

**Evento** — RN83, RN84, RN90

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| emprestimoId | referência a Empréstimo | condicional | obrigatório para evento de empréstimo; nulo no evento de contato |
| contatoId | referência a Contato | condicional | obrigatório somente no evento de remoção de dados do contato; exatamente um alvo preenchido |
| tipo | enumeração | sim | Registro, Renovação, Devolução parcial, Devolução, Perda, Recuperado, Cancelamento, Contestação, Retificação, Observação, Remoção de dados do contato |
| ocorridoEm | data e hora | sim | instante da gravação |
| dados | estrutura tipada | sim | fatos por tipo abaixo; não copiar nome/telefone do contato nem senhas; conteúdo pessoal sujeito a RN135 |
| saneadoEm | data e hora | não | indica remoção de conteúdo pessoal; ID, tipo e instante original permanecem |
| imutável | — | — | sem edição livre; exceções RN135 e RN141 |

**Aviso** — RN44 a RN50

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| emprestimoId | referência a Empréstimo | sim | — |
| tipo | enumeração | sim | Prazo próximo, Vence hoje, Atrasado, Perda possível |
| dataReferencia | data | sim | dia civil do aviso; único por (emprestimoId, dataReferencia) |
| geradoEm | data e hora | sim | entre 09:00:00 e fim do dia dataReferencia |
| vencimentoNaGeracao | data | sim | cópia do vencimento usado no aviso |
| diasAtrasoNaGeracao | inteiro | sim | 0 para prazo próximo/vence hoje; ≥1 para atrasado; 30 para perda possível |
| lido | booleano | sim | padrão falso |

**ConfirmaçãoOperação** — RN112 a RN116

Controle técnico conceitual de repetição; não é histórico do usuário nem armazena senhas ou cópia de campos pessoais. Resultados são referências aos registros atuais e campos não pessoais; saneamento nunca retorna conteúdo pessoal anterior. Identificadores e impressão da operação não podem reconstruir os dados removidos. A retenção acompanha a conta; a exclusão da conta remove também esse controle. Repetir exclusão após revogação não recria sessão ou conta.

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| confirmacaoId | identificador opaco | sim | único por conta e confirmação; criado antes do envio |
| tipoOperacao | enumeração | sim | uma das mutações definidas nesta especificação |
| alvoId | identificador | condicional | objeto, contato, empréstimo, aviso ou conta afetados; novo alvo atribuído na criação |
| versoesEsperadas | mapa de referência para inteiro | sim | registros lidos para decidir a mutação; vazio em cadastro novo |
| assinaturaEntrada | verificador não reversível | sim | permite comparar reenvio sem guardar senha nem campos pessoais em claro |
| resultadoReferencias | estrutura | sim | IDs e resultado não pessoal; leitura atual respeita remoção de dados |
| concluidaEm | data e hora | sim | instante da confirmação junto aos efeitos de negócio |

No cadastro da conta, a unicidade do e-mail e a confirmação de criação impedem duplicação; a confirmação passa a pertencer à conta criada. Login e logout gerenciam sessões e não produzem eventos de negócio. Versões de objeto e contato são verificadas quando a ação de empréstimo depende de sua elegibilidade, evitando corrida com bloqueio, arquivamento e remoção.

**Conteúdo de Evento por tipo** (campos de negócio explicitados; referências pertencem à mesma conta):

| Tipo | Fatos registrados |
|---|---|
| Registro | objetoId, contatoId, saída, vencimento original, estadoSaida, observação opcional |
| Renovação | número, dias, vencimento anterior e novo |
| Devolução parcial | data efetiva, IDs dos novos recebidos, observação opcional |
| Devolução | data efetiva, conservação, nível de dano, descrição/foto quando permitidos, IDs recebidos nessa ação |
| Perda | motivo, observação opcional, IDs recebidos e faltantes no momento da perda |
| Recuperado | data efetiva, conservação, avaliação, IDs recuperados |
| Cancelamento | motivo e confirmação de ausência de entrega real |
| Contestação | texto e vínculo com avaliação |
| Retificação | níveis e descrições anterior/atual; marca de privacidade quando aplicável |
| Observação | texto |
| Remoção de dados do contato | contatoId, instante e ocorrência de saneamento; nenhum identificador pessoal anterior |

### 8.2 Relacionamentos e cardinalidades

- Conta 1 — 0..N Objeto, Contato, Empréstimo, Componente, Renovação, DevoluçãoParcialItem, Evento, Aviso e ConfirmaçãoOperação; todas as referências ficam na mesma conta.
- Objeto 1 — 0..50 Componente (2..50 se conjunto; 0 se não).
- Objeto 1 — N Empréstimo; no máximo 1 com estado Ativo (RN29).
- Contato 1 — N Empréstimo (sem limite de Ativos, RN27).
- Empréstimo 1 — 0..2 Renovação; 1 — 0..N DevoluçãoParcialItem (≤ número de componentes); 1 — 1..N Evento de empréstimo; 1 — 0..N Aviso.
- Contato 1 — 0..1 Evento de remoção; existe mesmo sem empréstimos. Evento tem exatamente um alvo: empréstimo ou contato.
- Objeto 1 — 0..1 causa automática, vinculada a um empréstimo daquele objeto; motivo manual independente pode coexistir.

### 8.3 Regras de integridade e privacidade do modelo

- Exclusão isolada de Objeto/Contato só nas condições RN16 e RN25; exclusão da conta elimina todas as relações e arquivos (RN141).
- Empréstimo só muda pelas ações válidas; Evento mantém seus fatos, com exceções de saneamento e exclusão da conta. Remoção não deixa arquivo de dano órfão acessível.
- estado=Ativo implica encerradoEm nulo; Devolvido exige dataDevolucao, estadoVolta, avaliação e encerradoEm; Perdido exige perdaMotivo, perdaEm e encerradoEm; Cancelado exige motivo e encerradoEm. Campos exclusivos de outros estados ficam nulos. Observações são eventos.
- Recuperação só em Perdido, uma vez; recuperadoEm, recuperacaoRegistradaEm e avaliação são preenchidos juntos. avaliacaoRegistradaEm coincide com gravação da integral ou recuperação. retificadoEm e danoNivelAnterior são preenchidos juntos, apenas uma vez.
- quantidadeRenovacoes é igual ao número de linhas Renovação; vencimento atual é original mais soma de dias. Numeros 1/2 são únicos por empréstimo e sequenciais.
- Par (emprestimoId, componenteId) é único em DevoluçãoParcialItem; componente pertence ao objeto do empréstimo. Devolvido e Perdido recuperado exigem todas as peças recebidas; Perdido sem recuperação pode conservar recebimentos parciais.
- Contato anonimizado tem nome genérico, telefone/observação/motivo nulos, arquivado verdadeiro e anonimizadoEm preenchido. Campos textuais históricos obrigatórios saneados usam marcador com metadado de privacidade; isso é exceção explícita à validação de entrada, não texto digitado pelo dono.
- Não guardar identificadores pessoais redundantes em eventos, avisos ou confirmações. Fotos e arquivos só são acessíveis à conta proprietária. Nomes/fotos de objetos associados são revisados no fluxo RN136; não há promessa de detecção automática nem de anonimização jurídica absoluta.
- Estado e evento, causas do objeto, recebimentos, versões e confirmação técnica devem manter integridade transacional (RN116). Metadados de exclusão necessários para aplicar remoções a backups guardam apenas referências técnicas sem conteúdo pessoal e são eliminados ao concluir o expurgo.
- Nenhum campo de negócio guarda geolocalização, valores monetários ou identificadores de dispositivo. Dados de sessão são somente os necessários à autenticação e expiração.

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
| 12 | Desarquivar objeto com causa manual ou automática | Continua Indisponível; arquivamento não remove a causa | RN13, RN127, RN153 |
| 13 | Telefone com letras ou 12 dígitos | Recusa com "Telefone deve ter 10 ou 11 dígitos" | RN20 |
| 14 | Bloquear contato com 3 empréstimos ativos | Bloqueio aceito; os 3 continuam Ativos e renováveis | RN23 |
| 15 | Contato bloqueado e arquivado ao mesmo tempo | Ambas as marcas coexistem; novo empréstimo recusado por "Contato arquivado..." (verificação de arquivamento precede) | RN23, RN26 |
| 16 | Mesmo contato com 10 empréstimos ativos | Aceito | RN27 |
| 17 | Registrar empréstimo de objeto Emprestado | Recusa com "Objeto já emprestado"; nenhum registro | RN29, RN82 |
| 18 | Dois registros simultâneos do mesmo objeto (duas abas) | O primeiro gravado vence; o segundo é recusado na gravação | RN29, RN82 |
| 19 | Saída em 29/02/2028 e vencimento em 28/02/2029 | 365 dias: aceito | RN32 |
| 20 | Saída em 30/10/2026 e vencimento em 30/10/2027 | 365 dias: aceito; 31/10/2027 recusado | RN32 |
| 21 | Saída com 3 anos no passado, sem histórico posterior do objeto | Aceita dentro do domínio de datas; nasce atrasado; nenhum aviso pretérito | RN30, RN43, RN46, RN119 |
| 22 | Dono em outro fuso (ex.: Lisboa) registra às 01:00 locais de 31/10 | "Hoje" é 30/10/2026 (America/Fortaleza); saída "31/10/2026" é recusada como futura | RN33, RN37, RN38 |
| 23 | Relógio do celular adiantado 2 dias | Ignorado; cálculo pelo servidor | RN37 |
| 24 | Vencimento 06/11/2026 às 23:59:59 | Em dia | RN39 |
| 25 | Vencimento 06/11/2026 às 00:00:00 de 07/11 | Atrasado, 1 dia | RN39, RN41 |
| 26 | Empréstimo atrasado há 200 dias | Continua Ativo e atrasado; nenhum bloqueio ou pontuação | RN42 |
| 27 | Renovar às 23:59:59,999 do vencimento | Aceita | RN54 |
| 28 | Renovar às 00:00:00,000 do dia seguinte | Recusa "Empréstimo atrasado não pode ser renovado" | RN54 |
| 29 | Duplo clique ou reenvio da mesma confirmação de renovação | Um único efeito e evento; segunda confirmação distinta em versão antiga é recusada | RN52, RN112–RN114 |
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
| 41 | Retificação no 7º dia civil após a gravação, às 23:59:59,999 | Aceita; às 00:00 do 8º dia recusada, independentemente da data efetiva retroativa | RN66 |
| 42 | Retificar grave para nenhum com causa já liberada, sem empréstimo posterior | Evento preserva anterior; descrição atual fica nula; status é recalculado preservando outras causas | RN66, RN127–RN130 |
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
| 53 | Cancelar Ativo atrasado sem renovação nem parcial e sem entrega real | Aceito com motivo e confirmação; não entra nas contagens de atraso | RN80, RN81 |
| 54 | Devolver duas vezes | Segunda recusada "Este empréstimo já está encerrado" | RN82 |
| 55 | Ação recusada | Nenhum evento gravado | RN83 |
| 56 | Lista Ativos com empréstimo atrasado | Aparece em Ativos (destacado) e em Atrasados | RN85, RN86 |
| 57 | Aviso Prazo próximo para prazo de 1 ou 2 dias | 1 dia: horário anterior à saída e ao registro, não gera; 2 dias com saída hoje: gera se registro confirmado até 09:00 inclusive | RN45, RN46, RN147 |
| 58 | Sistema indisponível às 09:00 | Gera quando voltar no mesmo dia, revalidando situação; não repõe dia anterior | RN47, RN146, RN147 |
| 59 | Renovação às 08:00 do dia "vencimento − 2" | Aviso "Prazo próximo" daquele dia descartado; recalculado para novo vencimento − 2 | RN48 |
| 60 | Devolução às 08:59 do dia do vencimento | Aviso "Vence hoje" não é gerado | RN49 |
| 61 | Dia 30 de atraso | Gera "Perda possível" e não gera "Atrasado" | RN45, RN47 |
| 62 | Dia 31 de atraso em diante | Volta a gerar "Atrasado" diário até encerrar | RN45 |
| 63 | Remover dados de contato bloqueado sem empréstimo ativo | Aceito; motivo pessoal é apagado; fica arquivado e imutável, com fatos estruturados preservados | RN93, RN133–RN138 |
| 64 | Anonimizar contato com empréstimo Ativo | Recusa "Não é possível remover dados pessoais: há empréstimo ativo" | RN94 |
| 65 | Histórico com contato anonimizado | Exibe Contato removido; fatos estruturados preservados e conteúdo pessoal saneado | RN88, RN93, RN134, RN135 |
| 66 | Editar nome do contato após empréstimos | Histórico mostra o novo nome | RN24 |
| 67 | Cadastrar conta com senha de 7 ou 129 caracteres | Recusa; os limites inclusivos são 8 e 128 | RN99 |
| 68 | Acesso a objeto de outra conta pela URL | Recusa; dado não exibido | RN02 |
| 69 | Nome formado só por espaços | Recusado como vazio depois de trim e NFC | RN04, RN20, RN105, RN106 |
| 70 | Arquivo GIF renomeado para .jpg | Recusado pela validação de conteúdo; limite real é 5.000.000 bytes | RN07, RN108, RN158 |
| 71 | Telefone com +55 ou máscara | Recusado; somente 10/11 dígitos ASCII | RN109 |
| 72 | Componentes com nomes repetidos | Aceitos, diferenciados por ID e posição | RN122 |
| 73 | Editar composição após primeiro empréstimo Cancelado | Recusa; composição já tem histórico | RN121 |
| 74 | Parcial repete somente componentes já recebidos | Recusa; nenhum novo recebimento/evento | RN123 |
| 75 | Integral de conjunto com parciais anteriores | Preserva datas já recebidas e preenche faltantes na data integral | RN124 |
| 76 | Devolução/parcial efetiva antes da última renovação | Recusa por cronologia; data igual é aceita | RN118 |
| 77 | Novo empréstimo com saída antes do último retorno | Recusa; data igual ao último retorno é aceita | RN119 |
| 78 | Recuperação formal de conjunto ainda incompleto | Recusa; não libera perdido/incompleto; observação permitida se não anonimizado | RN125, RN137 |
| 79 | Liberar manualmente perdido ou incompleto | Recusa até recuperação completa | RN126 |
| 80 | Recuperar objeto arquivado e com motivo manual | Mantém Arquivado e motivo manual; só remove a causa da perda | RN127 |
| 81 | Retificar dano de empréstimo com outro registrado depois | Somente histórico é corrigido; nenhuma mudança no objeto atual | RN129 |
| 82 | Retificar para nível igual ao atual | Recusa, sem consumir retificação | RN131 |
| 83 | Retificar para leve sem descrição | Recusa; se contato anonimizado, não aceita texto e aplica marcador | RN130, RN137, RN157 |
| 84 | Contestação com nível original e atual nenhum | Recusa; não há dano para contestar | RN132 |
| 85 | Remoção de contato sem empréstimos | Cria evento no contato, sem empréstimo fictício | RN138 |
| 86 | Dados pessoais em texto, foto ou cópias históricas | Saneia todas as cópias dos empréstimos vinculados e confirmações, preservando fatos | RN134, RN135 |
| 87 | Nova observação após anonimização | Recusa; não reintroduz conteúdo pessoal | RN133, RN137 |
| 88 | Recuperação/retificação após anonimização | Permite fatos estruturados; descrição usa marcador e foto é desabilitada | RN137 |
| 89 | Exclusão de conta com 2 Ativos | Permite após senha e confirmação com aviso da quantidade; apaga todos os dados | RN140, RN141 |
| 90 | Exclusão com senha errada, desistência ou falha prévia à confirmação | Não remove dados nem parte do histórico | RN142, RN116 |
| 91 | Backup contém dados excluídos | Expurgo em até 30 dias; restauração reaplica remoções antes do acesso | RN139 |
| 92 | E-mail já cadastrado em outras maiúsculas/espaços externos | Recusa duplicidade após normalização | RN97, RN98 |
| 93 | Login errado e conta inexistente | Mesma mensagem genérica, sem sessão | RN100, RN101 |
| 94 | 30 dias exatos sem uso ou sessão encerrada pelo dono | Exige novo login; os dados permanecem | RN102, RN103 |
| 95 | Mesma confirmação com conteúdo diferente | Recusa sem efeitos novos | RN113 |
| 96 | Resposta de rede perdida após gravação | Informa resultado desconhecido; consulta ou reenvio idempotente verifica sem duplicar | RN112, RN115 |
| 97 | Processo falha entre gravação do estado e evento | Não permite resultado parcial: transação toda confirma ou toda falha | RN116, RN117 |
| 98 | Avisos empatados, históricos e listas com mesma data | Ordem determinística por ID; eventos crescentes, avisos recentes primeiro | RN143–RN145 |
| 99 | Sistema volta somente no dia seguinte às 10:00 | Só avalia aviso elegível do dia atual; não gera dias anteriores | RN146, RN147 |
| 100 | Empréstimo encerrado ou renovado durante falha do gerador | Revalida estado/vencimento atuais antes de gerar; não reconstrói situação passada | RN147 |
| 101 | Tela permanece aberta à meia-noite | Reconsulta servidor e atualiza atraso; relógio do cliente não é autoridade | RN148 |
| 102 | Perdido e Cancelado nas listas | Sem classificação de devolução nem dias de atraso aplicáveis | RN149, RN156 |
| 103 | Editar conservação sem Ativo e depois consultar empréstimo antigo | Cadastro muda, estadoSaida/estadoVolta históricos não mudam | RN150 |
| 104 | Novo cadastro | Objeto sem causas/arquivamento; contato sem marcas; aviso não lido; empréstimo sem renovações | RN155 |
| 105 | Cancelar sem declarar ausência de entrega real | Recusa mesmo sem renovação ou parcial | RN80 |
| 106 | Vários campos inválidos | Lista todos os erros de campo após validações anteriores e não salva | RN110, RN111 |
| 107 | Perdido ainda não recuperado recebe contestação/retificação | Recusa porque não há avaliação de dano | RN65, RN66, RN132 |
| 108 | Tentativa direta de parcial em objeto simples | Recusa mesmo que opção não apareça na interface | RN72 |
| 109 | Data 30/02/2027 ou soma além de 31/12/9999 | Recusa data/cálculo inválido, sem ajuste automático | RN107 |
| 110 | Remover/repor foto ou nome do objeto durante empréstimo | Aceita edição do cadastro; não altera foto da avaliação nem conservação histórica | RN15, RN151, RN159 |
| 111 | Dia 30 de atraso antes das 09:00 | Perda já pode ser marcada desde 00:00; não depende do aviso | RN73 |
| 112 | Aviso já lido após encerramento | Permanece consultável sem entrar no contador de não lidos | RN49, RN50, RN145 |


### 9.1 Rastreabilidade por história

A coluna de casos relaciona os casos da tabela acima que compartilham regras com a história; os cenários da própria HU complementam os limites. As guardas de estado da seção 7 e as validações transversais continuam aplicáveis. Toda RN01–RN159 está vinculada a pelo menos uma HU.

| História | Regras relacionadas | Estados/fluxo | Casos de borda relacionados |
|---|---|---|---|
| HU01 | RN04, RN05, RN06, RN07, RN08, RN09, RN11, RN105, RN106, RN108, RN111, RN155 | Cadastro de objeto | 1, 2, 3, 6, 7, 69, 70, 104, 106 |
| HU02 | RN04, RN10, RN67, RN72, RN121, RN122, RN123, RN124 | Composição | 1, 4, 5, 44, 69, 72, 73, 74, 75, 108 |
| HU03 | RN20, RN21, RN92, RN95, RN105, RN106, RN109, RN111, RN155 | Cadastro de contato | 13, 69, 71, 104, 106 |
| HU04 | RN22, RN23, RN42, RN55, RN133 | Bloqueio; Ativo preservado | 14, 15, 26, 31, 32, 63, 87 |
| HU05 | RN27, RN28, RN29, RN30, RN31, RN32, RN33, RN34, RN35, RN36, RN37, RN38, RN43, RN46, RN97, RN107, RN110, RN111, RN112, RN113, RN114, RN116, RN117, RN119, RN155 | Inexistente → Ativo | 16, 17, 18, 19, 20, 21, 22, 23, 29, 57, 77, 90, 92, 95, 96, 97, 104, 106, 109 |
| HU06 | RN48, RN51, RN52, RN53, RN54, RN55, RN56, RN57, RN112, RN113, RN114, RN115, RN116, RN117 | Ativo → Ativo | 27, 28, 29, 30, 31, 32, 59, 90, 95, 96, 97 |
| HU07 | RN49, RN58, RN59, RN60, RN61, RN62, RN63, RN64, RN82, RN83, RN112, RN115, RN116, RN117, RN118, RN120, RN124, RN157, RN158, RN159 | Ativo → Devolvido | 17, 18, 29, 33, 34, 35, 36, 37, 38, 39, 40, 54, 55, 60, 70, 75, 76, 83, 90, 96, 97, 110, 112 |
| HU08 | RN55, RN67, RN68, RN69, RN70, RN71, RN72, RN118, RN123, RN124 | Ativo parcial → Ativo/Devolvido | 31, 32, 43, 44, 45, 48, 74, 75, 76, 108 |
| HU09 | RN65, RN66, RN90, RN128, RN129, RN130, RN131, RN132, RN137, RN157, RN159 | Devolvido/Perdido recuperado preservados | 40, 41, 42, 63, 78, 81, 82, 83, 84, 87, 88, 107, 110 |
| HU10 | RN71, RN73, RN74, RN75, RN76, RN77, RN84, RN120, RN126 | Ativo → Perdido | 46, 47, 48, 79, 111 |
| HU11 | RN62, RN63, RN77, RN78, RN79, RN125, RN127, RN137, RN157, RN158 | Perdido → Perdido recuperado | 12, 36, 37, 39, 42, 49, 50, 51, 63, 70, 78, 80, 83, 87, 88 |
| HU12 | RN36, RN49, RN80, RN81, RN82, RN83, RN120 | Ativo → Cancelado | 17, 18, 40, 52, 53, 54, 55, 60, 105, 112 |
| HU13 | RN39, RN40, RN41, RN85, RN86, RN87, RN91, RN143, RN148, RN149, RN156 | Consulta de todos os estados | 24, 25, 56, 98, 101, 102 |
| HU14 | RN44, RN45, RN46, RN47, RN48, RN49, RN50, RN112, RN114, RN145, RN146, RN147, RN148, RN151, RN155 | Ativos; avisos preservados ao encerrar | 21, 29, 57, 58, 59, 60, 61, 62, 96, 98, 99, 100, 101, 104, 110, 112 |
| HU15 | RN88, RN89, RN90, RN91, RN138, RN143, RN144, RN151 | Histórico de todos os estados | 63, 65, 85, 98, 110 |
| HU16 | RN16, RN17, RN18, RN19, RN25, RN26, RN127, RN133, RN150, RN152, RN153 | Arquivado; sem Ativo | 10, 11, 12, 15, 42, 63, 80, 87, 103 |
| HU17 | RN24, RN90, RN92, RN93, RN94, RN95, RN96, RN133, RN134, RN135, RN136, RN137, RN138, RN139 | Contato removido; empréstimos encerrados preservados | 63, 64, 65, 66, 78, 83, 85, 86, 87, 88, 91 |
| HU18 | RN01, RN02, RN03, RN97, RN98, RN99, RN100, RN101, RN102, RN103, RN104, RN110, RN111, RN112, RN113, RN114, RN115, RN116 | Conta e sessão | 29, 67, 68, 90, 92, 93, 94, 95, 96, 97, 106 |
| HU19 | RN90, RN112, RN115, RN116, RN139, RN140, RN141, RN142 | Qualquer estado → removido com conta | 29, 89, 90, 91, 96, 97 |
| HU20 | RN07, RN11, RN12, RN13, RN14, RN15, RN20, RN21, RN22, RN23, RN24, RN27, RN121, RN122, RN126, RN127, RN128, RN129, RN150, RN151, RN152, RN153, RN154, RN155, RN159 | Disponível/Emprestado/Indisponível/Arquivado | 6, 7, 8, 9, 12, 13, 14, 15, 16, 32, 39, 42, 66, 69, 70, 72, 73, 79, 80, 81, 103, 104, 110 |

## 10. Requisitos não funcionais

Requisitos de uma implementação futura; nenhum teste de aplicativo foi executado nesta atividade documental. Valores aprovados em DEC02, DEC25, DEC45, DEC47, DEC50, DEC54 e DEC57.

| ID | Requisito | Critério verificável |
|---|---|---|
| RNF01 | Plataforma e responsividade | Operar nas 2 últimas versões estáveis de Chrome, Firefox, Safari e Edge existentes na data dos testes, registrando versões. Larguras de 360 a 1920 pixels CSS sem rolagem horizontal da página; verificar pelo menos 360, 768 e 1920 px e navegação por celular/computador |
| RNF02 | Desempenho | No protocolo abaixo, percentil 95 de até 2 s para listas, 1 s para ação sem upload e 15 s para upload de foto de 5.000.000 bytes |
| RNF03 | Acessibilidade | Satisfazer todos os critérios aplicáveis dos níveis A e AA da WCAG 2.1. Verificar teclado, foco, rótulos, erros associados, contraste de texto normal ≥4,5:1 e demais critérios aplicáveis. Adotar adicionalmente botões de ação com alvo ≥44×44 pixels CSS; esse valor é uma meta extra do produto, não classificação de requisito AA |
| RNF04 | Idioma e formatos | Interface pt-BR; datas dd/mm/aaaa; horas hh:mm em 24 h. Auditoria do histórico distingue data efetiva de instante de gravação, exibindo segundos/milissegundos quando necessários para conferir limites |
| RNF05 | Tempo oficial | Datas, vencimentos, avisos e expiração baseados no servidor; calendário de negócio America/Fortaleza. Alterar relógio/fuso do cliente não altera decisões aceitas |
| RNF06 | Segurança de acesso | Tráfego exclusivamente HTTPS, senha com hash adaptativo e salt, nenhuma senha em eventos/logs de domínio/confirmações; sessões expiram após 30 dias sem uso. Testes de todas as leituras/escritas recusam IDs de outras contas sem revelar conteúdo |
| RNF07 | Privacidade | Não coletar geolocalização, agenda do telefone ou identificadores de dispositivo. Dados limitados aos campos definidos; finalidade e restrições em Privacidade; testar saneamento de texto/fotos/cópias, proibição de reidentificação por edição e expurgo de backups até 30 dias. Não afirmar anonimização absoluta |
| RNF08 | Conectividade | Sem conexão antes do envio, mostrar "Sem conexão. Tente novamente."; sem confirmação depois do envio, usar RN115. Não manter fila offline nem dados de negócio persistentes no cliente; sessão pode persistir. Identificador da confirmação pode existir em memória durante a operação; ao reabrir, consultar o estado do servidor |
| RNF09 | Avisos | Agendamento às 09:00 Fortaleza, retomada no mesmo dia e nenhuma reposição de dias anteriores. Testes de reexecução, concorrência e encerramento/renovação entre agendamento e execução não geram aviso duplicado ou inválido |
| RNF10 | Integridade e repetição | Testes de concorrência e falha induzida mantêm um Ativo por objeto, no máximo duas renovações, uma avaliação/retificação/recuperação por empréstimo, recebimentos não duplicados e ação/evento consistentes. Mesma confirmação tem um efeito; versões antigas não sobrescrevem novas |

**Protocolo de RNF02:** base com 500 empréstimos por conta testada e 10 usuários simultâneos; rede limitada a 10 Mbps de download e 5 Mbps de upload, latência de ida e volta de 100 ms. Executar 30 medições por operação, do acionamento à lista utilizável ou confirmação visível; ordenar as 30 durações e usar a 29ª como percentil 95 (posto mais próximo). Registrar navegador, dispositivo, hardware do servidor, massa de dados, condições de cache e medições. Ação sem upload exclui transferência de arquivo; upload mede seleção confirmada até confirmação de armazenamento. O limite de 500 é massa de teste, não limite de cadastro. Testes funcionais continuam obrigatórios acima de 500, sem meta de tempo adicional nesta v1.

**Referência técnica:** [WCAG 2.1 — W3C](https://www.w3.org/TR/WCAG21/) e [critério 2.5.5, Target Size, nível AAA](https://www.w3.org/WAI/WCAG21/Understanding/target-size). Consulta documental em 03/11/2026; a meta de 44×44 é escolha aprovada pelo aluno.

## 11. Decisões e premissas

O aluno **Mateus Pinheiro Silva Rodrigues** assumiu expressamente as decisões e justificativas: DEC01–DEC26 em 30/10/2026, conforme transcrição preservada em prompts.md; DEC27–DEC57 em 03/11/2026, incluindo todos os detalhamentos do lote de revisão, sem alterações. DEC01–DEC49 provêm do processo documentado com Claude Code; as revisões e DEC50–DEC57 foram apresentadas nesta sessão do Codex/OpenAI e aprovadas pelo aluno. A origem anterior a esse histórico não foi verificada. Nenhuma destas escolhas numéricas é atribuída ao professor.

O professor exige cobertura das funcionalidades e perguntas, precisão, rastreabilidade, justificativas e registro de uso de IA. As alternativas escolhidas aqui são decisões do aluno, apoiado pela IA. A estrutura de doze seções e as quatro exclusões da seção 2.2 vêm do enunciado.

| ID | Decisão | Alternativas consideradas | Justificativa | Status |
|---|---|---|---|---|
| DEC01 | Só o dono usa a conta; o tomador é um contato cadastrado e não precisa instalar o app | Ambos com contas; tomador convidado | Escopo mais simples, sem autenticação de terceiros | Aprovada em 30/10/2026 |
| DEC02 | Aplicativo web responsivo acessível em celular e computador | Apenas Android/iOS; somente desktop | Documenta uma plataforma sem exigir implementação | Aprovada em 30/10/2026 |
| DEC03 | Cadastro do objeto exige nome, categoria e estado; foto opcional; categorias pré-definidas com opção "Outros" | Categorias livremente cadastráveis | Reduz ambiguidades no cadastro | Aprovada em 30/10/2026 |
| DEC04 | Cada objeto físico tem identificador individual e não pode ter dois empréstimos abertos simultaneamente | Unidades indistintas por quantidade | Evita empréstimos conflitantes | Aprovada em 30/10/2026 |
| DEC05 | Conjuntos são um objeto com componentes listados; devolução só é completa com todos os componentes | Cada peça como objeto separado | Preserva histórico do conjunto e define a devolução parcial | Aprovada em 30/10/2026 |
| DEC06 | Cada contato exige nome e aceita telefone opcional; nenhum dado de tomador é necessário para login | Exigir telefone/e-mail | Minimização de dados pessoais | Aprovada em 30/10/2026 |
| DEC07 | Prazo informado por data de devolução no calendário, sem prazo padrão por categoria; sugestão inicial de 7 dias | Duração numérica; prazo obrigatório por categoria | Facilita previsão sem impedir personalização | Aprovada em 30/10/2026 |
| DEC08 | Prazo obrigatório, de 1 a 365 dias corridos após a data de saída | Mesmo dia; sem prazo; ilimitado | Define limites de validação | Aprovada em 30/10/2026 |
| DEC09 | Data de saída não pode ser futura; vencimento às 23:59:59,999 no fuso America/Fortaleza | UTC; fuso do dispositivo; começo do dia | Evita interpretação contraditória do vencimento | Aprovada em 30/10/2026 |
| DEC10 | Atraso começa às 00:00 do dia seguinte ao vencimento, sem tolerância | Tolerância de 24/48 horas | Definição determinística | Aprovada em 30/10/2026 |
| DEC11 | Atraso gera alerta; não cria multa, pontuação nem bloqueio automático | Bloqueio/score automáticos | Não amplia o escopo desnecessariamente | Aprovada em 30/10/2026 |
| DEC12 | Avisos ao dono no painel: 2 dias antes, no vencimento e diariamente às 9h enquanto atrasado; encerram após devolução/perda; sem alertas de datas que já passaram | E-mail, push, aviso semanal | Frequência definida e simples de verificar | Aprovada em 30/10/2026 |
| DEC13 | Somente o dono renova, no máximo 2 vezes por empréstimo, com até 7 dias corridos por renovação | Tomador solicita; renovações ilimitadas | Impede renovações indefinidas | Aprovada em 30/10/2026 |
| DEC14 | Renovação soma dias à data atual de vencimento; empréstimos atrasados não podem ser renovados | Contar a partir de hoje; permitir atraso | Não apaga atraso nem encurta prazo | Aprovada em 30/10/2026 |
| DEC15 | Estado na saída e no retorno na mesma escala: novo, bom, regular, ruim | Texto livre; duas escalas | Permite comparação objetiva | Aprovada em 30/10/2026 |
| DEC16 | Dano classificado pelo dono como nenhum, leve, grave ou inutilizado, com descrição e foto opcional | Classificação automática ou pelo tomador | Autoridade e níveis explícitos | Aprovada em 30/10/2026 |
| DEC17 | Dano gera só registro e indisponibilidade se grave/inutilizado; sem cobrança, reparo ou reposição no app | Acordos e indenizações no sistema | Respeita as exclusões oficiais | Aprovada em 30/10/2026 |
| DEC18 | Tomador pede retificação por meio externo; dono registra nota de contestação no histórico, sem chat nem alteração automática | Contestação no app com conta de tomador | Registra contestação sem expandir para chat | Aprovada em 30/10/2026 |
| DEC19 | Devolução parcial não encerra o empréstimo; registra recebidos e faltantes até devolução integral ou perda | Encerrar com pendência | Evita declarar devolvido um conjunto incompleto | Aprovada em 30/10/2026 |
| DEC20 | Dono pode marcar perda/não devolução manualmente após 30 dias completos de atraso; antes disso permanece Ativo, em dia ou atrasado conforme o vencimento | Marcação automática; qualquer momento | Garante critério objetivo | Aprovada em 30/10/2026 |
| DEC21 | Objeto perdido que reaparece: registrar recuperação no histórico; não reabrir empréstimo; preservar o registro de perda | Reabrir o empréstimo perdido | Histórico sem apagamento de eventos | Aprovada em 30/10/2026 |
| DEC22 | Contatos e objetos com histórico não são excluídos fisicamente, apenas arquivados; arquivados não entram em novos empréstimos | Exclusão definitiva com cascata | Mantém rastreabilidade dos empréstimos | Aprovada em 30/10/2026 |
| DEC23 | Dono pode bloquear/desbloquear manualmente um amigo; bloqueado não recebe novos empréstimos, mas históricos e empréstimos abertos permanecem | Bloqueio automático por atraso | Critério simples e controle do dono | Aprovada em 30/10/2026 |
| DEC24 | Dados do contato usados só para identificação e acompanhamento, com acesso apenas do dono; remoção apaga identificadores e preserva fatos estruturados, com saneamento detalhado por DEC57 e sem garantia absoluta de não reidentificação | Reter indefinidamente; excluir todos os registros | Minimização, finalidade, direitos LGPD e preservação do histórico | Aprovada em 30/10/2026; detalhada em 03/11 por DEC46/DEC57 |
| DEC25 | Vencimento, prazos e avisos calculados pela data/hora oficial no fuso escolhido, sem depender do relógio local | Usar só horário do dispositivo | Consistência da regra temporal | Aprovada em 30/10/2026 |
| DEC26 | Ação inválida é recusada sem alterar registros, com mensagem específica; histórico só para ações aceitas | Aceitar sobreposição; duplicar eventos | Transições testáveis e integridade | Aprovada em 30/10/2026 |
| DEC27 | Estados Ativo, Devolvido, Perdido e Cancelado; atrasado calculado pela data | Persistir Atrasado e atualizar por rotina | O filtro acompanha o relógio, sem estado desatualizado | Aprovada em 03/11/2026 |
| DEC28 | Ativos por vencimento crescente; Atrasados por dias de atraso decrescentes; Encerrados por instante de encerramento decrescente | Excluir atrasados de Ativos; outra ordenação | Define exatamente o conteúdo de cada lista | Aprovada em 03/11/2026 |
| DEC29 | Objeto Disponível, Emprestado, Indisponível ou Arquivado; precedência Arquivado > Emprestado > Indisponível > Disponível | Arquivar automaticamente todo objeto danificado | Fecha o ciclo de disponibilidade | Aprovada em 03/11/2026 |
| DEC30 | Copiar conservação atual na saída, permitir ajuste e observação de até 300 caracteres; retorno atualiza conservação | Congelar conservação do cadastro | Permite comparação objetiva | Aprovada em 03/11/2026 |
| DEC31 | Dano diferente de nenhum exige descrição de 1–500 caracteres; inutilizado exige conservação ruim; uma contestação de 1–500 por avaliação; contestação exige dano original ou atual não nenhum | Descrição facultativa; várias contestações | Define validação e evita contestação sem dano registrado | Aprovada em 03/11/2026 |
| DEC32 | Uma retificação por avaliação, até o fim do 7º dia civil após a gravação da devolução/recuperação; exige nível diferente e descrição válida, não altera foto/conservação, respeita empréstimos posteriores | Dano imutável; correções ilimitadas | Corrige erro sem reescrever o histórico nem admitir dano sem descrição | Aprovada em 03/11/2026 |
| DEC33 | Devolução entre saída e hoje, padrão hoje; atraso pela data efetiva de devolução | Usar sempre data de registro | Distingue fato e momento da digitação | Aprovada em 03/11/2026 |
| DEC34 | Um estado Perdido, com motivo perda comunicada ou não devolução; observação até 500 | Dois estados de perda | Diferencia causas sem duplicar ciclo de vida | Aprovada em 03/11/2026 |
| DEC35 | Uma recuperação, sem reabrir o empréstimo, com data, conservação e dano | Reabrir empréstimo perdido | Preserva a perda e evita empréstimo de conjunto incompleto | Aprovada em 03/11/2026 |
| DEC36 | Parciais acumulativas por componente, sem desfazer recebidos; prazo e avisos seguem; pode renovar em dia | Encerrar com pendência | Não declara devolvido um conjunto incompleto nem duplica parciais vazias | Aprovada em 03/11/2026 |
| DEC37 | Cancelar Ativo sem renovação nem parcial, com motivo de 1–200 e confirmação de que não houve entrega real | B: não permitir cancelamento e usar devolução com observação | Corrige registro indevido sem fingir devolução; explicita ausência de entrega real | Aprovada em 03/11/2026 |
| DEC38 | Saída passada sem limite de antiguidade, nunca futura; já vencido nasce atrasado; sem avisos pretéritos | Limitar passado a 30 dias | Permite cadastrar empréstimos antigos sem sobrepor o mesmo objeto | Aprovada em 03/11/2026 |
| DEC39 | Avisos às 09:00 Fortaleza; um por empréstimo/dia; lidos continuam consultáveis; renovação recalcula futuros; encerramento interrompe novos | Apagar ao ler/encerrar; outros canais | Define leitura, retenção e cessação | Aprovada em 03/11/2026 |
| DEC40 | Renovar 1–7 dias sobre vencimento anterior, até duas vezes e até o fim do vencimento; bloqueio do contato e parcial não impedem | Impedir renovação nesses casos | Compatível com limites e decisões já aprovados | Aprovada em 03/11/2026 |
| DEC41 | Nomes 1–80; oito categorias; telefone 10/11 dígitos; fotos JPEG/PNG até 5 MB; conjuntos 2–50 | Nomes únicos; categorias livres | Torna os limites verificáveis | Aprovada em 03/11/2026 |
| DEC42 | Excluir objeto/contato só sem empréstimo; com histórico apenas arquivar; sem Ativo; desarquivamento permitido | Nunca excluir | Preserva rastreabilidade sem acumular cadastros nunca usados | Aprovada em 03/11/2026 |
| DEC43 | Sem limite de empréstimos simultâneos por contato | Limite numérico por contato | Evita bloqueio de produto não solicitado | Aprovada em 03/11/2026 |
| DEC44 | Nome, categoria e foto do objeto editáveis; conservação só sem Ativo; contato editável; histórico mostra nomes atuais | Congelar todos os nomes | Evita cópias desnecessárias de dados pessoais | Aprovada em 03/11/2026 |
| DEC45 | Conta individual por e-mail e senha; isolamento por dono; cadastro/login/sair, senha 8–128, e-mail validado, 30 dias sem uso; sem recuperação/troca de credenciais na v1 | Login social; conta do tomador | Fecha o acesso mínimo sem criar envio externo de e-mail ou outros perfis; perder senha não terá recuperação nesta v1 | Aprovada em 03/11/2026 |
| DEC46 | Remover dados identificadores de contato sem Ativo, irreversivelmente; arquivar e preservar fatos estruturados; 15 dias é meta de resposta do produto, sem dispensa geral da LGPD nem garantia de anonimização absoluta | Excluir todo histórico; preservar textos pessoais | Evita interpretar o prazo e a exceção legal fora do contexto; evita promessa técnica que o rascunho não cumpre | Aprovada em 03/11/2026 |
| DEC47 | Web responsiva 360–1920 px; dois últimos navegadores estáveis; pt-BR; HTTPS; metas de desempenho; acessibilidade WCAG 2.1 AA; protocolo RNF02, alvo 44×44 adicional, senha protegida e sessão limitada | Metas vagas; nenhum alvo de acessibilidade | A rubrica exige requisitos verificáveis, não apenas adjetivos | Aprovada em 03/11/2026 |
| DEC48 | Observação do contato e motivo do bloqueio opcionais, ambos até 500; sugestão saída+7 editável; Fortaleza para todas as contas; validar ao gravar | Manter detalhes implícitos | Fecha interpretações sem modificar as escolhas centrais | Aprovada em 03/11/2026 |
| DEC49 | Manter marcação de perda no 30º dia de atraso e aviso Perda possível nesse dia, no lugar de Atrasado | B: liberar perda no 1º dia de atraso; C: liberar a qualquer momento Ativo | Preserva decisão aprovada e corrige RN76, que chama indevidamente toda perda precoce de atrasada | Aprovada em 03/11/2026 |
| DEC50 | Excluir conta após senha atual e confirmação explícita, mesmo com Ativos; apagar todos os dados e arquivos da conta, revogar sessões; base ativa imediata, backups até 30 dias, restauração reaplica exclusão | Impedir exclusão com Ativos; só desativar | Define controle do dono sobre a conta e exceção explícita à preservação de histórico | Aprovada em 03/11/2026 |
| DEC51 | Preservar impedimentos ao arquivar; não liberar manualmente perdido/incompleto sem recuperação integral; retificação antiga não altera situação de empréstimo posterior | Liberar todo impedimento manualmente; sempre aplicar retificação ao objeto atual | Impede reemprestar objeto ainda desaparecido e evita retificação histórica sobrescrever situação atual | Aprovada em 03/11/2026 |
| DEC52 | Datas de devolução/parcial não retrocedem em relação a recebimentos anteriores nem à última renovação; novos registros retroativos não sobrepõem períodos do objeto | Aceitar datas contraditórias | Mantém coerência física e diferencia datas de fatos de timestamps do sistema | Aprovada em 03/11/2026 |
| DEC53 | Componentes, ordem e tipo simples/conjunto editáveis só antes do primeiro empréstimo; nomes de componentes podem repetir, com IDs distintos | Permitir alterar componentes com histórico | Impede histórico e devoluções referenciarem peças removidas | Aprovada em 03/11/2026 |
| DEC54 | Duplo clique/reenvio da mesma confirmação produz uma só ação; ações diferentes em abas antigas exigem atualização antes de confirmar novamente | Consumir duas renovações num clique duplo, como ocorria no caso 29 do rascunho | Protege o limite de renovações e o histórico contra repetição acidental | Aprovada em 03/11/2026 |
| DEC55 | Remover espaços externos de textos, normalizar Unicode NFC e contar pontos de código; vazio após trim é inválido quando obrigatório; 5 MB = 5.000.000 bytes | Contar bytes no texto; aceitar só espaços; usar 5 MiB | Fecha limites que hoje parecem concretos, mas têm interpretações diferentes | Aprovada em 03/11/2026 |
| DEC56 | Desempatar listas por ID; eventos por gravação e ID; avisos recentes primeiro; só recuperar avisos do dia corrente após falha; revalidar situação ao gerar | Acumular avisos de dias passados; ordenação variável | Fecha falhas, simultaneidade e ordenação sem gerar spam retroativo | Aprovada em 03/11/2026 |
| DEC57 | Remoção de dados pessoais também apaga textos livres e fotos dos empréstimos do contato e suas cópias nos eventos; preserva IDs, datas, estados e níveis; registra saneamento sem copiar dados removidos | Trocar só nome/telefone e manter dados pessoais no histórico | Resolve o conflito entre histórico imutável e remoção de identificadores | Aprovada em 03/11/2026 |

### 11.1 Detalhamento e limites das decisões aprovadas

- DEC31–DEC32: uma avaliação por empréstimo; contestação só se nível original/atual não nenhum. Retificação uma vez, até o fim da data de gravação + 7 dias, exige nível diferente e descrição se houver dano. Conservação e foto não são editadas. RN65, RN66, RN129–RN132, RN157–RN159.
- DEC35–DEC36 e DEC51: parciais acrescentam peças; integral completa recebimentos. Perda/incompletude exige recuperação formal completa, mesmo em objeto arquivado. Retificação de empréstimo antigo não afeta empréstimo posterior; causas manuais e arquivamento sobrevivem. RN123–RN129, RN154.
- DEC37-A: cancelamento só para registro indevido sem entrega real, sem renovação/parcial, com motivo; inclusive se a data registrada já venceu. DEC49 conserva o limiar de 30 dias e substitui somente o aviso desse dia; no 31º volta o aviso Atrasado. RN73, RN76, RN80, RN81.
- DEC38 e DEC52: datas civis válidas, sem limite de antiguidade adicional, mas cronologia consistente e sem inserir empréstimo anterior a retorno já registrado do mesmo objeto. O instante de digitação não é a data efetiva. RN107, RN118–RN120.
- DEC45: cadastro, login e sair; e-mail normalizado, no máximo 254 caracteres, validação explícita de RN97; senha 8–128, sem trim. Sem troca/recuperação nesta v1, conforme limitação aprovada. Não há conta do tomador.
- DEC46, DEC50 e DEC57: remoção do contato exige ausência de Ativos; preserva fatos estruturados e saneia conteúdo pessoal, inclusive cópias. O dono revisa nomes/fotos de objetos compartilhados, sem excluir esses objetos. Após remoção, fatos de recuperação/retificação ainda podem ser registrados sem textos/fotos. A exclusão da conta é integral e permite Ativos após aviso, senha e confirmação. Backups expurgados até 30 dias e restauração reaplica exclusões. RN93–RN95, RN133–RN142.
- DEC53–DEC55: composição fixa após primeiro empréstimo; confirmações repetidas são idempotentes; versões antigas exigem recarga; normalização e limites de entrada determinísticos. RN105–RN116, RN121, RN122.
- DEC56: desempate por ID, eventos crescentes, avisos recentes primeiro, geração só para o dia corrente, revalidação antes de gerar e atualização da tela à meia-noite/retorno ao primeiro plano. RN143–RN148.

**Privacidade e alcance legal:** o documento estabelece comportamentos de produto, sem declarar certificação de conformidade. O prazo de 15 dias do art. 19 da LGPD trata da declaração completa de confirmação/acesso; a meta de resposta da RN95 não o converte em prazo universal para eliminação. A exceção do art. 4º, I não é tratada aqui como dispensa geral para toda operação do serviço. Referência consultada em 03/11/2026: [LGPD, texto atualizado na Câmara dos Deputados](https://www2.camara.leg.br/legin/fed/lei/2018/lei-13709-14-agosto-2018-787077-normaatualizada-pl.html). Trocar o nome por um rótulo não garante, por si só, impossibilidade de reidentificação; por isso a operação é descrita como remoção de identificadores com saneamento.

## 12. Questões em aberto

Nenhuma decisão de produto pendente identificada após a consolidação e revisão documental de 03/11/2026. DEC01–DEC57 estão aprovadas e o nome do aluno foi confirmado. As limitações da v1 estão explícitas na seção 2.2 e não são decisões em aberto. A auditoria não equivale à execução de testes de um aplicativo; esta entrega contém exclusivamente a especificação e o registro de prompts.
