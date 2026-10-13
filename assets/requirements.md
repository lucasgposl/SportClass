# Requirements — Sport Class

## 1. Propósito principal

O Sport Class é uma aplicação web para organizar o campeonato interclasses da escola. Seu foco principal é cadastrar partidas, registrar resultados e apresentar a classificação das turmas por modalidade, substituindo o controle manual feito em cartazes por informações centralizadas e consistentes.

## 2. Atores

- **Organizador:** gerencia modalidades, turmas e partidas; registra resultados e administra as informações do campeonato.
- **Representante de turma:** consulta partidas, resultados e classificação. Não pode executar operações exclusivas do Organizador.
- **Visitante:** pode consultar informações públicas do campeonato, sem acesso às operações de gerenciamento.

## 3. Escopo incluído

- Cadastro e consulta das modalidades permitidas.
- Cadastro e consulta das turmas participantes.
- Cadastro, consulta, alteração e cancelamento de partidas.
- Registro de placares e situações de partidas.
- Cálculo da pontuação e classificação por modalidade.
- Filtros de partidas por modalidade, turma e período.
- Autenticação e controle de permissões para operações restritas.
- Interface para consulta e gerenciamento das funcionalidades acima.

## 4. Fora do escopo

- Geração automática de chaveamento ou sorteio da tabela.
- Fase mata-mata, chaves e playoffs.
- Súmula por atleta.
- Registro de gols, cartões, artilharia ou estatísticas individuais.
- Cadastro individual de alunos como atletas.
- Feed de notícias ou funcionalidades sociais que não sejam necessárias para organizar o campeonato.

## 5. Requisitos funcionais

### RF01 — Consultar e cadastrar modalidades

**História:** Como Organizador, quero disponibilizar as modalidades usadas no campeonato para selecioná-las nas partidas.

**Critérios de aceitação**
1. O sistema deve disponibilizar somente as modalidades previstas: Futsal, Vôlei, Basquete, Handebol e Queimada.
2. O Organizador pode cadastrar ou disponibilizar modalidades da lista permitida.
3. O sistema deve impedir modalidade fora da lista e duplicidades.
4. As modalidades devem poder ser selecionadas no cadastro de partidas.
5. A API deve retornar mensagens de erro claras quando os dados forem inválidos.

### RF02 — Cadastrar e consultar turmas

**História:** Como Organizador, quero cadastrar as turmas participantes para utilizá-las nas partidas.

**Critérios de aceitação**
1. O Organizador pode cadastrar uma turma com nome ou identificação.
2. O sistema deve impedir nomes/identificações duplicados.
3. As turmas cadastradas devem estar disponíveis para seleção nas partidas.
4. Uma turma pode participar de diferentes modalidades.

### RF03 — Cadastrar e gerenciar partidas

**História:** Como Organizador, quero cadastrar e gerenciar partidas para manter o calendário do campeonato atualizado.

**Critérios de aceitação**
1. Uma partida deve conter modalidade, duas turmas diferentes, data, horário e local.
2. A situação inicial deve ser `Agendada`.
3. O sistema deve impedir que uma turma jogue contra ela mesma.
4. O sistema deve impedir duas partidas da mesma turma no mesmo horário e modalidade.
5. O sistema deve validar campos obrigatórios e referências existentes.
6. O Organizador pode consultar, alterar e cancelar partidas.
7. O sistema deve informar erros de validação sem salvar dados inválidos.

### RF04 — Consultar e filtrar partidas

**História:** Como Representante ou Visitante, quero consultar partidas e aplicar filtros para encontrar os jogos de interesse.

**Critérios de aceitação**
1. A consulta deve exibir turmas, modalidade, data, horário, local e situação.
2. Deve ser possível filtrar por modalidade, turma e período.
3. Os filtros devem poder ser combinados.
4. Se não houver partidas correspondentes, o sistema deve informar que nenhum resultado foi encontrado.
5. A consulta não deve permitir alteração de dados por usuários sem permissão.

