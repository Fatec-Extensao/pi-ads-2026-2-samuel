# Especificação de modelagem UML

**Disciplina:** Engenharia de Software II — Prof. Rafael Hamamura  
**Sistema:** Gestão de Pesquisas por Questionários.

## Diagrama 1 — Casos de uso

O sistema tem os atores **Administrador**, **Respondente** e **Proprietário**. Os dez casos de uso UC01–UC10 estão detalhados em `../01_Requisitos/requisitos.md`.

- O administrador organiza pesquisas, categorias, questionários e acesso, monitora participações e gera relatórios agregados.
- O respondente autentica o acesso, preenche e conclui o questionário.
- O proprietário recebe resultados da pesquisa sob sua responsabilidade.
- Os relacionamentos UML `include` indicam que preencher/concluir exigem validação do acesso e que encaminhar resultados pressupõe a geração de relatório.

**Artefatos:** [diagrama de casos de uso](01_Casos_de_Uso/casos-de-uso.svg) e [fonte PlantUML](01_Casos_de_Uso/casos-de-uso.puml).

## Diagrama 2 — Classes conceituais

| Classe | Responsabilidade |
| --- | --- |
| Usuário | Representar identidade e credenciais protegidas. |
| Pesquisa | Identificar a pesquisa, seu administrador e seu proprietário. |
| Categoria | Agrupar o público e determinar o questionário aplicável. |
| Questão | Representar uma pergunta de uma categoria. |
| Opção | Representar uma alternativa de resposta válida para a questão. |
| Participação | Controlar vínculo usuário × pesquisa e estado de conclusão. |
| TokenAcesso | Autorizar temporariamente uma participação ainda pendente. |
| RespostaAnônima | Armazenar conjunto de respostas sem vínculo direto com o usuário. |
| ItemResposta | Registrar a opção marcada para cada questão. |

**Invariantes:** uma participação por par usuário/pesquisa; zero ou um token ativo por participação; token consumido após conclusão; resposta anônima sem referência a usuário/participação/token; integridade das alternativas da mesma categoria; uma opção por questão na versão acadêmica inicial.

**Relacionamentos:** Pesquisa 1:N Categoria; Categoria 1:N Questão; Questão 1:N Opção; Usuário e Pesquisa N:M por Participação; Participação 1:0..1 TokenAcesso; Categoria 1:N RespostaAnônima; RespostaAnônima 1:N ItemResposta; Opção 1:N ItemResposta.

**Artefatos:** [diagrama de classes](02_Diagrama_de_Classes/classes.svg) e [fonte PlantUML](02_Diagrama_de_Classes/classes.puml).

## Diagrama 3 — Sequência: responder e concluir questionário

Fluxo planejado com **Respondente → Interface Web → Serviço de Aplicação → PostgreSQL**.

1. Respondente informa credenciais e token.
2. Backend futuro autentica, verifica vínculo, token e participação pendente.
3. Backend consulta questões e opções da categoria e entrega ao frontend.
4. Respondente escolhe uma opção para cada questão e confirma envio.
5. Serviço revalida o contexto, inicia transação e chama a função SQL `finalizar_questionario()`.
6. O banco bloqueia token e participação, valida completude, grava resposta e itens, marca participação concluída e consome o token.
7. A transação é confirmada ou integralmente revertida; não se devolve o UUID da resposta ao usuário.

**Alternativas:** token inválido, participação previamente concluída, formulário incompleto, resposta incompatível, concorrência entre envios e incerteza em caso de conexão perdida.

**Artefatos:** [diagrama de sequência](03_Diagrama_de_Sequencia/responder-questionario.svg) e [fonte PlantUML](03_Diagrama_de_Sequencia/responder-questionario.puml).

## Situação de implementação

O diagrama de sequência separa os componentes **previstos** daqueles já representados no trabalho de banco de dados. Não há afirmação de que a interface, autenticação, backend ou autorização de serviço estejam implementados.

A modelagem tem caráter acadêmico e deve ser validada antes de uso produtivo. Embora as respostas não possuam referência direta à identidade, grupos pequenos e metadados de infraestrutura podem permitir reidentificação. Validade de tokens, canal de relatórios, política de divulgação e retenção continuam pendentes de decisão.
