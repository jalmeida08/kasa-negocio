# objetivo
Contém informações técnicas referente ao login e fluxos técnicos.

# Dados
- O match no momento do login deve ser númerode CPF ou CNPJ e senha
- O login deve ser realizado por autenticação e autorização utilizando a tecnologia OAuth 2.0
- Deve armazenar os dados especificado na sessão de dados técnico.
- Deve armazenar data de login da sessão atual e da última para informar ao usuário o último login.
- Deve ter flag de recuperaçã de senha
- Deve ter flag para conta ativa
- Deve flag de bloqueio temporário em caso de erro de login por mais de 3 vezes.
- Pode ter alerta bloqueante que afete o login por tempo definido por data e hora, como porexemplo:
  - Por mal uso o usuário pode sofrer bloqueio por X dias.
  - CPF pode ser banido permanentemente
- Vai pode ter perfil ou acesso de usuário padrão ou de administrador.
- Pode ter papeis conforme doucmentação de **negocio.md** conforme ator.
