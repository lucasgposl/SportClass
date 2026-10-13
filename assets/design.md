# Design — Sport Class

## 1. Propósito e princípio de desenvolvimento

O Sport Class organiza o campeonato interclasses. A primeira entrega técnica deve ser uma API backend funcional e testável para gerenciar modalidades, turmas e partidas. O frontend será desenvolvido depois que as rotas essenciais estiverem implementadas e testadas.

## 2. Arquitetura

A aplicação será dividida em três camadas:

```text
sport-class/
├── backend/
│   ├── server.js
│   ├── package.json
│   ├── .env
│   ├── config/
│   │   └── database.js
│   ├── models/
│   │   ├── index.js
│   │   ├── Usuario.js
│   │   ├── Modalidade.js
│   │   ├── Time.js
│   │   └── Partida.js
│   ├── controllers/
│   │   ├── modalidadeController.js
│   │   ├── timeController.js
│   │   ├── partidaController.js
│   │   ├── resultadoController.js
│   │   └── classificacaoController.js
│   ├── routes/
│   │   ├── modalidadeRoutes.js
│   │   ├── timeRoutes.js
│   │   ├── partidaRoutes.js
│   │   ├── resultadoRoutes.js
│   │   └── classificacaoRoutes.js
│   ├── middleware/
│   │   ├── autenticar.js
│   │   └── autorizarOrganizador.js
│   └── tests/
└── frontend/
    ├── package.json
    └── src/
        ├── services/
        │   └── api.js
        ├── components/
        ├── pages/
        └── styles/
```

### Tecnologias

- **Backend:** Node.js e Express.
- **Banco de dados:** PostgreSQL.
- **ORM:** Sequelize.
- **Frontend:** React, HTML, CSS e JavaScript.
- **Testes de rotas:** Postman ou Insomnia; testes automatizados podem ser adicionados para validar regras e respostas.
- **Autenticação:** mecanismo de autenticação implementado no backend, com verificação de permissões nas rotas protegidas.

PostgreSQL será a fonte persistente dos dados. O frontend não deve acessar o banco diretamente: todas as operações devem passar pela API.

## 3. Modelos de dados

### Usuario
- `id`: chave primária.
- `nome`: nome do usuário.
- `email`: e-mail único.
- `senhaHash`: hash da senha, nunca a senha em texto puro.
- `papel`: `ORGANIZADOR` ou `REPRESENTANTE`.

### Modalidade
- `id`: chave primária.
- `nome`: nome único, limitado à lista permitida.

### Time
- `id`: chave primária.
- `nome`: identificação única da turma.

### Partida
- `id`: chave primária.
- `modalidadeId`: chave estrangeira para Modalidade.
- `timeAId`: chave estrangeira para Time.
- `timeBId`: chave estrangeira para Time.
- `data`: data da partida.
- `horario`: horário da partida.
- `local`: local da partida.
- `placarA` e `placarB`: placares, inicialmente nulos.
- `situacao`: `AGENDADA`, `REALIZADA`, `CANCELADA` ou `WO`.

### Relacionamentos

```text
Modalidade 1 ─── N Partida
Time       1 ─── N Partida (como timeA)
Time       1 ─── N Partida (como timeB)
```

Os relacionamentos devem ser configurados nos Models do Sequelize. Regras como impedir que `timeAId` seja igual a `timeBId` devem ser validadas na API.

## 4. Contrato da API REST

Prefixo base: `/api`.

As respostas de erro devem utilizar JSON consistente, por exemplo:

```json
{
  "erro": {
    "mensagem": "Descrição do problema"
  }
}
```

### 4.1 Saúde da API

| Método | Rota | Acesso | Resultado esperado |
|---|---|---|---|
| GET | `/api/health` | Público | `200 OK`, confirma que a API está respondendo |

Exemplo de resposta:

```json
{
  "status": "ok",
  "servico": "Sport Class API"
}
```

### 4.2 Modalidades

| Método | Rota | Acesso | Finalidade |
|---|---|---|---|
| GET | `/api/modalidades` | Público | Listar modalidades |
| POST | `/api/modalidades` | Organizador | Cadastrar modalidade permitida |
| GET | `/api/modalidades/:id` | Público | Consultar modalidade por ID |

Exemplo de entrada para `POST /api/modalidades`:

```json
{
  "nome": "Futsal"
}
```

Respostas previstas: `201 Created` no cadastro; `400 Bad Request` para dados inválidos; `404 Not Found` para ID inexistente; `409 Conflict` para duplicidade.

