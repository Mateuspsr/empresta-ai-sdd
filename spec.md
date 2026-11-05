# Empresta Aí — Especificação v1 (MVP)

**Aluno:** Mateus Pinheiro Silva Rodrigues  
**Data:** 05/11/2026  
**Status:** FINAL v1-MVP. Revisão da especificação de 03/11/2026 (159 RN, 20 HU, 112 casos de borda, 57 decisões), reduzida ao menor conjunto suficiente para as nove funcionalidades obrigatórias, conforme orientação do professor. A versão anterior está preservada no histórico Git (commit `468ab41`). Especificação acadêmica; aplicativo não implementado.

---

## 1. Visão

Todo mundo já emprestou um livro, uma furadeira ou um jogo e nunca mais viu. O Empresta Aí é um aplicativo web para uma pessoa física, o dono, registrar o que emprestou, para quem e até quando, e acompanhar devoluções, renovações, atrasos, danos e perdas. O tomador não usa o aplicativo: é um contato cadastrado pelo dono. O resultado esperado é que o dono saiba, a qualquer momento, onde está cada objeto, o que vence ou está atrasado e qual o histórico de cada objeto e de cada amigo, sem cobrança, sem marketplace e sem chat.

**Hipótese de valor a testar quando o produto for construído:** um dono que registra os empréstimos no aplicativo e vê no painel o que vence e o que está atrasado recupera mais objetos e cobra os amigos no prazo, em comparação com quem depende da memória ou de anotações soltas. Indicadores que poderiam ser observados no futuro, não medidos nesta atividade documental: número de empréstimos registrados por dono; proporção de empréstimos devolvidos no prazo; proporção marcada como perdida; frequência de abertura do painel de avisos; feedback qualitativo sobre regras que atrapalham (por exemplo, o limite de duas renovações). O ciclo construir → medir → aprender decidirá o que entra na v2.

## 2. Escopo

### 2.1 Dentro do escopo (núcleo v1)

1. Cadastrar objetos com nome, categoria, foto opcional e estado de conservação, inclusive conjuntos com componentes.
2. Cadastrar amigos (contatos), com arquivamento e remoção simples de dados pessoais.
3. Registrar empréstimo com data de saída e data de vencimento.
4. Renovar empréstimo.
5. Registrar devolução integral ou parcial, com estado de conservação e nível de dano na volta.
6. Registrar perda ou não devolução e, se o objeto reaparecer, a recuperação.
7. Listar empréstimos ativos, atrasados e encerrados.
8. Avisar o dono, no painel do aplicativo, sobre vencimentos próximos e atrasos.
9. Consultar o histórico de um objeto e de um amigo.
10. Cancelar empréstimo registrado por engano.
11. Cadastrar a conta do dono, entrar, sair e excluir a conta.

Os itens 1–9 são as funcionalidades obrigatórias do enunciado; 10 e 11 são decisões de produto do aluno (DEC18, DEC24).

### 2.2 Fora do escopo

O aplicativo explicitamente **não faz**:

- Cobrança ou pagamento real de multas e indenizações; não calcula valores.
- Marketplace público de objetos entre desconhecidos.
- Chat entre usuários.
- Rastreamento por geolocalização.
- Conta ou acesso para o tomador; envio de avisos por e-mail, SMS ou push; pontuação de confiabilidade; bloqueio automático; funcionamento sem conexão.

### 2.3 Limites conhecidos da v1

Comportamentos deliberadamente simples nesta versão. Cada um está declarado onde se aplica e tem alternativa registrada em `backlog_v2.md`.

- **Fuso horário fixo:** todas as datas usam o calendário civil de America/Fortaleza, pelo relógio do servidor, para todas as contas (DEC09, RNF05). Donos em outro fuso veem o "hoje" de Fortaleza.
- **Avisos derivados:** o painel é calculado a cada consulta; não há avisos gravados, lidos/não lidos nem envio externo (DEC11).
- **Um campo de indisponibilidade:** o objeto tem uma única marca "Indisponível" com motivo. Dano grave e perda preenchem a marca automaticamente, mas o dono pode desligá-la a qualquer momento sem empréstimo ativo, inclusive em objeto marcado como perdido (DEC19).
- **Remoção simples de dados pessoais:** apaga nome, telefone e observação do contato; textos livres já gravados nos empréstimos (descrições de dano, observações, motivos) não são alterados. Não se afirma anonimização nem conformidade legal absoluta (DEC23).
- **Sem bloqueio de contato, sem contestação/retificação de dano, sem trilha detalhada de eventos, sem recuperação de senha** (DEC15, DEC21, DEC22, DEC24).

### 2.4 Backlog v2

Itens adiados, em ordem de valor: bloqueio de contato; contestação e retificação de dano; avisos persistidos com lido/não lido ou e-mail; higienização completa de textos históricos e backups; trilha de eventos por ação; idempotência explícita; fuso por conta; foto do dano; recuperação de senha. Detalhes em `backlog_v2.md`.

## 3. Glossário

Cada termo tem um único significado em toda a especificação. **Estado persistido** é um valor gravado; **classificação derivada** é calculada na consulta a partir de datas e campos, e nunca é gravada.

- **Dono:** pessoa física titular da conta; único usuário do aplicativo.
- **Tomador:** amigo que recebe o objeto. Não usa o aplicativo; é representado por um contato.
- **Contato:** registro de um tomador na conta do dono (nome; telefone e observação opcionais).
- **Objeto:** bem físico individual cadastrado pelo dono. Dois itens idênticos são dois objetos.
- **Conjunto:** objeto com 2 a 50 componentes nomeados (exemplo: videogame com dois controles).
- **Componente:** parte nomeada de um conjunto; não é emprestado separadamente.
- **Conservação:** classificação do objeto na escala novo, bom, regular, ruim.
- **Status do objeto (derivado):** Disponível, Emprestado, Indisponível ou Arquivado (RN10).
- **Indisponível:** marca ligada no objeto, com motivo, que impede novo empréstimo (RN11, RN12).
- **Arquivado:** objeto ou contato retirado de uso, preservado por ter histórico; não participa de novos empréstimos; reversível.
- **Empréstimo:** registro de que um objeto saiu com um tomador em uma data de saída e deve voltar até uma data de vencimento.
- **Data de saída:** dia civil em que o objeto foi entregue ao tomador.
- **Data de vencimento:** último dia em que a devolução é considerada no prazo. O empréstimo vence ao fim desse dia (23:59:59) em America/Fortaleza. "Vencimento original" é o informado no registro; "vencimento atual" é o original mais as renovações.
- **Hoje:** dia civil corrente em America/Fortaleza pelo relógio do servidor.
- **Ativo:** estado persistido: o objeto está com o tomador.
- **Em dia (derivado):** Ativo cujo vencimento atual é hoje ou posterior.
- **Atrasado (derivado):** Ativo cujo vencimento atual é anterior a hoje. Não é estado persistido.
- **Dias de atraso:** hoje menos vencimento atual, em dias corridos.
- **Renovação:** ação do dono que soma dias ao vencimento atual de um Ativo em dia.
- **Devolução integral:** ação que registra a volta do objeto completo, com conservação e dano, e encerra o empréstimo como Devolvido.
- **Devolução parcial:** ação, exclusiva de conjuntos, que registra componentes recebidos enquanto outros faltam; não encerra o empréstimo.
- **Devolvido:** estado persistido de empréstimo encerrado por devolução integral; classifica-se, de forma derivada, em "no prazo" ou "com atraso".
- **Dano:** avaria constatada pelo dono na volta ou na recuperação: nenhum, leve ou grave.
- **Perdido:** estado persistido e final de empréstimo encerrado porque o objeto não voltou, com motivo "perda comunicada" ou "não devolução".
- **Recuperado:** marca em empréstimo Perdido indicando que o objeto reapareceu e foi registrado de volta. Não reabre o empréstimo.
- **Cancelado:** estado persistido de empréstimo encerrado por registro indevido.
- **Encerrado:** qualquer empréstimo Devolvido, Perdido ou Cancelado.
- **Aviso (derivado):** item do painel calculado na consulta a partir dos Ativos: "Vence hoje", "Vence em breve" ou "Atrasado".
- **Painel:** tela inicial que lista os avisos e exibe o contador.
- **Histórico:** lista dos empréstimos de um objeto ou de um contato, com os fatos gravados em cada um.
- **Contato removido:** contato cujos dados pessoais foram apagados (RN19); permanece arquivado e imutável.

## 4. Atores e permissões

| Ator | Pode fazer | Não pode fazer |
|---|---|---|
| Dono (usuário autenticado) | Cadastrar, editar, arquivar, desarquivar e excluir objetos e contatos (RN05–RN16); ligar e desligar a marca Indisponível (RN11); registrar, renovar, devolver (integral e parcial), marcar perda, registrar recuperação e cancelar empréstimos (RN20–RN45); consultar painel, listas e históricos (RN28, RN47, RN48); remover dados pessoais de contato (RN19); excluir a conta (RN04); sair | Ver ou alterar dados de outra conta (RN02); editar livremente ou excluir empréstimo (RN46); renovar atrasado ou encerrado (RN34); marcar perda em dia (RN42); reabrir encerrado (RN43, RN46); alterar dano registrado (RN38) |
| Sistema | Derivar status do objeto (RN10), em dia/atrasado (RN25), avisos (RN28) e classificações; ligar a marca Indisponível por dano grave ou perda (RN12); recusar ações inválidas (RN46) | Criar, renovar ou encerrar empréstimos por conta própria; enviar avisos por canal externo (RN30) |
| Tomador | Nada no aplicativo. Pede renovação, comunica perda, discorda de dano ou pede remoção de dados por meio externo ao dono | Acessar o aplicativo (RN03) |

## 5. Regras de negócio

Cada RN é curta, atômica e verificável; cita a decisão de origem (seção 11). Datas são dias civis em America/Fortaleza (premissa DEC09). Textos têm espaços externos removidos antes da contagem e campo obrigatório vazio é recusado (RNF09).

### 5.1 Conta e acesso

- **RN01** — Cada conta pertence a um único dono e é identificada por e-mail único; o cadastro exige e-mail válido e senha de 8 a 128 caracteres. (DEC24)
- **RN02** — Objetos, contatos e empréstimos pertencem à conta que os criou e nunca são exibidos ou alterados por outra conta; acesso a registro de outra conta responde "Registro não encontrado". (DEC01, DEC24)
- **RN03** — O tomador não tem conta nem acesso; é representado exclusivamente por um contato. (DEC01)
- **RN04** — Excluir a conta exige a senha atual e confirmação explícita após exibir a quantidade de empréstimos Ativos; apaga conta, objetos, componentes, contatos, empréstimos e fotos, encerra a sessão e não permite restauração. Empréstimos Ativos não impedem a exclusão. (DEC24)

