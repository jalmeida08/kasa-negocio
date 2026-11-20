# Cabeçalho
- name: Kasa — Backend
- description: Regras, permissões, bloqueios e arquitetura do backend

# Objetivo
Contém regras, permissões e bloqueios para o backend. O backend é a **única fonte da verdade** do Kasa: toda regra de negócio, permissão, cálculo financeiro, acesso a dados e geração de documentos acontece aqui. Toda regra marcada como **[BLOQUEIO]** é inegociável e deve ser barrada em code review.

# Regra de ouro
- **[BLOQUEIO]** Somente o backend acessa o banco de dados. Nenhum outro sistema, frontend ou ferramenta externa pode ter credenciais de banco.
- **[BLOQUEIO]** Toda requisição deve ser autenticada (exceto rotas públicas explicitamente listadas) e **autorizada no servidor**, independentemente do que o frontend exibe ou esconde.
- **[BLOQUEIO]** Nunca confiar em dados vindos do cliente: tudo é validado novamente no backend, inclusive IDs, valores, papéis e permissões.
- **[BLOQUEIO]** Documentos oficiais (contratos, recibos, propostas formais, relatórios financeiros) são gerados exclusivamente pelo backend.
- **[BLOQUEIO]** Segredos (senhas de banco, chaves de API, chaves de criptografia) nunca ficam no código nem no repositório. Ficam em um cofre de segredos ou em variáveis de ambiente do servidor.
- **[BLOQUEIO]** Dados pessoais nunca são escritos em logs, mensagens de erro ou respostas que não precisem deles (LGPD).

---

- 1. Arquitetura
  - 1.1. O backend deve ser organizado por **domínios de negócio** (bounded contexts), alinhados aos micro-frontends:
    - 1.1.1. `identidade` — cadastro, login, convites, sessões, papéis.
    - 1.1.2. `imoveis` — cadastro de imóveis, anúncios de aluguel e venda, fotos.
    - 1.1.3. `compartilhamento` — concessão de acesso de imóveis a corretores e escopo (gestão / gestão + financeiro).
    - 1.1.4. `propostas` — propostas de locação e compra, negociação, aceite e fechamento.
    - 1.1.5. `locacao` — contratos de locação, período de acesso do inquilino.
    - 1.1.6. `financeiro` — lançamentos, valores recebidos, previsão de recebimentos.
    - 1.1.7. `pessoas` — proprietários, inquilinos, compradores e corretores.
    - 1.1.8. `documentos` — geração e armazenamento de documentos.
    - 1.1.9. `admin` — operações do administrador e auditoria.
  - 1.2. Abordagem inicial recomendada: **monólito modular** (um deploy, módulos isolados por domínio), preparado para extração em microsserviços quando houver necessidade real de escala.
  - 1.3. **[BLOQUEIO]** Um domínio não acessa as tabelas de outro domínio. A comunicação entre domínios é feita por **interfaces públicas do módulo** ou por **eventos de domínio**.
  - 1.4. Cada domínio segue camadas bem definidas:
    ```
    <dominio>/
      api/            # controllers/rotas, DTOs de entrada e saída
      application/    # casos de uso (orquestração)
      domain/         # entidades, regras de negócio, eventos (sem dependência de framework)
      infrastructure/ # repositórios, banco, integrações externas
    ```
  - 1.5. As regras de negócio ficam na camada `domain`. Controllers não contêm regra de negócio, e a camada `domain` não depende de banco ou framework.
  - 1.6. O frontend acessa o backend por um **API Gateway / BFF** único.

