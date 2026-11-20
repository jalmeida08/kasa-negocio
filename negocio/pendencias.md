# Cabeçalho
| Campo | Valor |
|---|---|
| Documento | Painel de pendências, bloqueios e melhorias |
| Status | Rascunho |
| Versão | 0.1 |
| Atualizado em | 2026-11-16 |
| Responsável | A definir |
| Fontes | Tabelas de pendências de todos os documentos de `negocio/` e a [auditoria de 2026-11-14](_revisoes/revisao-2026-11-14-auditoria-geral.md) |

# 1. Como usar
- Este arquivo **reúne** o que está em aberto. A **fonte da verdade** continua sendo a tabela de pendências de cada documento: a decisão é registrada lá (skill `/modelar`) e depois atualizada aqui.
- A **prioridade** é uma sugestão do Claude, conforme o que cada item trava. Pode ser mudada pelo responsável do negócio.
- Itens resolvidos saem deste painel; o registro fica em "Pendências resolvidas" do documento de origem.

| Prioridade | Quando |
|---|---|
| **Alta** | Trava uma regra já decidida, envolve segurança, LGPD ou jurídico, ou impede começar um assunto central. |
| **Média** | Define comportamento ou tela, mas não trava outras decisões. |
| **Baixa** | Ajuste de valor, tamanho de campo ou texto. |

# 2. Resumo
| Onde | Pendências abertas |
|---|---|
| [negocio.md](negocio.md) (GER) | 11 |
| [negocio-cadastro.md](negocio-cadastro.md), [fluxo-cadastro.md](fluxo/fluxo-cadastro.md), [tecnico-cadastro.md](tecnico/tecnico-cadastro.md) (CAD) | 15 |
| [fluxo-ativacao-cadastro.md](fluxo/fluxo-ativacao-cadastro.md) (ATV) | 4 |
| [fluxo-convite.md](fluxo/fluxo-convite.md) (CNV) | 3 |
| [fluxo-login.md](fluxo/fluxo-login.md), [tecnico-login.md](tecnico/tecnico-login.md) (LOG) | 13 |
| **Total** | **46** |

Além delas: 3 bloqueios críticos da auditoria (seção 3), 3 interpretações a confirmar (seção 4) e os pontos a melhorar das seções 6 e 7.

# 3. Bloqueios
O que impede avançar ou implementar com segurança hoje.

| # | Bloqueio | O que trava | Como destravar | Quem decide |
|---|---|---|---|---|
| BLQ-01 | **Bloqueio temporário modelado como estado da conta** (auditoria, achado 3). Uma conta `inativa` ou `suspensa` com 3 senhas erradas vira `bloqueada_temporariamente` e, no fim do prazo, volta para `ativa`. | Login e cadastro: ativa conta sem código ou apaga uma suspensão. | O bloqueio vira atributo (`bloqueado_ate`), sem mudar o estado. Decidir junto a PEND-LOG-02. | negócio |
| BLQ-02 | **Diferença de tempo de resposta durante o bloqueio** (achado 4). Sem calcular o hash, a resposta da conta bloqueada é mais rápida e revela que o CPF/CNPJ existe (DIR-01). | Login. | Comparação fictícia de hash também durante o bloqueio (ajuste na RT-LOG-12). | correção |
| BLQ-03 | **Ativação após cadastro duplicado** (achado 5, PEND-CAD-11). O E2 do cadastro leva para a ativação, mas nenhuma conta foi criada, e a tela de ativação mostra contatos mascarados de quem já tem conta. | Cadastro e ativação. | Decidir a PEND-CAD-11: tela idêntica, nenhum código chega, sem contatos mascarados no caso duplicado. | negócio |
| BLQ-04 | **Gravação do log de auditoria por procedure** (PEND-GER-12) contraria a regra de banco do projeto (procedures sem regra de negócio) e não registra quem consultou dado pessoal. | Auditoria do administrador (RN-GER-22) e LGPD. | Decidir a PEND-GER-12. | negócio |
| BLQ-05 | **Regras "decididas" que dependem de sugestões** (achados 6, 7 e 8): unicidade (PEND-CAD-05), canal verificado (PEND-ATV-01) e forma de confirmar o responsável legal (PEND-CAD-21). | Resposta neutra do cadastro, avisos ao titular (DIR-01) e cadastro de PJ. | Decidir as três pendências. | negócio |
| BLQ-06 | **Proposta em modelagem** (PEND-GER-04). É o centro do produto e não tem regra definida; o backend já antecipa uma máquina de estados. | Imóveis, locação, venda, comprador (PEND-CNV-13). | Modelar o assunto com `/modelar proposta`. | negócio |
| BLQ-07 | **Diretrizes fora de `backend/`, `frontend/` e dos agentes** (achados 11, 13 e 14). Erros `409`/`422` revelam existência; CNPJ alfanumérico ausente; agentes não leem as diretrizes nem exigem status `Aprovado`. | Qualquer implementação. | Atualizar os arquivos (lista na seção 7). Precisa de autorização para mexer fora de `negocio/`. | correção (autorização) |

