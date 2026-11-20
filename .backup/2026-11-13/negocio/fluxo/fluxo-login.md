# Objetivo
Contém Informações referente ao fluxo de login de qualquer usuário idependente do perfil do usuário.

# Cenários
- 1. Quando o usuário não tiver cadastro
  - Ao informar número de CPF e senha e o mesmo não tiver cadastro na base de dados, o sistema deverá retonar uma mensagem curta, clara e de texto amigável informando que o usuário não possui cadastro, exemplo:
    - Ops.. Não foi possível localizar o seu CPF
      - Ou alguma mensagem que passei a informação clara de que o usuário não possui cadastro e pode ter a possibilidade de explorar o sistema se efetuar o cadastro.
- 2. Quando o usuário não tiver ativo
  - Ao tentar logar o usuário deve receber uma mensagem que o seu usário ainda não está ativo e encaminhar o usuário para a tela de ativação.
- 3. Quando usuário informar senha incorreta
  - Ao tentar logar e informar uma senha incorreta deve informar que a senha está incorreta.
  
# Regras bloqueante do fluxo
- Nâo deve ser permitido logar se:
  - o usuário não tiver ativo.
  - O CPF não tiver cadastro na base
  - A senha estiver incorreta
  - Nâo for informado o usuário ou a senha.

# Regra de alerta no fluxo de login
- Deve alertar o usuário log após o ligin se tiver:
  - A flag de troca de senha estiver ativa
  - Tiver algum alerta bloqueante ao usuário

# Regra de ouro
- Todas as mensagens de alertas deve ser curta, clara e com texto amigável.