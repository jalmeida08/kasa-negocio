# Cabeçalho
| Campo | Valor |
|---|---|
| Documento | Negócio — Visão geral |
| Projeto | Kasa |
| Código | GER |
| Status | Rascunho |
| Versão | 0.5 |
| Atualizado em | 2026-11-16 |
| Responsável | A definir |
| Relacionados | [negocio-cadastro.md](negocio-cadastro.md), [README.md](README.md) |

# 1. Contexto
O Kasa é uma plataforma de gestão de imóveis que conecta **proprietários**, **inquilinos**, **corretores** e **compradores**. Ela centraliza os anúncios de aluguel e de venda, as propostas, o compartilhamento da gestão com corretores e a gestão financeira de cada imóvel.

# 2. Objetivo
Fazer a gestão de ofertas dos anúncios de aluguel de casas, apartamentos e kitnets e a gestão de anúncios de venda de casas. **[PENDENTE: PEND-GER-01]**

# 3. Escopo
- **Dentro do escopo:**
  - Cadastro e acesso à plataforma (espontâneo ou por convite).
  - Cadastro e gestão de imóveis (ativos).
  - Anúncios de aluguel e de venda.
  - Propostas de locação e de compra.
  - Compartilhamento da gestão de imóveis com corretores.
  - Gestão financeira por imóvel.
- **Fora do escopo (até nova definição):** **[SUGESTÃO]** **[PENDENTE: PEND-GER-10]**
  - Pagamento online dentro da plataforma.
  - Assinatura digital de contratos.
  - Aplicativo mobile nativo.

# 4. Atores
| Ator | Descrição |
|---|---|
| Proprietário | Dono de um ou mais imóveis. Cadastra, anuncia e faz a gestão dos seus imóveis. |
| Inquilino | Pessoa que loca um imóvel. |
| Corretor | Profissional que cadastra e faz a gestão de imóveis, próprios ou compartilhados por proprietários. **[PENDENTE: PEND-GER-02]** |
| Comprador | Pessoa interessada em comprar um imóvel. |
| Administrador | Dono do sistema, com permissão total. Existe um só (RN-GER-25). **[AJUSTE]** |

**Perfil × papel** **[SUGESTÃO]**: o **perfil** define o nível de acesso ao sistema (Padrão ou Administrador). O **papel** é o ator que a pessoa exerce em relação a um imóvel. Uma mesma pessoa pode acumular papéis (ver [negocio-cadastro.md](negocio-cadastro.md)).

# 5. Regras de negócio

## 5.1. Proprietário
| ID | Regra |
|---|---|
| RN-GER-01 | O proprietário pode cadastrar imóveis e fazer a gestão dos seus ativos (imóveis cadastrados). |
| RN-GER-02 | O proprietário pode fazer a gestão financeira vinculada a cada um dos seus imóveis. |
| RN-GER-03 | O proprietário pode cadastrar inquilinos e compradores, efetivando ou não o cadastro, para registro e gestão de propostas. |
| RN-GER-04 | O proprietário pode conceder a gestão de um imóvel a um ou mais corretores, escolhendo se a gestão financeira desse imóvel também é compartilhada. |
| RN-GER-05 | O corretor vê somente os imóveis compartilhados com ele. A gestão financeira de um imóvel compartilhado só aparece no painel do corretor se o proprietário a tiver concedido. |
| RN-GER-06 | **[SUGESTÃO]** O proprietário pode revogar o compartilhamento a qualquer momento, e o corretor perde o acesso imediatamente. |

## 5.2. Inquilino
| ID | Regra |
|---|---|
| RN-GER-07 | O inquilino vê somente os imóveis que está locando. |
| RN-GER-08 | Na locação por **tempo determinado**, o inquilino tem acesso ao imóvel somente durante o período definido pelo proprietário ou pelo corretor. **[AJUSTE]** |
| RN-GER-09 | Na locação por **tempo indeterminado**, o inquilino pode ver e fazer a gestão financeira do imóvel locado. **[PENDENTE: PEND-GER-03]** |
| RN-GER-10 | O proprietário ou o corretor pode convidar uma pessoa para ser inquilina de um imóvel (**convite para imóvel**), tenha ela cadastro ou não. Quem não tem conta se cadastra pelo link e já entra vinculado; quem tem conta aceita ou recusa a solicitação. Ver [fluxo-convite.md](fluxo/fluxo-convite.md). **[AJUSTE]** |
| RN-GER-11 | O inquilino pode aceitar uma proposta. **[PENDENTE: PEND-GER-04]** |