# 4. Interpretações a confirmar
Decisões registradas em 2026-11-16 em que o Claude precisou interpretar a resposta.

| # | Interpretação | Onde está | Pergunta |
|---|---|---|---|
| INT-01 | Só quem é convidado para ser **inquilino** ou **proprietário** confirma o CEP. O convidado para ser **corretor** não confirma. | RF-CNV-21 | O corretor também deve confirmar o CEP? |
| INT-02 | Depois do 3º CEP errado, o aceite fica bloqueado. Cancelar o convite e avisar quem convidou ficou como `[SUGESTÃO]`. | E10 | Aprova a sugestão? |
| INT-03 | O link leva o papel e o código de vínculo **criptografados**, como pedido. Há uma `[SUGESTÃO]` de usar só um token aleatório, com os dados guardados no servidor. | RF-CNV-23 | Criptografia no link ou token aleatório? |

# 5. Pendências de negócio abertas

## 5.1. Prioridade alta
| ID | Assunto | Pergunta | Recomendação | Origem |
|---|---|---|---|---|
| PEND-CAD-05 | Cadastro | O telefone também é único? Duas pessoas podem usar o mesmo e-mail? | E-mail único, sem diferenciar maiúsculas; telefone pode se repetir. Aprovar a RN-CAD-05. | [negocio-cadastro.md](negocio-cadastro.md) |
| PEND-ATV-01 | Ativação | O que é o "canal verificado"? Os dois contatos precisam ser verificados? | O canal da ativação fica verificado; o outro é verificado no primeiro acesso, sem bloquear. Avisos nunca vão para telefone fixo. | [fluxo-ativacao-cadastro.md](fluxo/fluxo-ativacao-cadastro.md) |
| PEND-CAD-11 | Cadastro | A ativação começa logo após o cadastro? | Sim, e o gatilho no login continua. No caso duplicado, a tela é idêntica e nenhum código chega (BLQ-03). | [fluxo-cadastro.md](fluxo/fluxo-cadastro.md) |
| PEND-LOG-02 | Login | Quanto dura o bloqueio temporário? É por conta ou também por IP? | 15 minutos por conta, mais limite por IP. Decidir junto com o BLQ-01. | [fluxo-login.md](fluxo/fluxo-login.md) |
| PEND-CAD-21 | Cadastro (PJ) | Como o responsável legal confirma o vínculo com a PJ? Uma PF responde por várias PJs? Como trocar o responsável? | CPF e senha na tela da PJ, com limite de tentativas próprio, que não bloqueia o login da PF. A PF precisa estar ativa. Uma PF pode responder por várias PJs. A troca é feita pelo suporte, com comprovação. | [negocio-cadastro.md](negocio-cadastro.md) |
| PEND-CAD-22 | Administrador | Como o administrador único é criado, trocado e recuperado? A conta dele pode ser usada como proprietário ou inquilino? | Criado na implantação; procedimento de emergência documentado e auditado; conta separada da conta pessoal do dono. | [negocio-cadastro.md](negocio-cadastro.md) |
| PEND-GER-12 | Auditoria | O log é gravado por procedure no banco? | O backend grava o evento numa tabela só de inclusão; triggers como segunda camada nas tabelas sensíveis (BLQ-04). | [negocio.md](negocio.md) |
| PEND-LOG-07 | Recuperação de senha | O fluxo de recuperação de senha não existe. | Criar `fluxo-recuperacao-senha.md`, com resposta única ("Se o CPF/CNPJ estiver cadastrado, enviaremos as instruções"). | [fluxo-login.md](fluxo/fluxo-login.md) |
| PEND-GER-04 | Proposta | Como funciona a proposta (contraproposta, validade, concorrência)? | Criar `negocio-proposta.md` com a máquina de estados (BLQ-06). | [negocio.md](negocio.md) |
| PEND-GER-05 | Imóvel | O corretor cadastra imóvel de terceiro? Quem é o proprietário? | O imóvel fica ligado a um proprietário (cadastrado ou convidado), e o corretor recebe o compartilhamento. | [negocio.md](negocio.md) |
| PEND-CNV-14 | Convite | O corretor convida o dono: o imóvel já está cadastrado? O corretor continua cuidando dele depois do aceite? | O corretor cadastra e convida; no aceite, o proprietário escolhe o escopo do corretor. Decidir junto com a PEND-GER-05. | [fluxo-convite.md](fluxo/fluxo-convite.md) |
| PEND-CAD-03 | Corretor | O CRECI é exigido? A plataforma valida? | Exigir número e UF do CRECI, com validação manual pelo administrador no início. | [negocio-cadastro.md](negocio-cadastro.md) |
| PEND-CAD-06 | Cadastro | Existe idade mínima para PF? | 18 anos (capacidade civil para contratos). | [negocio-cadastro.md](negocio-cadastro.md) |
| PEND-CAD-08 | LGPD | O que acontece com cadastros nunca ativados? | Excluir após 30 dias sem ativação. | [negocio-cadastro.md](negocio-cadastro.md) |
| PEND-LOG-13 | LGPD | Por quanto tempo o histórico de acessos é guardado? | 6 meses (Marco Civil da Internet). | [tecnico-login.md](tecnico/tecnico-login.md) |