### 5.2 Objetos

- **RN05** — O cadastro de objeto exige nome de 1 a 80 caracteres, categoria e conservação; a foto é opcional e admite um arquivo JPEG ou PNG de até 5 MB (5.000.000 bytes). (DEC03)
- **RN06** — A categoria é uma destas oito: Livros, Ferramentas, Jogos, Eletrônicos, Casa e cozinha, Esporte e lazer, Roupas e acessórios, Outros. (DEC03)
- **RN07** — A conservação usa a escala novo, bom, regular, ruim, no cadastro, na saída, na volta e na recuperação. (DEC13)
- **RN08** — Cada objeto é uma única unidade física; itens idênticos são cadastrados como objetos distintos e nomes podem repetir. (DEC04)
- **RN09** — Um objeto pode ser um conjunto com 2 a 50 componentes, cada um com nome de 1 a 80 caracteres; componentes não são emprestados separadamente. O tipo (simples ou conjunto) e a composição só podem mudar enquanto o objeto não tiver nenhum empréstimo registrado, em qualquer estado. (DEC05)
- **RN10** — O status do objeto é derivado, nesta ordem: Arquivado se estiver arquivado; Emprestado se tiver empréstimo Ativo; Indisponível se a marca Indisponível estiver ligada; senão Disponível. (DEC19)
- **RN11** — A marca Indisponível é um único campo com motivo de 1 a 200 caracteres. O dono liga ou desliga a marca, e edita o motivo, somente quando o objeto não tem empréstimo Ativo; desligar a marca não altera nenhum empréstimo. (DEC19)
- **RN12** — O sistema liga a marca Indisponível, substituindo o motivo anterior se houver, com motivo "Dano grave em dd/mm/aaaa" quando uma devolução ou recuperação registra dano grave, e com motivo "Perdido em dd/mm/aaaa" quando um empréstimo é marcado Perdido; desliga a marca na recuperação sem dano grave. O motivo preenchido pode ser editado pelo dono conforme RN11. (DEC19)
- **RN13** — Nome, categoria e foto do objeto podem ser editados a qualquer momento; a conservação só pode ser editada quando não há empréstimo Ativo. A edição não altera valores já gravados em empréstimos. (DEC20)
- **RN14** — Objeto sem nenhum empréstimo pode ser excluído, com componentes e foto. Objeto com ao menos um empréstimo, em qualquer estado, só pode ser arquivado. Arquivar exige que não haja empréstimo Ativo; objeto Arquivado não recebe novo empréstimo e pode ser desarquivado. (DEC20)

### 5.3 Contatos

- **RN15** — O cadastro de contato exige nome de 1 a 80 caracteres; telefone (10 ou 11 dígitos, sem máscara) e observação (até 500 caracteres) são opcionais; os três campos podem ser editados a qualquer momento, salvo em contato removido. Nenhum outro dado pessoal é coletado. (DEC06)
- **RN16** — Contato sem nenhum empréstimo pode ser excluído. Contato com ao menos um empréstimo só pode ser arquivado; arquivar exige que não haja empréstimo Ativo; contato Arquivado não recebe novo empréstimo e pode ser desarquivado. (DEC20)
- **RN17** — Não há limite de empréstimos Ativos por contato e não existe bloqueio de contato nesta versão; para impedir novos empréstimos a um amigo, o dono arquiva o contato. (DEC21)
- **RN18** — Os dados do contato servem apenas para identificar o tomador e acompanhar seus empréstimos; só o dono autenticado os vê e a finalidade é exibida na tela "Privacidade". (DEC23)
- **RN19** — Remover dados pessoais exige contato sem empréstimo Ativo e confirmação do dono; substitui o nome por "Contato removido", apaga telefone e observação e arquiva o contato permanentemente, sem edição ou desarquivamento posteriores. Seus empréstimos e fatos gravados permanecem; textos livres já gravados nos empréstimos não são alterados, e a tela de confirmação orienta o dono a revisá-los antes. (DEC23)

### 5.4 Registro de empréstimo

- **RN20** — Registrar empréstimo exige objeto Disponível, contato não arquivado e não removido, data de saída e data de vencimento. O estado na saída vem pré-preenchido com a conservação atual do objeto, pode ser ajustado e, ao gravar, passa a ser a conservação do objeto. (DEC04, DEC13)
- **RN21** — Um objeto tem no máximo um empréstimo Ativo por vez. (DEC04)
- **RN22** — A data de saída deve ser hoje ou anterior; não há limite de antiguidade. (DEC08)
- **RN23** — A data de vencimento deve estar entre saída + 1 e saída + 365 dias corridos, para qualquer categoria; vem pré-preenchida com saída + 7 e é editável. Não há empréstimo sem prazo. (DEC07)
- **RN24** — O registro válido cria o empréstimo no estado Ativo, com zero renovações e vencimento atual igual ao original; se o vencimento for anterior a hoje, o empréstimo já nasce atrasado. (DEC08)

### 5.5 Atraso

- **RN25** — Um Ativo é em dia até 23:59:59 do dia do vencimento atual e atrasado a partir de 00:00 do dia seguinte, sem tolerância. A classificação é derivada na consulta e não é gravada. (DEC10)
- **RN26** — Dias de atraso = hoje menos vencimento atual, em dias corridos. (DEC10)
- **RN27** — Atraso não gera multa, pontuação, bloqueio do contato nem impedimento de novos empréstimos a ele. (DEC10)

### 5.6 Avisos

- **RN28** — O painel calcula, a cada abertura ou atualização, três grupos a partir dos empréstimos Ativos: "Vence hoje" (vencimento atual = hoje), "Vence em breve" (vencimento atual = hoje + 1 ou hoje + 2) e "Atrasado" (vencimento atual < hoje, com os dias de atraso). Dentro de cada grupo os itens ficam por vencimento atual crescente. Cada item exibe objeto, contato, vencimento atual e, se atrasado, "há N dia(s)". (DEC11)
- **RN29** — O contador do painel, exibido em todas as telas, é a quantidade de Ativos nos três grupos. Avisos não são gravados nem marcados como lidos; um empréstimo sai do painel assim que é encerrado ou renovado para fora da janela. (DEC11)
- **RN30** — O painel é o único canal de aviso; não há e-mail, SMS, push nem mensagem ao tomador. (DEC11)

### 5.7 Renovação

- **RN31** — Somente o dono renova; pedido do tomador ocorre fora do aplicativo. (DEC12)
- **RN32** — Cada empréstimo admite no máximo 2 renovações. (DEC12)
- **RN33** — Cada renovação soma de 1 a 7 dias corridos ao vencimento atual, nunca a hoje; grava o novo vencimento atual e incrementa o contador de renovações. Com duas renovações o vencimento pode chegar a saída + 379 dias. (DEC12)
- **RN34** — A renovação só é permitida em empréstimo Ativo em dia; atrasado ou encerrado é recusado. Devolução parcial registrada não impede renovar. (DEC12)

### 5.8 Devolução integral e dano

- **RN35** — A devolução integral exige data entre a saída e hoje (padrão hoje), conservação na volta e nível de dano; aceita observação opcional de até 300 caracteres, usada inclusive para anotar discordância do tomador. (DEC14, DEC15, DEC16)
- **RN36** — O nível de dano é nenhum, leve ou grave, definido exclusivamente pelo dono; dano leve ou grave exige descrição de 1 a 500 caracteres. (DEC14)
- **RN37** — A devolução válida leva o empréstimo de Ativo a Devolvido, grava data, conservação, dano e observação, e a conservação do objeto passa a ser a da volta. É "no prazo" se a data for igual ou anterior ao vencimento atual e "com atraso" caso contrário, com dias de atraso = data da devolução menos vencimento atual. (DEC16)
- **RN38** — Dano grave liga a marca Indisponível do objeto (RN12). O dano gera apenas registro: o aplicativo não calcula, cobra nem controla reparo ou reposição, e o nível registrado não é alterado depois; discordâncias são anotadas na observação. (DEC14, DEC15)

### 5.9 Devolução parcial

- **RN39** — A devolução parcial é exclusiva de conjuntos; exige data entre a saída e hoje (sem verificação em relação a recebimentos anteriores), ao menos um componente ainda faltante marcado como recebido e ao menos um componente continuando faltante. Objeto simples só admite devolução integral. (DEC05)
- **RN40** — A parcial mantém o empréstimo Ativo: vencimento, classificação de atraso, avisos e renovação continuam valendo. Componente recebido não volta a faltante; parciais sucessivas são permitidas até completar o conjunto. (DEC05)
- **RN41** — Marcar o último componente faltante é uma devolução integral: exige conservação e dano e encerra o empréstimo conforme RN37. Componentes já recebidos conservam suas datas. (DEC05)

### 5.10 Perda e recuperação

- **RN42** — O dono pode marcar um Ativo como Perdido a partir do primeiro dia de atraso (hoje posterior ao vencimento atual), com motivo "perda comunicada" ou "não devolução". Em dia, a perda é recusada e o dono aguarda o vencimento ou o renova. (DEC17)
- **RN43** — Marcar perda leva o empréstimo de Ativo a Perdido, estado final que não volta a Ativo; grava data da perda e motivo, preserva os componentes já recebidos e liga a marca Indisponível do objeto (RN12). (DEC17, DEC19)
- **RN44** — Em empréstimo Perdido ainda não recuperado, o dono pode registrar uma única recuperação, com data entre a data da perda e hoje, conservação e dano (RN36). O empréstimo continua Perdido com a marca "recuperado em dd/mm/aaaa"; a conservação do objeto passa a ser a informada e a marca Indisponível é desligada, salvo dano grave (RN12). Emprestar o objeto de novo exige novo registro. (DEC17)

### 5.11 Cancelamento e recusas

- **RN45** — O dono pode cancelar um Ativo sem renovação e sem devolução parcial, com motivo de 1 a 200 caracteres; o empréstimo passa a Cancelado, a conservação do objeto não muda e ele volta a Disponível se não estiver arquivado nem com a marca Indisponível. Cancelado aparece em Encerrados e nos históricos, mas não entra em contagens de atraso nem de devolução. (DEC18)
- **RN46** — Toda ação sobre empréstimo valida o estado atual no momento da gravação; se o estado não admitir a ação, ela é recusada com a mensagem da seção 7.3 sem alterar nenhum registro. Empréstimo encerrado não admite renovação, devolução, perda nem cancelamento, e nenhum empréstimo admite edição livre ou exclusão isolada. (DEC18)

