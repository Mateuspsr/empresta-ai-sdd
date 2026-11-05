# Empresta Aí — Backlog v2

**Aluno:** Mateus Pinheiro Silva Rodrigues · **Data:** 05/11/2026

Itens retirados da v1 (MVP) para serem avaliados depois do ciclo construir → medir → aprender. Nenhum é promessa da v1. A ordem segue o valor estimado para o dono; a motivação diz por que o item foi adiado. Especificação detalhada de cada item existia na versão de 03/11/2026 (commit `468ab41`) e pode ser reaproveitada.

| Prioridade | Item | Valor para o dono | Motivação do adiamento | Gatilho para entrar |
|---|---|---|---|---|
| 1 | **Bloqueio de contato** com motivo; bloqueado não recebe novos empréstimos, mantém os ativos | Evita emprestar de novo a quem não devolve | Na v1, arquivar o contato cumpre o papel; bloqueio exige marca, motivo, precedência e desbloqueio | Donos pedirem distinção entre "amigo arquivado" e "amigo bloqueado" |
| 2 | **Contestação e retificação de dano** (uma contestação por avaliação; uma retificação em até 7 dias; repercussão na indisponibilidade) | Corrige erro de registro sem apagar o histórico | Observação na devolução basta para anotar discordância; retificação trazia regras de janela e de efeito no objeto | Erros de nível de dano registrados com frequência |
| 3 | **Avisos persistidos** com lido/não lido, geração diária e eventual envio por e-mail | Lembrete mesmo sem abrir o app | Painel derivado atende "avisar o dono"; persistência exige agendamento, reexecução e entidade Aviso | Donos relatarem que esquecem de abrir o painel |
| 4 | **Higienização completa de dados pessoais** (textos livres, fotos, cópias, backups com expurgo em prazo) | Remoção mais abrangente a pedido do amigo | Remoção simples com limitação declarada é suficiente para o MVP; saneamento exigia 7 regras e marcadores | Pedidos reais de remoção envolvendo textos históricos |
| 5 | **Trilha de eventos por ação** (registro, renovação, parcial, devolução, perda, recuperação, cancelamento, observações) | Histórico cronológico completo, inclusive data de cada renovação | Campos do empréstimo já respondem "o que aconteceu"; entidade Evento adiciona modelo e regras | Necessidade de auditoria ou de relatórios por período |
| 6 | **Idempotência explícita e controle de versão de registro** (confirmação de operação, "recarregue antes de confirmar") | Protege contra duplo clique e abas antigas | RNF08 já exige que o reenvio não produza efeito duplo; o mecanismo detalhado é decisão técnica | Duplicações observadas em uso real |
| 7 | **Fuso horário por conta** | Donos fora de Fortaleza veem o "hoje" local | Fuso fixo é premissa declarada da v1; fuso por conta muda cálculo de vencimento e avisos | Usuários em outro fuso |
| 8 | **Foto do dano** na devolução e na recuperação | Evidência visual da avaria | Descrição textual basta; foto traz validação, armazenamento e saneamento | Disputas frequentes sobre o dano |
| 9 | **Recuperação ou troca de senha e de e-mail** | Evita perda de acesso | Exige envio de e-mail, fora do escopo da v1 | Primeiros relatos de perda de senha |
| 10 | **Indisponibilidade por perda liberada só por recuperação**; cronologia entre empréstimos do mesmo objeto; "inutilizado" como nível próprio | Mais rigor no ciclo do objeto | Cada um exige regra de precedência ou validação extra que o MVP dispensa | Erros operacionais causados pela liberação livre ou por datas sobrepostas |

**Decisões técnicas futuras (não são promessas da v1):** agendador diário para avisos persistidos; tabela de eventos imutável; chave de idempotência por operação; política de retenção e expurgo de backups; fuso por conta armazenado no cadastro.