## 5.2. Prioridade média
| ID | Assunto | Pergunta | Recomendação | Origem |
|---|---|---|---|---|
| PEND-CNV-13 | Convite | O comprador pode enviar convites? Alguém convida para ser comprador? | Só indicação; ninguém é convidado para ser comprador. | [fluxo-convite.md](fluxo/fluxo-convite.md) |
| PEND-CNV-15 | Convite | O convite para corretor já diz o escopo (gestão ou gestão e financeiro)? | Sim; o proprietário pode mudar depois. | [fluxo-convite.md](fluxo/fluxo-convite.md) |
| PEND-GER-13 | Suporte | Como é o perfil de suporte? Usa CPF? Pode ser a mesma conta de inquilino ou proprietário? | Criado só pelo administrador, com CPF, segundo fator e conta separada; sem acesso ao financeiro; tudo auditado. | [negocio.md](negocio.md) |
| PEND-LOG-15 | Login | Os demais usuários podem usar segundo fator? O administrador passa para aplicativo autenticador? | Opcional para todos; aplicativo autenticador para o administrador numa fase seguinte. | [fluxo-login.md](fluxo/fluxo-login.md) |
| PEND-LOG-06 + PEND-LOG-12 | Sessão | Quanto dura a sessão? "Lembrar de mim"? Vários dispositivos? E na troca de senha? | 30 minutos de inatividade, com aviso; sem "lembrar de mim"; vários dispositivos; todas as sessões encerradas na troca de senha. As duas pendências tratam do mesmo assunto e podem ser unificadas. | [fluxo-login.md](fluxo/fluxo-login.md), [tecnico-login.md](tecnico/tecnico-login.md) |
| PEND-LOG-03 + PEND-LOG-09 | Login | A troca de senha é obrigatória antes de continuar? O que é a "flag de recuperação de senha"? | Obrigar a troca antes de liberar a área logada; tratar a flag como "troca de senha obrigatória". | [fluxo-login.md](fluxo/fluxo-login.md), [tecnico-login.md](tecnico/tecnico-login.md) |
| PEND-LOG-14 | Bloqueios | Quem aplica e remove bloqueios e banimentos? O usuário pode contestar? | Só o administrador, com motivo obrigatório e canal de contestação. | [tecnico-login.md](tecnico/tecnico-login.md) |
| PEND-LOG-10 | Papéis | Os papéis são gravados ou calculados a partir dos vínculos? | Calcular a partir dos vínculos. | [tecnico-login.md](tecnico/tecnico-login.md) |
| PEND-LOG-11 | Senha | Confirmar a política de senha (RT-SEN-01 a RT-SEN-05). | Aprovar como está. | [tecnico-login.md](tecnico/tecnico-login.md) |
| PEND-CAD-07 | Cadastro | Quais dados podem ser alterados depois? Trocar e-mail ou telefone exige verificação? | CPF/CNPJ não muda; troca de contato exige código. | [negocio-cadastro.md](negocio-cadastro.md) |
| PEND-ATV-02 | Ativação | Depois de ativar, entra direto ou faz login? | Entrar direto. | [fluxo-ativacao-cadastro.md](fluxo/fluxo-ativacao-cadastro.md) |
| PEND-ATV-03 | Ativação | Confirmar tamanho do código, tentativas e reenvios. | 6 dígitos, 5 tentativas, reenvio após 60 s, 5 envios por hora. Vale também para o segundo fator (RF-LOG-10). | [fluxo-ativacao-cadastro.md](fluxo/fluxo-ativacao-cadastro.md) |
| PEND-ATV-04 | Ativação | Qual provedor de SMS e WhatsApp? Há orçamento? | Começar com e-mail e SMS; WhatsApp depois. | [fluxo-ativacao-cadastro.md](fluxo/fluxo-ativacao-cadastro.md) |
| PEND-GER-01 | Imóvel | Quais tipos de imóvel em cada modalidade? Comercial entra? | Casa, apartamento, kitnet e comercial, em lista configurável. | [negocio.md](negocio.md) |
| PEND-GER-02 | Atores | A imobiliária é um ator? | Organização com corretores vinculados, numa fase posterior. | [negocio.md](negocio.md) |
| PEND-GER-03 | Financeiro | O que o inquilino faz na gestão financeira? | Vê vencimentos e histórico e anexa comprovantes; o registro oficial é do proprietário ou do corretor. | [negocio.md](negocio.md) |
| PEND-GER-06 | Anúncios | Os anúncios são públicos? Existe busca? | Públicos, sem dados pessoais; contato e proposta exigem login. | [negocio.md](negocio.md) |
| PEND-GER-07 | Contato | Como é a mensagem de contato? | Dentro da plataforma, sem expor telefone e e-mail até a proposta aceita. | [negocio.md](negocio.md) |
| PEND-GER-09 | Contrato | A plataforma gera o contrato? Controla reajuste? | Contrato a partir de modelo; reajuste calculado no backend. | [negocio.md](negocio.md) |
| PEND-GER-10 | Escopo | Confirmar o que fica fora do escopo inicial. | Fora: pagamento online, assinatura digital e app nativo. | [negocio.md](negocio.md) |

