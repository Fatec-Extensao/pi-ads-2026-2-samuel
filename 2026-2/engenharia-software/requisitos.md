# Levantamento de requisitos — Projeto Integrador II (2026-2)

**Disciplina:** Engenharia de Software II  
**Professor:** Rafael Hamamura  
**Sistema:** Sistema Web para Gestão de Pesquisas por Questionários  
**Estado:** documentação de requisitos para revisão acadêmica — não comprova implementação.

## 1. Objetivo e problema

Permitir que uma instituição planeje pesquisas por questionários, organize o público em categorias, controle a participação única de pessoas autorizadas, armazene respostas sem associação direta à identidade do respondente e disponibilize estatísticas ao responsável por cada pesquisa.

O projeto integra requisitos de Engenharia de Software, Desenvolvimento Web, Banco de Dados e Segurança da Informação. O levantamento descreve **o que o sistema deverá realizar**; não deve ser confundido com funcionalidades já entregues ou com garantia de anonimato absoluto.

## 2. Atores

| Ator | Responsabilidade prevista |
| --- | --- |
| **Administrador** | Criar e organizar pesquisas, definir categorias, questões e alternativas; preparar acessos; acompanhar participações agregadas; gerar e encaminhar relatórios. |
| **Respondente** | Autenticar-se, acessar pesquisa autorizada por token, responder às questões de sua categoria e concluir uma única vez. |
| **Proprietário de pesquisa** | Receber os resultados estatísticos agregados referentes às pesquisas sob sua responsabilidade. |

O administrador e o proprietário representam papéis de negócio, potencialmente exercidos pela mesma pessoa. O mecanismo efetivo de autorização desses papéis pertence ao futuro backend.

## 3. Requisitos funcionais

| ID | Requisito | Prioridade | Critério de aceite proposto |
| --- | --- | --- | --- |
| RF01 | Cadastrar pesquisa | Alta | Administrador registra título e associa responsável à pesquisa. |
| RF02 | Definir categorias | Alta | É possível cadastrar categorias vinculadas à pesquisa. |
| RF03 | Montar questionário específico por categoria | Alta | O questionário apresenta apenas as questões associadas à categoria autorizada. |
| RF04 | Cadastrar perguntas e opções | Alta | Cada questão fechada possui alternativas válidas relacionadas à mesma questão. |
| RF05 | Preparar credenciais individuais | Alta | O acesso de respondentes usa credenciais geradas individualmente pelo sistema. |
| RF06 | Gerar token de acesso | Alta | Cada convite ativo possui token individual, guardado apenas por hash no banco. |
| RF07 | Autenticar e validar acesso | Alta | O respondente autorizado só acessa pesquisa e categoria a que foi vinculado. |
| RF08 | Exibir questionário | Alta | O formulário mostra enunciados e alternativas da categoria correspondente. |
| RF09 | Registrar respostas | Alta | Uma submissão válida registra resposta anônima e seus itens. |
| RF10 | Impedir segunda participação | Alta | A mesma pessoa não conclui duas respostas na mesma pesquisa. |
| RF11 | Consumir e desvincular o token após conclusão | Alta | A finalização invalida e exclui o vínculo operacional do token. |
| RF12 | Finalização transacional | Alta | Falhas não deixam resposta parcial, participação concluída indevidamente ou token consumido. |
| RF13 | Monitorar participação agregada | Média | O administrador consulta totais de convidados e participantes, sem vincular votos a pessoas. |
| RF14 | Gerar relatórios estatísticos | Alta | As respostas são apresentadas em contagens por pesquisa, categoria, questão e alternativa. |
| RF15 | Encaminhar resultados ao proprietário | Média | O proprietário recebe somente os resultados autorizados da pesquisa, pelo canal a definir. |
| RF16 | Sinalizar erros de token e preenchimento | Média | Tentativas inválidas ou incompletas produzem mensagem e não gravam dados definitivos. |

