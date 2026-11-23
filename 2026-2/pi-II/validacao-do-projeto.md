# Plano e Relatório de Validação do Projeto

**Projeto:** Sistema Web para Gestão de Pesquisas por Questionários  
**Issue:** #12 — Validação do Projeto, Documentação Final e Mídia de Apresentação  
**Versão:** 1.0  
**Data de elaboração:** 23/11/2026

> **Importante:** este documento separa o plano de testes do resultado observado. Como não foi possível executar a aplicação nesta etapa, os resultados estão marcados como **Pendente de execução**. Preencher os resultados somente após testar a versão real do sistema e anexar evidências verdadeiras.

## 1. Objetivo

Verificar se o sistema atende às funções previstas no projeto: gerenciamento de pesquisas, definição de categorias, aplicação de questionários, acesso por código/senha, registro de respostas, acompanhamento de participação e emissão de relatórios estatísticos.

## 2. Ambiente de validação

| Item | Informação a registrar |
|---|---|
| URL/ambiente | A preencher após publicação |
| Data e horário | A preencher durante os testes |
| Navegador e versão | A preencher |
| Sistema operacional | A preencher |
| Dispositivo/resolução | A preencher |
| Commit/versão testada | A preencher |
| Responsável pelo teste | A preencher |
| Banco/API utilizados | A preencher, se aplicável |

## 3. Critérios de entrada

- [ ] Versão candidata disponível para teste.
- [ ] Páginas principais acessíveis.
- [ ] Dados fictícios preparados.
- [ ] Contas e permissões de teste disponíveis.
- [ ] Backend e banco disponíveis, caso façam parte da versão.
- [ ] Nenhum dado pessoal real utilizado nos testes.

## 4. Casos de teste funcionais

| ID | Funcionalidade | Procedimento | Resultado esperado | Resultado observado |
|---|---|---|---|---|
| VAL-01 | Acesso inicial | Abrir a página inicial pelo endereço publicado | Página carrega sem erro e apresenta navegação | Pendente de execução |
| VAL-02 | Navegação | Acessar cada item do menu | Cada link abre o destino correto | Pendente de execução |
| VAL-03 | Login administrativo | Informar credenciais válidas | Acesso permitido ao perfil correspondente | Pendente de execução |
| VAL-04 | Login inválido | Informar credenciais incorretas | Acesso negado e mensagem adequada | Pendente de execução |
| VAL-05 | Cadastro de pesquisa | Criar pesquisa com dados válidos | Pesquisa é registrada e aparece na listagem | Pendente de execução |
| VAL-06 | Validação de pesquisa | Enviar formulário com campos obrigatórios vazios | Sistema informa os campos que precisam ser corrigidos | Pendente de execução |
| VAL-07 | Categorias | Cadastrar ou associar categorias à pesquisa | Categorias são apresentadas corretamente | Pendente de execução |
| VAL-08 | Questionário | Abrir questionário de uma pesquisa ativa | Perguntas e alternativas são exibidas corretamente | Pendente de execução |
| VAL-09 | Código de acesso | Informar código válido e ainda não utilizado | Participante consegue iniciar a pesquisa | Pendente de execução |
| VAL-10 | Código inválido/reutilizado | Informar código inválido ou já utilizado | Sistema bloqueia a participação | Pendente de execução |
| VAL-11 | Envio de respostas | Responder campos obrigatórios e enviar | Respostas são registradas uma única vez | Pendente de execução |
| VAL-12 | Campos obrigatórios | Enviar questionário incompleto | Sistema solicita o preenchimento necessário | Pendente de execução |
| VAL-13 | Acompanhamento | Consultar participação durante a pesquisa | Indicadores refletem os registros existentes | Pendente de execução |
| VAL-14 | Relatórios | Solicitar relatório de uma pesquisa com dados fictícios | Totais e gráficos correspondem aos dados de teste | Pendente de execução |
| VAL-15 | Permissão de relatório | Acessar relatório sem permissão | Acesso negado | Pendente de execução |
| VAL-16 | Responsividade | Abrir páginas em celular e computador | Conteúdo permanece legível e utilizável | Pendente de execução |

## 5. Casos de teste de segurança e privacidade

| ID | Procedimento | Resultado esperado | Resultado observado |
|---|---|---|---|
| SEG-01 | Verificar se o site utiliza HTTPS | Comunicação protegida por HTTPS | Pendente de execução |
| SEG-02 | Inspecionar armazenamento da senha administrativa | Senha não está armazenada em texto puro | Pendente de execução |
| SEG-03 | Inspecionar registros da resposta | Resposta não contém nome ou identificador desnecessário | Pendente de execução |
| SEG-04 | Verificar relação entre código e resposta | Não existe vínculo que permita identificar o respondente pela resposta | Pendente de execução |
| SEG-05 | Consultar logs técnicos | Não há exposição de senhas, códigos em texto aberto ou conteúdo de respostas | Pendente de execução |
| SEG-06 | Tentar acessar função restrita | Perfil sem permissão não consegue acessar a função | Pendente de execução |
| SEG-07 | Testar grupo estatístico pequeno | Relatório evita exposição de pessoa por inferência | Pendente de execução |

## 6. Testes de compatibilidade e usabilidade

| ID | Verificação | Resultado esperado | Resultado observado |
|---|---|---|---|
| UX-01 | Navegação por teclado | Elementos interativos podem ser alcançados por teclado | Pendente de execução |
| UX-02 | Rótulos de formulários | Campos possuem rótulos claros e associados | Pendente de execução |
| UX-03 | Contraste e legibilidade | Texto permanece legível nas telas principais | Pendente de execução |
| UX-04 | Tela pequena | Não há rolagem horizontal indevida ou controles inacessíveis | Pendente de execução |
| UX-05 | Mensagens de erro | Erros são compreensíveis e orientam a correção | Pendente de execução |

## 7. Registro de defeitos

| ID | Descrição | Gravidade | Passos para reproduzir | Responsável | Situação |
|---|---|---|---|---|---|
| BUG-01 | A preencher após execução | A definir | A preencher | A definir | Não registrado |

## 8. Evidências que devem ser anexadas

- captura da página inicial publicada;
- captura da navegação e das telas principais;
- captura do formulário com validação de campos;
- captura da participação com dados fictícios;
- captura dos relatórios gerados com dados fictícios;
- captura do site em celular;
- captura da execução bem-sucedida do GitHub Actions;
- evidência técnica de que respostas não estão vinculadas aos códigos de acesso, sem exibir dados pessoais reais.

Nomear os arquivos de maneira objetiva, por exemplo: `VAL-01-home.png`, `VAL-06-validacao-formulario.png` e `SEG-04-separacao-codigo-resposta.png`.

## 9. Critério de encerramento

A validação poderá ser considerada concluída quando todos os casos críticos forem executados, os defeitos impeditivos forem corrigidos ou formalmente registrados, as evidências forem anexadas e a equipe confirmar que a versão testada corresponde ao commit informado.

## 10. Resultado geral

**Status na elaboração deste documento:** plano de validação preparado; execução ainda não comprovada.  
**Aprovado por:** a preencher após revisão da equipe e dos professores.  
**Data de aprovação:** a preencher.