## 5.3. Prioridade baixa
| ID | Assunto | Pergunta | Recomendação | Origem |
|---|---|---|---|---|
| PEND-CAD-09 | Cadastro | Dados do cadastro não concluído viram rascunho? | Não salvar no servidor (LGPD). | [fluxo-cadastro.md](fluxo/fluxo-cadastro.md) |
| PEND-CAD-15 | Cadastro | Tamanho mínimo do nome? Exigir sobrenome? | 3 caracteres e duas palavras (PF). | [tecnico-cadastro.md](tecnico/tecnico-cadastro.md) |
| PEND-CAD-17 | Cadastro | UF como lista em vez de texto livre? | Lista das 27 UFs. | [tecnico-cadastro.md](tecnico/tecnico-cadastro.md) |
| PEND-CAD-18 | Cadastro | Número do endereço com 5 caracteres basta? | Aumentar para 10. | [tecnico-cadastro.md](tecnico/tecnico-cadastro.md) |
| PEND-CAD-19 | Cadastro | E-mail com até 100 caracteres basta? | Manter 100 ou subir para 254. | [tecnico-cadastro.md](tecnico/tecnico-cadastro.md) |
| PEND-LOG-04 | Login | Bloqueio impede o login ou é alerta? | Já refletido no E6: bloqueio impede o login. Só falta confirmar e fechar. | [fluxo-login.md](fluxo/fluxo-login.md) |
| PEND-LOG-08 | Login | A lista de campos técnicos está completa? | Confirmar. | [tecnico-login.md](tecnico/tecnico-login.md) |

## 5.4. Pendências que podem ser fechadas
Ficaram sem sentido depois de decisões recentes. Basta confirmar.

| ID | Por quê | Proposta de fechamento |
|---|---|---|
| PEND-CAD-10 | Perguntava qual contato fica bloqueado no convite por celular. O convite agora é só por e-mail (PEND-CNV-01, RF-CNV-25). | No cadastro por convite, só o e-mail vem preenchido e bloqueado. |
| PEND-CAD-13 | No cadastro por convite, o e-mail já foi comprovado pelo clique no link. | Marcar o e-mail como verificado e pedir só a ativação pelo telefone, ou ativar direto. Depende da PEND-ATV-01. |

# 6. Pontos a melhorar na documentação (correções, sem decisão)
Da auditoria de 2026-11-14, ainda em aberto. Podem ser aplicados de uma vez, sem perguntas.