- 2. API
  - 2.1. Padrão REST com JSON, sobre HTTPS obrigatório.
  - 2.2. O contrato da API deve ser documentado em **OpenAPI** e mantido junto ao código. O frontend gera seus tipos a partir dele.
  - 2.3. A API é versionada (`/api/v1/...`). Quebras de contrato exigem uma nova versão.
  - 2.4. Os nomes de recursos são no plural e em português, sem acentos (ex.: `/imoveis`, `/propostas`, `/convites`).
  - 2.5. O formato de erro é padronizado (baseado na RFC 7807):
    ```json
    {
      "type": "validacao",
      "title": "Dados inválidos",
      "status": 422,
      "erros": [{ "campo": "cpf", "mensagem": "CPF inválido" }],
      "correlationId": "..."
    }
    ```
  - 2.6. Os códigos HTTP devem ser usados corretamente:
    - 2.6.1. `400` — requisição malformada.
    - 2.6.2. `401` — não autenticado.
    - 2.6.3. `403` — sem permissão.
    - 2.6.4. `404` — não encontrado ou sem permissão para saber que existe.
    - 2.6.5. `409` — conflito de estado.
    - 2.6.6. `422` — erro de validação.
    - 2.6.7. `429` — limite de requisições.
  - 2.7. Toda listagem é paginada, com tamanho máximo de página definido pelo servidor.
  - 2.8. Operações críticas (enviar e aceitar proposta, registrar pagamento) aceitam uma **chave de idempotência** (`Idempotency-Key`).
  - 2.9. As respostas expõem somente os campos necessários (DTOs de saída). **[BLOQUEIO]** Nunca retornar a entidade de banco diretamente.
  - 2.10. Os IDs expostos devem ser não sequenciais (ex.: UUID) para evitar enumeração.

- 3. Autenticação
  - 3.1. A sessão é entregue ao frontend em cookie `HttpOnly`, `Secure` e `SameSite`, com proteção CSRF.
  - 3.2. As senhas são armazenadas com hash forte e salt (ex.: Argon2 ou bcrypt). **[BLOQUEIO]** Nunca armazenar senha em texto puro nem com criptografia reversível.
  - 3.3. Política mínima de senha, bloqueio temporário após tentativas inválidas e limite de requisições no login.
  - 3.4. Recuperação de senha por token de uso único, com expiração curta.
  - 3.5. Os tokens de sessão têm expiração e renovação controladas, e podem ser revogados no logout ou na troca de senha.
  - 3.6. Autenticação em dois fatores disponível (obrigatória para administrador).
  - 3.7. Convites:
    - 3.7.1. Token de convite aleatório, de uso único e com expiração (ex.: 7 dias).
    - 3.7.2. O convite tem um tipo: **indicação** (só o e-mail) ou **convite para imóvel** (e-mail, imóvel e papel). Quem pode conceder cada papel depende da PEND-CNV-08 (RF-CNV-13, RF-CNV-15).
    - 3.7.3. No convite para imóvel, o vínculo com o imóvel e o papel é criado automaticamente quando o convidado sem conta conclui o cadastro e a ativação, ou quando o convidado com conta aceita a solicitação. Nenhum vínculo é criado sem esse aceite (RF-CNV-14, RF-CNV-16).
    - 3.7.4. Se o e-mail já tiver cadastro, nenhuma conta é criada nem duplicada, e nenhum link de cadastro é enviado. Na indicação, o titular recebe só um aviso (RF-CNV-12); no convite para imóvel, recebe uma solicitação de vínculo na área logada e um aviso por e-mail (RF-CNV-16).
    - 3.7.5. **Resposta neutra (DIR-01):** a resposta a quem convida é idêntica para e-mail novo ou já cadastrado (mesmo código HTTP, mesmo corpo e tempo equivalente), e a listagem de convites não pode revelar a diferença (RF-CNV-11).