Os critérios acima são objetivos de aceitação documental. Seu atendimento em funcionamento requer integrações e testes do sistema completo.

## 4. Requisitos não funcionais

| ID | Categoria | Requisito / verificação planejada |
| --- | --- | --- |
| RNF01 | Privacidade | Respostas não terão ligação direta com usuário, token ou participação. Verificar tabelas, consultas e registros. |
| RNF02 | Segurança | Senhas deverão ser protegidas por algoritmo apropriado; tokens precisam ser aleatórios e persistidos como hash. |
| RNF03 | Segurança | Fluxos autenticados e permissões impedirão acesso não autorizado ou SQL arbitrário pelo cliente. |
| RNF04 | Integridade | Operações de conclusão serão atômicas, consistentes e seguras diante de concorrência. |
| RNF05 | Usabilidade | Interface com mensagens claras, campos rotulados e orientação de erros. |
| RNF06 | Responsividade | Telas adaptadas a smartphone, tablet e computador, com abordagem mobile-first. |
| RNF07 | Acessibilidade | Navegação por teclado, rótulos, contraste e estrutura semântica apropriados. |
| RNF08 | Desempenho | A aplicação deverá responder em tempo adequado; a meta numérica depende de dimensionamento e medição. |
| RNF09 | Compatibilidade | Funcionamento nos navegadores modernos definidos na homologação. |
| RNF10 | Auditabilidade | Evidências de teste e eventos operacionais sem exposição de tokens, respostas individuais ou dados identificáveis. |
| RNF11 | LGPD | Definir base legal, finalidade, minimização, retenção e exclusão antes de operação real. |
| RNF12 | Manutenibilidade | Modelo conceitual, lógica de negócio e acesso aos dados documentados e separados em componentes. |

Não foram inventados prazos, percentuais de disponibilidade, limites de usuários simultâneos ou metas de latência porque ainda faltam dados para medi-los.

## 5. Regras de negócio

| ID | Regra | Observação |
| --- | --- | --- |
| RN01 | Um mesmo respondente pode concluir **no máximo uma vez por pesquisa**. | Outras pesquisas são permitidas. |
| RN02 | O token individual permite somente o acesso ao contexto autorizado. | Token não deve ser compartilhado ou utilizado após conclusão. |
| RN03 | A conclusão registra resposta e itens, marca participação e consome o token de forma atômica. | Uma falha deve permitir rollback. |
| RN04 | A resposta anônima não tem chave estrangeira para usuário, participação ou token. | A ausência da FK não elimina todos os riscos de reidentificação. |
| RN05 | Relatórios não associam escolhas a participantes identificáveis. | Grupos pequenos exigem regra de supressão a definir. |
| RN06 | O proprietário somente recebe estatísticas das pesquisas sob sua responsabilidade. | Canal e política de envio pendentes. |
| RN07 | A versão inicial do modelo admite uma categoria por participação em cada pesquisa. | Delimitação a revisar, não requisito irrevogável. |
| RN08 | A versão inicial do banco modela questões fechadas, obrigatórias, de escolha única. | Evolução de tipos depende de decisão. |
| RN09 | A finalização não divulga o UUID da resposta junto à identidade do respondente. | Logs e metadados requerem controles adicionais. |

## 6. Casos de uso

### UC01 — Criar pesquisa
**Ator principal:** Administrador.  
**Pré-condição:** autorização administrativa válida.  
**Fluxo principal:** (1) acessar cadastro; (2) informar informações da pesquisa; (3) vincular proprietário; (4) validar; (5) salvar; (6) confirmar criação.  
**Exceções:** campos obrigatórios ausentes, referência de proprietário inválida ou falha de persistência.

### UC02 — Definir categorias
**Ator:** Administrador.  
**Fluxo:** selecionar pesquisa, cadastrar categorias destinadas aos respondentes, validar duplicidades no mesmo contexto e salvar.

