# Warcraft-III-Bot-Score

GHost One Bot Score Html
![Screenshot](https://github.com/Slyhark/Warcraft-III-Bot-Score/blob/main/en.png)

TRIGGER MARIADB:
MY CODE EXAMPLE:
WHERE: 
VICTORY: 50 POINTS  
LOSE: 10 POINTS

//*******************************************************************  
//*******************************************************************  



DELIMITER //

CREATE TRIGGER `botscore_update` AFTER UPDATE ON `games`
FOR EACH ROW
BEGIN
    -- Eliminar todas las filas de dotaplayers
    DELETE FROM dotaplayers;

    -- Eliminar todas las filas de dotagames
    DELETE FROM dotagames;

    -- Insertar registros en dotaplayers basados en los datos de w3mmdplayers
    INSERT INTO dotaplayers (id, gameid, colour, item1, kills, deaths, towerkills, raxkills, courierkills)
    SELECT 
        ROW_NUMBER() OVER(ORDER BY gameid) AS id,
        wp.gameid, 
        wp.pid,
        CASE WHEN wp.flag = 'winner' THEN 'winner' ELSE 'loser' END,
        CASE WHEN wp.flag = 'winner' THEN 1 ELSE 0 END,
        CASE WHEN wp.flag = 'loser' THEN 1 ELSE 0 END,
        CASE WHEN wp.flag = 'winner' THEN 50 ELSE 0 END,
        CASE WHEN wp.flag = 'loser' THEN 10 ELSE 0 END,
        CASE WHEN wp.flag = 'winner' THEN 50 ELSE CASE WHEN wp.flag = 'loser' THEN 10 ELSE 0 END END
    FROM w3mmdplayers wp
    WHERE wp.category = 'sslgame';

    -- Insertar registros en dotagames basados en los datos de w3mmdplayers
    INSERT INTO dotagames (id, gameid, winner, min, sec)
    SELECT 
        ROW_NUMBER() OVER(ORDER BY wp.gameid) AS id,
        wp.gameid, 
        1 AS winner,
        g.duration DIV 60 AS min,
        g.duration % 60 AS sec
    FROM w3mmdplayers wp
    JOIN games g ON wp.gameid = g.id
    WHERE wp.category = 'sslgame' AND wp.flag = 'winner'
    GROUP BY wp.gameid;  -- Agrupar por gameid para evitar duplicados

END
//

CREATE TRIGGER `botscore_insert` AFTER INSERT ON `games`
FOR EACH ROW
BEGIN
    -- Eliminar todas las filas de dotaplayers
    DELETE FROM dotaplayers;

    -- Eliminar todas las filas de dotagames
    DELETE FROM dotagames;

    -- Insertar registros en dotaplayers basados en los datos de w3mmdplayers
    INSERT INTO dotaplayers (id, gameid, colour, item1, kills, deaths, towerkills, raxkills, courierkills)
    SELECT 
        ROW_NUMBER() OVER(ORDER BY gameid) AS id,
        wp.gameid, 
        wp.pid,
        CASE WHEN wp.flag = 'winner' THEN 'winner' ELSE 'loser' END,
        CASE WHEN wp.flag = 'winner' THEN 1 ELSE 0 END,
        CASE WHEN wp.flag = 'loser' THEN 1 ELSE 0 END,
        CASE WHEN wp.flag = 'winner' THEN 50 ELSE 0 END,
        CASE WHEN wp.flag = 'loser' THEN 10 ELSE 0 END,
        CASE WHEN wp.flag = 'winner' THEN 50 ELSE CASE WHEN wp.flag = 'loser' THEN 10 ELSE 0 END END
    FROM w3mmdplayers wp
    WHERE wp.category = 'sslgame';

    -- Insertar registros en dotagames basados en los datos de w3mmdplayers
    INSERT INTO dotagames (id, gameid, winner, min, sec)
    SELECT 
        ROW_NUMBER() OVER(ORDER BY wp.gameid) AS id,
        wp.gameid, 
        1 AS winner,
        g.duration DIV 60 AS min,
        g.duration % 60 AS sec
    FROM w3mmdplayers wp
    JOIN games g ON wp.gameid = g.id
    WHERE wp.category = 'sslgame' AND wp.flag = 'winner'
    GROUP BY wp.gameid;  -- Agrupar por gameid para evitar duplicados

END
//

CREATE TRIGGER `botscore_delete` AFTER DELETE ON `games`
FOR EACH ROW
BEGIN
    -- Eliminar todas las filas de dotaplayers
    DELETE FROM dotaplayers;

    -- Eliminar todas las filas de dotagames
    DELETE FROM dotagames;

    -- Insertar registros en dotaplayers basados en los datos de w3mmdplayers
    INSERT INTO dotaplayers (id, gameid, colour, item1, kills, deaths, towerkills, raxkills, courierkills)
    SELECT 
        ROW_NUMBER() OVER(ORDER BY gameid) AS id,
        wp.gameid, 
        wp.pid,
        CASE WHEN wp.flag = 'winner' THEN 'winner' ELSE 'loser' END,
        CASE WHEN wp.flag = 'winner' THEN 1 ELSE 0 END,
        CASE WHEN wp.flag = 'loser' THEN 1 ELSE 0 END,
        CASE WHEN wp.flag = 'winner' THEN 50 ELSE 0 END,
        CASE WHEN wp.flag = 'loser' THEN 10 ELSE 0 END,
        CASE WHEN wp.flag = 'winner' THEN 50 ELSE CASE WHEN wp.flag = 'loser' THEN 10 ELSE 0 END END
    FROM w3mmdplayers wp
    WHERE wp.category = 'sslgame';

    -- Insertar registros en dotagames basados en los datos de w3mmdplayers
    INSERT INTO dotagames (id, gameid, winner, min, sec)
    SELECT 
        ROW_NUMBER() OVER(ORDER BY wp.gameid) AS id,
        wp.gameid, 
        1 AS winner,
        g.duration DIV 60 AS min,
        g.duration % 60 AS sec
    FROM w3mmdplayers wp
    JOIN games g ON wp.gameid = g.id
    WHERE wp.category = 'sslgame' AND wp.flag = 'winner'
    GROUP BY wp.gameid;  -- Agrupar por gameid para evitar duplicados

END
//

DELIMITER ;
