# Objetivo
Contém informações básicas do fluxo de envio de convite para outro usuário na plataforma.

# Visão
- 1. Função de envio de convite

# Fluxo
- 1. Para o envio de convite:
  - O usuário precisa necessáriamente estar logado dentro da plataforma
  - Ir na função de administração d_a conta
    - Dentro da administração da conta vai ter a funcionalidade de envio de convite.
    - Deverá abrir uma tela para o usuário logado inserir as seguintes informações:
      - E-mail do usuário logado

# Regras:
  - Deve ser um email válido
  - Link de convite tem expiração de 7 dias a contar da data e hora do envio
  - Indicação fica atrelada ao usuário de origem
  - Um usuário pode convidar N pessoas.
  - Caso insira um e-mail já cadastrado barrar a indicação e retonar um mensagem curta, clara e com um texto amigável, não sendo possível um e-mail já cadastrado receber um convite.
  - Não é possível enviar convite para ele mesmo, exemplo:
    - Usuário Jefferson, com e-mail de jeff@hotmail.com enviar convite para jeff@hotmail.com.
      - Deve retonar uma mensagem especifica curta e clara.
