# Cabeçalho
- name: Kasa — Frontend
- description: Regras, permissões, bloqueios e arquitetura do frontend

# Objetivo
Contém regras, permissões e bloqueios para o frontend. Toda regra marcada como **[BLOQUEIO]** é inegociável e deve ser barrada em code review.

# Regra de ouro
- **[BLOQUEIO]** Definitivamente é bloqueada qualquer manipulação de forma direta ao banco de dados.
- **[BLOQUEIO]** Qualquer consulta ou persistência deve ser intermediada pelo backend.
- **[BLOQUEIO]** Qualquer documento gerado (contrato, recibo, proposta, relatório financeiro) deve ser intermediado pelo backend.
- **[BLOQUEIO]** O frontend nunca é a fonte da verdade: regras de negócio, permissões e cálculos financeiros são decididos pelo backend. O frontend apenas exibe e solicita.
- **[BLOQUEIO]** Nenhum segredo (chaves de API, credenciais, tokens de serviço) pode existir no código ou nas variáveis de ambiente do frontend.

---

- 1. Arquitetura
  - 1.1. O projeto deve ser em micro-frontend e focado em módulos.
  - 1.2. Deve existir um **Shell (host)** responsável por: layout base, autenticação, roteamento principal, carregamento dos micro-frontends e menu conforme o perfil do usuário.
  - 1.3. Cada micro-frontend representa um **domínio de negócio** e deve ser independente:
    - 1.3.1. `mfe-auth` — login, cadastro ("Sou novo por aqui"), aceite de convite, recuperação de senha.
    - 1.3.2. `mfe-imoveis` — cadastro e gestão dos ativos (imóveis), anúncios de aluguel e venda.
    - 1.3.3. `mfe-propostas` — envio, negociação, aceite e fechamento de propostas.
    - 1.3.4. `mfe-financeiro` — valores recebidos, previsão de recebimentos, gestão financeira por imóvel.
    - 1.3.5. `mfe-pessoas` — inquilinos, compradores, proprietários, corretores e convites.
    - 1.3.6. `mfe-admin` — painel do administrador.
  - 1.4. Deve existir uma biblioteca compartilhada de **Design System** (componentes visuais) e uma de **utilitários** (formatação, validação, cliente HTTP).
  - 1.5. **[BLOQUEIO]** Um micro-frontend não pode importar código diretamente de outro micro-frontend. O compartilhamento é feito somente pelas bibliotecas compartilhadas.
  - 1.6. A comunicação entre micro-frontends deve ser feita por **eventos com contrato definido** (ex.: `imovel:selecionado`, `proposta:aceita`) ou pela URL/rota. Não deve existir estado global compartilhado entre eles.
  - 1.7. Dependências críticas (framework, roteador, design system) devem ser compartilhadas como singleton para evitar versões duplicadas.
  - 1.8. A falha de um micro-frontend não pode derrubar o Shell nem os demais. Cada um deve ter um tratamento de erro isolado com uma tela de fallback.
  - 1.9. Cada micro-frontend deve ter build, testes e deploy independentes.

- 2. Estrutura de módulos (padrão interno de cada micro-frontend)
  - 2.1. Organização por funcionalidade (feature), não por tipo de arquivo:
    ```
    src/
      features/
        <funcionalidade>/
          components/   # componentes visuais da funcionalidade
          hooks/        # lógica de tela
          services/     # chamadas ao backend
          schemas/      # validações
          types/        # tipagens
          index.ts      # API pública da funcionalidade
      shared/           # itens reutilizados dentro do próprio MFE
      routes/
    ```
  - 2.2. Cada funcionalidade expõe apenas o que está no `index.ts`. O acesso a arquivos internos de outra funcionalidade é proibido.
  - 2.3. Componentes visuais não fazem chamadas HTTP diretamente. As chamadas ficam na camada `services/`.

- 3. Comunicação com o backend
  - 3.1. Toda comunicação deve passar pelo **API Gateway / BFF** do backend, usando HTTPS.
  - 3.2. Deve existir um **cliente HTTP único e compartilhado** que centralize: URL base, envio de credenciais, timeout, tratamento de erros e renovação de sessão.
  - 3.3. Os contratos de API devem ser tipados e, de preferência, gerados a partir da especificação do backend (ex.: OpenAPI).
  - 3.4. Os erros devem ser tratados de forma padronizada:
    - 3.4.1. `401` — redirecionar para o login.
    - 3.4.2. `403` — exibir a tela de "sem permissão".
    - 3.4.3. `422` — exibir os erros de validação nos campos.
    - 3.4.4. `5xx` — mensagem amigável, sem expor detalhes técnicos.
  - 3.5. Ações críticas (enviar proposta, aceitar proposta, registrar pagamento) devem impedir o duplo envio (botão desabilitado e chave de idempotência).
  - 3.6. Listagens devem ser paginadas pelo backend. O frontend não deve carregar listas completas.

