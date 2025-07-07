# **Taller de Funciones Definidas por el Usuario (UDFs) en MySQL**

# 

## Caso 1: Cálculo de Bonificaciones de Empleados

### 

```sql
DELIMITER//
CREATE FUNCTION calcular_bonificacion(salario DECIMAL(10,2)) RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN

	DECLARE bonificacion DECIMAL(10,2);
	DECLARE mensaje VARCHAR(100);
	IF salario < 2000 THEN
	 SET bonificacion = salario *0.10;
	 RETURN bonificacion;
	 
	ELSEIF salario >= 2000 AND salario <=5000 THEN
	 SET bonificacion = salario *0.07;
	   RETURN bonificacion;
	 
	ELSEIF salario > 5000 THEN
	 SET bonificacion = salario * 0.05;
	   RETURN bonificacion;
	 
	ELSE 
	 SET mensaje = 'NO OBTIENE BONIFICACION';
	END IF;
	
	
END//
```



## Caso 2: Cálculo de Edad de Clientes

### 

```sql
DELIMITER//
CREATE FUNCTION calcular_edad(fecha_nacimiento DATE) RETURNS INT
DETERMINISTIC
BEGIN

	DECLARE edad INT;
	SET edad = TIMESTAMPDIFF(YEAR, fecha_nacimiento, CURDATE());
	RETURN edad;
	
	END//
	DELIMITER;
	
```

​	

```sql
SELECT nombre,    fecha_nacimiento,  calcular_edad (fecha_nacimiento) AS
 edad FROM clientes;
```



## Caso 3: Formatear Números de Teléfono



```sql
DELIMITER //

CREATE FUNCTION formatear_telefono (telefono VARCHAR(20)) RETURNS VARCHAR(20)
DETERMINISTIC
BEGIN 
	DECLARE new_format VARCHAR(20);
	SET new_format = CONCAT(
        '(',
        SUBSTRING(telefono, 3, 3), ') ',
        SUBSTRING(telefono, 6, 3), '-',
        SUBSTRING(telefono, 9, 4)
    );
    RETURN new_format;
END//

DELIMITER ;

```



```sql
SELECT nombre, telefono, formatear_telefono(telefono) AS nuevo_formato FROM clientes;

```



## Caso 4: Clasificación de Productos por Precio



```sql
DELIMITER //

CREATE FUNCTION clasificar_precio(precio DECIMAL(10,2))
RETURNS VARCHAR(10)
DETERMINISTIC
BEGIN
    DECLARE categoria VARCHAR(10);

    IF precio < 50 THEN
        SET categoria = 'Bajo';
    ELSEIF precio BETWEEN 50 AND 200 THEN
        SET categoria = 'Medio';
    ELSE
        SET categoria = 'Alto';
    END IF;

    RETURN categoria;
END//

DELIMITER ;

```

```sql
SELECT nombre,precio, clasificar_precio(precio) FROM Productos AS categoria;
```







