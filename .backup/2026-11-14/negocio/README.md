# Documentação de negócio — Kasa

Índice e padrão dos documentos de **negócio**, **fluxo** e **técnico**. Todo documento novo deve partir de um dos modelos em `_modelos/`.

# 1. Tipos de documento
| Tipo | Pergunta que responde | Pasta | Modelo |
|---|---|---|---|
| **Negócio** | **O quê** e **por quê**: atores, regras, permissões e cenários | `negocio/` | `_modelos/modelo-negocio.md` |
| **Fluxo** | **Como** o usuário percorre a funcionalidade: passos, desvios, mensagens | `negocio/fluxo/` | `_modelos/modelo-fluxo.md` |
| **Técnico** | **Com quais dados e restrições**: campos, tipos, tamanhos, estados, segurança | `negocio/tecnico/` | `_modelos/modelo-tecnico.md` |

Uma funcionalidade completa tem os três documentos, que se referenciam entre si.

# 2. Nomes de arquivo
- Minúsculas, sem acentos, palavras separadas por hífen.
- `negocio-{assunto}.md`, `fluxo/fluxo-{assunto}.md`, `tecnico/tecnico-{assunto}.md`.
- O documento de visão geral é `negocio.md`.

# 3. Identificadores
Cada item recebe um ID para ser citado em outros documentos, nos critérios de aceite (Gherkin) e nos testes.

| Prefixo | Uso | Exemplo |
|---|---|---|
| `RN-{COD}-NN` | Regra de negócio | `RN-CAD-03` |
| `P{N}` / `A{N}` / `E{N}` | Passo do fluxo principal / fluxo alternativo / fluxo de exceção | `P3`, `A1`, `E2` |
| `RF-{COD}-NN` | Regra do fluxo | `RF-LOG-02` |
| `MSG-{COD}-NN` | Mensagem exibida ao usuário | `MSG-CNV-01` |
| `RT-{COD}-NN` | Regra técnica | `RT-LOG-04` |
| `PEND-{COD}-NN` | Pendência ou ponto a especificar | `PEND-ATV-02` |

Códigos de assunto: `GER` (visão geral), `CAD` (cadastro), `ATV` (ativação), `CNV` (convite), `LOG` (login), `SEN` (senha).

IDs nunca são reaproveitados. Uma regra removida fica riscada (`~~RN-CAD-03~~`) com o motivo no histórico.

# 4. Marcações
| Marcação | Significado |
|---|---|
| **[SUGESTÃO]** | Item incluído como melhoria, ainda não validado pelo responsável do negócio |
| **[AJUSTE]** | Texto corrigido em relação à versão anterior (o motivo fica no histórico) |
| **[PENDENTE: PEND-XXX-NN]** | Item que depende de uma decisão listada na seção de pendências |

Ao aprovar uma sugestão, basta remover a marcação e registrar no histórico.

**Pendência resolvida:** sai da tabela de pendências abertas e vai para a tabela **Pendências resolvidas** (ID, decisão, data), na mesma seção. As regras afetadas perdem a marcação `[PENDENTE]`, e a mudança é registrada no histórico. Assim fica guardado o que foi decidido e por quê.

# 5. Status do documento
`Rascunho` → `Em revisão` → `Aprovado`. Somente documentos **Aprovados** devem ser usados como base para implementação pelos agentes. Documentos em `Rascunho` ou `Em revisão` podem ser usados para planejamento, mas as pendências precisam ser resolvidas antes do código.

# 6. Índice
| Assunto | Negócio | Fluxo | Técnico |
|---|---|---|---|
| Visão geral | [negocio.md](negocio.md) | — | — |
| Cadastro | [negocio-cadastro.md](negocio-cadastro.md) | [fluxo-cadastro.md](fluxo/fluxo-cadastro.md) | [tecnico-cadastro.md](tecnico/tecnico-cadastro.md) |
| Ativação de conta | *(em negocio-cadastro.md)* | [fluxo-ativacao-cadastro.md](fluxo/fluxo-ativacao-cadastro.md) | *faltante* |
| Convite | *(em negocio.md)* | [fluxo-convite.md](fluxo/fluxo-convite.md) | *faltante* |
| Login | — | [fluxo-login.md](fluxo/fluxo-login.md) | [tecnico-login.md](tecnico/tecnico-login.md) |
| Recuperação de senha | — | *faltante* | *(parcial em tecnico-login.md)* |

# 7. Documentos faltantes
Assuntos citados nas regras, mas ainda sem documento:

| Assunto | Tipos que faltam | Citado em |
|---|---|---|
| Recuperação e troca de senha | fluxo, técnico | `tecnico-login.md`, `fluxo-login.md` |
| Ativação de conta | técnico (código, canais, limites) | `fluxo-ativacao-cadastro.md` |
| Convite | técnico (estados, token, limites) | `fluxo-convite.md` |
| Imóveis e anúncios | negócio, fluxo, técnico | `negocio.md` (RN-GER-01) |
| Propostas | negócio, fluxo, técnico | `negocio.md` (RN-GER-03, RN-GER-11, RN-GER-17) |
| Compartilhamento com corretor | negócio, fluxo, técnico | `negocio.md` (RN-GER-04 a RN-GER-06, RN-GER-15, RN-GER-16) |
| Locação (contrato e período) | negócio, fluxo, técnico | `negocio.md` (RN-GER-08, RN-GER-09) |
| Gestão financeira | negócio, fluxo, técnico | `negocio.md` (RN-GER-02, RN-GER-09, RN-GER-16) |
| Administração e bloqueios de usuário | negócio, fluxo | `tecnico-login.md` |
