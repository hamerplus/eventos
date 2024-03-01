
 
 
 
 
 
 
 DELIMITER $$

CREATE FUNCTION diferencia_fechas(fecha_evento DATE, fecha_comparar DATE)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE diff INT;
    SET diff = ABS(DATEDIFF(fecha_evento, fecha_comparar));
    RETURN diff;
END$$

DELIMITER ;


 DELIMITER $$

CREATE FUNCTION fechas_cercanas(fecha_evento DATE)
RETURNS int 
DETERMINISTIC
BEGIN
    DECLARE min_diff INT;
    SET min_diff = (SELECT MIN(diferencia_fechas(fecha_evento, fecha)) FROM tabla_eventos);
    
    RETURN (
        SELECT fecha
        FROM tabla_eventos
        WHERE diferencia_fechas(fecha_evento, fecha) = min_diff
        ORDER BY fecha
    );
END$$
DELIMITER ;