- 4. Autorização e permissões
  - 4.1. O modelo combina **papéis (RBAC)** e **permissões por recurso (ACL por imóvel)**.
  - 4.2. Um usuário pode ter vários papéis ao mesmo tempo (ex.: corretor e proprietário, comprador e proprietário).
  - 4.3. As regras derivadas do documento de negócio são:
    - 4.3.1. **Proprietário:** acesso total aos seus imóveis, aos seus inquilinos e compradores e ao financeiro dos seus imóveis.
    - 4.3.2. **Corretor:** acesso aos imóveis que cadastrou e aos imóveis compartilhados com ele, conforme o escopo concedido:
      - 4.3.2.1. Escopo `gestao`: gestão do inquilino e do valor final do aluguel.
      - 4.3.2.2. Escopo `gestao_financeira`: o escopo `gestao` mais os valores recebidos e a previsão de valores a receber.
    - 4.3.3. **Inquilino:** acesso somente ao imóvel locado, **dentro do período de vigência** definido pelo proprietário ou corretor. Na locação por tempo indeterminado, acessa também a gestão financeira do imóvel.
    - 4.3.4. **Comprador:** pode enviar propostas e mensagens e fechar propostas. Ao concluir a compra, passa a ser proprietário do imóvel.
    - 4.3.5. **Administrador:** permissão total, **com todas as ações registradas em auditoria**.
  - 4.4. A verificação de permissão acontece na camada de aplicação (caso de uso), nunca apenas na rota.
  - 4.5. **[BLOQUEIO]** Toda consulta a dados de um imóvel deve filtrar pelo vínculo do usuário com ele. Nunca buscar somente pelo ID recebido na requisição (proteção contra IDOR).
  - 4.6. A revogação de um compartilhamento ou o fim do período de locação retira o acesso imediatamente.
  - 4.7. O endpoint `/me/permissoes` retorna papéis e permissões por imóvel para o frontend montar a interface.

- 5. Regras de domínio
  - 5.1. **Propostas** seguem uma máquina de estados explícita:
    - `rascunho → enviada → em_negociacao → aceita → efetivada`
    - `enviada | em_negociacao → recusada | cancelada | expirada`
    - 5.1.1. Transições inválidas retornam `409`.
    - 5.1.2. Toda transição registra quem fez, quando e o valor proposto.
    - 5.1.3. Ao aceitar uma proposta, as demais propostas concorrentes do mesmo imóvel devem ser tratadas conforme regra definida (ex.: suspensas).
  - 5.2. **Imóveis** podem ter anúncio de aluguel, de venda ou ambos, com status (`rascunho`, `publicado`, `pausado`, `locado`, `vendido`).
  - 5.3. **Inquilino e comprador** podem ser cadastrados sem ser efetivados (pré-cadastro para propostas). A efetivação é uma ação explícita.
  - 5.4. **Transferência de propriedade:** quando uma venda é efetivada, o comprador recebe o papel de proprietário do imóvel e o histórico do proprietário anterior é preservado.
  - 5.5. As regras de domínio têm testes unitários obrigatórios.

- 6. Financeiro
  - 6.1. **[BLOQUEIO]** Valores monetários nunca usam ponto flutuante (`float`/`double`). Usar inteiro em centavos ou tipo decimal.
  - 6.2. A moeda padrão é BRL, armazenada explicitamente.
  - 6.3. Os lançamentos financeiros são **imutáveis**: correções são feitas por um lançamento de estorno ou ajuste, nunca por edição ou exclusão.
  - 6.4. Todo lançamento registra autor, data/hora, imóvel e origem (manual, contrato, integração).
  - 6.5. A previsão de recebimentos é calculada a partir dos contratos vigentes (valor, vencimento, reajuste).
  - 6.6. Reajustes de aluguel (ex.: IGP-M, IPCA) são calculados no backend, com o índice e a data de referência registrados.
  - 6.7. Os relatórios financeiros respeitam o escopo de permissão (ver 4.3.2).

