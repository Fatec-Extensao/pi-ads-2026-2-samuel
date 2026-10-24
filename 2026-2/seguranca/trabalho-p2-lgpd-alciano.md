# ANÁLISE DE RISCOS E ADEQUAÇÃO À LGPD DO SISTEMA WEB DE GESTÃO DE PESQUISAS POR QUESTIONÁRIOS

**Centro Estadual de Educação Tecnológica Paula Souza**  
Faculdade de Tecnologia de Lins Prof. Antonio Seabra  
Curso Superior de Tecnologia em Análise e Desenvolvimento de Sistemas

**Integrantes**

- IGOR RODRIGUES DOS SANTOS
- LUIS MIGUEL SCHIAZZA
- MATHEUS CARDOSO BISPO
- MARCELO OLIVEIRA SOUZA
- SAMUEL DA SILVA ROBLES GARCIA

**Disciplina:** Compliance e Segurança da Informação  
**Local:** LINS/SP  
**Período:** 2º SEMESTRE/2026

---

Este trabalho apresenta um diagnóstico documental do Sistema Web de Gestão de Pesquisas por Questionários, desenvolvido no Projeto Integrador II da FATEC Lins, e propõe medidas de adequação à Lei Geral de Proteção de Dados Pessoais (LGPD, Lei nº 13.709/2018). O escopo concentra-se na issue #10 de Compliance e Segurança da Informação: identificação de ameaças, avaliação de probabilidade e impacto e definição de ações de mitigação. O texto também atende às seções de direitos dos titulares e segurança técnica exigidas no modelo fornecido pelo professor.

## 1 INTRODUÇÃO

A digitalização das pesquisas de satisfação permite criar questionários, distribuir códigos de participação, coletar respostas e gerar indicadores de forma mais ágil. Porém, o processamento de respostas, e-mails institucionais, credenciais de acesso e registros operacionais pode envolver dados pessoais, ainda que o resultado pretendido seja estatístico e anônimo. Assim, a privacidade não depende somente de omitir o nome do participante: é necessário avaliar a possibilidade de identificação indireta por texto livre, horários, códigos e combinações de informações (BRASIL, 2018).

**Problema de pesquisa.** Como estruturar e evoluir o Sistema Web de Gestão de Pesquisas por Questionários para reduzir riscos de acesso indevido, fraude, vazamento de respostas e reidentificação dos participantes, respeitando os princípios e os direitos previstos na LGPD?

**Objetivo geral.** Diagnosticar os riscos de segurança e privacidade do sistema e propor requisitos técnicos, administrativos e operacionais de adequação à LGPD.

**Objetivos específicos.** (a) Classificar os dados tratados; (b) analisar a arquitetura e as funcionalidades disponíveis; (c) estabelecer medidas de acesso, anonimato, rastreabilidade e descarte; (d) elaborar matriz de riscos com probabilidade, impacto e mitigação; e (e) descrever mecanismos para atendimento aos direitos dos titulares.

**Justificativa.** Mesmo um questionário de satisfação aparentemente simples pode expor opiniões identificáveis, informações pessoais inseridas em comentários ou padrões de resposta de pequenos grupos. Falhas podem prejudicar participantes e a instituição, além de gerar responsabilização e sanções aplicáveis conforme a gravidade e o devido processo legal. O foco do trabalho é prevenção proporcional ao contexto acadêmico, sem afirmar conformidade jurídica ou implantação de controles ainda inexistentes (ANPD, 2021).

## 2 REFERENCIAL TEÓRICO

### 2.1 A Lei Geral de Proteção de Dados (LGPD)

A LGPD disciplina o tratamento de dados pessoais por organizações públicas e privadas. Dado pessoal é a informação relacionada a pessoa natural identificada ou identificável (por exemplo, nome e e-mail). Dado pessoal sensível compreende, entre outros, dados sobre saúde, religião, opinião política e biometria vinculada a pessoa natural. CPF e Carteira de Identidade Nacional (CIN) são, em regra, dados pessoais, mas não se tornam automaticamente dados sensíveis apenas por serem identificadores (BRASIL, 2018, art. 5º).