### 5.12 Listas e histórico

- **RN47** — A lista Ativos contém todos os Ativos, por vencimento atual crescente, com os atrasados destacados; Atrasados contém os Ativos com vencimento atual anterior a hoje, por dias de atraso decrescentes; Encerrados contém Devolvidos, Perdidos e Cancelados, por data de encerramento decrescente. (DEC22)
- **RN48** — O histórico do objeto lista todos os seus empréstimos, inclusive de contatos arquivados ou removidos; o histórico do contato lista todos os seus empréstimos, inclusive de objetos arquivados. Ambos por data de saída decrescente. (DEC20, DEC22)
- **RN49** — Cada empréstimo exibe, em listas e históricos: objeto e contato com seus nomes atuais, saída, vencimento original e atual, número de renovações, estado, classificação derivada (em dia, atrasado há N dias, no prazo, com atraso de N dias), conservação na saída e na volta, dano e descrição, observação, componentes recebidos e faltantes, motivo e data da perda, recuperação e motivo do cancelamento, quando existirem. (DEC22)

## 6. Histórias de usuário

Convenções: cada cenário é independente, com o dono autenticado e registros da própria conta. Hoje é **10/11/2026 (terça-feira)**, salvo data explícita. Contexto padrão: objeto "Furadeira Bosch", categoria Ferramentas, conservação "bom", Disponível; conjunto "PlayStation 5" com componentes "Console", "Controle 1" e "Controle 2"; contato "Ana Souza" não arquivada; saída 10/11/2026 e vencimento 17/11/2026 quando não ditos. Devolução sem outros valores usa conservação "bom" e dano "nenhum". Mensagens entre aspas são literais (catálogo em 7.3). Nomes e telefones são fictícios.

### HU01 — Cadastrar e manter objeto

**Como** dono, **quero** cadastrar meus objetos e controlar se estão disponíveis, **para** emprestar somente o que posso entregar.  
**Regras relacionadas:** RN05, RN06, RN07, RN08, RN10, RN11, RN13, RN14.

- **Cenário: cadastro válido**  
  Dado que estou na tela de novo objeto  
  Quando informo nome "Furadeira Bosch", categoria "Ferramentas", conservação "bom" e nenhuma foto  
  Então o objeto é salvo com status "Disponível" e aparece na lista de objetos
- **Cenário: nome vazio**  
  Dado que estou na tela de novo objeto com categoria "Livros" e conservação "novo"  
  Quando tento salvar com o nome vazio  
  Então vejo "Informe o nome do objeto" e nada é salvo
- **Cenário limite: nome com 80 e com 81 caracteres**  
  Dado que preenchi categoria e conservação válidas  
  Quando salvo um nome com exatamente 80 caracteres e, em cenário independente, com 81  
  Então o de 80 é salvo e o de 81 recebe "O nome deve ter de 1 a 80 caracteres" sem salvar
- **Cenário: foto inválida**  
  Dado que preenchi um objeto válido  
  Quando anexo um arquivo PDF ou uma imagem JPEG de 5.000.001 bytes e tento salvar  
  Então vejo "A foto deve ser JPEG ou PNG com até 5 MB" e nada é salvo
- **Cenário: marcar indisponível**  
  Dado que a "Furadeira Bosch" está Disponível  
  Quando ligo a marca Indisponível com motivo "Em revisão técnica"  
  Então o status passa a "Indisponível" e registrar empréstimo dela responde "Objeto indisponível"
- **Cenário: indisponível sem motivo**  
  Dado que a "Furadeira Bosch" está Disponível  
  Quando tento ligar a marca Indisponível sem motivo  
  Então vejo "Informe o motivo da indisponibilidade" e o status continua "Disponível"
- **Cenário: conservação bloqueada durante empréstimo**  
  Dado que a "Furadeira Bosch" está Emprestada  
  Quando tento alterar a conservação para "regular" e, em seguida, altero o nome para "Furadeira azul"  
  Então vejo "Objeto emprestado não pode ter o estado alterado"; o nome muda e o histórico passa a exibir "Furadeira azul"
- **Cenário: excluir com histórico**  
  Dado que o objeto "Cadeira dobrável" tem um empréstimo Devolvido  
  Quando aciono "Excluir"  
  Então vejo "Objeto com histórico só pode ser arquivado" e a opção "Arquivar" é oferecida

### HU02 — Cadastrar conjunto

**Como** dono, **quero** cadastrar um conjunto com componentes, **para** controlar devoluções incompletas.  
**Regras relacionadas:** RN05, RN09.

- **Cenário: conjunto válido**  
  Dado que marquei "É um conjunto" na tela de novo objeto  
  Quando informo nome "PlayStation 5", categoria "Eletrônicos", conservação "bom" e componentes "Console", "Controle 1", "Controle 2"  
  Então o objeto é salvo como conjunto com 3 componentes e status "Disponível"
- **Cenário limite: 1 ou 51 componentes**  
  Dado que marquei "É um conjunto"  
  Quando informo apenas "Console" e, em cenário independente, 51 componentes  
  Então em ambos vejo "Um conjunto precisa de 2 a 50 componentes" e nada é salvo
- **Cenário: componente sem nome**  
  Dado que um dos componentes está com o nome vazio  
  Quando tento salvar  
  Então vejo "Informe o nome de todos os componentes" e nada é salvo
- **Cenário: composição congelada**  
  Dado que o "PlayStation 5" tem um empréstimo Cancelado  
  Quando tento remover "Controle 2" da composição  
  Então vejo "Composição não pode ser alterada após o primeiro empréstimo" e os 3 componentes permanecem

### HU03 — Cadastrar e manter contato

**Como** dono, **quero** cadastrar um amigo e atender a um pedido de remoção dos dados dele, **para** registrar empréstimos respeitando sua privacidade.  
**Regras relacionadas:** RN15, RN16, RN17, RN18, RN19.

- **Cenário: cadastro mínimo**  
  Dado que estou na tela de novo contato  
  Quando informo nome "Ana Souza" e deixo telefone e observação vazios  
  Então o contato é salvo e aparece na lista como "Ana Souza"
- **Cenário: telefone inválido e válido**  
  Dado que informei nome "Ana Souza"  
  Quando salvo com telefone "859999999" e, em cenário independente, "85999990000"  
  Então o primeiro recebe "Telefone deve ter 10 ou 11 dígitos" sem salvar e o segundo é salvo
- **Cenário: tela de privacidade**  
  Dado que estou na tela de contato  
  Quando aciono "Privacidade"  
  Então vejo quais dados são guardados (nome, telefone, observação), a finalidade e a orientação de que pedidos de remoção são feitos ao dono fora do aplicativo
- **Cenário: remoção de dados pessoais**  
  Dado que "Ana Souza", telefone "85999990000", observação "Vizinha", tem apenas empréstimos encerrados  
  Quando aciono "Remover dados pessoais", leio o aviso para revisar textos dos empréstimos e confirmo  
  Então o contato passa a "Contato removido", telefone e observação ficam vazios, fica Arquivado sem opção de editar ou desarquivar, e seus empréstimos continuam nos históricos
- **Cenário: remoção com empréstimo ativo**  
  Dado que "Ana Souza" tem um empréstimo Ativo  
  Quando aciono "Remover dados pessoais"  
  Então vejo "Não é possível remover dados pessoais: há empréstimo ativo" e nada muda
- **Cenário: arquivar com histórico**  
  Dado que "Ana Souza" tem um empréstimo Devolvido e nenhum Ativo  
  Quando aciono "Excluir" e, depois, "Arquivar"  
  Então a exclusão responde "Contato com histórico só pode ser arquivado", o arquivamento é aceito e registrar empréstimo para ela responde "Contato arquivado não pode receber empréstimos"

### HU04 — Registrar empréstimo

**Como** dono, **quero** registrar que emprestei um objeto, **para** acompanhar a devolução.  
**Regras relacionadas:** RN17, RN20, RN21, RN22, RN23, RN24.

- **Cenário: registro válido com prazo sugerido**  
  Dado que a "Furadeira Bosch" está Disponível com conservação "bom"  
  Quando seleciono objeto e contato e aceito saída "10/11/2026" e vencimento pré-preenchido "17/11/2026"  
  Então o empréstimo é salvo Ativo e em dia, estado na saída "bom", e o objeto passa a "Emprestado"
- **Cenário: ajuste do estado na saída**  
  Dado que o objeto tem conservação "bom"  
  Quando altero o estado na saída para "regular" e salvo  
  Então o empréstimo grava estado na saída "regular" e a conservação do objeto passa a "regular"
- **Cenário: saída futura**  
  Dado que hoje é 10/11/2026  
  Quando informo saída "11/11/2026"  
  Então vejo "A data de saída não pode ser futura" e nada é salvo
- **Cenário limite: vencimento em 0, 365 e 366 dias**  
  Dado saída "10/11/2026"  
  Quando informo vencimento "10/11/2026", "10/11/2027" e "11/11/2027" em cenários independentes  
  Então o de 365 dias é salvo e os outros dois recebem "O vencimento deve ser de 1 a 365 dias após a saída" sem salvar
- **Cenário: objeto já emprestado**  
  Dado que a "Furadeira Bosch" tem empréstimo Ativo  
  Quando tento registrar outro empréstimo dela  
  Então vejo "Objeto já emprestado" e nada é salvo
- **Cenário: registro retroativo já vencido**  
  Dado que hoje é 10/11/2026  
  Quando registro saída "01/09/2026" e vencimento "08/09/2026"  
  Então o empréstimo é salvo Ativo, aparece em Atrasados com 63 dias de atraso e no painel em "Atrasado"
- **Cenário: dois empréstimos para o mesmo contato**  
  Dado que "Ana Souza" já tem a "Furadeira Bosch" emprestada  
  Quando registro o "Dom Casmurro" para ela  
  Então o segundo empréstimo é aceito

### HU05 — Renovar empréstimo

**Como** dono, **quero** renovar um empréstimo, **para** adiar o vencimento a pedido do amigo.  
**Regras relacionadas:** RN31, RN32, RN33, RN34.

- **Cenário: primeira renovação**  
  Dado um Ativo com vencimento 17/11/2026 e hoje 10/11/2026  
  Quando aciono "Renovar" com 7 dias  
  Então o vencimento atual passa a 24/11/2026, o vencimento original continua 17/11/2026 e o contador exibe "1 de 2 renovações"