### 4.3 Turmas

| Método | Rota | Acesso | Finalidade |
|---|---|---|---|
| GET | `/api/turmas` | Público | Listar turmas |
| POST | `/api/turmas` | Organizador | Cadastrar turma |
| GET | `/api/turmas/:id` | Público | Consultar turma por ID |

Exemplo de entrada para `POST /api/turmas`:

```json
{
  "nome": "2º Ano A"
}
```

Respostas previstas: `201 Created`, `400 Bad Request`, `404 Not Found` e `409 Conflict`, conforme o caso.

### 4.4 Partidas

| Método | Rota | Acesso | Finalidade |
|---|---|---|---|
| GET | `/api/partidas` | Público | Listar partidas e aceitar filtros |
| POST | `/api/partidas` | Organizador | Cadastrar partida |
| GET | `/api/partidas/:id` | Público | Consultar partida específica |
| PUT | `/api/partidas/:id` | Organizador | Alterar dados de uma partida |
| PATCH | `/api/partidas/:id/cancelar` | Organizador | Cancelar partida |

Filtros de `GET /api/partidas`:
- `modalidadeId`
- `timeId`
- `dataInicial`
- `dataFinal`

Exemplo de consulta:

```http
GET /api/partidas?modalidadeId=1&timeId=2&dataInicial=2026-10-01&dataFinal=2026-10-31
```

Exemplo de entrada para `POST /api/partidas`:

```json
{
  "modalidadeId": 1,
  "timeAId": 2,
  "timeBId": 3,
  "data": "2026-10-20",
  "horario": "14:00",
  "local": "Quadra da escola"
}
```

Validações: campos obrigatórios; modalidade e turmas existentes; turmas diferentes; conflito de horário e modalidade; situação inicial `AGENDADA`.

Respostas previstas: `201 Created`, `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found` e `409 Conflict`.

### 4.5 Resultados

| Método | Rota | Acesso | Finalidade |
|---|---|---|---|
| PATCH | `/api/partidas/:id/resultado` | Organizador | Registrar ou corrigir o resultado |
| PATCH | `/api/partidas/:id/wo` | Organizador | Registrar W.O. conforme a regra do campeonato |

Exemplo de entrada para registrar resultado:

```json
{
  "placarA": 2,
  "placarB": 1
}
```

A API deve validar os placares, a existência da partida e a regra de data definida nos requisitos. Ao salvar um resultado válido, deve atualizar a situação e recalcular a classificação afetada.

Para W.O., o sistema deve identificar a turma beneficiada e aplicar o placar 3 × 0. A turma beneficiada deve ser informada de maneira explícita no contrato da rota durante a implementação, sem inferir isso automaticamente.

### 4.6 Classificação

| Método | Rota | Acesso | Finalidade |
|---|---|---|---|
| GET | `/api/modalidades/:id/classificacao` | Público | Consultar classificação de uma modalidade |

A resposta deve incluir a turma, pontos, jogos considerados, vitórias, empates, derrotas e saldo. A ordenação deve seguir pontos, saldo e confronto direto, conforme os requisitos.

## 5. Estratégia de testes da API

As rotas devem ser testadas antes do início do desenvolvimento das páginas. Os testes devem cobrir:

- API disponível e conexão com PostgreSQL.
- Cadastro, consulta, duplicidade e validação de modalidades.
- Cadastro e consulta de turmas.
- Cadastro, consulta, alteração e cancelamento de partidas.
- Filtros combinados.
- Rejeição de partidas inválidas e conflitos de horário.
- Registro de resultados e bloqueio de resultados fora da regra de data.
- Cálculo de pontuação, saldo, desempate e W.O.
- Permissões do Organizador e do Representante.
- Códigos HTTP e estrutura das respostas de erro.

Exemplo de teste manual:

```http
GET http://localhost:3000/api/health
```

Resultado esperado: status HTTP `200` e JSON com `status` igual a `ok`.

Outro exemplo:

```http
GET http://localhost:3000/api/modalidades
```

Resultado esperado: status HTTP `200` e uma lista JSON de modalidades.

A API deve ser considerada pronta para integração quando as rotas essenciais passarem nos testes e as regras de negócio estiverem validadas.

## 6. Design do frontend por página

O frontend será iniciado após a conclusão dos testes essenciais da API. Não começar por dashboard: priorizar a consulta e o gerenciamento de partidas.

