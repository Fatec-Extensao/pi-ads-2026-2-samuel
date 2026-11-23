# Relatório de Adequação à LGPD e Mecanismos de Anonimato

**Projeto:** Sistema Web para Gestão de Pesquisas por Questionários  
**Disciplina:** Compliance e Segurança da Informação  
**Professor:** Prof. Alciano Gustavo Genovez de Oliveira  
**Issue:** #11 — Relatório de Adequação à LGPD e Mecanismos de Anonimato  
**Repositório:** [pi-ads-2026-2-samuel](https://github.com/Fatec-Extensao/pi-ads-2026-2-samuel)  
**Versão do documento:** 1.0  
**Data:** 23/11/2026

> **Escopo e ressalva:** este relatório é uma análise acadêmica e uma especificação de controles para o projeto. O README do repositório descreve o uso de senhas anônimas, categorias de respondentes, coleta de respostas, monitoramento de participação e geração de relatórios. Como o código-fonte não foi disponibilizado para esta análise, os controles abaixo são apresentados como requisitos e recomendações; não se afirma que já estejam implementados nem que o sistema possua certificação ou conformidade jurídica integral. A base legal definitiva e os prazos de retenção devem ser aprovados pelo controlador dos dados, considerando cada pesquisa.

## 1. Objetivo

Avaliar os principais pontos de privacidade do sistema de pesquisas por questionários e definir medidas para que a aplicação colete somente os dados necessários, preserve o sigilo das respostas e reduza a possibilidade de identificação dos participantes.

O sistema tem como finalidade permitir que uma instituição crie pesquisas, defina categorias de participantes, disponibilize questionários, receba respostas e consulte resultados estatísticos. O anonimato é um requisito central porque a pessoa deve poder responder sem que sua identidade seja associada ao conteúdo enviado.

## 2. Dados e operações considerados

A análise considera os seguintes grupos de informação, que deverão ser confirmados com o modelo de dados e o código da aplicação:

| Informação | Finalidade prevista | Risco de privacidade | Diretriz |
|---|---|---|---|
| Cadastro do administrador/pesquisador | Criar e administrar pesquisas | Acesso indevido à conta | Coletar apenas dados necessários à gestão; proteger credenciais |
| Dados de configuração da pesquisa | Definir título, período, perguntas e categorias | Exposição de informações institucionais | Restringir edição e publicação a usuários autorizados |
| Categoria do respondente | Segmentar resultados | Reidentificação em grupos pequenos | Usar categorias amplas e evitar cruzamentos que identifiquem pessoas |
| Código/senha de acesso | Permitir participação e controlar uso único | Associação indireta entre pessoa e resposta | Não armazenar relação entre identidade civil e conteúdo respondido |
| Respostas | Produzir análise estatística | Exposição de opiniões ou dados pessoais inseridos livremente | Não solicitar identificação; orientar a não informar nomes ou dados pessoais nos campos abertos |
| Indicadores agregados | Acompanhar participação e resultados | Inferência sobre grupos pequenos | Exibir totais e estatísticas, aplicando supressão quando houver risco de identificação |
| Logs técnicos | Diagnóstico e segurança | IP, horário e identificadores podem permitir correlação | Minimizar campos, limitar acesso e estabelecer prazo de retenção |

A lista é uma proposta de inventário. Antes de produção, a equipe deve compará-la com os campos efetivamente existentes no banco de dados, nos formulários, nos cookies, no serviço de hospedagem e nos registros de acesso.

## 3. Princípios da LGPD aplicáveis

O tratamento de dados deve observar, especialmente, os princípios do art. 6º da Lei nº 13.709/2018:

- **Finalidade:** utilizar os dados para objetivos específicos e informados aos participantes.
- **Adequação:** manter o tratamento compatível com a finalidade apresentada na pesquisa.
- **Necessidade:** não solicitar nome, CPF, e-mail, telefone ou outros identificadores quando não forem indispensáveis.
- **Livre acesso e transparência:** explicar de forma clara o funcionamento da pesquisa, o uso das respostas e o canal de contato do controlador.
- **Segurança e prevenção:** adotar controles para impedir acessos não autorizados e incidentes.
- **Não discriminação:** não utilizar respostas para finalidades discriminatórias ou incompatíveis.
- **Responsabilização e prestação de contas:** manter registros das decisões, dos controles adotados e das revisões realizadas.

A base legal aplicável não deve ser escolhida automaticamente. O controlador deve documentar a hipótese do art. 7º da LGPD adequada à finalidade e ao contexto de cada pesquisa. Quando houver dados pessoais sensíveis, também deverá ser avaliada a hipótese legal pertinente do art. 11. A simples existência de um aviso de privacidade ou de um botão de aceite não substitui essa análise.

## 4. Mecanismo proposto para acesso anônimo

### 4.1 Emissão dos códigos

1. O administrador configura a pesquisa e define a quantidade de códigos necessários.
2. O sistema gera códigos aleatórios, únicos e difíceis de adivinhar, sem utilizar nome, matrícula, CPF, e-mail ou qualquer dado do participante como parte do código.
3. O código é entregue ao participante por um canal definido pela instituição.
4. A aplicação armazena somente o verificador do código (por exemplo, hash com algoritmo apropriado), e não o código em texto aberto após sua emissão, sempre que a arquitetura permitir.
5. O sistema não mantém uma tabela que relacione o código ao nome ou cadastro do participante.

### 4.2 Utilização do código

1. O participante informa o código na tela de acesso.
2. O sistema verifica se o código é válido, pertence à pesquisa indicada e ainda não foi utilizado.
3. Após a validação, o sistema registra apenas o estado de utilização do código, sem gravar uma chave que permita relacioná-lo ao conteúdo da resposta.
4. A resposta é gravada com identificador técnico aleatório, sem nome, matrícula, e-mail, IP ou identificador do código de acesso.
5. O sistema impede a reutilização do mesmo código.

### 4.3 Separação lógica

O cadastro/controle dos códigos e o armazenamento das respostas devem permanecer separados. Não deve existir chave estrangeira, log ou tabela de auditoria que permita reconstruir a relação entre um código específico e uma resposta específica. A equipe deve testar também se logs do servidor, ferramentas de analytics, cookies ou horários muito precisos permitem realizar essa correlação por outro caminho.

**Atenção:** caso a instituição consiga relacionar o código a uma pessoa, ou caso logs e metadados permitam essa associação, as respostas não devem ser chamadas de anônimas. Nesse cenário, trata-se de dado pseudonimizado ou identificável por meios indiretos, e a LGPD continua aplicável. A anonimização depende da impossibilidade razoável de identificação, considerando os meios técnicos disponíveis e os dados auxiliares que possam ser utilizados.

## 5. Controles técnicos e administrativos

| Controle | Aplicação recomendada | Prioridade |
|---|---|---|
| Minimização | Remover campos identificadores que não sejam necessários à pesquisa | Alta |
| Autenticação administrativa | Senhas fortes, armazenamento seguro de hash de senha e proteção contra tentativas repetidas | Alta |
| Controle de acesso | Separar permissões de administrador, pesquisador e visualizador de relatórios | Alta |
| Transporte seguro | Utilizar HTTPS em todas as páginas e requisições | Alta |
| Separação de dados | Não relacionar códigos de participação e respostas | Alta |
| Proteção do banco | Restringir credenciais, permissões e acesso externo; manter cópias de segurança protegidas | Alta |
| Logs mínimos | Não registrar respostas completas, códigos em texto aberto ou dados excessivos | Alta |
| Retenção | Definir prazo por pesquisa e eliminar ou anonimizar os dados ao final da necessidade | Alta |
| Relatórios | Exibir dados agregados e avaliar risco de reidentificação em grupos pequenos | Alta |
| Atualizações | Manter dependências, servidor e bibliotecas atualizados | Média |
| Incidentes | Definir responsáveis, contenção, registro e avaliação de comunicação | Alta |
| Treinamento | Orientar administradores sobre acesso, exportação e compartilhamento | Média |

## 6. Proteção contra reidentificação em relatórios

Mesmo sem nome ou CPF, uma resposta pode ser identificada quando a categoria possui poucas pessoas ou quando diversos filtros são combinados. Para reduzir esse risco:

- evitar a exibição de resultados de grupos muito pequenos;
- não disponibilizar cruzamentos excessivos de idade, setor, cargo, turma ou localização;
- suprimir ou agrupar categorias raras;
- limitar exportações detalhadas a pessoas autorizadas;
- não divulgar respostas abertas que contenham nomes, locais ou fatos que identifiquem participantes;
- revisar relatórios antes de sua publicação externa.

Pode ser adotado um limite mínimo configurável de participantes por grupo como controle de redução de risco. Esse limite é uma decisão técnica do projeto, não um número fixado pela LGPD.

## 7. Transparência ao participante

Antes do início do questionário, apresentar um aviso em linguagem simples contendo:

- identificação e contato do controlador da pesquisa;
- finalidade e público-alvo;
- quais informações são coletadas e quais não são solicitadas;
- base legal aplicável, definida pelo controlador;
- prazo ou critério de retenção;
- quem terá acesso aos resultados e como serão divulgados;
- informação sobre o uso de códigos de acesso e sobre a separação entre código e resposta;
- canal para dúvidas e exercício de direitos, quando houver tratamento de dados pessoais.

Texto ilustrativo para adaptação pela instituição:

> Esta pesquisa tem a finalidade de coletar opiniões para análise institucional. O questionário não solicita seu nome ou documento de identificação. O código de acesso serve para permitir uma única participação e, conforme a configuração técnica adotada, não deve ser associado ao conteúdo das respostas. Os resultados serão apresentados de forma agrupada. Evite inserir nomes ou outros dados pessoais nos campos de resposta aberta. Para dúvidas sobre o tratamento de dados, utilize o canal de contato informado pela instituição responsável.

O texto acima é um modelo e só poderá afirmar que o código não é associado às respostas depois que a implementação e os logs forem verificados.

## 8. Direitos dos titulares e retenção

Quando houver tratamento de dados pessoais, o controlador deverá disponibilizar canal para receber e encaminhar solicitações dos titulares, observando os direitos do art. 18 da LGPD, como confirmação de tratamento, acesso, correção, anonimização, bloqueio ou eliminação, conforme aplicável.

O projeto deverá documentar, para cada pesquisa, o prazo de retenção das respostas, dos códigos de participação, dos logs e dos backups. O término de uma pesquisa não implica apagar automaticamente tudo sem análise: devem ser observadas as hipóteses legais de conservação e os limites técnicos. Encerrada a finalidade e inexistindo hipótese válida de conservação, os dados pessoais deverão ser eliminados ou anonimizados de forma adequada.

## 9. Incidentes de segurança

A instituição deve estabelecer procedimento para detectar, registrar, avaliar e responder a incidentes. O procedimento deve contemplar contenção do acesso, preservação de evidências, avaliação dos dados e titulares afetados, correção da vulnerabilidade e análise da necessidade de comunicação à ANPD e aos titulares, conforme a LGPD e a regulamentação aplicável.

## 10. Checklist de adequação

- [ ] Inventariar todos os campos de formulários, tabelas, cookies, logs e integrações.
- [ ] Definir controlador, operadores e responsáveis pelo tratamento.
- [ ] Documentar finalidade e hipótese legal para cada operação.
- [ ] Confirmar que códigos de acesso não identificam diretamente participantes.
- [ ] Confirmar tecnicamente que códigos, logs e respostas não podem ser correlacionados.
- [ ] Remover coleta desnecessária de IP, localização, dispositivo e identificadores de analytics.
- [ ] Garantir HTTPS e controle de acesso administrativo.
- [ ] Definir retenção, descarte, backup e procedimento para incidentes.
- [ ] Revisar risco de reidentificação em categorias e relatórios.
- [ ] Publicar aviso de privacidade validado pelo controlador.
- [ ] Executar os testes descritos neste relatório e anexar evidências reais.

## 11. Casos de teste de anonimato

| ID | Cenário | Resultado esperado | Evidência a guardar |
|---|---|---|---|
| LGPD-01 | Enviar resposta sem fornecer nome ou documento | A resposta é registrada sem campos identificadores | Captura do formulário e estrutura de dados fictícios |
| LGPD-02 | Tentar utilizar novamente o mesmo código | A segunda tentativa é bloqueada | Captura da mensagem e registro de teste |
| LGPD-03 | Consultar a resposta no banco após uso de um código | Não existe vínculo entre o código e o conteúdo da resposta | Esquema/consulta com dados fictícios |
| LGPD-04 | Consultar logs de aplicação e hospedagem | Logs não expõem resposta, código em texto aberto ou identificador desnecessário | Trecho de log sanitizado |
| LGPD-05 | Filtrar relatório por categoria com poucos participantes | O sistema oculta ou agrupa dados quando houver risco de identificação | Captura do relatório de teste |
| LGPD-06 | Tentar acessar relatório sem permissão | A aplicação nega o acesso | Captura da resposta de acesso negado |
| LGPD-07 | Acessar o sistema por HTTP | O acesso é redirecionado para HTTPS ou recusado | Evidência do navegador |

**Situação dos testes na elaboração deste documento:** pendentes. Não foram executados contra a aplicação porque o código e o ambiente de execução não estavam disponíveis para esta análise.

## 12. Conclusão

O anonimato precisa ser garantido pela arquitetura, pelo banco de dados, pelos logs e pela forma de divulgação dos relatórios, e não apenas pela ausência de um campo de nome no formulário. A separação entre códigos de participação e respostas, a minimização de dados e o controle de reidentificação são os pontos mais importantes para este projeto.

A adequação deve ser confirmada por inspeção do código, do banco de dados, dos serviços de hospedagem e por testes documentados. Este relatório fornece os critérios para essa verificação, mas não substitui auditoria técnica ou avaliação jurídica.

## 13. Referências

- BRASIL. **Lei nº 13.709, de 14 de agosto de 2018 — Lei Geral de Proteção de Dados Pessoais (texto compilado)**. Disponível em: <https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709compilado.htm>.
- AUTORIDADE NACIONAL DE PROTEÇÃO DE DADOS (ANPD). **Guia orientativo sobre segurança da informação para agentes de tratamento de pequeno porte**. Disponível em: <https://www.gov.br/anpd/pt-br/centrais-de-conteudo/materiais-educativos-e-publicacoes/guia-orientativo-sobre-seguranca-da-informacao-para-agentes-de-tratamento-de-pequeno-porte>.
- ANPD. **Resolução CD/ANPD nº 2, de 27 de janeiro de 2022**. Disponível em: <https://www.gov.br/anpd/pt-br/acesso-a-informacao/institucional/atos-normativos/regulamentacoes_anpd/resolucao-cd-anpd-no-2-de-27-de-janeiro-de-2022>.