## 5.3. Corretor
| ID | Regra |
|---|---|
| RN-GER-12 | O corretor tem as mesmas permissões das regras RN-GER-01, RN-GER-02 e RN-GER-03. **[PENDENTE: PEND-GER-05]** |
| RN-GER-13 | O corretor pode cadastrar proprietários e convidá-los para os imóveis pelos quais responde (convite para imóvel, RF-CNV-15). **[PENDENTE: PEND-CNV-14]** |
| RN-GER-14 | O corretor também pode ser proprietário de imóveis. |
| RN-GER-15 | Quando o proprietário concede a **gestão** do imóvel, o corretor pode fazer a gestão do inquilino e do valor final do aluguel. |
| RN-GER-16 | Quando o proprietário concede a **gestão e a gestão financeira** do imóvel, o corretor também pode fazer a gestão dos valores recebidos e da previsão de valores a receber. |

## 5.4. Comprador
| ID | Regra |
|---|---|
| RN-GER-17 | O comprador pode enviar propostas e mensagens de contato. **[PENDENTE: PEND-GER-04, PEND-GER-07]** |
| RN-GER-18 | O comprador pode fechar uma proposta de um imóvel com o proprietário ou com o corretor. |
| RN-GER-19 | Ao concluir a compra, o comprador passa a ser o proprietário do imóvel. |
| RN-GER-20 | O comprador pode ser proprietário de outros imóveis. |

## 5.5. Administrador
| ID | Regra |
|---|---|
| RN-GER-21 | O administrador é o **dono do sistema** e tem **permissão total**, inclusive sobre os dados pessoais e financeiros de todos os usuários. **[AJUSTE]** |
| RN-GER-25 | Existe **um único administrador** no sistema. O perfil não pode ser concedido a outra pessoa pela plataforma (ver [negocio-cadastro.md](negocio-cadastro.md), RN-CAD-10). **[PENDENTE: PEND-CAD-22]** |
| RN-GER-22 | Todas as ações do administrador são registradas em **tabelas de log** (trilha de auditoria): **quem** fez, **o quê** e **quando**, inclusive os acessos a dados pessoais (LGPD). O objetivo é o controle do sistema. A forma de gravar o log está na **[PENDENTE: PEND-GER-12]**. **[AJUSTE]** |

## 5.6. Gerais
| ID | Regra |
|---|---|
| RN-GER-23 | Uma pessoa pode acumular papéis (proprietário, inquilino, corretor e comprador), conforme [negocio-cadastro.md](negocio-cadastro.md). |
| RN-GER-24 | O cadastro pode ser feito espontaneamente, pela opção "Sou novo por aqui", ou por convite, pelo link recebido. Ver [fluxo-cadastro.md](fluxo/fluxo-cadastro.md). **[AJUSTE]** |

# 6. Permissões
| Ação | Proprietário | Corretor | Inquilino | Comprador | Administrador |
|---|---|---|---|---|---|
| Cadastrar imóvel | ✅ | ✅ | ❌ | ❌ | ✅ |
| Gerir imóvel | ✅ os seus | ⚠️ os que cadastrou e os compartilhados | ❌ | ❌ | ✅ |
| Ver imóvel | ✅ os seus | ⚠️ os que cadastrou e os compartilhados | ⚠️ o locado, no período | ⚠️ anunciados **[PENDENTE: PEND-GER-06]** | ✅ |
| Gestão financeira do imóvel | ✅ | ⚠️ só com escopo financeiro | ⚠️ só na locação por tempo indeterminado | ❌ | ✅ |
| Gerir inquilino e valor do aluguel | ✅ | ⚠️ com escopo de gestão | ❌ | ❌ | ✅ |
| Compartilhar imóvel com corretor | ✅ | ❌ | ❌ | ❌ | ✅ |
| Cadastrar inquilino ou comprador | ✅ | ✅ | ❌ | ❌ | ✅ |
| Cadastrar proprietário | ❌ | ✅ | ❌ | ❌ | ✅ |
| Enviar indicação | ✅ | ✅ | ✅ | **[PENDENTE: PEND-CNV-13]** | ✅ |
| Enviar convite para imóvel | ⚠️ como corretor ou inquilino, nos seus imóveis | ⚠️ como proprietário ou inquilino, nos imóveis pelos quais responde | ❌ | ❌ | ✅ |
| Enviar proposta | **[PENDENTE: PEND-GER-04]** | **[PENDENTE: PEND-GER-04]** | ❌ | ✅ | — |
| Aceitar proposta | **[PENDENTE: PEND-GER-04]** | **[PENDENTE: PEND-GER-04]** | ✅ | ✅ (fechar) | — |

