# Sport Class — Campeonato Interclasses

## Sobre o projeto

O **Sport Class** é uma aplicação web desenvolvida para facilitar a organização e o acompanhamento dos campeonatos esportivos entre as turmas da escola.

O sistema centraliza o cadastro de modalidades, turmas, partidas e resultados, substituindo o controle manual feito em cartazes. Dessa forma, estudantes e representantes podem consultar os jogos e acompanhar a classificação com informações organizadas.

## Objetivo

- Facilitar a organização dos campeonatos entre as turmas.
- Permitir o cadastro das modalidades esportivas.
- Cadastrar as turmas participantes.
- Registrar e consultar as partidas do campeonato.
- Registrar os resultados dos jogos.
- Exibir a classificação por modalidade.
- Facilitar o acompanhamento do campeonato pelos representantes das turmas.

## Funcionalidades

### Cadastro e gerenciamento

- Cadastro de modalidades esportivas:
  - Futsal
  - Vôlei
  - Basquete
  - Handebol
  - Queimada
- Cadastro e consulta das turmas participantes.
- Cadastro e gerenciamento de partidas com:
  - Times participantes
  - Data e horário
  - Local
  - Modalidade

### Consulta de partidas

- Listagem das partidas cadastradas.
- Consulta por turma, modalidade e período.
- Exibição da situação da partida:
  - Agendada
  - Realizada
  - Cancelada
  - W.O.

### Resultados

- Registro e atualização de placares pelo organizador, conforme as regras do campeonato.
- Atualização da situação da partida após o registro do resultado.
- Cálculo da pontuação a partir dos resultados registrados.

### Classificação

A classificação é organizada por modalidade e calculada com base nos resultados cadastrados.

- Vitória: 3 pontos.
- Empate: 1 ponto para cada equipe.
- Derrota: 0 pontos.
- W.O.: aplicação da regra de resultado definida para o campeonato.
- Critérios de desempate documentados para o projeto:
  1. Pontos.
  2. Saldo.
  3. Confronto direto.

### Perfis de acesso

**Organizador**
- Cadastra modalidades e turmas.
- Cadastra e gerencia partidas.
- Registra resultados.
- Gerencia as informações do campeonato.

**Representante de turma**
- Consulta partidas e resultados.
- Consulta a classificação.
- Utiliza os filtros de consulta disponíveis.

As operações administrativas devem ser autorizadas no backend. A interface ocultar ou exibir um botão, por si só, não substitui a verificação de permissões pela API.

## Tecnologias utilizadas

### Frontend
- React
- Vite
- HTML
- CSS
- JavaScript

### Backend
- Node.js
- Express

### Banco de dados
- PostgreSQL
- Sequelize (ORM para comunicação entre a aplicação e o banco de dados)

## API

O Sport Class utiliza uma API REST desenvolvida com Node.js e Express. Ela centraliza as operações de cadastro, consulta e gerenciamento das modalidades, turmas, partidas e resultados.

A API também disponibiliza a classificação por modalidade, calculada a partir dos resultados armazenados no PostgreSQL. O Sequelize é utilizado para mapear os modelos da aplicação e realizar operações no banco de dados.

Principais grupos de rotas previstos:

- `/api/health` — verifica se a API está funcionando.
- `/api/modalidades` — consulta e cadastra modalidades.
- `/api/turmas` — consulta e cadastra turmas.
- `/api/partidas` — consulta e gerencia partidas.
- `/api/modalidades/:id/classificacao` — consulta a classificação de uma modalidade.

## Regras do campeonato

- Somente o organizador pode criar modalidades, cadastrar turmas, gerenciar partidas e registrar resultados.
- O representante de turma tem acesso às consultas permitidas.
- Uma turma não pode jogar contra ela mesma.
- Uma turma não pode possuir duas partidas no mesmo horário e modalidade.
- O resultado só pode ser lançado quando as regras de data e situação da partida permitirem.
- A classificação é calculada com base nos resultados registrados.
- O campeonato utiliza o sistema de pontos corridos, conforme as regras definidas para o projeto.

## Fora do escopo

- Geração automática de chaveamento.
- Sorteio automático da tabela.
- Fase mata-mata.
- Chaves e playoffs.
- Súmula por atleta.
- Registro individual de gols, cartões ou artilharia.
- Cadastro individual de alunos como atletas.

## Instalação e execução

> **Observação:** os comandos abaixo são um modelo de referência. A execução depende da estrutura de pastas, dos scripts existentes nos arquivos `package.json` e das configurações locais do PostgreSQL. Ajuste os caminhos conforme a organização final do repositório.

### Pré-requisitos

- Node.js e npm instalados.
- PostgreSQL instalado e em execução.
- Um banco de dados criado para o Sport Class.

### 1. Obter o projeto

Baixe ou clone o repositório e abra a pasta do projeto no terminal.

### 2. Configurar o backend

Entre na pasta do backend (caso ele esteja em uma pasta separada) e instale as dependências:

```bash
npm install
```

Configure as variáveis de ambiente de conexão com o PostgreSQL no arquivo `.env`, usando os nomes esperados pela configuração da aplicação. Não compartilhe nem envie o arquivo `.env` com senhas reais.

Exemplo ilustrativo — os nomes das variáveis precisam corresponder ao código do projeto:

```env
DB_HOST=localhost
DB_PORT=5432
DB_NAME=sport_class
DB_USER=seu_usuario
DB_PASSWORD=sua_senha
PORT=3000
```

Inicie o backend com o script configurado no `package.json`. Por exemplo, se existir um script chamado `dev`:

```bash
npm run dev
```

Se o projeto usar outro nome de script, execute o comando correspondente definido no `package.json`.

### 3. Configurar o frontend

Entre na pasta do frontend (caso esteja separada) e instale as dependências:

```bash
npm install
```

Configure a URL base da API conforme a variável de ambiente ou o arquivo de configuração usado pelo frontend.

Se houver um script `dev` no `package.json`, inicie o frontend com:

```bash
npm run dev
```

O Vite informará no terminal o endereço local para abrir a aplicação no navegador.

### 4. Testes

Execute os testes disponíveis usando o script configurado no `package.json`. Por exemplo, se houver um script `test`:

```bash
npm test
```

Os comandos acima são exemplos: use os scripts que estiverem realmente definidos no projeto.

## Estrutura prevista do projeto

A estrutura definitiva pode variar conforme a implementação. Uma organização possível é:

```text
sport-class/
├── backend/
│   ├── config/
│   ├── models/
│   ├── controllers/
│   ├── routes/
│   ├── tests/
│   ├── .env.example
│   ├── package.json
│   └── server.js
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   └── styles/
│   └── package.json
├── requirements.md
├── design.md
├── tasks.md
└── README.md
```

Essa árvore é uma referência de organização, não uma afirmação de que todas essas pastas e arquivos já foram criados.

## Documentação do projeto

- `requirements.md` — requisitos funcionais, não funcionais e regras do sistema.
- `design.md` — arquitetura, API, banco de dados e organização da interface.
- `tasks.md` — lista ordenada de tarefas para implementar e testar o projeto.

## Autores

- Lucas Gabriel
- Styven Bôtto