### RF05 — Registrar resultados

**História:** Como Organizador, quero registrar o resultado de uma partida para atualizar a classificação.

**Critérios de aceitação**
1. Somente o Organizador pode registrar ou alterar resultados.
2. O sistema deve aceitar os placares dos dois times.
3. O resultado só pode ser registrado depois da data da partida.
4. Ao salvar um resultado válido, a situação deve ser atualizada para `Realizada`.
5. O Organizador pode definir a partida como `Cancelada` ou `W.O.`.
6. O sistema deve guardar os dados do resultado e atualizar a classificação correspondente.
7. O sistema deve informar erros e não concluir operações inválidas.

### RF06 — Calcular pontuação e classificação

**História:** Como usuário, quero consultar a classificação por modalidade para acompanhar o desempenho das turmas.

**Critérios de aceitação**
1. A classificação deve considerar somente partidas com resultado válido registrado.
2. Vitória vale 3 pontos; empate vale 1 ponto para cada turma; derrota vale 0 pontos.
3. Em caso de W.O., o resultado deve seguir a regra do campeonato, com placar 3 × 0 para a turma beneficiada.
4. A classificação deve ser separada por modalidade.
5. A ordem deve considerar pontos, saldo e confronto direto, nessa sequência.
6. Após o registro ou alteração válida de um resultado, a classificação deve refletir os dados atuais.
7. Partidas canceladas não devem contribuir para a pontuação.

### RF07 — Autenticação e permissões

**História:** Como Organizador ou Representante, quero acessar apenas as funcionalidades autorizadas ao meu perfil.

**Critérios de aceitação**
1. O Organizador deve autenticar-se para executar operações de gerenciamento.
2. O Representante pode consultar partidas, resultados e classificação.
3. O Representante não pode cadastrar, alterar ou cancelar partidas nem registrar resultados.
4. A API deve validar permissões no backend; esconder botões no frontend não é suficiente.
5. Requisições sem autenticação ou sem permissão devem receber resposta HTTP apropriada.

## 6. Requisitos não funcionais

### RNF01 — Arquitetura
O sistema deve separar frontend, API backend e banco de dados.

### RNF02 — Persistência
Os dados oficiais de modalidades, turmas, partidas, usuários e resultados devem ser persistidos no PostgreSQL.

### RNF03 — Tecnologias
O backend deve utilizar Node.js e Express, com Sequelize como ORM. O frontend deve utilizar React e consumir a API HTTP.

### RNF04 — Validação
A API deve validar entradas e aplicar as regras de negócio antes de gravar alterações.

### RNF05 — Segurança
Credenciais e configurações sensíveis devem ser mantidas em variáveis de ambiente. As permissões devem ser verificadas no backend.

### RNF06 — Usabilidade e responsividade
As páginas devem ser compreensíveis e funcionar em telas desktop e dispositivos móveis.

### RNF07 — Testabilidade
As rotas do backend devem poder ser testadas independentemente do frontend.

### RNF08 — Consistência
O cálculo da classificação deve utilizar os resultados persistidos e não depender de valores calculados apenas no navegador.

## 7. Regras de negócio

- Modalidades permitidas: Futsal, Vôlei, Basquete, Handebol e Queimada.
- Uma partida envolve duas turmas diferentes e uma modalidade.
- A situação inicial de uma partida é `Agendada`.
- Uma turma não pode ter duas partidas no mesmo horário e modalidade.
- O resultado só pode ser registrado depois da data da partida.
- Pontuação: vitória = 3, empate = 1 para cada turma, derrota = 0.
- W.O. utiliza o placar 3 × 0 para a turma beneficiada.
- Critérios de classificação: pontos, saldo e confronto direto.
- Somente o Organizador executa operações de gerenciamento.

## 8. Critério geral de conclusão

Uma funcionalidade será considerada concluída quando atender aos critérios de aceitação, persistir corretamente os dados quando aplicável, respeitar as permissões e possuir testes adequados.