Legenda: ✅ permitido · ❌ não permitido · ⚠️ permitido com condição.

# 7. Cenários
Os cenários com personagens estão em [negocio-cadastro.md](negocio-cadastro.md), seção 7.

# 8. Glossário
| Termo | Significado |
|---|---|
| Imóvel / ativo | Casa, apartamento ou kitnet cadastrado na plataforma. |
| Anúncio | Oferta pública de um imóvel para aluguel ou venda. |
| Proposta | Oferta de locação ou de compra feita sobre um imóvel. |
| Efetivar | Confirmar um cadastro (de inquilino ou comprador) ou uma proposta, tornando-o oficial. |
| Compartilhamento | Concessão, pelo proprietário, da gestão de um imóvel a um corretor. |
| Escopo de gestão | O que o corretor pode fazer no imóvel compartilhado: só gestão, ou gestão e financeiro. |
| Gestão financeira | Controle dos valores recebidos e da previsão de valores a receber de um imóvel. |
| Convite | Chamado enviado por um usuário a outra pessoa, por e-mail. Tem dois tipos: indicação e convite para imóvel. **[AJUSTE]** |
| Indicação | Convite para conhecer o Kasa, sem vínculo com imóvel. |
| Convite para imóvel | Convite que liga a pessoa a um imóvel com um papel (ex.: inquilino). Também chamado de convite de vínculo. |
| Solicitação de vínculo | Pedido, na área logada, para quem já tem conta aceitar ou recusar um convite para imóvel. |

# 9. Pendências e pontos a especificar
| ID | Ponto | Por que importa | Sugestão |
|---|---|---|---|
| PEND-GER-01 | Quais tipos de imóvel entram em cada modalidade? A venda é só de casas? Imóvel comercial é aceito? | O cenário de [negocio-cadastro.md](negocio-cadastro.md) cita imóvel comercial, mas o objetivo só fala de casas, apartamentos e kitnets. | Aluguel e venda para casa, apartamento, kitnet e comercial, com o tipo como lista configurável. |
| PEND-GER-02 | A **imobiliária** é um ator? | O texto original cita "corretor/imobiliária" e "corretora". Uma imobiliária (CNPJ) pode ter vários corretores e precisa de regras próprias. | Tratar imobiliária como organização com corretores vinculados, numa fase posterior. |
| PEND-GER-03 | O que o inquilino faz na "gestão financeira" (RN-GER-09)? Ver vencimentos? Registrar pagamentos? Por que só na locação por tempo indeterminado? | Define as telas e as permissões do inquilino. | Inquilino vê vencimentos e o histórico de pagamentos e anexa comprovantes; o registro oficial é do proprietário ou do corretor. |
| PEND-GER-04 | **Em modelagem.** As regras atuais de proposta (RN-GER-03, RN-GER-11, RN-GER-17, RN-GER-18) são as iniciais e ainda estão sendo modeladas. Como funciona a proposta? Quem envia proposta ao inquilino (RN-GER-11)? Existe contraproposta? Prazo de validade? O que acontece com as outras propostas quando uma é aceita? | É o centro do produto e não tem regra definida. | Criar `negocio-proposta.md` com os estados: rascunho, enviada, em negociação, aceita, efetivada, recusada, cancelada, expirada. |
| PEND-GER-05 | O corretor cadastra imóveis de terceiros (RN-GER-12)? Nesse caso, quem é o proprietário do imóvel? | Sem isso, não dá para saber quem é o dono dos dados e do financeiro. | O imóvel cadastrado pelo corretor fica vinculado a um proprietário (cadastrado ou convidado), e o corretor recebe o compartilhamento automaticamente. |
| PEND-GER-06 | Os anúncios são públicos (visitante sem login vê)? Existe busca com filtros? | Define se existe uma área pública e o que ela mostra. | Anúncios públicos sem dados pessoais do proprietário; contato e proposta exigem login. |
| PEND-GER-07 | Como é a "mensagem de contato"? Chat dentro da plataforma, e-mail? Os contatos ficam visíveis? | Privacidade (LGPD) e escopo técnico. | Mensagens dentro da plataforma, sem expor telefone e e-mail até a proposta ser aceita. |
| PEND-GER-09 | A plataforma gera o contrato de locação? Controla reajuste anual e índice (IGP-M, IPCA)? | Afeta a gestão financeira e os documentos. | Gerar o contrato a partir de modelo e calcular reajustes no backend. |
| PEND-GER-10 | Confirmar o que fica fora do escopo inicial (seção 3). | Evita retrabalho e expectativas erradas. | Manter fora: pagamento online, assinatura digital e app nativo. |
| PEND-GER-12 | Como o log de auditoria é gravado (RN-GER-22)? A proposta é gravar por **procedure no banco**. | As regras de banco do projeto dizem que procedures e triggers não contêm regra de negócio (agente `dba-revisor`). Além disso, o banco não sabe sozinho **quem** é o usuário da aplicação, e uma **consulta** a dado pessoal (que a LGPD pede para registrar) não dispara procedure nem trigger. | O backend grava o evento (quem, o quê, quando, de onde) numa tabela de auditoria **somente inserção**. Como segunda camada, triggers no banco registram alterações nas tabelas sensíveis, recebendo o usuário da aplicação pela sessão do banco. |
| PEND-GER-13 | Como é o **perfil de suporte**? A pessoa do suporte entra com CPF? Pode usar a mesma conta como inquilino ou proprietário? O que ela pode ver e fazer? | O administrador é único (RN-GER-25). Um suporte sem perfil próprio acabaria usando a conta do dono. Misturar uso pessoal e de suporte amplia o estrago de um vazamento e confunde a auditoria. | Perfil `suporte` criado só pelo administrador, com CPF, segundo fator obrigatório e conta separada da conta pessoal. Vê só os dados necessários ao atendimento (sem financeiro), com todas as ações auditadas (RN-GER-22). |

