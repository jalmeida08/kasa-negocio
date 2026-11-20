# Contexto
Especificação do cadastro de usuário com detalhamento por perfil.

# Objetivo
Exibir e especificar todos os passos o cadastro e perfis de cada ator que devera atuar no sistema.

# Visão
- 1. Atores
  - 1.1. Proprietário
  - 1.2. Inquilino
  - 1.3. Corretor
  - 1.4. Comprador
  - 1.5. Administrador

- 2. Perfil
    - 2.1. Padrão
    - 2.2. Administrador

# Cadastro
- Geral:
    - Os dados basicos vão se enquadrar em 3 categorias:
        - Dados básico: [número de CPF ou CNPJ, nome completo, data de nascimento]
        - Endereço: [CEP, logradouro, número, bairro ou distrito, cidade, UF]
        - Contato: [Número de telefone, email]
    - Todos e qualquer usuário deverá conter essas informações, sendo não obrigatório os dados de endereço, porém, para os dados básicos nome, e número de CPF deve ser preenchidos obrigatoriamente, já para os dados de contato o email e telefone deve ser de preenchimento obrigatório.
- Todo e qualquer cadastrato deverá conter um perfil, sendo que o perfil de administrador é restrito somente a quem administra o sistema, deverá ser o usuário master.
- Cada perfil terá sua responsabilidade, podendo ser comulativo ou não, como o exemplo citado na sessão de cenários.

# Cenários
1. Alguns dos cenários ipoteticos que pode acontecer no sistema.
- Uma pessoa com o nome de Jefferson, um usuário padrão que atua como proprietário de casa, que disponibiliza para alugúem, já efetuou compras de outro imóvel residêncial e possui um imóvel comercial locado (atuando como inquilino).
- Uma pessoa com nome de Rita que é proprietária de um imóvel comercial e disponibiliza para aluguel e tem uma corretora que gerencial o aluguel.
- Uma pessoa com nome de Manoel que é corretor e gerencial o imóvel da pessoa chamada Rita. 
- Qualquer pessoa ou usuário pode acomular atores/funções ou efetuar ações, como por exemplo:
    - Uma pessoa corretora/proprietária pode possui em sua gestão N imóveis ou não.
    - Pode ser proprietário
    - Pode ser inquilino
    - Pode ser comprador
    - Pode ser corretora.


