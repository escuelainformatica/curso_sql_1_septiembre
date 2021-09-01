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

