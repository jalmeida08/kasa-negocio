# Objetivo
Contém informações básicas do fluxo de cadastro na plataforma.

# Visão
- 1. Cadastro espontâneo
- 2. Cadastro por link de convite

# Fluxo
- 1. Para o cadastro espontâneo:
  - O usuário deverá acessar a plataforma, onde deve acessar a opção de "Sou novo por aqui" (que deve ser um hiper link "clicável").
  - Deverá ser redirecionado para a tela de novo membro, onde as informações deve ser exibida em sessções ou staps
    - Devrá ser exibido para o usuário 3 steps:
      - Dados básico: [número de CPF ou CNPJ, nome completo, data de nascimento]
      - Endereço: [CEP, logradouro, número, bairro ou distrito, cidade, UF]
      - Contato: [Número de telefone, email]
    - Cada step deve ser validado em sua própria camada ( backend e frontend ).
    - Os steps deve iniciar bloqueados e sendo desbloqueados a partir dos preenchimento dos campos obrigatórios.
    - Sendo possível navegar em todos os staps sem perder as informações já preenchidas.
    - Sendo possível alterar o que já foi preenchido, sempre validado o que deve ser obrigatório.
    - Deve ser possível enviar o formulário após a conclusão das informações.
  - Após o envio de todas as informações o usuário deverá efetuar o cadastro de uma senha
    - A senha deve ser validada conforme as documentações **negocio/fluxo/fluxo-login.md**
    - Sistema deve processar fluxo de ativação de conta na documentação de de fluxo de ativação de cadastro **negocio/fluxo/fluxo-ativacao-cadastro.md**
- 2. Para o cadastro por convite
  - Um usuário já cadastrado e com o seu cadastro devidamente ativo podera enviar um link de convite para um email ou celular conforme documentação de **negocio/fluxo/fluxo-convite.md**
  - O usuário convidado deverá acessar o link de convite.
    - Link de convite não pode estar expirado
  - Deve abrir a mesma tela de cadastro espontánel, porém com o dado de contato de convite preenchido, exemplo:
    - Se o convite foi por email ou celular já deve aparecer a informação na tela para o usuário, não sendo possível alterar.
