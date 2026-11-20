# Objetivo
Contém informações técnicas de cadastro de usuário, como informações do que pode ou não fazer.

# Dados
- 1. Cadastro básico: Item obrigatório
  - Número de CPF ou CNPJ
    - Campo obrigatório, deve ser uma string de tamanho mínimo de 11 e máximo de 14, não deve conter nada além de números.
  - Nome completo:
    - Campo obrigatório, deve ser uma String de tamanho máximo de 200 caracteres.
  - Data de nascimento
    - Campo obrigatório, deve ser um capo do tipo data, não deve conter hora.
- 2. Endereço: Item optativo (não obrigatório, porém quando informado deve conter os seguintes campo dentro das regras)
  - CEP
    - Campo obrigatório, deve ser uma String, deve conter somente númerico de tamanho limitado em 8 caracteres
  - Logradouro
    - Campo obrigatório, deve possuir texto livre de tamanho máximo de 200 caracteres.
  - Número
    - Campo não obrigatório, do tipo string, deve ser de texto livre de tamanho máximo de 5 caracteres.
  - Bairro ou distrito
    - Campo obrigatório, deve ser do tipo String e de texto livre, de no máximo 100 caracteres.
  - Cidade
    - Campo obrigatório, deve ser do tipo String e de texto livre, de no máximo 100 caracteres.
  - UF
    - Campo obrigatório, deve ser do tipo String e de texto livre, de no máximo 2 caracteres.
- 3. Contato: Item obrigatório
  - Número de telefone
    - Campo obrigatório, deve ser do tipo String e deve permitir somente númerico, de no máximo 11 e nínimo 10 caracteres.
  - email
    - Campo obrigatório, deve ser do tipo String e deve permitir texto(email), de no máximo 100 e nínimo 7 caracteres.

