// Restrições de id único para usuários da plataforma, tanto para ouvintes como também para artistas
CREATE CONSTRAINT listener_id_unique FOR (l:Listener) REQUIRE l.id IS UNIQUE;

CREATE CONSTRAINT creator_id_unique FOR (c:Creator) REQUIRE c.id IS UNIQUE;

// Criação dos Ouvintes (Listeners)
MERGE (josa:Listener {id: "U002-1", name: "Joseph", listen_time: 51})
MERGE (jonas:Listener {id: "U001-1", name: "Jonas", listen_time: 129})
MERGE (jean:Listener {id: "U003-1", name: "Jean", listen_time: 139})

// Criação dos Artistas (Creators)
MERGE (prince:Creator {id: "A001", name: "Prince", monthly_listeners: 21138214})
MERGE (bruno:Creator {id: "A002", name: "Bruno Mars", monthly_listeners: 132784710})
MERGE (weekend:Creator {id: "A003", name: "The Weeknd", monthly_listeners: 114842831})

// Criação das Músicas e  dos Gêneros
MERGE (dance:Song {title: "Dance With Me", album: "The Romantic"})
MERGE (timeless:Song {title: "Timeless", album: "Hurry Up Tomorrow"})
MERGE (delirious:Song {title: "Delirious", album: "1999"})

MERGE (rap:Genre {name: "Rap"})
MERGE (pop:Genre {name: "Pop"})
MERGE (rock:Genre {name: "Rock"})

// Criação das Conexões
MERGE (josa)-[:FOLLOW {since: 2015}]->(prince)
MERGE (josa)-[:FOLLOW {since: 2019}]->(weekend)
MERGE (josa)-[:LIKED {replays: 37, }]->(timeless)

MERGE (jean)-[:FOLLOW {since: 2017}]->(prince)
MERGE (jean)-[:LIKED {replays: 8}]->(dance)

MERGE (jonas)-[:FOLLOW {since: 2019}]->(bruno)
MERGE (jonas)-[:FOLLOW {since: 2021}]->(weekend)
MERGE (jonas)-[:LIKED {replays: 21}]->(timeless)

// Conexões entre Artista e Música, Música e Gênero
MERGE (bruno)-[:SANG]->(dance)
MERGE (weekend)-[:SANG]->(timeless)
MERGE (prince)-[:SANG]->(delirious)

MERGE (dance)-[:CATALOGED_AS]->(pop)
MERGE (timeless)-[:CATALOGED_AS]->(rap)
MERGE (delirious)-[:CATALOGED_AS]->(rock)
