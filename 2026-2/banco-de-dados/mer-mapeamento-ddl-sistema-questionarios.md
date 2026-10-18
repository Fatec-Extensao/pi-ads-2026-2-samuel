MER, MAPEAMENTO E SCRIPT DDL

Projeto Integrador II - Sistema de Questionarios

Aluno: Samuel da Silva Robles Garcia

1) Descricao do sistema

O sistema permite que um administrador crie questionarios e perguntas. Os respondentes recebem credenciais geradas automaticamente e um token individual para acessar a pesquisa. As respostas ficam registradas, e o administrador envia os resultados para o proprietario de cada pesquisa.

Premissas adotadas: cada questionario tem um administrador responsavel e um proprietario; cada respondente recebe um token por questionario e pode realizar um envio de respostas por token. As perguntas podem ser de texto, escolha unica ou multipla escolha.

2) MER - Entidades e atributos

USUARIO: id_usuario (PK), nome, email, senha_hash, perfil, criado_em

QUESTIONARIO: id_questionario (PK), titulo, descricao, status, criado_em, id_admin (FK), id_proprietario (FK)

PERGUNTA: id_pergunta (PK), enunciado, tipo, obrigatoria, ordem, id_questionario (FK)

OPCAO_PERGUNTA: id_opcao (PK), texto_opcao, ordem, id_pergunta (FK)

TOKEN_ACESSO: id_token (PK), token_hash, gerado_em, expira_em, utilizado_em, id_usuario (FK), id_questionario (FK)

RESPOSTA: id_resposta (PK), enviada_em, id_token (FK)

ITEM_RESPOSTA: id_item (PK), resposta_texto, id_resposta (FK), id_pergunta (FK), id_opcao (FK, opcional)

ENVIO_RESULTADO: id_envio (PK), enviado_em, observacao, id_questionario (FK), id_admin (FK), id_proprietario (FK)

3) Relacionamentos e cardinalidades

Usuario (admin) 1:N Questionario; Usuario (proprietario) 1:N Questionario.

Questionario 1:N Pergunta; Pergunta 1:N Opcao_Pergunta.

Usuario (respondente) 1:N Token_Acesso; Questionario 1:N Token_Acesso.

Token_Acesso 1:0..1 Resposta; Resposta 1:N Item_Resposta.

Pergunta 1:N Item_Resposta; Opcao_Pergunta 1:N Item_Resposta (opcional).

Questionario 1:N Envio_Resultado; Usuario 1:N Envio_Resultado (admin e proprietario).

4) Diagrama MER

PK = chave primaria; FK = chave estrangeira. Um usuario pode exercer um dos perfis: ADMIN, PROPRIETARIO ou RESPONDENTE.

5) Mapeamento para o modelo relacional

USUARIO (id_usuario (PK), nome, email, senha_hash, perfil, criado_em)

QUESTIONARIO (id_questionario (PK), titulo, descricao, status, criado_em, id_admin (FK), id_proprietario (FK))

PERGUNTA (id_pergunta (PK), enunciado, tipo, obrigatoria, ordem, id_questionario (FK))

OPCAO_PERGUNTA (id_opcao (PK), texto_opcao, ordem, id_pergunta (FK))

TOKEN_ACESSO (id_token (PK), token_hash, gerado_em, expira_em, utilizado_em, id_usuario (FK), id_questionario (FK))

RESPOSTA (id_resposta (PK), enviada_em, id_token (FK))

ITEM_RESPOSTA (id_item (PK), resposta_texto, id_resposta (FK), id_pergunta (FK), id_opcao (FK, opcional))

ENVIO_RESULTADO (id_envio (PK), enviado_em, observacao, id_questionario (FK), id_admin (FK), id_proprietario (FK))

Regras: email de usuario unico; token_hash unico; um token por usuario e questionario; no maximo uma resposta por token; ordem das perguntas e opcoes nao pode se repetir dentro do mesmo conjunto.

6) Script DDL - PostgreSQL

Os comandos abaixo criam as tabelas e as restricoes na ordem necessaria.

