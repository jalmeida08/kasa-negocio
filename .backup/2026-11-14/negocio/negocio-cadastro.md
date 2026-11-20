# Cabeçalho
| Campo | Valor |
|---|---|
| Documento | Negócio — Cadastro de usuário |
| Código | CAD |
| Status | Rascunho |
| Versão | 0.3 |
| Atualizado em | 2026-11-13 |
| Responsável | A definir |
| Relacionados | [negocio.md](negocio.md), [fluxo-cadastro.md](fluxo/fluxo-cadastro.md), [fluxo-ativacao-cadastro.md](fluxo/fluxo-ativacao-cadastro.md), [tecnico-cadastro.md](tecnico/tecnico-cadastro.md) |

# 1. Contexto
Toda pessoa, física ou jurídica, que usa o Kasa precisa de um cadastro único, independentemente dos papéis que exerce. Uma mesma pessoa pode ser proprietária de um imóvel, inquilina de outro e compradora de um terceiro.

# 2. Objetivo
Especificar os dados do cadastro de pessoa física e de pessoa jurídica, os perfis de acesso e como os papéis (atores) se acumulam.

# 3. Escopo
- **Dentro do escopo:** dados do cadastro (PF e PJ), perfis de acesso, acúmulo de papéis, cenários.
- **Fora do escopo:** o passo a passo das telas (em [fluxo-cadastro.md](fluxo/fluxo-cadastro.md)), a ativação da conta (em [fluxo-ativacao-cadastro.md](fluxo/fluxo-ativacao-cadastro.md)) e os tipos e tamanhos dos campos (em [tecnico-cadastro.md](tecnico/tecnico-cadastro.md)).

# 4. Atores envolvidos
| Ator | Participação neste assunto |
|---|---|
| Visitante | Faz o cadastro espontâneo. |
| Convidado | Faz o cadastro a partir de um convite. |
| Proprietário, inquilino, corretor, comprador | Papéis que o usuário cadastrado pode exercer, acumulados ou não. |
| Administrador | Perfil restrito a quem administra o sistema. |

# 5. Regras de negócio

## 5.1. Dados do cadastro (todos)
| ID | Regra |
|---|---|
| RN-CAD-01 | Todo usuário tem os dados organizados em três grupos: **dados básicos** (que variam entre pessoa física e jurídica, ver 5.2 e 5.3), **endereço** (CEP, logradouro, número, bairro ou distrito, cidade, UF) e **contato** (telefone, e-mail). **[AJUSTE]** |
| RN-CAD-02 | O cadastro é de **pessoa física (PF)**, identificada pelo CPF, ou de **pessoa jurídica (PJ)**, identificada pelo CNPJ. O tipo é reconhecido pelo documento informado. **[AJUSTE]** |
| RN-CAD-03 | O endereço é opcional. Quando informado, deve estar completo conforme [tecnico-cadastro.md](tecnico/tecnico-cadastro.md). |
| RN-CAD-04 | No contato, o telefone e o e-mail são obrigatórios. O telefone pode ser fixo ou celular (ver RF-ATV-11). |
| RN-CAD-05 | **[SUGESTÃO]** O CPF/CNPJ e o e-mail são únicos: não pode existir mais de um cadastro com o mesmo valor. **[PENDENTE: PEND-CAD-05]** |
| RN-CAD-06 | **[SUGESTÃO]** O cadastro exige o aceite dos Termos de Uso e da Política de Privacidade, com registro da versão aceita (LGPD). |
| RN-CAD-08 | Um cadastro só pode ser usado depois de ativado. Ver [fluxo-ativacao-cadastro.md](fluxo/fluxo-ativacao-cadastro.md). |

## 5.2. Pessoa física (PF)
| ID | Regra |
|---|---|
| RN-CAD-16 | Os dados básicos da PF são: **CPF**, **nome completo** e **data de nascimento**, todos obrigatórios. |
| RN-CAD-07 | **[SUGESTÃO]** A PF precisa ter 18 anos ou mais. **[PENDENTE: PEND-CAD-06]** |

