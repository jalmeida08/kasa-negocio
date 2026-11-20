# Cabeçalho
| Campo | Valor |
|---|---|
| Documento | Negócio — {Assunto} |
| Código | {COD} |
| Status | Rascunho / Em revisão / Aprovado |
| Versão | 0.1 |
| Atualizado em | AAAA-MM-DD |
| Responsável | {nome} |
| Relacionados | [fluxo-{assunto}.md](fluxo/fluxo-{assunto}.md), [tecnico-{assunto}.md](tecnico/tecnico-{assunto}.md) |

# 1. Contexto
Por que este assunto existe. Qual problema do usuário ou do negócio ele resolve.

# 2. Objetivo
O que este documento especifica, em 1 a 3 frases.

# 3. Escopo
- **Dentro do escopo:** o que está coberto.
- **Fora do escopo:** o que **não** está coberto (e onde está, se estiver em outro documento).

# 4. Atores envolvidos
| Ator | Participação neste assunto |
|---|---|
| {Ator} | {O que faz aqui} |

# 5. Regras de negócio
Uma regra por linha, escrita de forma verificável (alguém consegue dizer se foi cumprida ou não).

| ID | Regra |
|---|---|
| RN-{COD}-01 | {Quem} pode/deve/não pode {o quê}, {em qual condição}. |

# 6. Permissões
| Ação | {Ator 1} | {Ator 2} | {Ator 3} |
|---|---|---|---|
| {Ação} | ✅ | ❌ | ⚠️ {condição} |

# 7. Cenários
Exemplos concretos, com personagens, que ajudam a validar as regras.

- **Cenário 1 — {título}:** {descrição}. Regras envolvidas: RN-{COD}-01.

# 8. Glossário
| Termo | Significado |
|---|---|
| {Termo} | {Definição} |

# 9. Pendências e pontos a especificar
| ID | Ponto | Por que importa | Sugestão |
|---|---|---|---|
| PEND-{COD}-01 | {Dúvida ou lacuna} | {Impacto se não for definido} | {Proposta inicial, se houver} |

## Pendências resolvidas
| ID | Decisão | Data |
|---|---|---|
| PEND-{COD}-NN | {O que foi decidido} | AAAA-MM-DD |

# 10. Histórico
| Data | Versão | Alteração | Autor |
|---|---|---|---|
| AAAA-MM-DD | 0.1 | Criação | {nome} |