## Pendências resolvidas
| ID | Decisão | Data |
|---|---|---|
| PEND-GER-08 | O administrador é o dono do sistema, com permissão total, inclusive sobre dados pessoais e financeiros (RN-GER-21). Existe um só administrador (RN-GER-25). | 2026-11-16 |
| PEND-GER-11 | O convite de inquilino é um **convite para imóvel**, vinculado ao imóvel e ao papel. Quem já tem conta recebe uma solicitação de vínculo para aceitar ou recusar (RN-GER-10; [fluxo-convite.md](fluxo/fluxo-convite.md), RF-CNV-13 a RF-CNV-16). | 2026-11-14 |

# 10. Histórico
| Data | Versão | Alteração | Autor |
|---|---|---|---|
| 2026-11-12 | 0.1 | Criação | Autor original |
| 2026-11-13 | 0.2 | Padronização no modelo de negócio; IDs nas regras; matriz de permissões; glossário; pendências. **[AJUSTE]** RN-GER-08: "tempo definitivo" → "tempo determinado". **[AJUSTE]** Numeração duplicada 2.3.2 corrigida. **[AJUSTE]** RN-GER-24: o convite pode chegar por e-mail ou celular (ver PEND-CNV-01). | Claude |
| 2026-11-13 | 0.3 | PEND-GER-04 marcada como **em modelagem**: as regras de proposta são as iniciais. | Claude |
| 2026-11-14 | 0.4 | Resolvida PEND-GER-11: convites separados em indicação e convite para imóvel. **[AJUSTE]** RN-GER-10 reescrita. RN-GER-13 ligada à PEND-CNV-08. Permissões de convite na matriz. Glossário: convite, indicação, convite para imóvel e solicitação de vínculo. | Claude |
| 2026-11-16 | 0.5 | Permissões de convite conforme PEND-CNV-08 (resolvida em fluxo-convite.md). **[AJUSTE]** RN-GER-13: o corretor convida proprietários pelo convite para imóvel (PEND-CNV-14). Resolvida PEND-GER-08: o administrador é o dono do sistema, com permissão total (RN-GER-21), e existe um só (nova RN-GER-25). Ator e matriz de permissões atualizados. RN-GER-22 aprovada: ações do administrador registradas em tabelas de log (quem, o quê, quando). Novas PEND-GER-12 (gravação do log por procedure) e PEND-GER-13 (perfil de suporte). | Claude |