## 5.3. Pessoa jurídica (PJ)
| ID | Regra |
|---|---|
| RN-CAD-17 | Os dados básicos da PJ são: **CNPJ** (obrigatório), **razão social** (obrigatória), **nome fantasia** (opcional) e **data de abertura** (obrigatória). |
| RN-CAD-18 | **[SUGESTÃO]** A PJ informa um **responsável legal** (nome completo e CPF), que responde pela conta. **[PENDENTE: PEND-CAD-20]** |
| RN-CAD-19 | A PJ entra na plataforma com o CNPJ e a senha, como a PF entra com o CPF (ver [fluxo-login.md](fluxo/fluxo-login.md)). |
| RN-CAD-20 | **[SUGESTÃO]** A PJ pode exercer os papéis de proprietária, inquilina e compradora (ex.: empresa que aluga um imóvel comercial). O papel de corretor para PJ depende da definição de imobiliária. **[PENDENTE: PEND-GER-02]** |

## 5.4. Perfis de acesso
| ID | Regra |
|---|---|
| RN-CAD-09 | Todo cadastro tem um perfil de acesso: **Padrão** ou **Administrador**. |
| RN-CAD-10 | O perfil Administrador é restrito a quem administra o sistema (usuário master). **[PENDENTE: PEND-CAD-04]** |

## 5.5. Papéis (atores)
| ID | Regra |
|---|---|
| RN-CAD-11 | Cada papel tem as suas responsabilidades, definidas em [negocio.md](negocio.md). |
| RN-CAD-12 | Os papéis são acumuláveis: uma pessoa pode ser proprietária, inquilina, compradora e corretora ao mesmo tempo, e ter N imóveis na sua gestão, ou nenhum. |
| RN-CAD-13 | **[SUGESTÃO]** O papel não é escolhido no cadastro. Ele surge dos vínculos: quem cadastra um imóvel vira proprietário dele, quem aceita uma locação vira inquilino, e assim por diante. |
| RN-CAD-14 | **[SUGESTÃO]** Para exercer o papel de corretor, o usuário precisa informar um registro CRECI válido. **[PENDENTE: PEND-CAD-03]** |

## 5.6. Direitos do titular (LGPD)
| ID | Regra |
|---|---|
| RN-CAD-15 | **[SUGESTÃO]** O usuário pode consultar, corrigir e exportar os seus dados e solicitar a exclusão da conta, respeitando os prazos legais de guarda de registros financeiros e contratuais. **[PENDENTE: PEND-CAD-07]** |

# 6. Permissões
| Ação | Padrão | Administrador |
|---|---|---|
| Usar as funções dos papéis que exerce | ✅ | ✅ |
| Alterar os próprios dados | ✅ **[PENDENTE: PEND-CAD-07]** | ✅ |
| Ver dados de outros usuários | ⚠️ somente o necessário dos seus vínculos | ✅ com auditoria (RN-GER-22) |
| Conceder perfil de administrador | ❌ | **[PENDENTE: PEND-CAD-04]** |

# 7. Cenários
- **Cenário 1 — Jefferson, vários papéis ao mesmo tempo:** usuário padrão (PF), proprietário de uma casa que anuncia para aluguel. Já comprou outro imóvel residencial pela plataforma e é inquilino de um imóvel comercial. Regras envolvidas: RN-CAD-12, RN-CAD-13.
- **Cenário 2 — Rita, proprietária com corretor:** proprietária (PF) de um imóvel comercial anunciado para aluguel. A gestão do aluguel é feita por um corretor. Regras envolvidas: RN-GER-04, RN-GER-15.
- **Cenário 3 — Manoel, corretor:** corretor que faz a gestão do imóvel da Rita. Pode também ter imóveis próprios. Regras envolvidas: RN-GER-05, RN-GER-14, RN-CAD-14.
- **Cenário 4 — acúmulo de papéis:** qualquer usuário pode ser, ao mesmo tempo, proprietário, inquilino, comprador e corretor, e ter N imóveis na sua gestão, ou nenhum. Regras envolvidas: RN-CAD-12.
- **Cenário 5 — empresa inquilina:** a "Padaria Pão Quente Ltda." (PJ) se cadastra com o CNPJ, informa a razão social, a data de abertura e o responsável legal, e aluga o imóvel comercial da Rita. Regras envolvidas: RN-CAD-17, RN-CAD-18, RN-CAD-20.