- **Cenário: renovação parte do vencimento, não de hoje**  
  Dado vencimento 17/11/2026 e hoje 12/11/2026  
  Quando renovo por 3 dias  
  Então o vencimento atual passa a 20/11/2026
- **Cenário limite: no dia do vencimento e no dia seguinte**  
  Dado vencimento 17/11/2026  
  Quando renovo por 7 dias às 23:30 de 17/11/2026 e, em cenário independente, às 00:05 de 18/11/2026  
  Então a primeira é aceita (vencimento 24/11/2026) e a segunda recebe "Empréstimo atrasado não pode ser renovado"
- **Cenário limite: terceira renovação**  
  Dado um Ativo em dia com 2 renovações  
  Quando tento renovar  
  Então vejo "Limite de 2 renovações atingido" e nada muda
- **Cenário limite: 0 ou 8 dias**  
  Dado um Ativo em dia sem renovações  
  Quando informo 0 dias e, em cenário independente, 8 dias  
  Então vejo "A renovação deve ser de 1 a 7 dias" e nada muda
- **Cenário: encerrado**  
  Dado um empréstimo Devolvido  
  Quando tento renovar  
  Então vejo "Este empréstimo já está encerrado" e nada muda

### HU06 — Registrar devolução integral

**Como** dono, **quero** registrar a volta do objeto com seu estado, **para** encerrar o empréstimo e comparar com a saída.  
**Regras relacionadas:** RN35, RN36, RN37, RN38, RN46.

- **Cenário: devolução no prazo sem dano**  
  Dado Ativo com saída "bom", vencimento 17/11/2026 e hoje 15/11/2026  
  Quando registro devolução com data "15/11/2026", conservação "bom" e dano "nenhum"  
  Então o empréstimo fica Devolvido "no prazo", o objeto volta a "Disponível" com conservação "bom" e aparece em Encerrados
- **Cenário: devolução com atraso**  
  Dado vencimento 17/11/2026 e hoje 21/11/2026  
  Quando registro devolução com data "21/11/2026"  
  Então o empréstimo fica Devolvido "com atraso de 4 dias"
- **Cenário: devolução retroativa no prazo**  
  Dado vencimento 17/11/2026 e hoje 21/11/2026  
  Quando registro devolução com data "17/11/2026"  
  Então o empréstimo fica Devolvido "no prazo"
- **Cenário: data fora do intervalo**  
  Dado saída 10/11/2026 e hoje 15/11/2026  
  Quando informo data de devolução "09/11/2026" e, em cenário independente, "16/11/2026"  
  Então vejo "A data de devolução deve estar entre a saída e hoje" e nada é salvo
- **Cenário: dano sem descrição**  
  Dado Ativo com hoje 15/11/2026  
  Quando informo dano "leve" e descrição vazia  
  Então vejo "Descreva o dano" e nada é salvo
- **Cenário: dano grave**  
  Dado Ativo com hoje 15/11/2026  
  Quando registro conservação "ruim", dano "grave" e descrição "Mandril travado"  
  Então o empréstimo fica Devolvido, o objeto fica "Indisponível" com motivo "Dano grave em 15/11/2026" e conservação "ruim"
- **Cenário: discordância do amigo**  
  Dado Ativo com hoje 15/11/2026  
  Quando registro dano "leve", descrição "Risco na lateral" e observação "Ana afirma que o risco já existia"  
  Então o histórico exibe dano "leve", a descrição e a observação, e não há opção de alterar o dano depois
- **Cenário: devolução duplicada**  
  Dado um empréstimo já Devolvido  
  Quando tento registrar devolução de novo  
  Então vejo "Este empréstimo já está encerrado" e nada muda

### HU07 — Registrar devolução parcial

**Como** dono, **quero** registrar que só parte de um conjunto voltou, **para** não dar o empréstimo como encerrado.  
**Regras relacionadas:** RN39, RN40, RN41.

- **Cenário: parcial válida**  
  Dado Ativo do "PlayStation 5" com saída 10/11/2026, vencimento 17/11/2026 e hoje 13/11/2026  
  Quando registro parcial em "13/11/2026" marcando "Console" e "Controle 1" recebidos  
  Então o empréstimo continua Ativo com "Falta: Controle 2", o vencimento permanece 17/11/2026 e ele continua em Ativos
- **Cenário: nada novo recebido**  
  Dado que "Console" já foi recebido em 13/11/2026 e hoje é 14/11/2026  
  Quando envio parcial marcando apenas "Console"  
  Então vejo "Marque ao menos um novo componente recebido" e nada é salvo
- **Cenário: último componente vira integral**  
  Dado parcial de "Console" e "Controle 1" em 13/11/2026, vencimento 17/11/2026 e hoje 19/11/2026  
  Quando marco "Controle 2" recebido em "19/11/2026" com conservação "bom" e dano "nenhum"  
  Então o empréstimo fica Devolvido "com atraso de 2 dias", os três componentes aparecem recebidos com suas datas e o objeto volta a "Disponível"
- **Cenário: renovação após parcial**  
  Dado Ativo de conjunto com parcial registrada, vencimento 17/11/2026, zero renovações e hoje 15/11/2026  
  Quando renovo por 5 dias  
  Então o vencimento atual passa a 22/11/2026
- **Cenário: objeto simples**  
  Dado Ativo da "Furadeira Bosch"  
  Quando abro a devolução  
  Então a opção de devolução parcial não é exibida

### HU08 — Registrar perda e recuperação

**Como** dono, **quero** marcar que o objeto não voltou e, se reaparecer, registrar a volta, **para** manter o histórico fiel e o objeto fora de novos empréstimos enquanto sumido.  
**Regras relacionadas:** RN12, RN42, RN43, RN44.

- **Cenário: perda no primeiro dia de atraso**  
  Dado vencimento 17/11/2026 e hoje 18/11/2026  
  Quando marco perda com motivo "não devolução"  
  Então o empréstimo fica Perdido, o objeto fica "Indisponível" com motivo "Perdido em 18/11/2026" e aparece em Encerrados
- **Cenário: perda em dia**  
  Dado vencimento 17/11/2026 e hoje 17/11/2026  
  Quando tento marcar perda  
  Então vejo "Perda só pode ser registrada após o vencimento" e nada muda
- **Cenário: motivo ausente**  
  Dado vencimento 17/11/2026 e hoje 20/11/2026  
  Quando tento marcar perda sem motivo  
  Então vejo "Informe o motivo da perda" e nada muda
- **Cenário: perda com parcial**  
  Dado Ativo do "PlayStation 5" com "Console" recebido em 13/11/2026, vencimento 17/11/2026 e hoje 25/11/2026  
  Quando marco perda com motivo "perda comunicada"  
  Então o empréstimo fica Perdido com "Recebidos: Console. Faltantes: Controle 1, Controle 2" e o objeto fica "Indisponível" com motivo "Perdido em 25/11/2026"
- **Cenário: recuperação sem dano**  
  Dado empréstimo Perdido em 18/11/2026 e hoje 02/12/2026  
  Quando registro recuperação com data "02/12/2026", conservação "regular" e dano "nenhum"  
  Então o empréstimo exibe "Perdido — recuperado em 02/12/2026", não aparece em Ativos e o objeto fica "Disponível" com conservação "regular"
- **Cenário: recuperação com dano grave**  
  Dado empréstimo Perdido em 18/11/2026 e hoje 02/12/2026  
  Quando registro recuperação com conservação "ruim", dano "grave" e descrição "Tela trincada"  
  Então o objeto fica "Indisponível" com motivo "Dano grave em 02/12/2026"
- **Cenário: data anterior à perda e segunda recuperação**  
  Dado empréstimo Perdido em 18/11/2026 e hoje 02/12/2026  
  Quando informo data "17/11/2026" e, em cenário independente, tento registrar uma segunda recuperação  
  Então vejo "A data de recuperação deve estar entre a perda e hoje" e, no segundo caso, "Recuperação já registrada"; nada muda

### HU09 — Cancelar empréstimo registrado por engano

**Como** dono, **quero** cancelar um registro feito por engano, **para** não falsear o histórico.  
**Regras relacionadas:** RN45, RN46.

- **Cenário: cancelamento válido**  
  Dado Ativo sem renovação nem parcial  
  Quando aciono "Cancelar" com motivo "Registro duplicado"  
  Então o empréstimo fica Cancelado, o objeto volta a "Disponível" com a mesma conservação e o registro aparece em Encerrados sem classificação de atraso
- **Cenário: com renovação ou parcial**  
  Dado um Ativo com 1 renovação e, em cenário independente, um conjunto Ativo com parcial  
  Quando tento cancelar  
  Então vejo "Empréstimo com renovação ou devolução parcial não pode ser cancelado" e nada muda
- **Cenário: motivo vazio**  
  Dado Ativo sem renovação nem parcial  
  Quando tento cancelar sem motivo  
  Então vejo "Informe o motivo do cancelamento" e nada muda
- **Cenário: cancelar encerrado**  
  Dado um empréstimo Devolvido  
  Quando tento cancelar  
  Então vejo "Este empréstimo já está encerrado" e nada muda

### HU10 — Listar ativos, atrasados e encerrados

**Como** dono, **quero** ver meus empréstimos por situação, **para** saber o que cobrar.  
**Regras relacionadas:** RN25, RN26, RN27, RN45, RN47, RN49.

- **Cenário: lista Ativos**  
  Dado três Ativos com vencimentos 17/11/2026, 12/11/2026 e 08/11/2026, hoje 10/11/2026  
  Quando abro Ativos  
  Então vejo os três na ordem 08/11 (destacado "atrasado há 2 dias"), 12/11, 17/11
- **Cenário: lista Atrasados**  
  Dado os mesmos três Ativos  
  Quando abro Atrasados  
  Então vejo apenas o de 08/11/2026 com "atrasado há 2 dias"
- **Cenário limite: vira atrasado à meia-noite**  
  Dado vencimento 10/11/2026  
  Quando são 10/11/2026 23:59 e depois 11/11/2026 00:00  
  Então o empréstimo não está em Atrasados às 23:59 e está em Atrasados às 00:00 com "há 1 dia"
- **Cenário: Encerrados**  
  Dado hoje 07/12/2026, um Devolvido em 21/11/2026 (vencimento 17/11/2026), um Perdido em 06/12/2026 e um Cancelado em 11/11/2026  
  Quando abro Encerrados  
  Então vejo na ordem 06/12 (Perdido), 21/11 (Devolvido com atraso de 4 dias), 11/11 (Cancelado, sem classificação de atraso)
- **Cenário: lista vazia**  
  Dado que não há empréstimos atrasados  
  Quando abro Atrasados  
  Então vejo "Nenhum empréstimo atrasado"

