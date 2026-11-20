# Cabecalho
- description: Sistema meu imovel legal
- name: Kasa


# Introdução
O sistema deve fazer a gestão de ofertas dos anúncios de lugueis de casas, apartamentos e kitnetes, gestão de anúncio de venda de casa.

# Visão 
- 1. Atores
  - 1.1. proprietário
  - 1.2. inquilino
  - 1.3. corretor
  - 1.4. comprador
  - 1.5. administrador

- 2. Acessos
  - 2.1. Proprietário
    - 2.1.1. Pode cadastrar imóveis, fazer a gestão dos ativos (imóveis cadastrados)
    - 2.1.2. fazer a gestão financeira vinculado ao imóvel
    - 2.1.3. fazer o cadastro de inquilino ou comprador (ambos com possibilidade de efetivar ou não, para cadastro e gestão de propostas)
    - 2.1.4. conceder acesso a gestão dos imóveis a um ou N corretos, com possibilidade de dividir ou não a gestão financeira do imóvel compartilhado
    - 2.1.5. corretor observar somente os imóveis compartilhados com opção de ver a gestão financeira daquele imóvel em seu painel de gestão
  - 2.2. Inquilino
    - 2.2.1. deve ver somente o imóvel ao qual está locandopelo tempo definitivo pelo proprietário ou corretor/imobiliária
    - 2.2.2. pode ver e fazer a gestão financeira dos imóveis locados por tempo indeterminado
    - 2.2.3. pode aceitar proposta
    - 2.2.4. quando não possui cadastro prévio pode recebe convite do proprietário/corretor
  - 2.3. Corretor
    - 2.3.1. possui itens (2.1.1, 2.1.2, 2.1.3)
    - 2.3.2. pode cadastrar proprietário e enviar convites para proprietário do imóvel
    - 2.3.2. pode ser proprietário de imóvel
    - 2.3.4. quando indicado por proprietário para gestão do imóvel:
      - 2.3.4.1. pode fazer a gestão do inquilino 
      - 2.3.4.2. pode fazer a gestão do valor final do aluguel
    - 2.3.5. quando indicado por proprietário para gestão do imóvel e financeira:
      - 2.3.5.1. pode fazer a gestão de valores recebido e previsão de valores a receber
  - 2.4. Comprador
    - 2.4.1. pode enviar proposta, enviar mensagem de contato.
    - 2.4.2. pode fechar proposta em imóvel com proprietário ou corretor
    - 2.4.3. pode virar proprietário de um imóvel
    - 2.4.4. pode ser proprietário de outro imóvel
  - 2.5. Administrador
    - 2.5.1. permissão full no sistema

- 3. Cadastro e acessos a plataforma 
  - 3.1. Cadastro pode ser feito espontaneamente pelo função de cadastrar pelo portal ou por convite 
  - 3.2. Quando realizado de forma espontanea:
    - 3.2.1. Deve ir na função de "Sou novo por aqui" e deve informar os dados pessoais necessários para cadastro.
  - 3.3. Quando realizado por convite:
    - 3.3.1. Deve seguir o link recebido por e-mail até o portal e informar os dados pessoais necessários para cadastro.
  