- 4. Autenticação e autorização
  - 4.1. **[BLOQUEIO]** Os tokens de sessão não podem ser armazenados em `localStorage` ou `sessionStorage`. Devem ficar em cookie `HttpOnly`, `Secure` e `SameSite`.
  - 4.2. As permissões do usuário são fornecidas pelo backend após o login (perfil + permissões por imóvel).
  - 4.3. O frontend usa as permissões apenas para **esconder ou desabilitar** elementos de tela e rotas. A proteção real é sempre do backend.
  - 4.4. Um usuário pode ter **múltiplos papéis** (ex.: corretor que também é proprietário, comprador que também é proprietário). A interface deve permitir alternar o contexto/papel ativo.
  - 4.5. As permissões são **por imóvel**, não apenas por perfil:
    - 4.5.1. O corretor visualiza somente os imóveis compartilhados com ele.
    - 4.5.2. A área financeira de um imóvel compartilhado só aparece se o proprietário concedeu essa permissão.
    - 4.5.3. O inquilino visualiza somente o imóvel que está locando, durante o período definido.
  - 4.6. As rotas devem ter guardas (guards) de autenticação e de permissão.
  - 4.7. A sessão deve expirar por inatividade, com aviso prévio ao usuário.
  - 4.8. O fluxo de convite deve validar o token do convite no backend antes de exibir o formulário de cadastro.

- 5. Validações
  - 5.1. Deve sempre conter validações das informações inseridas pelo usuário.
  - 5.2. As validações devem ser declaradas em **schemas** reutilizáveis (um schema por formulário/entidade).
  - 5.3. A validação no frontend serve à experiência do usuário. O backend **sempre** valida novamente.
  - 5.4. Validações obrigatórias de domínio brasileiro:
    - 5.4.1. CPF e CNPJ (dígito verificador).
    - 5.4.2. CEP (formato e busca de endereço via backend).
    - 5.4.3. Telefone e celular com DDD.
    - 5.4.4. E-mail.
    - 5.4.5. Valores monetários (BRL, sem valores negativos quando não aplicável).
    - 5.4.6. Datas (ex.: fim do contrato maior que o início).
  - 5.5. Os campos devem ter máscara de entrada, mas o valor enviado ao backend deve estar limpo (sem máscara).
  - 5.6. As mensagens de erro devem ser claras, em português e exibidas junto ao campo.
  - 5.7. Os uploads (fotos do imóvel, documentos) devem validar tipo e tamanho antes do envio, e o envio é feito para o backend.

- 6. Proteção de dados (LGPD)
  - 6.1. Deve sempre proteger os dados pessoais focando a LGPD.
  - 6.2. **Minimização:** coletar e exibir somente os dados necessários para cada tela.
  - 6.3. **Mascaramento:** dados sensíveis devem ser exibidos mascarados por padrão (ex.: CPF `***.456.789-**`), com opção de revelar somente para quem tem permissão.
  - 6.4. **[BLOQUEIO]** Não é permitido armazenar dados pessoais em `localStorage`, `sessionStorage`, IndexedDB ou cookies não essenciais.
  - 6.5. **[BLOQUEIO]** Não é permitido enviar dados pessoais em URLs (query string ou path), logs, ferramentas de analytics ou monitoramento de erros.
  - 6.6. Deve existir um banner de **consentimento de cookies**, e as ferramentas de analytics só carregam após o consentimento.
  - 6.7. O cadastro deve exigir o aceite dos **Termos de Uso** e da **Política de Privacidade**, com registro feito pelo backend.
  - 6.8. O usuário deve ter na interface acesso aos direitos do titular: visualizar, corrigir, exportar e solicitar a exclusão dos seus dados, sempre via backend.
  - 6.9. Os dados pessoais devem ser limpos da memória/cache do frontend ao fazer logout.

- 7. Segurança
  - 7.1. **[BLOQUEIO]** É proibido renderizar HTML vindo do usuário ou da API sem sanitização (ex.: `innerHTML`, `dangerouslySetInnerHTML`, `v-html`).
  - 7.2. É obrigatório configurar a **Content Security Policy (CSP)** e os headers de segurança.
  - 7.3. As dependências devem passar por auditoria de vulnerabilidades no pipeline de CI.
  - 7.4. Links externos devem usar `rel="noopener noreferrer"`.
  - 7.5. Não é permitido usar `eval` ou construir código dinamicamente.

