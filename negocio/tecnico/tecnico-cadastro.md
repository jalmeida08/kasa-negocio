# Cabeçalho
| Campo | Valor |
|---|---|
| Documento | Técnico — Cadastro de usuário |
| Código | CAD |
| Status | Rascunho |
| Versão | 0.5 |
| Atualizado em | 2026-11-16 |
| Responsável | A definir |
| Relacionados | [negocio-cadastro.md](../negocio-cadastro.md), [fluxo-cadastro.md](../fluxo/fluxo-cadastro.md), [tecnico-login.md](tecnico-login.md) |

# 1. Objetivo
Especificar os campos do cadastro de usuário, pessoa física e jurídica (tipos, tamanhos, obrigatoriedade e validações), e as restrições técnicas.

# 2. Dados
Coluna **LGPD**: `Pessoal` = dado pessoal; `Sensível` = dado pessoal que exige proteção reforçada (criptografia e mascaramento, ver `backend/backend.md` item 8.2); `—` = não pessoal.

## 2.1. Identificação — obrigatória para todos
| Campo | Tipo | Obrigatório | Tamanho | Formato / validação | LGPD | Observação |
|---|---|---|---|---|---|---|
| CPF ou CNPJ | Texto | Sim | 11 ou 14 | **11 caracteres (CPF):** somente números. **14 caracteres (CNPJ):** alfanumérico, com letras maiúsculas (A–Z) e números; os 2 últimos caracteres (dígitos verificadores) são numéricos. Em ambos, validar os dígitos verificadores. **[AJUSTE]** | Sensível | Tamanhos 12 e 13 são inválidos. |
| Tipo de pessoa | Lista | Sim (automático) | — | `PF` (11 caracteres) ou `PJ` (14 caracteres). | — | Definido pelo tamanho do documento (RT-CAD-09). |

## 2.2. Dados básicos — pessoa física (PF)
Obrigatórios quando o tipo de pessoa é `PF`.

| Campo | Tipo | Obrigatório | Tamanho | Formato / validação | LGPD | Observação |
|---|---|---|---|---|---|---|
| Nome completo | Texto | Sim | máx. 200 | **[SUGESTÃO]** Mínimo de 3 caracteres e ao menos duas palavras. | Pessoal | **[PENDENTE: PEND-CAD-15]** |
| Data de nascimento | Data | Sim | — | Somente data, sem hora. Não pode ser futura. **[SUGESTÃO]** Idade mínima de 18 anos (RN-CAD-07). | Pessoal | RN-CAD-16. |

## 2.3. Dados básicos — pessoa jurídica (PJ)
Obrigatórios quando o tipo de pessoa é `PJ`.

| Campo | Tipo | Obrigatório | Tamanho | Formato / validação | LGPD | Observação |
|---|---|---|---|---|---|---|
| Razão social | Texto | Sim | máx. 200 | Mínimo de 3 caracteres. | — | RN-CAD-17. |
| Nome fantasia | Texto | Não | máx. 200 | Texto livre. | — | |
| Data de abertura | Data | Sim | — | Somente data, sem hora. Não pode ser futura. | — | |
| Responsável legal | Referência | Sim | — | Usuário PF com cadastro no Kasa, confirmado no cadastro da PJ. | Pessoal | RN-CAD-18 e RT-CAD-15. O nome e o CPF vêm do cadastro da PF, sem cópia. **[PENDENTE: PEND-CAD-21]** |

## 2.4. Endereço — grupo opcional
Quando o endereço é informado, os campos marcados como "Condicional" passam a ser obrigatórios.

| Campo | Tipo | Obrigatório | Tamanho | Formato / validação | LGPD | Observação |
|---|---|---|---|---|---|---|
| CEP | Texto | Condicional | 8 | Somente números. | Pessoal | **[SUGESTÃO]** Preenche o restante do endereço automaticamente (consulta pelo backend). |
| Logradouro | Texto | Condicional | máx. 200 | Texto livre. | Pessoal | |
| Número | Texto | Não | máx. 5 | Texto livre. **[SUGESTÃO]** Aceitar "S/N". | Pessoal | **[PENDENTE: PEND-CAD-18]** |
| Complemento | Texto | Não | máx. 100 | Texto livre. | Pessoal | **[SUGESTÃO]** Campo novo (apartamento, bloco, casa). |
| Bairro ou distrito | Texto | Condicional | máx. 100 | Texto livre. | Pessoal | |
| Cidade | Texto | Condicional | máx. 100 | Texto livre. | Pessoal | |
| UF | Texto | Condicional | 2 | **[AJUSTE]** Uma das 27 UFs (lista), em vez de texto livre. | Pessoal | **[PENDENTE: PEND-CAD-17]** |

