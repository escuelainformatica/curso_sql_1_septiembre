# curso_sql_1_septiembre

```sql

CREATE FUNCTION TotalVentasPorVendedor
(
	@idvendedor int
)
RETURNS int
AS
BEGIN
	-- TRANSACT-SQL
	declare @total int	 -- crear una variable
	-- hice una consultas
	select @total= sum(id.quantity*id.unitPrice) from Invoices i
		inner join InvoiceDetails id on i.idInvoice=id.idInvoice
	where idSeller=@idvendedor

	-- devuelvo los valores
	return @total
	

END
GO


```

## Como crear una función

```
CREATE FUNCTION <INDICAR EL NOMBRE>
(
<AGREGO LOS ARGUMENTOS>
)
RETURNS <INDICO EL TIPO DE DATOS A REGRESAR>
AS
BEGIN
<AQUI VA EL CODIGO>
END
GO
```

## Detalles

1) La función debe regresar un valor.  Lo regreso con la función return <valor>

## Definir una variable dentro de la función

```sql
declare @variablenueva <tipo de dato>
-- declare @total int
-- declare @texto varchar
```

Las variables solo viven dentro  de la función. 

## Asignar un valor de una variable

```sql
set @variable=<valor>
-- o tambien
select @variable=<valor>
-- o tambien (usando una consulta)
select @variable=20 from table where...
-- ejemplos:
-- set @total=2000
-- select @total=sum(id) from tabla

```

## Regresar un valor

```sql
return <valor>
-- ejemplo:
-- return @variable
```

## Bloque de código

Un bloque de código permite agrupar muchas operaciones de transact-sql.

```sql
BEGIN
<codigo>
END
```

## Ejemplo de función

```sql
CREATE FUNCTION FuncionCiudad
(
    -- se puede crear una funcion sin argumento
)
RETURNS varchar(50)
AS
BEGIN
	DECLARE @nombre varchar(50)
	-- esta consulta devuelve varios valores pero solo guarda el primer valor
	SELECT @nombre=name from cities -- asigna el primer valor o nulo
	RETURN @nombre
END
GO
```

## Fechas

Mostrar todas las ventas en enero 2019

```
SELECT InvoiceDetails.quantity, Invoices.creationDate
FROM     InvoiceDetails INNER JOIN
                  Invoices ON InvoiceDetails.idInvoice = Invoices.idInvoice
WHERE creationDate BETWEEN '2019-01-01 00:00:00' AND '2019-01-31 23:59:59'
```

Usando funciones pre-creadas

```
SELECT InvoiceDetails.quantity, Invoices.creationDate
FROM     InvoiceDetails INNER JOIN
                  Invoices ON InvoiceDetails.idInvoice = Invoices.idInvoice
WHERE month(creationDate)=1 and year(creationdate)=2019
```

Usando funciones pre-creada datepart

```
SELECT InvoiceDetails.quantity, Invoices.creationDate
FROM     InvoiceDetails INNER JOIN
                  Invoices ON InvoiceDetails.idInvoice = Invoices.idInvoice
WHERE DATEPART(MONTH,creationDate)=1 and DATEPART(year,creationdate)=2019
```

## Consolidad

```sql
CREATE FUNCTION CANTIDAD_POR_PRODUCTO_Y_MES(
	@IDPRODUCTO INT,
	@NUMMES INT,
	@NUMAÑO INT
)
RETURNS INT
AS
BEGIN
	DECLARE @TOTAL INT
	SELECT @total=sum(InvoiceDetails.quantity)
		FROM     InvoiceDetails INNER JOIN
						  Invoices ON InvoiceDetails.idInvoice = Invoices.idInvoice
		WHERE DATEPART(MONTH,creationDate)=@NUMMES 
		and DATEPART(year,creationdate)=@NUMAÑO
			and sku = @IDPRODUCTO
	return @total
END
```

Uso de la consulta

```sql
select name,
	dbo.cantidad_por_producto_y_mes(sku,1,2019) as enero,
	dbo.cantidad_por_producto_y_mes(sku,2,2019) as febrero,
	dbo.cantidad_por_producto_y_mes(sku,3,2019) as marzo,
	dbo.cantidad_por_producto_y_mes(sku,4,2019) as abril
	from Skus
```

Usando queries anidadas

```sql
select name,
	(SELECT sum(InvoiceDetails.quantity)
		FROM     InvoiceDetails INNER JOIN
						  Invoices ON InvoiceDetails.idInvoice = Invoices.idInvoice
		WHERE DATEPART(MONTH,creationDate)=1 
		and DATEPART(year,creationdate)=2019
			and sku = skus.sku) as enero,
	(SELECT sum(InvoiceDetails.quantity)
		FROM     InvoiceDetails INNER JOIN
						  Invoices ON InvoiceDetails.idInvoice = Invoices.idInvoice
		WHERE DATEPART(MONTH,creationDate)=2 
		and DATEPART(year,creationdate)=2019
			and sku = skus.sku) as febrero,
	(SELECT sum(InvoiceDetails.quantity)
		FROM     InvoiceDetails INNER JOIN
						  Invoices ON InvoiceDetails.idInvoice = Invoices.idInvoice
		WHERE DATEPART(MONTH,creationDate)=3
		and DATEPART(year,creationdate)=2019
			and sku = skus.sku) as marzo,
	(SELECT sum(InvoiceDetails.quantity)
		FROM     InvoiceDetails INNER JOIN
						  Invoices ON InvoiceDetails.idInvoice = Invoices.idInvoice
		WHERE DATEPART(MONTH,creationDate)=4 
		and DATEPART(year,creationdate)=2019
			and sku = skus.sku) as abril
	from Skus
```

## funciones que devuelven una tabla

```sql

CREATE FUNCTION Nombredeciudad_por_pais
(	
	@nombrepais varchar(50)
)
RETURNS TABLE -- <-- eso indica que devuelve una tabla (y no es necesario usar return)
AS
RETURN 
(
	-- esta consulta devuelve el ultimo select
	SELECT Cities.name
		FROM     Cities INNER JOIN
						  Countries ON Cities.idCountry = Countries.idCountry
		WHERE  (Countries.name = @nombrepais)
)
GO

```

usar la consulta

```sql
select * 
	from dbo.Nombredeciudad_por_pais('chile')
```

