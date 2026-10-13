# Tasks — Sport Class

> Ordem obrigatória: backend e testes da API primeiro; frontend somente depois que as rotas essenciais estiverem verificadas. O projeto começa pelas operações centrais do campeonato, não por dashboard.

## 1. Preparação do backend

- [ ] Criar a pasta `backend`
- [ ] Inicializar o projeto Node.js com `npm init -y`
- [ ] Instalar Express, Sequelize, PostgreSQL driver e dotenv
- [ ] Configurar o script de desenvolvimento no `package.json`
- [ ] Criar `server.js`
- [ ] Configurar o Express e o middleware de JSON
- [ ] Criar a rota `GET /api/health`
- [ ] Executar a API localmente
- [ ] Testar `GET /api/health` no Postman ou Insomnia
- [ ] Configurar o tratamento centralizado de erros

## 2. Banco de dados e Models

- [ ] Criar o banco PostgreSQL `sport_class`
- [ ] Criar `config/database.js`
- [ ] Configurar credenciais em `.env`
- [ ] Garantir que `.env` não seja enviado ao repositório
- [ ] Criar `models/index.js`
- [ ] Criar Model `Modalidade`
- [ ] Criar Model `Time`
- [ ] Criar Model `Partida`
- [ ] Criar Model `Usuario`
- [ ] Definir chaves primárias, estrangeiras e restrições
- [ ] Configurar relacionamentos Sequelize
- [ ] Testar a conexão e a persistência dos dados

## 3. API de modalidades

- [ ] Criar `modalidadeController.js`
- [ ] Criar `modalidadeRoutes.js`
- [ ] Implementar `GET /api/modalidades`
- [ ] Implementar `GET /api/modalidades/:id`
- [ ] Implementar `POST /api/modalidades`
- [ ] Validar a lista permitida: Futsal, Vôlei, Basquete, Handebol e Queimada
- [ ] Impedir duplicidades
- [ ] Testar sucesso, entrada inválida, duplicidade e ID inexistente

## 4. API de turmas

- [ ] Criar `timeController.js`
- [ ] Criar `timeRoutes.js`
- [ ] Implementar `GET /api/turmas`
- [ ] Implementar `GET /api/turmas/:id`
- [ ] Implementar `POST /api/turmas`
- [ ] Validar campos obrigatórios
- [ ] Impedir nomes duplicados
- [ ] Testar sucesso, validações e duplicidade

## 5. API de partidas

- [ ] Criar `partidaController.js`
- [ ] Criar `partidaRoutes.js`
- [ ] Implementar `GET /api/partidas`
- [ ] Implementar `GET /api/partidas/:id`
- [ ] Implementar `POST /api/partidas`
- [ ] Implementar `PUT /api/partidas/:id`
- [ ] Implementar `PATCH /api/partidas/:id/cancelar`
- [ ] Definir situação inicial como `AGENDADA`
- [ ] Validar modalidade e turmas existentes
- [ ] Impedir que a mesma turma ocupe os dois lados da partida
- [ ] Impedir conflitos de horário e modalidade
- [ ] Validar os campos obrigatórios
- [ ] Implementar filtros por modalidade, turma e período
- [ ] Testar todas as rotas e os códigos HTTP

## 6. Resultados e classificação

- [ ] Criar `resultadoController.js` e suas rotas
- [ ] Implementar `PATCH /api/partidas/:id/resultado`
- [ ] Validar os placares recebidos
- [ ] Aplicar a regra de data definida nos requisitos
- [ ] Atualizar a situação para `REALIZADA` após resultado válido
- [ ] Implementar `PATCH /api/partidas/:id/wo`
- [ ] Identificar explicitamente a turma beneficiada pelo W.O.
- [ ] Aplicar placar 3 × 0 no W.O.
- [ ] Criar `classificacaoController.js` e suas rotas
- [ ] Implementar `GET /api/modalidades/:id/classificacao`
- [ ] Calcular 3 pontos para vitória, 1 para empate e 0 para derrota
- [ ] Calcular saldo
- [ ] Implementar desempate por pontos, saldo e confronto direto
- [ ] Excluir partidas canceladas da pontuação
- [ ] Recalcular a classificação após alteração válida de resultado
- [ ] Testar vitórias, empates, derrotas, W.O., cancelamentos e desempates

## 7. Autenticação e permissões

- [ ] Definir autenticação no backend
- [ ] Implementar middleware de autenticação
- [ ] Implementar verificação do papel Organizador
- [ ] Restringir criação e alteração de modalidades, turmas e partidas
- [ ] Restringir registro e alteração de resultados
- [ ] Permitir consultas públicas previstas nos requisitos
- [ ] Garantir que Representantes não executem operações de gerenciamento
- [ ] Testar requisições sem autenticação e sem permissão

## 8. Fechamento dos testes do backend

- [ ] Testar `GET /api/health`
- [ ] Testar todas as rotas de modalidades
- [ ] Testar todas as rotas de turmas
- [ ] Testar todas as rotas de partidas
- [ ] Testar resultados e W.O.
- [ ] Testar classificação e critérios de desempate
- [ ] Testar filtros combinados
- [ ] Testar permissões
- [ ] Conferir códigos HTTP e formato das mensagens de erro
- [ ] Corrigir os problemas encontrados
- [ ] Confirmar que a API pode ser utilizada sem o frontend

## 9. Frontend — iniciar somente após os testes essenciais da API

- [ ] Criar a pasta `frontend`
- [ ] Inicializar React com Vite
- [ ] Criar `services/api.js` para centralizar chamadas HTTP
- [ ] Criar componentes compartilhados de mensagens de erro e sucesso
- [ ] Criar página de consulta de partidas
- [ ] Criar filtros por modalidade, turma e período
- [ ] Criar página de cadastro e edição de partidas
- [ ] Criar página de registro de resultados
- [ ] Criar página de classificação por modalidade
- [ ] Criar página de login
- [ ] Criar páginas de gerenciamento de modalidades e turmas
- [ ] Implementar estados de carregamento, lista vazia e erro
- [ ] Aplicar a paleta de cores confirmada no Figma
- [ ] Tornar as páginas responsivas
- [ ] Não criar dashboard sem requisito que justifique sua existência

## 10. Integração e entrega

- [ ] Integrar cada página à rota correspondente da API
- [ ] Testar os fluxos principais pela interface
- [ ] Testar permissões também no backend
- [ ] Verificar se a classificação corresponde aos resultados armazenados
- [ ] Corrigir erros encontrados
- [ ] Revisar consistência entre `requirements.md`, `design.md` e `tasks.md`
- [ ] Atualizar o README para refletir a arquitetura Node.js, Express, Sequelize e PostgreSQL
- [ ] Conferir que as cores do `design.md` correspondem aos valores reais do Figma
- [ ] Preparar o sistema para apresentação