### Página: Consulta de partidas
- `ListaPartidas`: apresenta partidas retornadas pela API.
- `CardPartida`: mostra turmas, modalidade, data, horário, local e situação.
- `FiltrosPartidas`: agrupa os filtros.
- `FiltroModalidade`, `FiltroTurma` e `FiltroPeriodo`: selecionam os critérios.
- `EstadoVazio`: informa quando não há partidas.
- `MensagemErro`: mostra falhas de consulta.

### Página: Cadastro/edição de partida
- `FormularioPartida`: reúne os campos da partida.
- `SeletorModalidade`: carrega modalidades da API.
- `SeletorTimes`: carrega as turmas.
- `CampoData`, `CampoHorario` e `CampoLocal`.
- `BotaoSalvar` e `MensagemValidacao`.

A página só estará disponível para Organizador autenticado.

### Página: Registro de resultado
- `FormularioResultado`: informa os placares.
- `AcaoRegistrarWO`: registra W.O. com identificação da turma beneficiada.
- `ResumoPartida`: exibe os dados da partida.
- `MensagemResultado`: confirma sucesso ou apresenta erros.

### Página: Classificação
- `SeletorModalidade`: escolhe a modalidade.
- `TabelaClassificacao`: mostra pontos, jogos, vitórias, empates, derrotas e saldo.
- `EstadoVazio` e `MensagemErro`.

### Página: Login
- `FormularioLogin`: recebe credenciais.
- `CampoEmail` e `CampoSenha`.
- `MensagemErroLogin`.

### Página: Gerenciamento de modalidades e turmas
- `FormularioModalidade` e `ListaModalidades`.
- `FormularioTurma` e `ListaTurmas`.
- `MensagemValidacao` e `MensagemSucesso`.

## 7. Identidade visual e cores

A paleta abaixo foi transcrita das capturas de tela dos estilos do Figma. Os valores são apresentados em HEX; a aplicação específica de cada cor é orientada pelo painel do organizador mostrado no protótipo.

| Token CSS | HEX | Aplicação sugerida conforme o protótipo |
|---|---|---|
| `--cor-fundo` | `#0D1117` | Fundo principal escuro |
| `--cor-superficie` | `#151F32` | Áreas de conteúdo e painéis |
| `--cor-superficie-secundaria` | `#21262D` | Superfícies e elementos secundários |
| `--cor-superficie-terciaria` | `#1E2D4A` | Cards, painéis e áreas destacadas |
| `--cor-borda` | `#30363D` | Bordas e divisórias |
| `--cor-primaria` | `#2563EB` | Botões principais, links e destaques |
| `--cor-texto` | `#F8FAFC` | Texto principal em fundos escuros |
| `--cor-texto-claro` | `#FFFFFF` | Texto ou ícones brancos |
| `--cor-texto-secundario` | `#8B949E` | Rótulos e informações auxiliares |
| `--cor-texto-suave` | `#E2E8F0` | Texto secundário com maior contraste |
| `--cor-sucesso` | `#10B981` | Estados concluídos e confirmações |
| `--cor-destaque-lilas` | `#818CF8` | Destaques auxiliares |
| `--cor-destaque-laranja` | `#FF6B00` | Destaques pontuais, como alertas visuais |

### Variáveis CSS

```css
:root {
  --cor-fundo: #0D1117;
  --cor-superficie: #151F32;
  --cor-superficie-secundaria: #21262D;
  --cor-superficie-terciaria: #1E2D4A;
  --cor-borda: #30363D;
  --cor-primaria: #2563EB;
  --cor-texto: #F8FAFC;
  --cor-texto-claro: #FFFFFF;
  --cor-texto-secundario: #8B949E;
  --cor-texto-suave: #E2E8F0;
  --cor-sucesso: #10B981;
  --cor-destaque-lilas: #818CF8;
  --cor-destaque-laranja: #FF6B00;
}
```

A interface deve priorizar o fundo escuro, painéis em tons azul-marinho e ações principais em azul, seguindo a captura do painel do organizador. Lilás e laranja aparecem como cores de destaque pontual. Os percentuais de opacidade mostrados em algumas linhas do Figma correspondem a usos com transparência; os HEX acima representam as cores-base.

## 8. Critérios de conclusão do design

- A API e suas rotas estão documentadas e testáveis.
- Os modelos e relacionamentos estão definidos.
- O frontend está separado do backend e consome apenas a API.
- Cada página tem seus componentes especificados.
- A implementação respeita o escopo definido em `requirements.md`.
- A paleta foi documentada com os valores HEX visíveis nas capturas do Figma.