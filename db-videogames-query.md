## Todo

### SELECT

## 1- Selezionare tutte le software house americane (3)

SELECT \*
FROM
software_houses
WHERE
country = 'United States';

## 2- Selezionare tutti i giocatori della cittÃ di 'Rogahnland' (2)

SELECT \*
FROM
players
WHERE
city = 'Rogahnland';

## 3- Selezionare tutti i giocatori il cui nome finisce per "a" (220)

SELECT \*
FROM
players
WHERE
name LIKE '%a';

## 4- Selezionare tutte le recensioni scritte dal giocatore con ID = 800 (11)

SELECT \*
FROM
reviews
WHERE
player_id = 800

## 5- Contare quanti tornei ci sono stati nell'anno 2015 (9)

SELECT
COUNT(\*) AS numero_di_tornei
FROM
tournaments
WHERE
year = 2015

## 6- Selezionare tutti i premi che contengono nella descrizione la parola 'facere' (2)

SELECT \*
FROM
`awards`
WHERE
description LIKE '%facere%';

## 7- Selezionare tutti i videogame che hanno la categoria 2 (FPS) o 6 (RPG), mostrandoli una sola volta (del videogioco vogliamo solo l'ID) (287)

SELECT DISTINCT
v.ID
FROM
videogames v
JOIN
category_videogame cv ON v.ID = cv.videogame_id
JOIN
categories c ON cv.category_id = c.ID
WHERE
c.id IN (2, 6)

## 8- Selezionare tutte le recensioni con voto compreso tra 2 e 4 (2947)

SELECT \*
FROM
reviews
WHERE
rating BETWEEN 2 AND 4;

## 9- Selezionare tutti i dati dei videogiochi rilasciati nell'anno 2020 (46)

SELECT \*
FROM
videogames
WHERE
YEAR(release_date) = 2020;

## 10- Selezionare gli id videogame che hanno ricevuto almeno una recensione da 5 stelle, mostrandoli una sola volta (443)

SELECT DISTINCT
videogame_id
FROM
reviews
WHERE
rating = 5;

## 11- Selezionare il numero e la media delle recensioni per il videogioco con ID = 412 (review number = 12, avg_rating = 3.16 circa)

SELECT
COUNT(\*) AS numero_recensioni,
AVG(rating) AS media_voti
FROM
reviews
WHERE
videogame_id = 412;

## 12- Selezionare il numero di videogame che la software house con ID = 1 ha rilasciato nel 2018 (13)

SELECT
COUNT(\*) AS numero_videogiochi
FROM
videogames v
JOIN software_houses sh ON
v.software_house_id = sh.ID
WHERE
sh.ID = 1 AND YEAR(v.release_date) = 2018;

### GROUP BY

## 1- Contare quante software house ci sono per ogni paese (3)

SELECT
country,
COUNT(\*) AS numero_software_house
FROM
software_houses
GROUP BY
country;

## 2- Contare quante recensioni ha ricevuto ogni videogioco (del videogioco vogliamo solo l'ID) (500)

SELECT
videogame_id,
COUNT(\*) AS numero_recensioni
FROM
reviews
GROUP BY
videogame_id;

## 3- Contare quanti videogiochi hanno ciascuna classificazione PEGI (della classificazione PEGI vogliamo solo l'ID) (13)

SELECT
pegi_label_id AS pegi_id,
COUNT(\*) AS numero_videogiochi
FROM
pegi_labels
JOIN
pegi_label_videogame ON pegi_labels.id = pegi_label_videogame.pegi_label_id
GROUP BY
pegi_label_id;

## 4- Mostrare il numero di videogiochi rilasciati ogni anno (11)

SELECT
YEAR(release_date) AS anno_di_rilascio,
COUNT(\*) AS numero_videogiochi
FROM
videogames
GROUP BY
YEAR(release_date);

## 5- Contare quanti videogiochi sono disponbiili per ciascun device (del device vogliamo solo l'ID) (7)

SELECT
dv.device_id,
COUNT(\*) AS numero_videogiochi
FROM
device_videogame dv
JOIN
videogames v ON dv.videogame_id = v.ID
GROUP BY
dv.device_id;

## 6- Ordinare i videogame in base alla media delle recensioni (del videogioco vogliamo solo l'ID) (500)

SELECT
v.ID AS videogame_id,
AVG(r.rating) AS media_recensioni
FROM
videogames v
JOIN
reviews r ON v.ID = r.videogame_id
GROUP BY
v.ID
ORDER BY
media_recensioni DESC;

### JOIN

## 1- Selezionare i dati di tutti giocatori che hanno scritto almeno una recensione, mostrandoli una sola volta (996)

SELECT DISTINCT
p.\*
FROM
players p
WHERE
p.ID IN(
SELECT DISTINCT
r.player_id
FROM
reviews r
);

## 2- Sezionare tutti i videogame dei tornei tenuti nel 2016, mostrandoli una sola volta (226)

SELECT DISTINCT
v.\*
FROM
tournaments t
JOIN tournament_videogame tv ON
t.ID = tv.tournament_id
JOIN videogames v ON
tv.videogame_id = v.ID
WHERE
t.year = 2016;