## 2.5. Contato — grupo obrigatório
| Campo | Tipo | Obrigatório | Tamanho | Formato / validação | LGPD | Observação |
|---|---|---|---|---|---|---|
| Telefone | Texto | Sim | 10–11 | Somente números (DDD + número). **[SUGESTÃO]** DDD válido. | Pessoal | Fixo ou celular. |
| Tipo de telefone | Lista | Sim (automático) | — | `celular` (11 dígitos, com 9 depois do DDD) ou `fixo` (10 dígitos). | — | Define os canais de ativação disponíveis (RT-CAD-11). |
| E-mail | Texto | Sim | 7–100 | Formato de e-mail. **[SUGESTÃO]** Gravado em minúsculas e único (RN-CAD-05). | Pessoal | **[PENDENTE: PEND-CAD-19]** |

## 2.6. Controle do cadastro — **[SUGESTÃO]**
Campos preenchidos pelo sistema, não pelo usuário.

| Campo | Tipo | Obrigatório | Formato / validação | LGPD | Observação |
|---|---|---|---|---|---|
| Status da conta | Lista | Sim | Ver estados em [tecnico-login.md](tecnico-login.md). | — | Começa como `inativa`. |
| Origem do cadastro | Lista | Sim | `espontaneo` ou `convite` | — | |
| Convite de origem | Referência | Condicional | Obrigatório se a origem for `convite`. | — | Vínculo com o usuário que convidou (RF-CNV-04). O convite indica o tipo (indicação ou para imóvel) e, se for para imóvel, o papel e o código de vínculo do imóvel (RF-CNV-13, RF-CNV-23). |
| Versão dos termos aceitos | Texto | Sim | — | — | RN-CAD-06. |
| Data e hora do aceite | Data e hora | Sim | — | Pessoal | Com o IP de origem. |
| Criado em / atualizado em | Data e hora | Sim | UTC | — | |

# 3. Estados
Os estados da conta ficam em [tecnico-login.md](tecnico-login.md), seção 3.

# 4. Regras técnicas
| ID | Regra |
|---|---|
| RT-CAD-01 | Os campos com máscara na tela (CPF/CNPJ, CEP, telefone) são enviados e gravados **sem máscara**. |
| RT-CAD-02 | **[SUGESTÃO]** Os campos de texto são gravados sem espaços no início e no fim. |
| RT-CAD-03 | **[SUGESTÃO]** O CPF/CNPJ é único no banco (índice único). |
| RT-CAD-04 | **[SUGESTÃO]** O e-mail é único, sem diferenciar maiúsculas de minúsculas. |
| RT-CAD-05 | Cada etapa do cadastro tem validação própria no frontend e no backend (RF-CAD-04). **[PENDENTE: PEND-CAD-09]** |
| RT-CAD-06 | Se o grupo de endereço for informado, todos os campos condicionais tornam-se obrigatórios. |
| RT-CAD-07 | A senha nunca é gravada nem registrada em texto puro (ver [tecnico-login.md](tecnico-login.md)). |
| RT-CAD-08 | **[SUGESTÃO]** A consulta de CEP é feita pelo backend, nunca pelo frontend direto em serviço externo (regra de ouro do `frontend/frontend.md`). |
| RT-CAD-09 | O tipo de pessoa é definido pelo tamanho do documento: 11 caracteres = `PF` (CPF, somente números); 14 caracteres = `PJ` (CNPJ, alfanumérico). |
| RT-CAD-10 | O CNPJ é gravado com as letras em maiúsculas. A tela converte automaticamente o que for digitado em minúsculas. |
| RT-CAD-11 | O tipo de telefone é definido pelo número: 11 dígitos com 9 depois do DDD = `celular`; 10 dígitos = `fixo`. Telefone fixo não pode receber código por SMS nem por WhatsApp (RF-ATV-11). |
| RT-CAD-12 | Os dados básicos exigidos dependem do tipo de pessoa: seção 2.2 para PF e seção 2.3 para PJ. Campos do outro tipo são rejeitados pelo backend. |
| RT-CAD-13 | A unicidade do CPF/CNPJ e do e-mail e a lista de documentos banidos são verificadas **só no envio final** do cadastro, nunca na validação de cada etapa. A resposta é idêntica em todos os casos: mesma tela, mesmo código HTTP e tempo de resposta equivalente (RN-CAD-22). |
| RT-CAD-14 | Quando o envio é recusado por duplicidade ou banimento, nenhuma conta é criada e nenhum código de ativação é enviado. O titular da conta existente recebe o aviso MSG-CAD-07 pelo canal verificado. **[SUGESTÃO]** No máximo 1 aviso por hora para o mesmo titular, para não virar spam. |
| RT-CAD-15 | Cada CNPJ tem uma única credencial de acesso (RN-CAD-21). O responsável legal é uma referência ao usuário PF: a PJ não guarda cópia do nome nem do CPF dele. |