| # | Ponto | Onde |
|---|---|---|
| MEL-01 | Citar a mensagem em cada passo que a usa (MSG-ATV-05 no P7 da ativação; MSG-LOG-08 no A2 do login). | Auditoria M1 |
| MEL-02 | Alinhar os diagramas às tabelas: ativação (E4, E5, espera do A1), login (A1 a A3), cadastro (P6, bloqueio no E7), convite (E4, E5). | Auditoria M2 |
| MEL-03 | Definir no glossário os termos de bloqueio: bloqueio temporário, bloqueio administrativo, suspensão, banimento. | Auditoria M3 |
| MEL-04 | Incluir finalidade e retenção (LGPD) no modelo técnico e nos campos que não têm. | Auditoria M5 |
| MEL-05 | Especificar o índice cego (HMAC) para buscar e garantir unicidade do CPF/CNPJ criptografado e da lista de banidos. | Auditoria M6 |
| MEL-06 | Remover "(e-mail/SMS)" das mensagens de aviso e considerar o telefone fixo (DIR-04); prever o E4 da ativação sem outro canal. | Auditoria M7 |
| MEL-07 | Marcar como `[SUGESTÃO]` o uso único do convite e o captcha onde aparecem como decididos. | Auditoria M11 |
| MEL-08 | Unificar pendências duplicadas: PEND-LOG-06/12 e o registro do telefone fixo (PEND-CAD-16 × PEND-ATV-05). | Auditoria M12 |
| MEL-09 | Definir quem guarda credencial e estado: Kasa ou Keycloak (ADR). | Auditoria M13 |
| MEL-10 | Incluir "Pendências resolvidas" no `tecnico-login.md` e a coluna "Tamanho" nas tabelas técnicas que não têm. | Auditoria B1 |
| MEL-11 | Atualizar o índice e os faltantes do README (negócio do login, recuperação de senha, avisos de segurança) e incluir este painel. | Auditoria B2 |
| MEL-12 | Corrigir o histórico 0.2 de `negocio.md`, que cita "e-mail ou celular" na RN-GER-24. | Auditoria B3 |
| MEL-13 | Redação do CNPJ: "exatamente 14 caracteres", exemplo de máscara de CNPJ e MSG-CAD-01 sem "números". | Auditoria B4 |
| MEL-14 | MSG-LOG-02 com termos vagos (depende da PEND-LOG-02) e emoji na MSG-CNV-03. | Auditoria B5 |
| MEL-15 | Aplicar a DIR-02 (PF, PJ ou ambas) nas regras fora do cadastro: atores, aceite dos termos pela PJ, convites. | Auditoria 9 |
| MEL-16 | Lacunas da PJ no cadastro: PF recém-criada (inativa) confirmando vínculo; dados da PJ durante o desvio do A5; bloqueio de má-fé pelo E7. Decidir junto com a PEND-CAD-21. | Auditoria M9 |
| MEL-17 | Documento banido recebe a MSG-CAD-07, que diz "é só entrar". Recomendação: não enviar aviso nesse caso. | Auditoria M10 |
| MEL-18 | Há 63 marcações `[SUGESTÃO]` nos documentos. Validar as que estão em regras obrigatórias antes de pedir o status `Aprovado`. | Todos |

# 7. Fora de `negocio/` (precisa de autorização)
| # | Arquivo | O que ajustar |
|---|---|---|
| EXT-01 | `backend/backend.md` | DIR-01: mesma resposta e tempo para existente e inexistente (login, cadastro, convite, recuperação), sem `409`/`422` que revelem existência. DIR-02 nos domínios de pessoas. DIR-03 no 10.3 e no 8.2. DIR-04 nas notificações. Marcar como provisórios os itens que antecipam pendências (proposta, financeiro do inquilino, corretor de terceiros). |
| EXT-02 | `frontend/frontend.md` | Sem validação assíncrona de existência; telas neutras; campos de PF e PJ pelo tamanho do documento; CNPJ alfanumérico; telefone fixo sem SMS/WhatsApp; textos com chave igual ao ID `MSG-`. |
| EXT-03 | `.claude/agents/*` | Ler a seção 8 do README (diretrizes), só implementar documento `Aprovado`, usar `negocio/_criterios/` e parar diante de `[PENDENTE]`/`[SUGESTÃO]`. Ajustes de CNPJ, HMAC e Keycloak por agente. |
| EXT-04 | `docs/stack.md` | Confirmar a proposta de stack (status PROPOSTA). |
| EXT-05 | `.claude/skills/modelar/SKILL.md` e `.claude/agents/negocio-revisor.md` | Manter este painel atualizado a cada rodada de modelagem e de revisão. |

# 8. Histórico
| Data | Versão | Alteração | Autor |
|---|---|---|---|
| 2026-11-16 | 0.1 | Criação: consolidação das 46 pendências abertas, dos bloqueios e dos pontos a melhorar da auditoria de 2026-11-14 que continuam em aberto. | Claude |