## 3- Mostrare le categorie di ogni videogioco (1718)

SELECT
v.ID AS videogame_id,
c.\*
FROM
videogames v
JOIN category_videogame cv ON
v.ID = cv.videogame_id
JOIN categories c ON
cv.category_id = c.ID;

## 4- Selezionare i dati di tutte le software house che hanno rilasciato almeno un gioco dopo il 2020, mostrandoli una sola volta (6)

SELECT DISTINCT
sh.\*
FROM
software_houses sh
JOIN videogames v ON
sh.ID = v.software_house_id
WHERE
YEAR(v.release_date) > 2020;

## 5- Selezionare i premi ricevuti da ogni software house per i videogiochi che ha prodotto (55)

SELECT
sh.ID AS software_house_id,
a.\*
FROM
software_houses sh
JOIN videogames v ON
sh.ID = v.software_house_id
JOIN award_videogame av ON
v.ID = av.videogame_id
JOIN awards a ON
av.award_id = a.id;

## 6- Selezionare categorie e classificazioni PEGI dei videogiochi che hanno ricevuto recensioni da 4 e 5 stelle, mostrandole una sola volta (3363)

## 7- Selezionare quali giochi erano presenti nei tornei nei quali hanno partecipato i giocatori il cui nome inizia per 'S' (474)

SELECT DISTINCT
v.id AS videogame_id,
v.name AS videogame_name
FROM
tournaments t
JOIN tournament_videogame tv ON
t.id = tv.tournament_id
JOIN videogames v ON
tv.videogame_id = v.id
JOIN player_tournament pt ON
t.id = pt.tournament_id
JOIN players p ON
pt.player_id = p.id
WHERE
p.name LIKE 'S%';

## 8- Selezionare le cittÃ in cui Ã¨ stato giocato il gioco dell'anno del 2018 (36)

SELECT DISTINCT
t.city
FROM
tournaments t
JOIN tournament_videogame tv ON
t.id = tv.tournament_id
JOIN award_videogame av ON
tv.videogame_id = av.videogame_id
JOIN awards a ON
av.award_id = a.id
WHERE
av.year = 2018;

## 9- Selezionare i giocatori che hanno giocato al gioco piÃ¹ atteso del 2018 in un torneo del 2019 (3306)

## 10- Selezionare i dati della prima software house che ha rilasciato un gioco, assieme ai dati del gioco stesso (software house id : 5)

SELECT
sh._,
v._
FROM
software_houses sh
JOIN videogames v ON
sh.id = v.software_house_id
ORDER BY
v.created_at ASC
LIMIT 1;

## 11- Selezionare i dati del videogame (id, name, release_date, totale recensioni) con piÃ¹ recensioni (videogame id : potrebbe uscire 449 o 398, sono entrambi a 20)

SELECT
v.id AS videogame_id,
v.name AS videogame_name,
v.release_date,
COUNT(r.id) AS total_reviews
FROM
videogames v
JOIN reviews r ON
v.id = r.videogame_id
GROUP BY
v.id, v.name, v.release_date
ORDER BY
total_reviews DESC
LIMIT 2;

## 12- Selezionare la software house che ha vinto piÃ¹ premi tra il 2015 e il 2016 (software house id : potrebbe uscire 3 o 1, sono entrambi a 3)

SELECT
sh.id AS software_house_id,
sh.name AS software_house_name,
total_awards
FROM
software_houses sh
JOIN (
SELECT
sh.id,
sh.name,
COUNT(aw.id) AS total_awards
FROM
software_houses sh
JOIN videogames v ON
sh.id = v.software_house_id
JOIN award_videogame av ON
v.id = av.videogame_id
JOIN awards aw ON
av.award_id = aw.id
WHERE
CAST(av.year AS UNSIGNED) BETWEEN 2015 AND 2016
GROUP BY
sh.id, sh.name
) AS subquery ON
sh.id = subquery.id AND subquery.total_awards = (
SELECT MAX(total_awards) FROM (
SELECT
sh.id,
COUNT(aw.id) AS total_awards
FROM
software_houses sh
JOIN videogames v ON
sh.id = v.software_house_id
JOIN award_videogame av ON
v.id = av.videogame_id
JOIN awards aw ON
av.award_id = aw.id
WHERE
CAST(av.year AS UNSIGNED) BETWEEN 2015 AND 2016
GROUP BY
sh.id
) AS max_subquery
);

## 13- Selezionare le categorie dei videogame i quali hanno una media recensioni inferiore a 2 (10)

SELECT
c.id AS category_id,
c.name AS category_name
FROM
categories c
WHERE EXISTS (
SELECT 1
FROM
category_videogame cv
JOIN
videogames v ON cv.videogame_id = v.id
LEFT JOIN
reviews r ON v.id = r.videogame_id
WHERE
cv.category_id = c.id
GROUP BY
v.id
HAVING
AVG(r.rating) < 2 OR AVG(r.rating) IS NULL
);
S