- 7. Banco de dados
  - 7.1. Toda alteração de estrutura é feita por **migrations** versionadas no repositório. **[BLOQUEIO]** Não é permitido alterar o banco manualmente em produção.
  - 7.2. O usuário de banco da aplicação tem privilégio mínimo (sem permissão de DDL em produção).
  - 7.3. Toda tabela possui `id`, `criado_em`, `atualizado_em` e, quando aplicável, `criado_por`.
  - 7.4. Exclusão lógica (soft delete) para entidades de negócio, respeitando as regras de exclusão da LGPD (ver 8.7).
  - 7.5. Consultas parametrizadas ou via ORM. **[BLOQUEIO]** Não é permitido concatenar entrada do usuário em SQL.
  - 7.6. Operações que alteram mais de uma entidade devem ser transacionais.
  - 7.7. Backups automáticos, criptografados e com teste periódico de restauração.
  - 7.8. Índices para os filtros mais usados (imóvel, proprietário, status, período).

- 8. Proteção de dados (LGPD)
  - 8.1. Deve existir um **mapeamento dos dados pessoais** (quais dados, finalidade, base legal, tempo de retenção).
  - 8.2. Dados sensíveis (CPF, RG, dados bancários, documentos enviados) são **criptografados em repouso**, em nível de campo, com as chaves em um cofre de segredos.
  - 8.3. Toda comunicação usa TLS.
  - 8.4. O backend aplica o **mascaramento** dos dados antes de enviá-los, conforme a permissão de quem consulta.
  - 8.5. Consentimentos e aceites (Termos de Uso, Política de Privacidade, cookies) são registrados com versão do documento, data/hora e IP.
  - 8.6. Endpoints para os direitos do titular: acesso, correção, portabilidade (exportação) e solicitação de exclusão.
  - 8.7. A exclusão ou anonimização respeita as obrigações legais de retenção (ex.: registros financeiros e contratuais pelo prazo exigido em lei).
  - 8.8. Os acessos a dados pessoais por administrador ou corretor ficam registrados em trilha de auditoria.
  - 8.9. Ambientes de desenvolvimento e homologação **não usam dados reais** de produção. Quando necessário, os dados devem ser anonimizados.

- 9. Documentos e arquivos
  - 9.1. A geração de contratos, recibos, propostas e relatórios é feita no backend a partir de **templates versionados**.
  - 9.2. Todo documento gerado registra o template, a versão, os dados de origem e o autor.
  - 9.3. Os arquivos ficam em um storage privado (ex.: object storage). **[BLOQUEIO]** Nunca usar bucket público.
  - 9.4. O download é feito por **URL assinada e temporária**, gerada após a verificação de permissão.
  - 9.5. Os uploads são validados no servidor (tipo real do arquivo, tamanho, verificação de malware) e renomeados internamente.
  - 9.6. As fotos dos imóveis são processadas no backend (redimensionamento, formatos otimizados e remoção de metadados como geolocalização).

- 10. Validações
  - 10.1. Toda entrada é validada por schema na borda da API (DTO de entrada).
  - 10.2. As regras de negócio são validadas novamente na camada de domínio.
  - 10.3. Validações brasileiras obrigatórias: CPF/CNPJ, CEP, telefone, e-mail, datas e valores monetários.
  - 10.4. Os campos de texto têm limite de tamanho, e o conteúdo é sanitizado antes de ser armazenado ou exibido em documentos.

- 11. Eventos e processamento assíncrono
  - 11.1. Tarefas demoradas ou externas (envio de e-mail, geração de documentos, processamento de imagens) rodam em **fila**.
  - 11.2. Os eventos de domínio (ex.: `PropostaAceita`, `ConviteAceito`, `LocacaoEncerrada`) são publicados de forma confiável (ex.: padrão outbox).
  - 11.3. Os consumidores de fila são idempotentes e têm política de nova tentativa e fila de falhas (dead letter).
  - 11.4. Tarefas agendadas: expiração de convites e propostas, fim do período de acesso do inquilino e geração das previsões financeiras.