```sql
-- Sistema de Questionarios - Projeto Integrador II
-- PostgreSQL | Script DDL

CREATE TABLE usuario (
    id_usuario BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    nome VARCHAR(120) NOT NULL,
    email VARCHAR(180) NOT NULL UNIQUE,
    senha_hash TEXT NOT NULL,
    perfil VARCHAR(15) NOT NULL,
    criado_em TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT ck_usuario_perfil
        CHECK (perfil IN ('ADMIN', 'PROPRIETARIO', 'RESPONDENTE'))
);

CREATE TABLE questionario (
    id_questionario BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    titulo VARCHAR(180) NOT NULL,
    descricao TEXT,
    status VARCHAR(15) NOT NULL DEFAULT 'RASCUNHO',
    criado_em TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    id_admin BIGINT NOT NULL,
    id_proprietario BIGINT NOT NULL,
    CONSTRAINT fk_questionario_admin FOREIGN KEY (id_admin)
        REFERENCES usuario (id_usuario),
    CONSTRAINT fk_questionario_proprietario FOREIGN KEY (id_proprietario)
        REFERENCES usuario (id_usuario),
    CONSTRAINT ck_questionario_status
        CHECK (status IN ('RASCUNHO', 'PUBLICADO', 'ENCERRADO'))
);

CREATE TABLE pergunta (
    id_pergunta BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    id_questionario BIGINT NOT NULL,
    enunciado TEXT NOT NULL,
    tipo VARCHAR(20) NOT NULL,
    obrigatoria BOOLEAN NOT NULL DEFAULT TRUE,
    ordem INT NOT NULL,
    CONSTRAINT fk_pergunta_questionario FOREIGN KEY (id_questionario)
        REFERENCES questionario (id_questionario),
    CONSTRAINT uq_pergunta_ordem UNIQUE (id_questionario, ordem),
    CONSTRAINT ck_pergunta_tipo
        CHECK (tipo IN ('TEXTO', 'ESCOLHA_UNICA', 'MULTIPLA_ESCOLHA')),
    CONSTRAINT ck_pergunta_ordem CHECK (ordem > 0)
);

CREATE TABLE opcao_pergunta (
    id_opcao BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    id_pergunta BIGINT NOT NULL,
    texto_opcao VARCHAR(300) NOT NULL,
    ordem INT NOT NULL,
    CONSTRAINT fk_opcao_pergunta FOREIGN KEY (id_pergunta)
        REFERENCES pergunta (id_pergunta),
    CONSTRAINT uq_opcao_ordem UNIQUE (id_pergunta, ordem),
    CONSTRAINT uq_opcao_id_pergunta UNIQUE (id_opcao, id_pergunta),
    CONSTRAINT ck_opcao_ordem CHECK (ordem > 0)
);

CREATE TABLE token_acesso (
    id_token BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    id_usuario BIGINT NOT NULL,
    id_questionario BIGINT NOT NULL,
    token_hash TEXT NOT NULL UNIQUE,
    gerado_em TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    expira_em TIMESTAMP NOT NULL,
    utilizado_em TIMESTAMP,
    CONSTRAINT fk_token_usuario FOREIGN KEY (id_usuario)
        REFERENCES usuario (id_usuario),
    CONSTRAINT fk_token_questionario FOREIGN KEY (id_questionario)
        REFERENCES questionario (id_questionario),
    CONSTRAINT uq_token_usuario_questionario
        UNIQUE (id_usuario, id_questionario),
    CONSTRAINT ck_token_expiracao CHECK (expira_em > gerado_em)
);

CREATE TABLE resposta (
    id_resposta BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    id_token BIGINT NOT NULL UNIQUE,
    enviada_em TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_resposta_token FOREIGN KEY (id_token)
        REFERENCES token_acesso (id_token)
);

CREATE TABLE item_resposta (
    id_item BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    id_resposta BIGINT NOT NULL,
    id_pergunta BIGINT NOT NULL,
    id_opcao BIGINT,
    resposta_texto TEXT,
    CONSTRAINT fk_item_resposta FOREIGN KEY (id_resposta)
        REFERENCES resposta (id_resposta),
    CONSTRAINT fk_item_pergunta FOREIGN KEY (id_pergunta)
        REFERENCES pergunta (id_pergunta),
    CONSTRAINT fk_item_opcao_pergunta FOREIGN KEY (id_opcao, id_pergunta)
        REFERENCES opcao_pergunta (id_opcao, id_pergunta),
    CONSTRAINT ck_item_conteudo CHECK (
```
        (id_opcao IS NOT NULL AND resposta_texto IS NULL)
        OR
        (id_opcao IS NULL AND resposta_texto IS NOT NULL
         AND LENGTH(TRIM(resposta_texto)) > 0)
    )
);

-- Evita repetir o mesmo texto em uma mesma pergunta/resposta.
```sql
CREATE UNIQUE INDEX uq_item_texto
    ON item_resposta (id_resposta, id_pergunta)
    WHERE id_opcao IS NULL;

-- Evita que uma alternativa seja marcada duas vezes.
CREATE UNIQUE INDEX uq_item_alternativa
    ON item_resposta (id_resposta, id_pergunta, id_opcao)
    WHERE id_opcao IS NOT NULL;

CREATE TABLE envio_resultado (
    id_envio BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    id_questionario BIGINT NOT NULL,
    id_admin BIGINT NOT NULL,
    id_proprietario BIGINT NOT NULL,
    enviado_em TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    observacao TEXT,
    CONSTRAINT fk_envio_questionario FOREIGN KEY (id_questionario)
        REFERENCES questionario (id_questionario),
    CONSTRAINT fk_envio_admin FOREIGN KEY (id_admin)
        REFERENCES usuario (id_usuario),
    CONSTRAINT fk_envio_proprietario FOREIGN KEY (id_proprietario)
        REFERENCES usuario (id_usuario)
);

-- Observacoes de implementacao:
-- 1) Gerar senhas e tokens com algoritmo seguro no backend.
-- 2) Guardar somente os hashes; nao enviar hashes aos respondentes.
-- 3) Conferir no backend se os perfis de admin/proprietario estao corretos.
-- 4) Conferir se a pergunta pertence ao questionario do token.
-- 5) Conferir validade/uso do token e marcar uso em transacao.
-- 6) Conferir se o destinatario do resultado e o proprietario da pesquisa.
-- 7) O envio real de e-mails/relatorios e feito pela aplicacao, nao pelo DDL.
```