# 5. Validações
| Validação | Frontend | Backend |
|---|---|---|
| Obrigatoriedade por etapa e por tipo de pessoa | ✅ | ✅ |
| Tamanho e formato dos campos | ✅ | ✅ |
| Dígitos verificadores de CPF e CNPJ (incluindo o CNPJ alfanumérico) | ✅ | ✅ |
| CPF/CNPJ e e-mail já cadastrados, e documento banido | — | ✅ (só no envio final, RT-CAD-13) |
| Responsável legal confirmado (PJ) | — | ✅ |
| Idade mínima (PF) e datas não futuras | ✅ | ✅ |
| UF válida e CEP existente | ✅ (lista de UFs) | ✅ |
| Tipo de telefone (fixo ou celular) | ✅ | ✅ |
| Convite válido (no cadastro por convite) | — | ✅ |

# 6. Rotinas, integrações e eventos
| Item | Tipo | Descrição |
|---|---|---|
| Consulta de CEP | Integração | **[SUGESTÃO]** Serviço externo de CEP, consultado pelo backend. |
| Conta criada | Evento | **[SUGESTÃO]** Dispara o início da ativação e atualiza o convite para `aceito`. |
| Conta ativada por convite para imóvel | Evento | Cria o vínculo com o imóvel e o papel do convite (RF-CNV-14). |
| Tentativa de cadastro duplicado | Evento | Envia o aviso MSG-CAD-07 ao titular da conta existente (RT-CAD-14). |
| Limpeza de contas não ativadas | Rotina | **[SUGESTÃO]** Exclui as contas inativas há mais de 30 dias (PEND-CAD-08). |

# 7. Segurança e LGPD
- O CPF/CNPJ é criptografado em repouso e aparecem mascarados nas telas (ex.: `***.456.789-**`), exceto para o próprio titular.
- Os dados pessoais nunca aparecem em logs, URLs ou ferramentas de analytics (`frontend/frontend.md` e `backend/backend.md`).
- Limite de tentativas por IP e captcha no cadastro, como proteção adicional à resposta neutra (RT-CAD-13).
- O aceite dos termos é registrado com versão, data, hora e IP.

# 8. Pendências e pontos a especificar
| ID | Ponto | Por que importa | Sugestão |
|---|---|---|---|
| PEND-CAD-15 | Qual o tamanho mínimo do nome? Exigir nome e sobrenome? | Evita cadastros com nome incompleto. | Mínimo de 3 caracteres e ao menos duas palavras (PF). |
| PEND-CAD-17 | Confirmar a troca da UF de texto livre por lista. | Texto livre permite valores inválidos (ex.: "XX"). | Lista das 27 UFs. |
| PEND-CAD-18 | O número com 5 caracteres basta? | Endereços rurais ou com "KM" podem passar disso. | Aumentar para 10 caracteres. |
| PEND-CAD-19 | O e-mail com no máximo 100 caracteres basta? | O padrão permite até 254. Raramente faz diferença. | Manter 100 ou aumentar para 254. |

## Pendências resolvidas
| ID | Decisão | Data |
|---|---|---|
| PEND-CAD-14 | O documento aceita caracteres alfanuméricos quando tem mais de 11 caracteres (CNPJ, 14). Com 11 caracteres (CPF), só números. | 2026-11-13 |
| PEND-CAD-16 | O telefone pode ser fixo ou celular. Com telefone fixo, o usuário não recebe código por SMS nem por WhatsApp (RT-CAD-11, RF-ATV-11). | 2026-11-13 |

# 9. Histórico
| Data | Versão | Alteração | Autor |
|---|---|---|---|
| — | 0.1 | Criação, como `Informacoes-tecnicas-cadastro.md` | Autor original |
| 2026-11-13 | 0.2 | Arquivo renomeado para `tecnico-cadastro.md`. Padronização no modelo técnico; tabelas de campos; coluna LGPD; regras técnicas; validações; pendências. **[AJUSTE]** UF passa a ser lista (ver PEND-CAD-17). **[AJUSTE]** Campos do endereço passam a ser "Condicionais" em vez de "obrigatórios", deixando claro que só valem quando o endereço é informado. | Claude |
| 2026-11-13 | 0.3 | Resolvidas PEND-CAD-14 (CNPJ alfanumérico) e PEND-CAD-16 (telefone fixo). Dados básicos separados em PF (2.2) e PJ (2.3), conforme PEND-CAD-01 e PEND-CAD-02 de `negocio-cadastro.md`. Novos: tipo de telefone e RT-CAD-09 a RT-CAD-12. | Claude |
| 2026-11-14 | 0.4 | Decisões de PEND-CAD-12 (resposta neutra) e PEND-CAD-20 (responsável PF cadastrado, um acesso por CNPJ). **[AJUSTE]** Os campos "nome" e "CPF do responsável legal" viraram uma referência ao usuário PF. Novas RT-CAD-13 a RT-CAD-15 e o evento de tentativa duplicada. Convite de origem com tipo, imóvel e papel e evento de vínculo após a ativação (PEND-CNV-02). | Claude |
| 2026-11-16 | 0.5 | Campo "Convite de origem": o convite para imóvel guarda o papel e o código de vínculo do imóvel (RF-CNV-23). | Claude |