### UC03 — Compor questionário
**Ator:** Administrador.  
**Fluxo:** selecionar categoria; adicionar questões; definir alternativas; conferir integridade do questionário antes da publicação.  
**Exceção:** questão sem opções válidas, contexto inválido ou pesquisa já congelada para alteração.

### UC04 — Preparar acesso
**Ator:** Administrador.  
**Fluxo:** selecionar pesquisa e respondente, definir categoria autorizada, registrar participação pendente e disponibilizar credenciais/token individual.  
**Exceção:** participação já existente ou categoria incompatível.

### UC05 — Autenticar e validar acesso
**Ator:** Respondente.  
**Fluxo:** informar credenciais e token; serviço valida identidade, vínculo e situação da participação; apresentar questionário da categoria.  
**Exceções:** credenciais inválidas, token inexistente/consumido ou participação concluída.

### UC06 — Preencher questionário
**Ator:** Respondente.  
**Pré-condição:** UC05 autorizado.  
**Fluxo:** visualizar questões, selecionar uma alternativa por questão, revisar as escolhas e solicitar conclusão.  
**Exceção:** alternativas inválidas ou questionário incompleto.

### UC07 — Concluir questionário
**Ator:** Respondente.  
**Pré-condição:** acesso válido e formulário completo.  
**Fluxo:** submeter token e respostas; bloquear participação/token; validar preenchimento; gravar resposta anônima e itens; marcar participação concluída; excluir token; confirmar transação; informar sucesso sem devolver identificador da resposta.  
**Fluxos alternativos:** falha de validação provoca rollback; tentativas concorrentes com o mesmo token não podem ambas concluir; após perda de conexão a situação deve ser consultada antes de repetir operação.

### UC08 — Acompanhar participação agregada
**Ator:** Administrador.  
**Fluxo:** selecionar pesquisa e visualizar totais de convites/participações concluídas, sem acesso ao vínculo voto–pessoa.

### UC09 — Gerar relatório agregado
**Ator:** Administrador.  
**Fluxo:** solicitar estatísticas; agrupar respostas por pesquisa/categoria/pergunta/alternativa; aplicar política de privacidade vigente; preparar resultado.  
**Exceção:** grupos insuficientes devem ser tratados conforme regra futura de supressão.

### UC10 — Encaminhar ou receber resultados
**Atores:** Administrador e Proprietário.  
**Fluxo:** gerar relatório agregado, conferir autorização do proprietário e entregar pelo meio aprovado.  
**Pendência:** canal, formato e momento da entrega ainda não definidos.

## 7. Rastreabilidade de requisitos e UML

| Elemento | RFs relacionados |
| --- | --- |
| UC01–UC04 | RF01–RF06 |
| UC05 | RF07, RF16 |
| UC06–UC07 | RF08–RF12, RF16 |
| UC08–UC10 | RF13–RF15 |
| Diagrama de classes | RF01–RF12 e RN01–RN09 |
| Diagrama de sequência | RF07–RF12 e RN01–RN04 |

## 8. Limites de privacidade e escopo acadêmico

- Separar dados identificáveis dos votos é necessário, mas **não garante anonimato absoluto**: horários, categorias com poucos respondentes, logs, metadados do banco, backups e observação de transações podem reidentificar pessoas.
- Definir limiar mínimo de grupo, supressão complementar e política de divulgação antes de oferecer relatórios reais.
- O banco de dados descrito no trabalho acadêmico não significa que autenticação, aplicação web, autorização, envio de credenciais e relatórios estejam implementados.
- Prazo dos tokens, canal de entrega, retenção de logs, categorias múltiplas e novos tipos de perguntas aguardam definição.

## 9. Critérios de validação documental

- [x] Identificados atores, requisitos funcionais e não funcionais.
- [x] Identificadas regras de negócio e fluxos principais de casos de uso.
- [x] Previsto diagrama de casos de uso em formato SVG e fonte editável.
- [ ] Validação com responsáveis do projeto / professor.
- [ ] Testes funcionais da aplicação web e do backend (fora desta etapa documental).
