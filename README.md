1.	DELIMITER //: Esto establece un delimitador personalizado para el bloque de código. Usamos // en lugar del delimitador predeterminado ; para evitar conflictos en la sintaxis interna del procedimiento almacenado.
	2.	CREATE PROCEDURE fechas_cercanas(IN evento_fecha DATE): Esta línea crea un nuevo procedimiento almacenado llamado fechas_cercanas. Toma un parámetro de entrada evento_fecha que esperamos sea una fecha.
	3.	BEGIN: Esto marca el comienzo del cuerpo del procedimiento almacenado.
	4.	SELECT fecha FROM tabla_eventos: Esta es una consulta SQL que selecciona todas las fechas de la tabla tabla_eventos.
	5.	ORDER BY ABS(DATEDIFF(evento_fecha, fecha)): Ordena los resultados de la consulta según la diferencia absoluta en días entre la fecha del evento (evento_fecha) y cada fecha en la tabla tabla_eventos.
	6.	LIMIT 5: Esto limita los resultados a solo las primeras 5 filas, que serán las fechas más cercanas al evento.
	7.	END //: Marca el final del cuerpo del procedimiento almacenado.
	8.	DELIMITER ;: Restaura el delimitador predeterminado a ;, terminando la definición del procedimiento almacenado.
 
 
 
 
 
 
 
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