### HU11 — Ver avisos de prazo e atraso no painel

**Como** dono, **quero** ver no painel o que vence e o que está atrasado, **para** agir a tempo.  
**Regras relacionadas:** RN28, RN29, RN30.

- **Cenário: vence em breve e vence hoje**  
  Dado Ativos da "Furadeira Bosch" (vencimento 12/11/2026) e do "Dom Casmurro" (vencimento 10/11/2026), hoje 10/11/2026  
  Quando abro o painel  
  Então vejo "Vence em breve: Furadeira Bosch — Ana Souza — 12/11/2026", "Vence hoje: Dom Casmurro — Ana Souza" e o contador mostra 2
- **Cenário: atrasado**  
  Dado Ativo com vencimento 17/11/2026 e hoje 19/11/2026  
  Quando abro o painel  
  Então vejo "Atrasado: Furadeira Bosch — Ana Souza — venceu em 17/11/2026, há 2 dias"
- **Cenário limite: três dias antes não aparece**  
  Dado Ativo com vencimento 13/11/2026 e hoje 10/11/2026  
  Quando abro o painel  
  Então o empréstimo não aparece e o contador mostra 0; em 11/11/2026 ele aparece em "Vence em breve"
- **Cenário: devolução remove o aviso**  
  Dado Ativo atrasado exibido no painel  
  Quando registro a devolução e volto ao painel  
  Então o empréstimo não aparece mais e o contador diminui em 1, sem qualquer marcação de lido
- **Cenário: renovação recalcula**  
  Dado Ativo com vencimento 10/11/2026 exibido em "Vence hoje", hoje 10/11/2026  
  Quando renovo por 7 dias e volto ao painel  
  Então o empréstimo não aparece mais (vencimento 17/11/2026 está fora da janela de 2 dias)

### HU12 — Consultar histórico por objeto e por amigo

**Como** dono, **quero** ver tudo que aconteceu com um objeto ou com um amigo, **para** decidir se empresto de novo.  
**Regras relacionadas:** RN19, RN48, RN49.

- **Cenário: histórico do objeto**  
  Dado hoje 25/11/2026, "Furadeira Bosch" com um Devolvido (Ana, saída 10/11/2026, vencimento 17/11/2026, devolução 15/11/2026) e um Ativo (Bruno, saída 20/11/2026, vencimento 27/11/2026)  
  Quando abro o histórico do objeto  
  Então vejo primeiro o de Bruno (Ativo, em dia) e depois o de Ana (Devolvido no prazo), com saída, vencimentos, conservação na saída e na volta e dano
- **Cenário: histórico do contato com objeto arquivado**  
  Dado que "Ana Souza" devolveu a "Furadeira Bosch" e o objeto foi arquivado  
  Quando abro o histórico de Ana  
  Então o empréstimo aparece com o nome atual do objeto e a marca "objeto arquivado"
- **Cenário: contato removido**  
  Dado um empréstimo Devolvido com dano "leve" e descrição "Risco na lateral" cujo contato teve os dados removidos  
  Quando abro o histórico do objeto  
  Então o tomador aparece como "Contato removido" e datas, conservação, dano e descrição permanecem como gravados
- **Cenário: renovações e recuperação no histórico**  
  Dado um Perdido com 2 renovações (vencimento original 17/11/2026, atual 01/12/2026) recuperado em 20/12/2026  
  Quando abro o histórico  
  Então vejo "2 renovações", "vencimento original 17/11/2026, atual 01/12/2026", "Perdido em 02/12/2026 (não devolução)" e "recuperado em 20/12/2026"
- **Cenário: sem histórico**  
  Dado um objeto nunca emprestado  
  Quando abro o histórico  
  Então vejo "Nenhum empréstimo registrado"

### HU13 — Cadastrar conta, entrar, sair e excluir a conta

**Como** dono, **quero** uma conta individual que eu possa encerrar, **para** manter meus registros separados e sob meu controle.  
**Regras relacionadas:** RN01, RN02, RN03, RN04.

- **Cenário: cadastro e login**  
  Dado que não existe a conta dono@example.test  
  Quando cadastro e-mail "dono@example.test" e senha "Exemplo123" e depois entro com essas credenciais  
  Então a conta é criada vazia e a sessão abre no painel
- **Cenário limite: senha com 7, 8, 128 e 129 caracteres**  
  Dado um e-mail novo válido  
  Quando cadastro cada senha em cenário independente  
  Então 8 e 128 são aceitas; 7 e 129 recebem "Senha deve ter de 8 a 128 caracteres"
- **Cenário: e-mail duplicado ou inválido**  
  Dado que dono@example.test já existe  
  Quando tento cadastrar "dono@example.test" de novo e, em cenário independente, "dono@"  
  Então vejo "E-mail já cadastrado" e "Informe um e-mail válido", sem criar conta
- **Cenário: credenciais inválidas**  
  Dado a tela de login  
  Quando uso e-mail inexistente ou senha incorreta  
  Então vejo "E-mail ou senha inválidos" e nenhuma sessão é criada
- **Cenário: isolamento**  
  Dado contas A e B e um objeto de B  
  Quando, autenticado em A, acesso o endereço do objeto de B  
  Então vejo "Registro não encontrado" e nada de B é exibido
- **Cenário: sair**  
  Dado uma sessão válida  
  Quando aciono "Sair" e tento acessar o painel  
  Então vejo "Entre para continuar" e os registros da conta permanecem
- **Cenário: excluir conta**  
  Dado conta com 2 Ativos, objetos, contatos e fotos  
  Quando aciono "Excluir conta", leio o aviso sobre os 2 Ativos, informo a senha correta e confirmo  
  Então todos os dados da conta são removidos, a sessão termina e vejo "Conta excluída"
- **Cenário: senha errada na exclusão**  
  Dado conta autenticada  
  Quando confirmo a exclusão com senha incorreta  
  Então vejo "Senha atual incorreta" e nada é removido

## 7. Estados do empréstimo

### 7.1 Estados persistidos e classificações derivadas

Estados persistidos: **Ativo**, **Devolvido**, **Perdido**, **Cancelado**. Não há reabertura. Classificações derivadas: em Ativo, **em dia** ou **atrasado** (RN25) e **parcial** se há componente recebido; em Devolvido, **no prazo** ou **com atraso** (RN37); em Perdido, **recuperado** se há recuperação (RN44). Cancelado não tem classificação de atraso (RN45).

### 7.2 Transições

Recusa conserva o estado anterior e não altera nada (RN46).

| Estado atual | Ação | Condição | Próximo estado / resultado | Regra |
|---|---|---|---|---|
| Inexistente | Registrar | Objeto Disponível, contato não arquivado/removido, saída ≤ hoje, vencimento saída+1..+365 | Ativo | RN20–RN24 |
| Ativo em dia | Renovar | Menos de 2 renovações; 1–7 dias | Ativo com novo vencimento atual | RN32–RN34 |
| Ativo atrasado | Renovar | — | Recusa: "Empréstimo atrasado não pode ser renovado" | RN34 |
| Ativo em dia | Renovar | Já há 2 renovações | Recusa: "Limite de 2 renovações atingido" | RN32 |
| Ativo | Devolução integral | Data entre saída e hoje; conservação; dano válido | Devolvido | RN35–RN37 |
| Ativo (conjunto) | Devolução parcial | ≥1 novo recebido e ≥1 faltante | Ativo parcial | RN39, RN40 |
| Ativo (conjunto) | Receber último faltante | Conservação e dano válidos | Devolvido | RN41 |
| Ativo atrasado | Marcar perda | Motivo informado | Perdido | RN42, RN43 |
| Ativo em dia | Marcar perda | — | Recusa: "Perda só pode ser registrada após o vencimento" | RN42 |
| Ativo | Cancelar | Sem renovação nem parcial; motivo | Cancelado | RN45 |
| Ativo | Cancelar | Com renovação ou parcial | Recusa: "Empréstimo com renovação ou devolução parcial não pode ser cancelado" | RN45 |
| Perdido não recuperado | Registrar recuperação | Data entre perda e hoje; conservação; dano | Perdido recuperado | RN44 |
| Perdido recuperado | Registrar recuperação | — | Recusa: "Recuperação já registrada" | RN44 |
| Devolvido, Perdido ou Cancelado | Renovar, devolver, marcar perda ou cancelar | — | Recusa: "Este empréstimo já está encerrado" | RN46 |
| Devolvido ou Cancelado | Registrar recuperação | — | Recusa: "Ação disponível apenas para empréstimo perdido" | RN44, RN46 |
| Qualquer | Exclusão da conta | Senha e confirmação | Removido com a conta | RN04 |

### 7.3 Mensagens de recusa

Mensagens literais. Valor inválido não é gravado nem truncado; com vários campos inválidos, todas as mensagens são exibidas junto aos campos (RNF09).