- 12. Notificações
  - 12.1. Os e-mails (convite, proposta recebida, proposta aceita, vencimento) são enviados por um serviço de envio, a partir de templates.
  - 12.2. Os e-mails não contêm dados sensíveis, apenas o link para o portal.
  - 12.3. O usuário pode gerenciar suas preferências de notificação, exceto as transacionais obrigatórias.

- 13. Segurança
  - 13.1. Seguir o **OWASP Top 10** e o **OWASP API Security Top 10**.
  - 13.2. Limite de requisições (rate limit) por IP e por usuário, mais restritivo em login, cadastro e convites.
  - 13.3. CORS restrito aos domínios do frontend.
  - 13.4. Headers de segurança configurados (HSTS, entre outros).
  - 13.5. Auditoria de vulnerabilidades das dependências e análise estática (SAST) no pipeline.
  - 13.6. As mensagens de erro em produção não expõem stack trace, SQL ou detalhes internos.

- 14. Observabilidade
  - 14.1. Logs estruturados (JSON) com `correlationId`, usuário (por ID, nunca por dado pessoal), rota, status e duração.
  - 14.2. **[BLOQUEIO]** Não registrar em logs senha, token, CPF, dados bancários ou o corpo de requisições com dados pessoais.
  - 14.3. Métricas (latência, taxa de erro, filas) e alertas para falhas críticas.
  - 14.4. Endpoints de saúde (`/health`, `/ready`).
  - 14.5. **Trilha de auditoria** separada dos logs técnicos para ações sensíveis: login, mudança de permissão, compartilhamento, aceite de proposta, lançamento financeiro, acesso a dados pessoais e ações do administrador.

- 15. Qualidade de código
  - 15.1. Tipagem estrita e lint/formatação automáticos no pipeline.
  - 15.2. Testes obrigatórios:
    - 15.2.1. Unitários para regras de domínio, máquina de estados de propostas, cálculos financeiros e regras de permissão.
    - 15.2.2. De integração para repositórios e endpoints (com banco real em container).
    - 15.2.3. De autorização: para cada endpoint, testar o acesso negado de quem **não** deveria acessar (inquilino de outro imóvel, corretor sem escopo financeiro etc.).
  - 15.3. Cobertura mínima sugerida: 90% em `domain` e 80% no geral.
  - 15.4. Todo merge exige pull request com revisão e pipeline aprovado.
  - 15.5. Conventional Commits e versionamento semântico.
  - 15.6. Todo endpoint novo ou alterado atualiza o contrato OpenAPI no mesmo pull request.

- 16. Ambientes e deploy
  - 16.1. Ambientes: desenvolvimento, homologação e produção, isolados entre si (banco, storage e segredos).
  - 16.2. Configuração por variáveis de ambiente, com os segredos vindos de um cofre de segredos.
  - 16.3. Aplicação empacotada em container, com deploy automatizado e possibilidade de rollback.
  - 16.4. As migrations rodam no pipeline de deploy, de forma compatível com a versão anterior (sem quebrar o deploy em andamento).

---

# Resumo de bloqueios
| # | Bloqueio |
|---|---|
| B1 | Acesso ao banco por qualquer coisa que não seja o backend |
| B2 | Requisição sem autorização verificada no servidor |
| B3 | Confiar em dados do cliente sem validar |
| B4 | Documentos oficiais gerados fora do backend |
| B5 | Segredos no código ou no repositório |
| B6 | Dados pessoais em logs, erros ou respostas desnecessárias |
| B7 | Domínio acessando tabelas de outro domínio |
| B8 | Retornar entidade de banco diretamente na API |
| B9 | Senha em texto puro ou criptografia reversível |
| B10 | Consulta de imóvel sem filtrar pelo vínculo do usuário (IDOR) |
| B11 | Ponto flutuante para valores monetários |
| B12 | Alteração manual de banco em produção |
| B13 | SQL concatenado com entrada do usuário |
| B14 | Bucket de arquivos público |