- 8. Documentos e arquivos
  - 8.1. **[BLOQUEIO]** O frontend não gera documentos oficiais (contratos, recibos, propostas formais, relatórios financeiros).
  - 8.2. O frontend solicita a geração ao backend e faz o download por um **link temporário e assinado**.
  - 8.3. A pré-visualização de documentos deve usar o arquivo retornado pelo backend.

- 9. Estado e dados
  - 9.1. Os dados vindos do servidor devem ser gerenciados por uma camada de cache de dados do servidor (server state), com invalidação após mutações.
  - 9.2. O estado local de tela deve ficar o mais próximo possível do componente que o usa.
  - 9.3. Os filtros e a paginação de listagens devem refletir na URL (sem dados pessoais) para permitir compartilhar e voltar.

- 10. Interface, Design System e acessibilidade
  - 10.1. Todos os micro-frontends devem usar o Design System compartilhado. É proibido criar componentes base duplicados.
  - 10.2. Layout **mobile-first** e responsivo.
  - 10.3. Acessibilidade mínima **WCAG 2.1 AA** (contraste, navegação por teclado, rótulos em campos, textos alternativos em imagens).
  - 10.4. Idioma padrão `pt-BR`, com textos centralizados (preparados para internacionalização).
  - 10.5. A formatação deve seguir o padrão brasileiro: moeda `R$ 1.234,56`, data `dd/mm/aaaa`, fuso `America/Sao_Paulo`.
  - 10.6. Toda tela deve tratar os estados de **carregando**, **vazio**, **erro** e **sucesso**.
  - 10.7. Suporte a tema claro e escuro.

- 11. Qualidade de código
  - 11.1. TypeScript em modo estrito. O uso de `any` é proibido sem justificativa.
  - 11.2. Lint e formatação automáticos, validados no pipeline.
  - 11.3. Testes obrigatórios:
    - 11.3.1. Unitários para schemas de validação, utilitários e regras de exibição por permissão.
    - 11.3.2. De componente para formulários e fluxos principais.
    - 11.3.3. End-to-end para os fluxos críticos: cadastro, convite, login, cadastro de imóvel, proposta e aceite.
  - 11.4. Cobertura mínima definida por micro-frontend (sugestão: 80% em schemas e utilitários).
  - 11.5. Todo merge exige pull request com revisão e pipeline aprovado.
  - 11.6. Padrão de commits (ex.: Conventional Commits) e versionamento semântico das bibliotecas compartilhadas.

- 12. Performance
  - 12.1. Os micro-frontends e as rotas devem ser carregados sob demanda (lazy loading).
  - 12.2. As imagens dos imóveis devem ser otimizadas (formatos modernos, tamanhos responsivos, carregamento tardio).
  - 12.3. Deve existir um orçamento de performance (tamanho de bundle e Core Web Vitals) validado no pipeline.

- 13. Observabilidade
  - 13.1. Os erros de frontend devem ser enviados a uma ferramenta de monitoramento **sem dados pessoais** (ver 6.5).
  - 13.2. Cada requisição deve carregar um identificador de correlação para rastreio junto ao backend.
  - 13.3. Os logs de console não podem ir para produção.

- 14. Ambientes e deploy
  - 14.1. Ambientes mínimos: desenvolvimento, homologação e produção.
  - 14.2. As configurações por ambiente são públicas por natureza e não podem conter segredos (ver Regra de ouro).
  - 14.3. Cada micro-frontend é versionado e publicado de forma independente, com possibilidade de rollback.
  - 14.4. Quebras de contrato entre micro-frontends (eventos, rotas) exigem nova versão e comunicação prévia.

---

# Resumo de bloqueios
| # | Bloqueio |
|---|---|
| B1 | Acesso direto ao banco de dados |
| B2 | Consulta ou persistência sem passar pelo backend |
| B3 | Geração de documentos oficiais no frontend |
| B4 | Regra de negócio, permissão ou cálculo financeiro decidido só no frontend |
| B5 | Segredos no código ou nas variáveis do frontend |
| B6 | Importação direta entre micro-frontends |
| B7 | Token de sessão em `localStorage`/`sessionStorage` |
| B8 | Dados pessoais em storage do navegador, URL, logs ou analytics |
| B9 | Renderização de HTML sem sanitização |