| Validação | Mensagem | Regra |
|---|---|---|
| Nome de objeto/contato/componente ausente | Informe o nome do objeto / Informe o nome do contato / Informe o nome de todos os componentes | RN05, RN09, RN15 |
| Nome fora de 1–80 | O nome deve ter de 1 a 80 caracteres | RN05, RN09, RN15 |
| Categoria ausente | Selecione uma categoria | RN05 |
| Conservação ausente | Informe o estado de conservação | RN05, RN35 |
| Foto inválida | A foto deve ser JPEG ou PNG com até 5 MB | RN05 |
| Conjunto fora de 2–50 | Um conjunto precisa de 2 a 50 componentes | RN09 |
| Alterar composição com empréstimo | Composição não pode ser alterada após o primeiro empréstimo | RN09 |
| Ligar/desligar marca em objeto Emprestado | Objeto emprestado não pode ter a disponibilidade alterada | RN11 |
| Marca sem motivo | Informe o motivo da indisponibilidade | RN11 |
| Editar conservação de Emprestado | Objeto emprestado não pode ter o estado alterado | RN13 |
| Arquivar com Ativo | Não é possível arquivar: há empréstimo ativo | RN14, RN16 |
| Excluir com histórico | Objeto com histórico só pode ser arquivado / Contato com histórico só pode ser arquivado | RN14, RN16 |
| Telefone inválido | Telefone deve ter 10 ou 11 dígitos | RN15 |
| Observação do contato > 500 | Observação deve ter no máximo 500 caracteres | RN15 |
| Remover dados com Ativo | Não é possível remover dados pessoais: há empréstimo ativo | RN19 |
| Editar contato removido | Contato removido não pode ser editado | RN19 |
| Objeto Emprestado / Indisponível / Arquivado no registro | Objeto já emprestado / Objeto indisponível / Objeto arquivado | RN20, RN21 |
| Contato arquivado ou removido no registro | Contato arquivado não pode receber empréstimos | RN20 |
| Saída futura | A data de saída não pode ser futura | RN22 |
| Vencimento fora do intervalo ou ausente | O vencimento deve ser de 1 a 365 dias após a saída / Informe a data de vencimento | RN23 |
| Data inexistente | Informe uma data válida | RN22, RN23, RN35, RN44 |
| Renovação fora de 1–7 | A renovação deve ser de 1 a 7 dias | RN33 |
| Devolução fora de saída..hoje | A data de devolução deve estar entre a saída e hoje | RN35, RN39 |
| Dano ausente | Informe o nível de dano | RN36 |
| Dano leve/grave sem descrição | Descreva o dano | RN36 |
| Observação da devolução > 300 | Observação deve ter no máximo 300 caracteres | RN35 |
| Parcial sem novo componente | Marque ao menos um novo componente recebido | RN39 |
| Perda sem motivo | Informe o motivo da perda | RN42 |
| Recuperação fora de perda..hoje | A data de recuperação deve estar entre a perda e hoje | RN44 |
| Cancelamento sem motivo | Informe o motivo do cancelamento | RN45 |
| Recuperação em empréstimo não Perdido | Ação disponível apenas para empréstimo perdido | RN44, RN46 |
| Renovar atrasado | Empréstimo atrasado não pode ser renovado | RN34 |
| Terceira renovação | Limite de 2 renovações atingido | RN32 |
| Perda em dia | Perda só pode ser registrada após o vencimento | RN42 |
| Segunda recuperação | Recuperação já registrada | RN44 |
| Cancelar com renovação ou parcial | Empréstimo com renovação ou devolução parcial não pode ser cancelado | RN45 |
| Ação em empréstimo encerrado | Este empréstimo já está encerrado | RN46 |
| Motivo (indisponibilidade/cancelamento) > 200 | Motivo deve ter no máximo 200 caracteres | RN11, RN45 |
| E-mail inválido / duplicado | Informe um e-mail válido / E-mail já cadastrado | RN01 |
| Senha fora de 8–128 | Senha deve ter de 8 a 128 caracteres | RN01 |
| Login incorreto | E-mail ou senha inválidos | RN01 |
| Sem sessão | Entre para continuar | RNF06 |
| Senha incorreta na exclusão | Senha atual incorreta | RN04 |
| Registro inexistente ou de outra conta | Registro não encontrado | RN02 |
| Listas e históricos vazios | Nenhum empréstimo ativo / Nenhum empréstimo atrasado / Nenhum empréstimo encerrado / Nenhum empréstimo registrado | RN47, RN48 |

### 7.4 Status do objeto

Derivado por RN10: Arquivado > Emprestado > Indisponível > Disponível.

| Situação | Ação | Resultado | Regra |
|---|---|---|---|
| Disponível | Registrar empréstimo | Emprestado | RN20, RN24 |
| Disponível ou Indisponível, sem Ativo | Ligar / desligar marca | Indisponível / Disponível | RN11 |
| Emprestado | Devolução integral sem dano grave | Disponível (ou Indisponível se a marca já estava ligada; Arquivado não ocorre, pois exige sem Ativo) | RN37 |
| Emprestado | Devolução integral com dano grave | Indisponível, motivo "Dano grave em …" | RN12, RN38 |
| Emprestado | Marcar perda | Indisponível, motivo "Perdido em …" | RN12, RN43 |
| Emprestado | Cancelar | Disponível, salvo marca ligada | RN45 |
| Emprestado | Arquivar, excluir, editar conservação, ligar/desligar marca | Recusa | RN11, RN13, RN14 |
| Indisponível por perda | Recuperação sem dano grave / com dano grave | Disponível / Indisponível com motivo "Dano grave em …" | RN12, RN44 |
| Sem Ativo | Arquivar / Desarquivar | Arquivado / status recalculado, marca preservada | RN10, RN14 |
| Sem nenhum empréstimo | Excluir | Removido com componentes e foto | RN14 |

## 8. Modelo de dados

Modelo conceitual (tipos indicam domínio, não esquema executável). Toda entidade tem `id` único e imutável; todas, exceto Conta, têm `contaId` e só referenciam registros da mesma conta (RN02). Datas são dias civis; "data e hora" é instante do servidor. Classificações derivadas não são atributos gravados. Seis entidades, conforme DEC26.

**Conta** — RN01, RN02, RN04

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| email | texto | sim | único; formato válido |
| senhaHash | texto | sim | hash da senha de 8–128 caracteres; nunca em claro (RNF06) |

**Objeto** — RN05–RN14

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| nome | texto | sim | 1–80 |
| categoria | enumeração | sim | uma das 8 de RN06 |
| conservacao | enumeração | sim | novo, bom, regular, ruim |
| foto | arquivo | não | JPEG/PNG ≤ 5.000.000 bytes; 0..1 |
| ehConjunto | booleano | sim | verdadeiro exige 2–50 Componentes; imutável após o primeiro empréstimo |
| indisponivel | booleano | sim | padrão falso; alterável só sem Empréstimo Ativo |
| indisponivelMotivo | texto | condicional | 1–200; obrigatório se indisponivel; nulo caso contrário |
| arquivado | booleano | sim | padrão falso; verdadeiro exige nenhum Empréstimo Ativo |
| status (derivado) | enumeração | — | RN10 |

**Componente** — RN09

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| objetoId | referência a Objeto | sim | objeto com ehConjunto verdadeiro |
| nome | texto | sim | 1–80 |

**Contato** — RN15–RN19

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| nome | texto | sim | 1–80; "Contato removido" após remoção de dados |
| telefone | texto | não | 10 ou 11 dígitos; nulo após remoção |
| observacao | texto | não | ≤ 500; nulo após remoção |
| arquivado | booleano | sim | padrão falso; verdadeiro exige nenhum Ativo |
| dadosRemovidosEm | data e hora | não | preenchido torna o contato imutável e arquivado |

**Empréstimo** — RN20–RN49

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| objetoId | referência a Objeto | sim | no máximo um Empréstimo Ativo por objeto (RN21) |
| contatoId | referência a Contato | sim | no registro: não arquivado, não removido |
| dataSaida | data | sim | ≤ hoje |
| vencimentoOriginal | data | sim | saída+1 ≤ valor ≤ saída+365 |
| vencimentoAtual | data | sim | = original + dias das renovações |
| quantidadeRenovacoes | inteiro | sim | 0..2 |
| estadoSaida | enumeração | sim | escala RN07 |
| estado | enumeração | sim | Ativo, Devolvido, Perdido, Cancelado |
| dataDevolucao | data | condicional | obrigatória se Devolvido; saída ≤ valor ≤ hoje |
| conservacaoVolta | enumeração | condicional | obrigatória se Devolvido ou recuperado |
| danoNivel | enumeração | condicional | nenhum, leve, grave; obrigatório se Devolvido ou recuperado |
| danoDescricao | texto | condicional | 1–500 se danoNivel ≠ nenhum; nulo se nenhum |
| observacao | texto | não | ≤ 300; informada na devolução |
| perdaMotivo | enumeração | condicional | perda comunicada, não devolução; obrigatório se Perdido |
| dataPerda | data | condicional | obrigatória se Perdido; = hoje no ato da perda |
| dataRecuperacao | data | não | só em Perdido; perda ≤ valor ≤ hoje; única |
| cancelamentoMotivo | texto | condicional | 1–200; obrigatório se Cancelado |
| encerradoEm | data e hora | condicional | instante da devolução, perda ou cancelamento; nulo se Ativo |
| classificacao (derivada) | enumeração | — | em dia/atrasado (Ativo); no prazo/com atraso (Devolvido); recuperado (Perdido) |
| diasAtraso (derivado) | inteiro | — | RN26 ou RN37 |

**ComponenteRecebido** — RN39–RN41, RN43

| Atributo | Tipo | Obrigatório | Restrição |
|---|---|---|---|
| emprestimoId | referência a Empréstimo | sim | empréstimo de objeto conjunto |
| componenteId | referência a Componente | sim | componente do objeto do empréstimo; único por empréstimo |
| recebidoEm | data | sim | saída ≤ valor ≤ hoje; irreversível |

**Relacionamentos:** Conta 1 — 0..N Objeto, Contato, Empréstimo. Objeto 1 — 0..50 Componente (2..50 se conjunto; 0 se simples). Objeto 1 — 0..N Empréstimo, no máximo 1 Ativo. Contato 1 — 0..N Empréstimo. Empréstimo 1 — 0..N ComponenteRecebido (≤ número de componentes). Componente sem linha em ComponenteRecebido está faltante.

**Integridade:** Ativo implica encerradoEm nulo; Devolvido exige dataDevolucao, conservacaoVolta, danoNivel e encerradoEm, e todos os componentes recebidos; Perdido exige perdaMotivo, dataPerda e encerradoEm, e pode ter recebimentos parciais; Cancelado exige cancelamentoMotivo e encerradoEm e nenhum recebimento nem renovação. Campos de outros estados ficam nulos. Exclusão de Objeto ou Contato só sem Empréstimo (RN14, RN16); a exclusão da conta remove tudo (RN04). Uma gravação confirma todos os seus efeitos ou nenhum (RNF08).

## 9. Casos de borda e erros

Cada caso aponta a RN que enuncia o comportamento.

