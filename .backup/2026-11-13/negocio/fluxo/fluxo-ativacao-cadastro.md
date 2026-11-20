# Objetivo
Contem informações do fluxo de ativação de cadastro de um novo usuário.

# Fluxo de ativação de cadastro
- Ao tentar logar pela primeira vez e a conta não tiver ativa o sistema deve abrir uma tela de validaçao perguntando para onde enviar o código de validação de conta( email , whatsapp ou sms ).
- Só deve ser possível logar se a conta estiver ativa, conforme documentação de fluxo de login **negocio/fluxo/fluxo-ativacao-cadastro.md**.
- O código enviado tem prazo de 10 min para expirar.
- Sera possível ter somente um código ativo (válido)
- Ao tentar enviar outro o anterior devera ser invalidado
- Deverá ter uma rotina para invalidar código fora do prazo de validade.