Titular é a pessoa a quem se referem os dados; controlador é quem decide as finalidades e os meios essenciais do tratamento; operador atua em nome do controlador. No projeto acadêmico, a identificação formal do controlador deve ser validada com a instituição e com a organização responsável pela pesquisa; equipe desenvolvedora e eventual provedor de hospedagem não devem ter seus papéis presumidos sem análise contratual (ANPD, 2022).

Os princípios pertinentes são: finalidade (uso para propósito informado), adequação (compatibilidade entre coleta e finalidade), necessidade (coleta mínima), transparência (informação compreensível), segurança (proteção contra acessos e perdas), prevenção (redução antecipada dos riscos), não discriminação e responsabilização/prestação de contas. Antes de colocar pesquisas reais em produção, cada operação necessita de finalidade, hipótese legal válida (arts. 7º ou 11, conforme os dados), base documental e responsável definidos. Consentimento não é obrigatório em todas as situações, e sua utilização exige requisitos próprios.

Anonimização efetiva pressupõe que a pessoa não possa ser identificada com meios técnicos razoáveis e disponíveis. Retirar o nome de um banco não basta se houver vínculo com código de acesso, IP, dados demográficos ou texto livre. Dados pseudonimizados, quando reversíveis ou vinculáveis, continuam sujeitos à LGPD (BRASIL, 2018, arts. 5º e 12).

### 2.2 O ecossistema de um sistema de pesquisas por questionários

O projeto contempla, no front-end demonstrativo, perfis de colaborador, aluno e professor; cadastro de categorias, pesquisas, perguntas e questionários; solicitação/geração de códigos; resposta a questionário; monitoramento; e relatórios. Os grupos de informação relevantes para a futura versão operacional são:

- Dados cadastrais e de autenticação: e-mail institucional, perfil, credenciais e permissões de colaboradores ou participantes, somente se necessários. E-mail é dado pessoal; senhas devem ser armazenadas exclusivamente como verificadores criptográficos seguros.
- Dados de participação: código temporário, condição de uso, pesquisa autorizada e eventos mínimos de prevenção a fraude. Deve existir separação entre controle de acesso e respostas, para evitar correlação individual.
- Dados de pesquisa: categoria, questões, alternativas, avaliação, recomendação e comentário livre. Comentários podem conter dados pessoais e, conforme o conteúdo, dados sensíveis inseridos espontaneamente.
- Dados analíticos e técnicos: contagens e percentuais agregados, registros de erro e auditoria com data/hora e ação administrativa. IP, identificadores de sessão e pequenos grupos podem permitir identificação indireta, exigindo necessidade e proteção específicas.

O protótipo atual não processa nem armazena essas informações em servidor. As telas simulam operações no navegador com dados fictícios; por isso, o inventário acima representa, sobretudo, o tratamento previsto para uma implementação real. O formulário existente recomenda que o participante não inclua dados pessoais em comentários.

### 2.3 Segurança da Informação no Desenvolvimento de Software

Privacy by Design e Privacy by Default significam incorporar proteção ao projeto e estabelecer as configurações mais restritivas por padrão, em vez de adicioná-las somente após incidentes. No sistema analisado, isso implica não coletar identificadores desnecessários, separar dados de login das respostas, limitar acesso aos relatórios e avaliar a reidentificação antes de publicar resultados (BRASIL, 2018, art. 46).

Confidencialidade: somente gestores autorizados devem acessar cadastros, configurações e indicadores; respostas individuais não podem ficar expostas a outros respondentes. Integridade: perguntas, alternativas, submissões e totais dos relatórios devem resistir a alterações indevidas, duplicidades e falhas parciais. Disponibilidade: o serviço deve manter acesso durante o período da pesquisa, contar com restauração testada e informar indisponibilidade sem revelar informações sensíveis.

Esses pilares exigem gestão de identidades, revisão de permissões, transações no banco, backups, atualização de dependências, validação de entradas e verificação periódica dos controles. A ANPD recomenda combinar medidas técnicas e administrativas, de acordo com o risco e a capacidade do agente de tratamento (ANPD, 2021).

## 3 METODOLOGIA

