# Documentação Técnica Final do Projeto

**Projeto:** Sistema Web para Gestão de Pesquisas por Questionários  
**Curso:** Análise e Desenvolvimento de Sistemas — FATEC Lins  
**Semestre:** 2026/2  
**Projeto Integrador ADS II:** Prof. João Luís Cardoso de Moraes  
**Versão:** 1.0  
**Data:** 23/11/2026

## 1. Identificação

O projeto tem como objetivo desenvolver um sistema web para criação, aplicação e análise de pesquisas institucionais por questionários. A solução foi proposta para instituições que necessitam coletar opiniões e organizar resultados por categorias de participantes.

## 2. Equipe

| Integrante | Perfil GitHub informado no README |
|---|---|
| Samuel da Silva Robles Garcia | [@samuelrobles012-lgtm](https://github.com/samuelrobles012-lgtm) |
| Igor Rodrigues | [@TicooooIg](https://github.com/TicooooIg) |
| Luis Miguel Schiazza | [@luismschiazza](https://github.com/luismschiazza) |
| Marcelo Oliveira Souza | [@eaecelo](https://github.com/eaecelo) |
| Matheus Bispo | [@Matheusxdx2](https://github.com/Matheusxdx2) |

## 3. Problema e justificativa

Instituições podem precisar consultar diferentes públicos por meio de questionários, organizar participantes por categorias e analisar respostas para apoiar decisões. O projeto propõe centralizar esse processo em uma aplicação web, reduzindo a dispersão das informações e facilitando a leitura dos resultados.

A privacidade é um requisito essencial: a aplicação foi concebida para permitir participação por código/senha sem associar a identidade do respondente ao conteúdo de suas respostas. Essa característica precisa ser confirmada por testes técnicos na implementação final.

## 4. Objetivo geral

Desenvolver um sistema web para gerenciar pesquisas institucionais por questionários, desde a configuração até a coleta e análise das respostas.

## 5. Objetivos específicos

- cadastrar e gerenciar pesquisas;
- definir categorias de participantes;
- criar questionários específicos por categoria;
- permitir acesso à pesquisa por código/senha de participação;
- coletar e armazenar respostas;
- acompanhar a participação;
- gerar relatórios estatísticos e analíticos;
- aplicar medidas básicas de segurança e privacidade alinhadas à LGPD.

## 6. Escopo funcional

| Módulo | Responsabilidade prevista |
|---|---|
| Administração | Gerenciar pesquisas e configurações |
| Categorias | Organizar os públicos respondentes |
| Questionários | Definir perguntas e alternativas |
| Acesso do participante | Validar código de participação e liberar o questionário |
| Respostas | Receber e armazenar as respostas |
| Monitoramento | Exibir indicadores de participação |
| Relatórios | Consolidar respostas em estatísticas e análises |

A tabela descreve o escopo do projeto conforme o README. A disponibilidade efetiva de cada função deve ser confirmada na versão publicada e no relatório de validação.

## 7. Organização do repositório

O README do repositório define a organização das entregas do semestre:

```text
2026-2/
├── desenvolvimento-web/
│   ├── src/                  # Código-fonte das páginas web
│   └── wireframes/           # Protótipos e esquemas de tela
├── engenharia-software/      # Requisitos e diagramas UML
├── banco-de-dados/           # Modelagem e scripts SQL
└── seguranca/                # Análise de riscos e documentos LGPD
```

O grupo deve manter a estrutura alinhada aos arquivos efetivamente existentes no repositório e evitar registrar como concluído um artefato que ainda não foi produzido.

## 8. Tecnologias

O README do projeto descreve a interface como uma solução web estática/front-end, utilizando HTML, CSS e JavaScript básico. As tecnologias de backend, banco de dados, hospedagem e bibliotecas efetivamente utilizadas devem ser preenchidas após conferência do código-fonte.

| Camada | Tecnologia | Situação |
|---|---|---|
| Estrutura das páginas | HTML5 | Conforme escopo documentado; confirmar código |
| Estilização | CSS3 | Conforme escopo documentado; confirmar código |
| Interatividade do navegador | JavaScript | Conforme escopo documentado; confirmar código |
| Backend/API | A preencher | Não confirmado neste documento |
| Banco de dados | A preencher | Não confirmado neste documento |
| Hospedagem | GitHub Pages (proposta) | Depende da execução do deploy |

## 9. Segurança e privacidade

A solução deve aplicar minimização de dados, autenticação administrativa, controle de acesso, comunicação HTTPS, proteção dos registros, política de retenção e separação entre códigos de participação e respostas. Os relatórios devem apresentar dados agregados e evitar a identificação indireta de respondentes em grupos pequenos.

O documento específico `../seguranca/lgpd-e-anonimato.md` apresenta o diagnóstico, os controles recomendados e os casos de teste. O arquivo `../seguranca/registro-de-tratamento.md` contém o modelo de inventário para preenchimento pelo controlador da pesquisa.

## 10. Publicação

A estratégia documentada para publicação do front-end estático é o GitHub Pages, automatizado por GitHub Actions. O procedimento está descrito em `../desenvolvimento-web/publicacao-e-deploy.md`.

**URL pública:** preencher após o deploy e teste em navegador.  
**Commit publicado:** preencher com o hash da versão implantada.  
**Data:** preencher após a publicação.

## 11. Validação

Os testes funcionais, de segurança, privacidade, compatibilidade e usabilidade estão descritos em `validacao-do-projeto.md`. A equipe deve registrar os resultados observados e anexar evidências reais antes de marcar a validação como concluída.

## 12. Limitações conhecidas a confirmar

- O GitHub Pages hospeda front-end estático e não executa sozinho uma API ou banco de dados.
- A existência de telas não comprova que os formulários persistem dados.
- O uso de códigos anônimos não comprova anonimato se existirem logs ou tabelas que permitam correlacionar códigos, pessoas e respostas.
- Relatórios estatísticos precisam ser testados com dados fictícios e revisados quanto ao risco de reidentificação.

## 13. Conclusão

O Sistema Web para Gestão de Pesquisas por Questionários integra conhecimentos de desenvolvimento web, modelagem de dados, engenharia de software e segurança da informação. A documentação final organiza o escopo funcional, a estrutura das entregas, a estratégia de privacidade, o processo de publicação e o plano de validação.

O encerramento técnico depende da conferência dos artefatos implementados, da execução dos testes e da inclusão do endereço público e das evidências. Este documento não declara funcionalidades ou resultados de testes que ainda não tenham sido comprovados.

## 14. Referências

- Repositório do projeto: <https://github.com/Fatec-Extensao/pi-ads-2026-2-samuel>.
- BRASIL. Lei nº 13.709/2018 — LGPD: <https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709compilado.htm>.
- ANPD. Guia orientativo sobre segurança da informação: <https://www.gov.br/anpd/pt-br/centrais-de-conteudo/materiais-educativos-e-publicacoes/guia-orientativo-sobre-seguranca-da-informacao-para-agentes-de-tratamento-de-pequeno-porte>.
- GitHub Docs. GitHub Pages: <https://docs.github.com/en/pages>.