| # | Situação | Comportamento esperado | Regra |
|---|---|---|---|
| 1 | Nome de objeto só com espaços | Recusa "Informe o nome do objeto" (espaços externos são removidos antes de validar) | RN05, RNF09 |
| 2 | Dois objetos com o mesmo nome | Ambos salvos com ids distintos | RN08 |
| 3 | Foto PNG de 5.000.000 bytes; de 5.000.001; GIF renomeado .jpg | Primeira aceita; as outras recusadas pelo conteúdo e tamanho | RN05 |
| 4 | Tentar emprestar só um componente | Opção inexistente; empréstimo é sempre do conjunto | RN09 |
| 5 | Alterar composição de conjunto com empréstimo Cancelado | Recusa; Cancelado conta como empréstimo | RN09 |
| 6 | Desarquivar objeto com marca Indisponível ligada | Status Indisponível; a marca é preservada pelo arquivamento | RN10, RN14 |
| 7 | Ligar marca Indisponível em objeto Emprestado | Recusa "Objeto emprestado não pode ter a disponibilidade alterada" | RN11 |
| 8 | Desligar marca "Perdido em …" sem registrar recuperação | Permitido; objeto volta a Disponível e o empréstimo continua Perdido sem recuperação (limitação declarada em 2.3) | RN11 |
| 9 | Editar conservação de objeto Emprestado | Recusa "Objeto emprestado não pode ter o estado alterado"; nome continua editável | RN13 |
| 10 | Editar conservação sem Ativo e consultar empréstimo antigo | Cadastro muda; estadoSaida e conservacaoVolta históricos não mudam | RN13 |
| 11 | Excluir objeto com um empréstimo Cancelado | Recusa; só arquivar | RN14 |
| 12 | Telefone com +55, máscara, letras ou 12 dígitos | Recusa "Telefone deve ter 10 ou 11 dígitos" | RN15 |
| 13 | Contato com 10 empréstimos Ativos | Aceito; não há limite | RN17 |
| 14 | Contato atrasado há 45 dias recebe novo empréstimo | Aceito; atraso não bloqueia; para impedir, o dono arquiva o contato | RN17, RN27 |
| 15 | Remover dados de contato com empréstimos encerrados que têm descrição de dano | Nome/telefone/observação apagados; descrição de dano permanece (limitação declarada) | RN19 |
| 16 | Registrar empréstimo para "Contato removido" | Recusa "Contato arquivado não pode receber empréstimos" | RN19, RN20 |
| 17 | Dois registros simultâneos do mesmo objeto (duas abas) | Só o primeiro grava; o segundo recebe "Objeto já emprestado" | RN21, RNF08 |
| 18 | Saída 29/02/2028 e vencimento 28/02/2029 | 365 dias: aceito | RN23 |
| 19 | Saída há 3 anos e vencimento há 3 anos | Aceita; nasce atrasado; painel mostra "Atrasado" com os dias | RN22, RN24, RN28 |
| 20 | Dono em Lisboa às 01:00 locais de 11/11/2026 | Hoje é 10/11/2026 em Fortaleza; saída "11/11/2026" é recusada como futura | RN22, RNF05 |
| 21 | Vencimento 17/11/2026 às 23:59:59 e às 00:00:00 de 18/11 | Em dia; depois atrasado há 1 dia | RN25, RN26 |
| 22 | Atrasado há 200 dias | Continua Ativo e atrasado; sem bloqueio ou pontuação | RN27 |
| 23 | Renovar às 23:59:59 do vencimento / às 00:00:00 do dia seguinte | Aceita / Recusa "Empréstimo atrasado não pode ser renovado" | RN34 |
| 24 | Duas renovações de 7 dias em prazo de 365 | Vencimento atual = saída + 379; aceito | RN33 |
| 25 | Renovar conjunto com parcial registrada | Aceita se em dia | RN34 |
| 26 | Devolver com data igual à saída | Aceita; "no prazo" | RN35, RN37 |
| 27 | Devolver com data futura | Recusa "A data de devolução deve estar entre a saída e hoje" | RN35 |
| 28 | Saiu "ruim" e volta "bom" | Aceito; conservação do objeto passa a "bom" | RN37 |
| 29 | Dano "nenhum" com conservação pior que na saída | Aceito; o dono define o nível | RN36 |
| 30 | Dano "leve" sem descrição | Recusa "Descreva o dano" | RN36 |
| 31 | Parcial marcando todos os componentes | Tratada como integral: exige conservação e dano | RN41 |
| 32 | Parcial em objeto simples por chamada direta | Recusa, mesmo sem opção na interface | RN39 |
| 33 | Desmarcar componente já recebido | Recusa; recebimento é irreversível | RN40 |
| 34 | Perda em dia (tomador avisou que perdeu) | Recusa "Perda só pode ser registrada após o vencimento"; dono aguarda o vencimento | RN42 |
| 35 | Perda com devolução parcial | Perdido; recebidos preservados; objeto Indisponível "Perdido em …" | RN43 |
| 36 | Recuperação com dano grave | Empréstimo Perdido recuperado; objeto Indisponível "Dano grave em …" | RN12, RN44 |
| 37 | Segunda recuperação | Recusa "Recuperação já registrada" | RN44 |
| 38 | Emprestar objeto recuperado | Novo registro; o Perdido permanece no histórico | RN44 |
| 39 | Cancelar Ativo já atrasado, sem renovação nem parcial | Aceito com motivo; não entra em contagens de atraso | RN45 |
| 40 | Cancelar após renovação | Recusa "Empréstimo com renovação ou devolução parcial não pode ser cancelado" | RN45 |
| 41 | Devolver duas vezes | Segunda recusada "Este empréstimo já está encerrado" | RN46 |
| 42 | Ação recusada | Nenhum registro alterado | RN46 |
| 43 | Atrasado nas listas | Aparece em Ativos (destacado) e em Atrasados | RN47 |
| 44 | Perdido e Cancelado em Encerrados | Sem classificação no prazo/com atraso nem dias de atraso | RN45, RN49 |
| 45 | Vence em 3 dias | Não aparece no painel; aparece quando faltarem 2 dias | RN28 |
| 46 | Devolução ou perda registrada | Empréstimo sai do painel na próxima consulta; contador recalculado | RN29 |
| 47 | Tela do painel aberta à meia-noite | Valores só mudam ao atualizar a consulta; não há atualização automática exigida | RN28, RNF05 |
| 48 | Acesso a objeto de outra conta pelo endereço | "Registro não encontrado"; nenhum dado exibido | RN02 |
| 49 | Senha com 7 ou 129 caracteres | Recusa; limites inclusivos 8 e 128 | RN01 |
| 50 | Exclusão de conta com Ativos | Permitida após aviso da quantidade, senha e confirmação | RN04 |
| 51 | Parcial com data anterior a um recebimento já registrado | Aceita se estiver entre a saída e hoje; a v1 não verifica cronologia entre recebimentos | RN39 |

### 9.1 Rastreabilidade por história

| História | Regras | Fluxo / estados | Casos de borda |
|---|---|---|---|
| HU01 | RN05, RN06, RN07, RN08, RN10, RN11, RN13, RN14 | Cadastro e status do objeto | 1, 2, 3, 6, 7, 8, 9, 10, 11 |
| HU02 | RN05, RN09 | Composição | 4, 5 |
| HU03 | RN15, RN16, RN17, RN18, RN19 | Cadastro, arquivamento e remoção de dados | 12, 13, 14, 15, 16 |
| HU04 | RN17, RN20, RN21, RN22, RN23, RN24 | Inexistente → Ativo | 13, 16, 17, 18, 19, 20 |
| HU05 | RN31, RN32, RN33, RN34 | Ativo → Ativo | 23, 24, 25 |
| HU06 | RN35, RN36, RN37, RN38, RN46 | Ativo → Devolvido | 26, 27, 28, 29, 30, 41, 42 |
| HU07 | RN39, RN40, RN41 | Ativo parcial → Ativo / Devolvido | 25, 31, 32, 33, 51 |
| HU08 | RN12, RN42, RN43, RN44 | Ativo → Perdido → Perdido recuperado | 8, 34, 35, 36, 37, 38 |
| HU09 | RN45, RN46 | Ativo → Cancelado | 39, 40, 41, 42 |
| HU10 | RN25, RN26, RN27, RN45, RN47, RN49 | Consulta de todos os estados | 21, 22, 43, 44 |
| HU11 | RN28, RN29, RN30 | Avisos derivados | 19, 45, 46, 47 |
| HU12 | RN19, RN48, RN49 | Histórico | 10, 15, 44 |
| HU13 | RN01, RN02, RN03, RN04 | Conta e sessão | 48, 49, 50 |

Cobertura das nove funcionalidades do enunciado: 1 objetos → HU01, HU02; 2 contatos → HU03; 3 empréstimo → HU04; 4 renovação → HU05; 5 devolução com estado → HU06, HU07; 6 perda → HU08; 7 listas → HU10; 8 avisos → HU11; 9 histórico → HU12. Toda RN01–RN49 está ligada a pelo menos uma HU.

## 10. Requisitos não funcionais

Requisitos de uma implementação futura; nenhum teste de aplicativo foi executado nesta atividade documental.

| ID | Requisito | Critério verificável |
|---|---|---|
| RNF01 | Plataforma | Aplicativo web responsivo nas duas últimas versões estáveis de Chrome, Firefox, Safari e Edge; larguras de 360 a 1920 px CSS sem rolagem horizontal, verificando 360, 768 e 1920 px |
| RNF02 | Desempenho | Com até 500 empréstimos na conta e rede de 10 Mbps, 95% das aberturas de lista ou painel em até 2 s e 95% das ações sem upload em até 1 s, medidas em 30 execuções |
| RNF03 | Acessibilidade | Critérios A e AA da WCAG 2.1 aplicáveis: navegação por teclado, foco visível, rótulos, erros associados aos campos, contraste de texto ≥ 4,5:1 |
| RNF04 | Idioma e formatos | Interface em pt-BR; datas dd/mm/aaaa; horas hh:mm em 24 h |
| RNF05 | Tempo oficial e fuso (premissa da v1) | "Hoje", vencimentos, atraso e avisos usam o relógio do servidor e o calendário civil de America/Fortaleza para todas as contas; alterar relógio ou fuso do dispositivo não muda nenhum resultado. Fuso por conta está em `backlog_v2.md` |
| RNF06 | Segurança de acesso | Tráfego exclusivamente HTTPS; senha armazenada com hash adaptativo e salt; sessão expira após 30 dias sem uso; toda leitura e escrita verifica o pertencimento à conta e responde "Registro não encontrado" para registro de outra conta |
| RNF07 | Privacidade | Coletar somente os campos da seção 8; nenhuma geolocalização, agenda ou identificador de dispositivo; tela "Privacidade" com finalidade e orientação de remoção; a remoção de dados (RN19) é verificável pelos campos apagados, sem afirmação de anonimização absoluta |
| RNF08 | Integridade | Cada ação grava todos os seus efeitos ou nenhum; testes de concorrência mantêm um Ativo por objeto, no máximo duas renovações, uma recuperação e nenhum recebimento duplicado; o reenvio do mesmo formulário (duplo clique) não produz segundo efeito |
| RNF09 | Validação de entrada | Espaços externos removidos antes de contar caracteres; limites contados em caracteres; e-mail válido tem até 254 caracteres, um único "@" com partes não vazias e domínio com ponto; valor inválido não é gravado nem truncado; todas as mensagens de campo são exibidas juntas |

## 11. Decisões e premissas