A pesquisa é aplicada quanto à natureza, predominantemente qualitativa quanto à abordagem, exploratório-descritiva quanto aos objetivos e documental quanto aos procedimentos. Foram examinados o modelo de trabalho fornecido pelo professor, o README e os arquivos da branch padrão “atividade-06” do repositório pi-ads-2026-2-igor, o código estático, os testes registrados e o conteúdo das issues e pull requests disponíveis na consulta realizada em 23 de outubro de 2026. Não foram feitas entrevistas com titulares, auditoria de produção, testes de intrusão ou avaliação jurídica formal.

A análise normativa baseia-se na Lei nº 13.709/2018 e em orientações oficiais da Agência Nacional de Proteção de Dados (ANPD) sobre segurança, direitos dos titulares, agentes de tratamento e comunicação de incidentes. Para cada cenário da issue #10 foi atribuída probabilidade (P) e impacto (I), em escala ordinal de 1 a 3. O índice P × I é utilizado apenas como ferramenta acadêmica de priorização, não como cálculo estatístico de frequência real.

Critérios: probabilidade baixa (1), média (2) ou alta (3), conforme facilidade de exploração, exposição e controles existentes; impacto baixo (1), médio (2) ou alto (3), conforme dano potencial a pessoas e ao funcionamento da pesquisa. Prioridade: baixa (1–2), moderada (3–4) e alta (6–9). As notas são estimativas preliminares e devem ser revistas após implementação do backend, testes técnicos e validação dos responsáveis.

## 4 DESENVOLVIMENTO: DIAGNÓSTICO E ADEQUAÇÃO DO SOFTWARE

O projeto analisado é uma demonstração acadêmica em HTML5, CSS e JavaScript, com 12 páginas, validações locais e questionário em etapas. Conforme a documentação do repositório, NÃO existem autenticação real, API, banco de dados, envio ou persistência de respostas. O código fictício DEMO-2026 permite demonstrar o fluxo e não constitui credencial válida. Os relatórios exibem números simulados. A distinção entre “verificado no protótipo” e “recomendado para produção” impede atribuir à aplicação proteções que ela ainda não oferece.

*Tabela 1 – Diagnóstico por módulo e adaptação necessária à LGPD.*

| **Módulo**                 | **Dados/risco principal**                            | **Situação verificada e adaptação proposta**                                                                                                                                        |
|----------------------------|------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Acesso e perfis            | E-mail, senha, perfil; acesso indevido               | Atual: login demonstrativo sem verificação no servidor. Futuro: autenticação robusta, hash Argon2id, sessões seguras, MFA para gestão, RBAC e limitação de tentativas.              |
| Categorias e pesquisas     | Conteúdo, datas e gestão de formulários              | Atual: validação apenas no navegador. Futuro: autorização por perfil, validação no servidor, controle de alterações e integridade das versões da pesquisa.                          |
| Códigos de participação    | Convites e códigos; fraude e vínculo indireto        | Atual: códigos fictícios, não persistidos. Futuro: tokens aleatórios de uso único, prazo curto, verificador protegido e consumo transacional sem vínculo persistente com respostas. |
| Respostas e comentários    | Avaliações e texto livre; vazamento/ reidentificação | Atual: avanço/retorno local, sem armazenamento. Futuro: coleta mínima, separação de bases, validação contextual, proteção contra XSS e cuidado com pequenos grupos.                 |
| Relatórios e monitoramento | Indicadores agregados; divulgação indireta           | Atual: números fixos fictícios. Futuro: autorização por pesquisa, agregação e regra de supressão de resultados de grupos reduzidos.                                                 |
| Auditoria e infraestrutura | Logs, falhas, disponibilidade e backups              | Atual: não há infraestrutura de aplicação. Futuro: logs administrativos sem respostas ou credenciais, TLS, criptografia, backup cifrado e testes de restauração.                    |

### 4.1 Painel do titular e direitos dos participantes

O artigo 18 da LGPD prevê, nos limites legais, confirmação da existência de tratamento, acesso, correção, anonimização, bloqueio ou eliminação de dados desnecessários ou irregulares, portabilidade quando aplicável, informação sobre compartilhamento e revogação do consentimento. A exclusão não é irrestrita: há hipóteses de conservação previstas em lei, especialmente no art. 16 (BRASIL, 2018; ANPD, s.d.).

