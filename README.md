# Music_Recommendations_with_Neo4j
Código Cypher simples de recomendação de músicas com base nos artistas seguidos pelo usuário.
A simple Cypher code for song recommendations based on the users' followed artists.

Este repositório contém a solução de um desafio de código do bootcamp da DIO focado em modelagem de dados em grafos utilizando Neo4j. O projeto consiste na construção de um modelo de dados em grafo para oferecer recomendações de músicas para usuários de uma plataforma, explorando como entidades e relacionamentos podem ser estruturados e consultados dentro do ecossistema do Neo4j.

# REQUISITOS DO DESAFIO
Você foi contratado para desenvolver um sistema de recomendação de músicas que utiliza grafos para identificar padrões de escuta e sugerir novas faixas aos usuários. O sistema deve ser capaz de:

- Representar usuários, artistias, músicas e gêneros em um grafo
- Representar interações (escutar, curtir, seguir) como propriedades

# RASCUNHO DO MODELO
Modelagem do grafo com a ferramenta [Arrows App](https://arrows.app/)

![image alt](https://github.com/PedroLPfelipe/Music_Recommendations_with_Neo4j/blob/bcfe583b33fb66810449e3145aa8bdd34d791b67/images/modelagem_arrowsapp.png)

# CATEGORIAS DE NÓS
**User**: usuários da plataforma, separados entre ouvintes e criadores.

**Music**: criação de algum artista dentro da plataforma, podendo ser distinguido entre song (uma única música) ou album (coletânea de músicas).

**Genre**: o tipo musical que abrange determinadas criações registradas na plataforma.

![image alt](https://github.com/PedroLPfelipe/Music_Recommendations_with_Neo4j/blob/bcfe583b33fb66810449e3145aa8bdd34d791b67/images/general_labels_visualization.png)

Ao usar a query **CALL db.schema.visualization()** os principais tipos de nós presentes no grafo são apresentados

___________________________________________________________________________________________________________________________________

![image alt](https://github.com/PedroLPfelipe/Music_Recommendations_with_Neo4j/blob/bcfe583b33fb66810449e3145aa8bdd34d791b67/images/three_users_visualization.png)

Ao usar a query **MATCH (l:Listener) RETURN l** é possível visulizar apenas os nós dos usuários ouvintes dentro da plataforma.

# TIPOS DE RELACIONAMENTOS
**FOLLOW**: relaciona um *Listener* a um *Creator*. Denotando quando um usuário passou a seguir um determinado artista, possuindo a propriedade *since*, com o ano que a ação de seguir o artista foi feita.

**LIKED**: relaciona um *Listener* a um *Song*. Evidência qual música específica o usuário gostou dentro da plataforma, possuindo a propriedade *replays*, com a quantidade de vezes que o usuário escutou a música.

**SANG**: relaciona um *Creator* a um *Song*. Responde a pergunta "qual música esse artista cantou?".

**CATALOGED_AS**: relaciona um *Song* a um *Genre*. Mostra a conexão que existe entre uma música e um estilo musical.

![image alt](https://github.com/PedroLPfelipe/Music_Recommendations_with_Neo4j/blob/bcfe583b33fb66810449e3145aa8bdd34d791b67/images/Jean_relationship_visualization.png)

Relacionamentos do nó *Listener* com a propriedade *nome:Jean*, feita com a query **MATCH (l:Listener{name:'Jean'}) RETURN l**

É possível perceber que o/a usuário(a) 'Jean' segue um artista mas não possui uma música desse artista registrada como *Liked*.

# Neo4j aura e a recomendação

![image alt](https://github.com/PedroLPfelipe/Music_Recommendations_with_Neo4j/blob/d6bd2a38af93193c56d0c167fa34506c3ecd5b09/images/all_visualization_neo4j.jpeg)

A plataforma [Neo4j aura](https://neo4j.com) permite navegar facilmente entre conexões, como:

● encontrar todos os usuários ouvintes e/ou criadores (artistas) dentro da platafomra

● descobrir quais usuários curtiram a mesma música

● rankear o gênero musical preferido com base nas avaliações de 100 usuários

___________________________________________________________________________________________________________________________________

Ao fazer a sugestão de uma música, tendo como base um artista seguido por dois usuários distintos, onde um deles não possui a propriedade *Liked*, é gerado o seguinte resultado:

![image alt](https://github.com/PedroLPfelipe/Music_Recommendations_with_Neo4j/blob/d6bd2a38af93193c56d0c167fa34506c3ecd5b09/images/Song_recommendation.jpeg)

**Query utilizada**

MATCH (você:Listener {name: "Joseph"})-[:FOLLOW]->(artista:Creator)<-[:FOLLOW]-(outro:Listener)

MATCH (outro)-[:LIKED]->(musica_recomendada:Song)

WHERE NOT (você)-[:LIKED]->(musica_recomendada)

RETURN DISTINCT musica_recomendada.title AS Sugestão, artista.name AS Porque_voce_segue
