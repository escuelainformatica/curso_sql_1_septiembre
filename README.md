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

22