Situação atual: o protótipo não contém painel do titular, base de cadastros, canal automatizado para direitos nem lógica de eliminação. Proposta: acrescentar área acessível de “Privacidade e solicitações” com aviso claro, identificação do controlador e canal de contato; permitir ao titular acompanhar pedidos relativos aos dados de cadastro e ao consentimento eventualmente coletado. Cada solicitação deverá receber protocolo, verificação proporcional de identidade, encaminhamento ao responsável e resposta nos termos legais.

Respostas efetivamente anonimizadas não poderão ser localizadas por identidade de participante; essa limitação deve constar no aviso, sem prometer exclusão individual do que não é identificável. Se existir qualquer elo reversível entre convite e resposta, ele deverá ser tratado e protegido como dado pessoal até a anonimização eficaz. A retenção, eventual descarte e a possibilidade de retificação dependem do tipo de dado e da finalidade registrada.

### 4.2 Segurança técnica do sistema

Em transporte, recomenda-se HTTPS com TLS 1.3, aceitando TLS 1.2 com configuração segura quando necessário; rejeitar HTTP em produção. Para dados pessoais armazenados, recomenda-se criptografia autenticada AES-256-GCM em campos selecionados, além de criptografia dos volumes e backups, com chaves fora do repositório e segregadas dos dados. AES-256-GCM protege confidencialidade e detecta adulteração, desde que chaves e nonces sejam gerenciados corretamente; criptografia do disco isoladamente não substitui a autorização da aplicação.

Senhas de usuários não devem ser criptografadas reversivelmente: devem receber hash de senha Argon2id com sal único e parâmetros atualizados. Códigos de convite, se validados por comparação, podem ter verificadores HMAC-SHA-256 com segredo mantido no servidor; recomenda-se duração restrita e invalidação atômica após uso. Esses algoritmos são escolhas técnicas PROPOSTAS; nenhum deles está implantado no front-end atual.

Auditoria: implementar logs estruturados de criação/edição de questionário, geração e consumo de convite, acesso a relatórios, alterações de permissões e falhas de autenticação, contendo evento, data/hora, identificador técnico do responsável e resultado. Não registrar senha, token bruto, resposta textual ou dado pessoal desnecessário. A recomendação inicial é utilizar logging nativo da API e serviço centralizado com acesso restrito, sem pressupor aquisição de software comercial. O prazo de guarda dos logs e sua base legal deverão ser definidos antes da implantação.

Backups e descarte: realizar cópias automáticas cifradas, com restrição de leitura e testes periódicos de restauração; excluir ou anonimizar dados quando cessar a necessidade, respeitando retenções legais. Como parâmetro inicial sujeito a aprovação institucional, sugere-se revisar dados operacionais a cada 90 dias e definir prazo documental de retenção por finalidade. Evitar números fixos apresentados como exigência legal.

No caso de incidente que possa acarretar risco ou dano relevante aos titulares, o controlador deverá avaliar a necessidade de comunicar à ANPD e aos titulares, conforme a LGPD e a Resolução CD/ANPD nº 15/2024. Em regra, essa comunicação deve ocorrer em até três dias úteis a partir do conhecimento de que o incidente afetou dados pessoais, ressalvados prazos específicos. A equipe deve manter procedimento de detecção, contenção, investigação, registro de evidências e comunicação (ANPD, 2024).

### 4.3 Matriz de riscos de segurança e privacidade — issue #10

A matriz abaixo atende diretamente à issue #10, incluindo injeção de SQL, vazamento de respostas e fraude/manipulação. A probabilidade se refere ao cenário de uma futura versão com servidor e banco de dados; como tais componentes ainda não existem no protótipo, a pontuação é uma hipótese de projeto, não evidência de vulnerabilidade já explorável na aplicação publicada.

*Tabela 2 – Matriz preliminar de riscos (P = probabilidade; I = impacto).*