# 8. Glossário
| Termo | Significado |
|---|---|
| PF | Pessoa física, identificada pelo CPF. |
| PJ | Pessoa jurídica (empresa), identificada pelo CNPJ. |
| Razão social | Nome oficial da empresa, registrado na Receita Federal. |
| Nome fantasia | Nome comercial pelo qual a empresa é conhecida. |
| Responsável legal | Pessoa física que responde pela empresa na plataforma. |
| Perfil | Nível de acesso ao sistema: Padrão ou Administrador. |
| Papel | Ator que a pessoa exerce em relação a um imóvel: proprietário, inquilino, corretor ou comprador. |
| Usuário master | Administrador do sistema. |
| CRECI | Registro profissional obrigatório para corretores de imóveis no Brasil. |

# 9. Pendências e pontos a especificar
| ID | Ponto | Por que importa | Sugestão |
|---|---|---|---|
| PEND-CAD-03 | O CRECI é exigido do corretor? A plataforma valida o registro? | Atuar como corretor sem CRECI é irregular no Brasil. | Exigir o número do CRECI e a UF ao ativar o papel de corretor, com validação manual pelo administrador no início. |
| PEND-CAD-04 | Como um administrador é criado? Existe um só (master) ou vários? | Define a segurança do perfil mais poderoso. | Primeiro administrador criado na implantação; os demais concedidos por um administrador, com auditoria. |
| PEND-CAD-05 | O telefone também é único? Duas pessoas podem usar o mesmo e-mail (ex.: casal)? | Afeta a recuperação de senha e a ativação. | E-mail único; telefone pode se repetir. |
| PEND-CAD-06 | Existe idade mínima para PF? | Contratos de locação e compra exigem capacidade civil. | 18 anos. |
| PEND-CAD-07 | Quais dados o usuário pode alterar depois do cadastro? CPF/CNPJ pode ser alterado? Trocar e-mail ou telefone exige nova verificação? | Segurança da conta e integridade dos contratos. | CPF/CNPJ não pode ser alterado pelo usuário; troca de e-mail ou telefone exige verificação por código. |
| PEND-CAD-08 | O que acontece com cadastros que nunca são ativados? | LGPD: não manter dados pessoais sem finalidade. | Excluir automaticamente após 30 dias sem ativação. |
| PEND-CAD-20 | O responsável legal da PJ precisa ser um usuário cadastrado? Uma PJ pode ter mais de um usuário (ex.: sócios, funcionários)? | Define quem acessa a conta da empresa e quem assina pelos contratos. | No início, o responsável legal é só um dado (nome e CPF), e a conta da PJ tem um único acesso. Vários usuários por empresa numa fase posterior, junto com a imobiliária (PEND-GER-02). |

## Pendências resolvidas
| ID | Decisão | Data |
|---|---|---|
| PEND-CAD-01 | A data de nascimento é obrigatória para **pessoa física** (RN-CAD-16). Para PJ, não se aplica. | 2026-11-13 |
| PEND-CAD-02 | Regras separadas para PF (RN-CAD-16) e PJ (RN-CAD-17 a RN-CAD-20). A PJ informa CNPJ, razão social, nome fantasia (opcional), data de abertura e responsável legal. | 2026-11-13 |

# 10. Histórico
| Data | Versão | Alteração | Autor |
|---|---|---|---|
| 2026-11-12 | 0.1 | Criação | Autor original |
| 2026-11-13 | 0.2 | Padronização no modelo de negócio; IDs nas regras; permissões; glossário; pendências. **[AJUSTE]** RN-CAD-02: "nome e CPF" → "CPF ou CNPJ e nome completo", alinhado ao fluxo e ao técnico. | Claude |
| 2026-11-13 | 0.3 | Resolvidas PEND-CAD-01 e PEND-CAD-02: regras separadas para PF (seção 5.2) e PJ (seção 5.3). **[AJUSTE]** RN-CAD-01 e RN-CAD-02 reescritas. RN-CAD-07 passa a valer só para PF. Novas: RN-CAD-16 a RN-CAD-20, PEND-CAD-20 e cenário 5. RN-CAD-04 passa a citar o telefone fixo (RF-ATV-11). | Claude |