O aluno **Mateus Pinheiro Silva Rodrigues** assumiu as decisões abaixo como escolhas de produto. A coluna **Origem** indica a decisão da versão de 03/11/2026 (DEC01–DEC57 antigas, preservadas no commit `468ab41`) que foi mantida, simplificada ou substituída, ou "nova" quando a escolha surgiu nesta revisão. As decisões antigas não citadas aqui foram retiradas da v1 (idempotência, versionamento, expurgo de backup, janela de retificação, causas múltiplas, desempates, agendamento de avisos) e, quando ainda fazem sentido, constam de `backlog_v2.md`. As escolhas da revisão MVP foram aprovadas pelo aluno em 05/11/2026 após análise com Claude Code (ver `prompts.md`, Interações 06 e 07).

| ID | Decisão | Alternativas consideradas | Justificativa | Origem |
|---|---|---|---|---|
| DEC01 | Só o dono usa o aplicativo; o tomador é um contato cadastrado | Conta para o tomador; tomador convidado | Menor escopo; sem autenticação de terceiros | DEC01 antiga, mantida |
| DEC02 | Aplicativo web responsivo | App nativo; só desktop | Uma plataforma documentável sem exigir implementação | DEC02 antiga, mantida |
| DEC03 | Objeto com nome 1–80, oito categorias fixas, conservação e foto opcional JPEG/PNG ≤ 5 MB | Categorias livres; foto obrigatória | Cadastro sem ambiguidade e limites verificáveis | DEC03/DEC41 antigas, mantidas |
| DEC04 | Cada objeto é uma unidade física com no máximo um empréstimo Ativo | Unidades por quantidade | Evita empréstimos conflitantes | DEC04 antiga, mantida |
| DEC05 | Conjunto é um objeto com 2–50 componentes; composição congelada após o primeiro empréstimo; parcial registra recebidos e não encerra; receber o último encerra como integral | Cada peça como objeto; encerrar com pendência; recuperação formal por peça | Responde à pergunta sobre devolução parcial com o mínimo de regras | DEC05/DEC19/DEC36/DEC53 antigas, simplificadas |
| DEC06 | Contato com nome obrigatório; telefone e observação opcionais; nenhum outro dado | Exigir telefone/e-mail | Minimização de dados pessoais | DEC06/DEC48 antigas, mantidas |
| DEC07 | Prazo por data de vencimento, de 1 a 365 dias após a saída, sugestão +7, sem prazo por categoria e sem empréstimo sem prazo | Duração numérica; prazo por categoria; sem máximo | Responde ao bloco "prazo" com valores testáveis | DEC07/DEC08 antigas, mantidas |
| DEC08 | Saída não futura e sem limite de antiguidade; vencimento ao fim do dia; empréstimo já vencido nasce atrasado; sem verificação de cronologia entre empréstimos do mesmo objeto | Limitar o passado; impedir saída anterior a retorno anterior | Permite registrar empréstimos antigos sem a cronologia complexa criticada pelo professor | DEC09/DEC38 antigas, mantidas; DEC52 antiga retirada |
| DEC09 | **Premissa da v1:** fuso America/Fortaleza e relógio do servidor para todas as contas | Fuso do dispositivo; fuso por conta | Regra temporal única e testável; fuso por conta adiado | DEC09/DEC25/DEC48 antigas, declaradas como premissa (2.3, RNF05) |
| DEC10 | Atraso a partir de 00:00 do dia seguinte ao vencimento, sem tolerância, como classificação derivada; sem multa, pontuação ou bloqueio | Tolerância de 24/48 h; estado Atrasado gravado; bloqueio automático | Determinístico e sem rotina de atualização | DEC10/DEC11/DEC27 antigas, mantidas |
| DEC11 | Avisos derivados no painel a cada consulta: "Vence hoje", "Vence em breve" (até 2 dias), "Atrasado há N dias", com contador; sem gravação, leitura ou canal externo | Avisos gerados às 09:00 e persistidos com lido/não lido (v1 anterior); e-mail/push | Satisfaz "avisar o dono" sem agendamento, reexecução nem entidade Aviso; era a maior fonte de complexidade | Nova; substitui DEC12/DEC39/DEC49/DEC56 antigas |
| DEC12 | Só o dono renova, no máximo 2 vezes, de 1 a 7 dias sobre o vencimento atual, apenas em dia; parcial não impede | Tomador pede no app; renovações ilimitadas; contar de hoje; renovar atrasado | Impede renovações indefinidas e não apaga atraso | DEC13/DEC14/DEC40 antigas, mantidas |
| DEC13 | Conservação na mesma escala no cadastro, saída, volta e recuperação; na saída copiada e ajustável, atualizando o objeto | Texto livre; congelar a do cadastro | Permite comparar saída e volta objetivamente | DEC15/DEC30 antigas, mantidas |
| DEC14 | Dano nenhum, leve ou grave, definido pelo dono, com descrição obrigatória se houver dano; gera só registro; grave torna o objeto indisponível | Quatro níveis com "inutilizado" (v1 anterior); foto do dano; acordo de reparo no app | Perde granularidade de relatório sem afetar o MVP; respeita a exclusão de cobrança | DEC16/DEC17/DEC31 antigas, simplificadas |
| DEC15 | Sem contestação nem retificação de dano no app; discordância do tomador é anotada na observação da devolução; o dano registrado é definitivo | Contestação única e retificação em 7 dias (v1 anterior) | Observação externa basta na v1; retira janela de retificação e regras de repercussão no objeto | Substitui DEC18/DEC32 antigas; detalhes em `backlog_v2.md` |
| DEC16 | Devolução com data efetiva entre saída e hoje; "no prazo" ou "com atraso" pela data efetiva | Usar a data do registro | Distingue o fato do momento da digitação | DEC33 antiga, mantida |
| DEC17 | Perda a partir do 1º dia de atraso, com motivo "perda comunicada" ou "não devolução"; Perdido é final; uma recuperação com data, conservação e dano, sem reabrir | Esperar 30 dias com aviso "Perda possível" (v1 anterior); perda a qualquer momento; reabrir o empréstimo | Segue a orientação do professor; a recuperação simples mitiga a perda marcada cedo | DEC20/DEC49 antigas alteradas; DEC21/DEC34/DEC35 antigas mantidas |
| DEC18 | Cancelamento de Ativo sem renovação nem parcial, com motivo; ação inválida é recusada sem alterar registros; sem edição livre ou exclusão de empréstimo | Exigir confirmação "não houve entrega real" (v1 anterior); não permitir cancelamento | Corrige registro indevido com uma regra | DEC26/DEC37 antigas, simplificadas |
| DEC19 | Um único campo Indisponível com motivo; dano grave e perda preenchem automaticamente; recuperação libera; dono pode ligar/desligar sempre que não houver Ativo, inclusive em objeto perdido | Causas manual e automática com precedência e liberação restrita (v1 anterior); perdido só liberado por recuperação | Sem precedência de causas, como pediu o professor; a liberação livre é limitação declarada | Nova; substitui DEC29/DEC51 antigas |
| DEC20 | Excluir objeto/contato só sem empréstimo; com histórico apenas arquivar, sem Ativo; nome/categoria/foto editáveis sempre; conservação só sem Ativo; históricos exibem nomes atuais | Exclusão em cascata; congelar nomes | Preserva rastreabilidade sem copiar dados | DEC22/DEC42/DEC44 antigas, mantidas |
| DEC21 | Sem bloqueio de contato na v1; arquivar cumpre o papel de impedir novos empréstimos | Bloqueio manual com motivo (v1 anterior); bloqueio automático por atraso | Responde explicitamente à pergunta do enunciado; funcionalidade adiada por valor | Substitui DEC23 antiga; item 1 do `backlog_v2.md` |
| DEC22 | Listas com ordenação simples, com empates resolvidos livremente pela implementação; histórico mostra os campos gravados no empréstimo, sem trilha de eventos | Desempate por ID como regra; entidade Evento por ação (v1 anterior) | Suficiente para "consultar o histórico"; perde a data de cada renovação, adiada | DEC28 antiga simplificada; DEC56 antiga retirada |
| DEC23 | Dados do contato só para identificação e acompanhamento, visíveis só ao dono; remoção simples apaga nome, telefone e observação e arquiva permanentemente; textos históricos não são saneados; sem afirmação de conformidade legal absoluta | Saneamento de textos, fotos e cópias com marcador (v1 anterior); apagar todo o histórico | Remoção explícita e barata; a limitação é declarada em vez de prometida | DEC24/DEC46 antigas, simplificadas; substitui DEC57 antiga |
| DEC24 | Conta por e-mail e senha de 8–128; cadastro, login, sair e exclusão integral mediante senha e confirmação; sem recuperação de senha; sessão, HTTPS e hash em RNF | Login social; conta do tomador; impedir exclusão com Ativos | Isolamento mínimo sem envio de e-mail; detalhes técnicos fora das RN | DEC45/DEC50 antigas, simplificadas |
| DEC25 | Requisitos técnicos (HTTPS, hash, sessão, transação atômica, duplo envio, validação de entrada, acessibilidade, desempenho) ficam em RNF, não em RN | Mantê-los como RN (v1 anterior) | Separa nível de produto de nível técnico, como criticou o professor | DEC47/DEC54/DEC55 antigas, movidas para RNF |
| DEC26 | Modelo com seis entidades: Conta, Objeto, Componente, Contato, Empréstimo, ComponenteRecebido | Dez entidades com Renovação, Evento, Aviso e ConfirmaçãoOperação (v1 anterior); sete com Evento | Comporta as nove funcionalidades e a devolução parcial; renovação vira contador e vencimento atual | Nova |
| DEC27 | Especificação dimensionada como MVP: 49 RN, 13 HU, 51 casos de borda, com núcleo v1, limites declarados e backlog v2 separados | Manter 159 RN; forçar exatamente 40 RN | Menor conjunto suficiente para as nove funcionalidades; cada RN custa desenvolvimento e teste | Nova |

**Privacidade e alcance legal:** a spec define comportamentos de produto, sem declarar conformidade jurídica. A remoção de dados (RN19) apaga identificadores do cadastro, mas textos livres históricos podem conter referências a pessoas; por isso a operação é descrita como remoção de dados pessoais, não como anonimização.

## 12. Questões em aberto

Nenhuma. As quatro escolhas que dependiam do aluno nesta revisão (momento da perda, modelo sem trilha de eventos, campo único de indisponibilidade com liberação livre, conteúdo do commit) foram decididas em 05/11/2026 e constam de DEC17, DEC19, DEC22 e DEC26. Os limites da v1 estão na seção 2.3 e não são decisões pendentes. Esta entrega contém exclusivamente a especificação e o registro de prompts; nenhum teste de aplicativo foi executado.