| **Risco e consequência**                                                                 | **P / I** | **Prioridade** | **Medidas de mitigação recomendadas**                                                                                                  |
|------------------------------------------------------------------------------------------|-----------|----------------|----------------------------------------------------------------------------------------------------------------------------------------|
| R01 – Injeção de SQL em cadastros, pesquisas e relatórios; leitura ou alteração indevida | 2 / 3     | 6 – Alta       | Queries parametrizadas, privilégios mínimos no banco, validação no servidor, revisão e testes negativos.                               |
| R02 – Vazamento de respostas ou comentários; exposição de participantes                  | 2 / 3     | 6 – Alta       | Controle de acesso por pesquisa, segregação de dados, criptografia, agregação, logs de leitura administrativa e testes de autorização. |
| R03 – Fraude: respostas repetidas ou códigos adivinhados; distorção dos resultados       | 3 / 2     | 6 – Alta       | Tokens aleatórios de uso único com expiração; rate limiting; transações; detecção de abuso sem criar perfil nominal.                   |
| R04 – Reidentificação por texto livre, horário ou grupos pequenos; quebra do anonimato   | 2 / 3     | 6 – Alta       | Coleta mínima, orientações no formulário, redução de metadados, supressão de grupos pequenos e revisão de anonimização.                |
| R05 – Gestor não autorizado acessa relatórios ou altera perguntas; abuso de privilégio   | 2 / 3     | 6 – Alta       | RBAC no backend, checagem de propriedade do recurso, MFA para gestores, auditoria e revisão de acessos.                                |
| R06 – XSS em comentários ou títulos; execução de código no navegador                     | 2 / 3     | 6 – Alta       | Escapar conteúdo na saída, usar APIs seguras do DOM, Content-Security-Policy e testes de entrada maliciosa.                            |
| R07 – Perda de dados, falha de serviço ou ransomware; interrupção da pesquisa            | 2 / 3     | 6 – Alta       | Backups cifrados, restore testado, atualização e monitoramento, plano de continuidade e menor privilégio.                              |
| R08 – Retenção excessiva de registros e logs; tratamento sem necessidade                 | 2 / 2     | 4 – Moderada   | Inventário por finalidade, tabela de retenção, descarte seguro, revisão de logs e responsabilização do controlador.                    |
| R09 – Aviso insuficiente ou fundamento legal inadequado; direitos prejudicados           | 2 / 3     | 6 – Alta       | Aviso de privacidade, registro de finalidades/hipóteses legais, canal de direitos e revisão pela instituição.                          |
| R10 – Exposição acidental de token/senha em URL, código ou repositório                   | 2 / 3     | 6 – Alta       | Não incluir segredos em URL, aplicar secret scanning, variáveis de ambiente, rotação e revisão de logs.                                |

A matriz identifica prioridade ALTA para cenários capazes de comprometer confidencialidade, integridade ou legitimidade das pesquisas. Não foi demonstrada exploração de SQL injection no sistema atual: não existe interface com banco de dados. Da mesma forma, o relatório de riscos NÃO atesta que os controles recomendados já foram testados; eles deverão virar requisitos verificáveis da implementação futura.

### 4.4 Plano de tratamento, responsáveis e evidências esperadas

Priorização sugerida: primeira etapa — formalizar controlador, finalidades, bases legais, escopo dos dados e requisitos de acesso; segunda — implementar API com autenticação, autorização, validação e persistência transacional; terceira — isolar códigos de participação e respostas, restringir relatórios e aplicar proteção contra reidentificação; quarta — documentar política de retenção, incidentes e direitos dos titulares; quinta — executar testes funcionais, de segurança, restauração e revisão documental antes do uso com dados reais.

Responsáveis propostos: grupo desenvolvedor (requisitos, implementação e testes); orientador/responsável institucional (validação do escopo e do tratamento); controlador formalmente designado (decisões de finalidade, canal dos titulares e resposta a incidentes). A existência e os nomes desses responsáveis devem ser confirmados antes de eventual publicação com dados reais.

Evidências exigíveis para futura conclusão: diagramas e inventário atualizados; matriz revisada; scripts do banco e testes de integridade; casos de teste de SQLi, XSS, autorização e repetição de códigos; logs sem conteúdo sensível; teste de backup/restauração; aviso de privacidade aprovado; procedimento de atendimento de direitos e registro de incidente. O fechamento de uma issue acadêmica, por si só, não demonstra que essas evidências estejam incorporadas ao código.

## 5 CONSIDERAÇÕES FINAIS / CONCLUSÃO

O objetivo de diagnosticar riscos e propor controles foi atendido em nível documental. O levantamento distinguiu funcionalidades disponíveis no protótipo e funcionalidades planejadas para uma solução operacional; classificou os grupos de dados; relacionou princípios da LGPD à aplicação; e apresentou matriz de probabilidade, impacto e mitigação alinhada à issue #10. Não se pode concluir, contudo, que o sistema esteja adequado à LGPD ou seguro para dados reais, pois não há backend, banco de dados, autenticação real, persistência nem auditoria de produção.

A adequação vai além de cumprir formalidades: a separação entre identidade e respostas, a prevenção de duplicidades, a limitação de acesso e a transparência fortalecem a confiabilidade dos resultados e a credibilidade do projeto. O próximo passo técnico é transformar as propostas em requisitos aprovados, controles implementados e evidências testadas, com revisão jurídica e institucional quando o sistema passar a tratar dados pessoais efetivos.

Como avaliação do grupo, o protótipo oferece uma base apropriada para demonstrar navegação e validação de formulários, mas a qualidade exigida de uma aplicação real depende especialmente de políticas de privacidade, autorização de gestores, integridade dos dados e governança de incidentes. A abordagem preventiva permite evoluir o software sem confundir “questionário sem campo nome” com anonimização comprovada.

## REFERÊNCIAS

BRASIL. Lei nº 13.709, de 14 de agosto de 2018. Lei Geral de Proteção de Dados Pessoais (LGPD). Brasília, DF: Presidência da República, 2018. Disponível em: https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709compilado.htm. Acesso em: 23 out. 2026.

AGÊNCIA NACIONAL DE PROTEÇÃO DE DADOS (ANPD). Guia orientativo sobre segurança da informação para agentes de tratamento de pequeno porte. Brasília, DF: ANPD, 2021. Disponível em: https://www.gov.br/anpd/pt-br/centrais-de-conteudo/materiais-educativos-e-publicacoes/guia-orientativo-sobre-seguranca-da-informacao-para-agentes-de-tratamento-de-pequeno-porte. Acesso em: 23 out. 2026.

AGÊNCIA NACIONAL DE PROTEÇÃO DE DADOS (ANPD). Guia orientativo para definições dos agentes de tratamento de dados pessoais e do encarregado. Brasília, DF: ANPD, 2022. Disponível em: https://www.gov.br/anpd/pt-br/centrais-de-conteudo/materiais-educativos-e-publicacoes/guia-orientativo-para-definicoes-dos-agentes-de-tratamento-de-dados-pessoais-e-do-encarregado. Acesso em: 23 out. 2026.

AGÊNCIA NACIONAL DE PROTEÇÃO DE DADOS (ANPD). Direito dos titulares. Brasília, DF: ANPD, [s.d.]. Disponível em: https://www.gov.br/anpd/pt-br/assuntos/titular-de-dados-1/direito-dos-titulares. Acesso em: 23 out. 2026.

AGÊNCIA NACIONAL DE PROTEÇÃO DE DADOS (ANPD). Comunicação de incidente de segurança. Brasília, DF: ANPD, 2024. Disponível em: https://www.gov.br/anpd/pt-br/canais_atendimento/agente-de-tratamento/comunicado-de-incidente-de-seguranca-cis. Acesso em: 23 out. 2026.

AGÊNCIA NACIONAL DE PROTEÇÃO DE DADOS (ANPD). Guia orientativo: tratamento de dados pessoais para fins acadêmicos e para a realização de estudos e pesquisas. Brasília, DF: ANPD, 2024. Disponível em: https://www.gov.br/anpd/pt-br/centrais-de-conteudo/materiais-educativos-e-publicacoes/guia-orientativo-tratamento-de-dados-pessoais-para-fins-academicos-e-para-a-realizacao-de-estudos-e-pesquisas. Acesso em: 23 out. 2026.

FATEC-EXTENSAO. pi-ads-2026-2-igor: Sistema Web para Gestão de Pesquisas por Questionários. GitHub, 2026. Disponível em: https://github.com/Fatec-Extensao/pi-ads-2026-2-igor. Acesso em: 23 out. 2026.

FATEC-EXTENSAO. Issue #10: \[Segurança\] Análise de Riscos Básica e Ameaças Web. GitHub, 2026. Disponível em: https://github.com/Fatec-Extensao/pi-ads-2026-2-igor/issues/10. Acesso em: 23 out. 2026.
