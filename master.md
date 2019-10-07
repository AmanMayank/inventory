- Add design of table in schema migration
```
	{
	Version:     24,
	Description: "Add Closing Stocks",
	Script: `
CREATE TABLE customers (
id   BIGINT(20) UNSIGNED NOT NULL AUTO_INCREMENT,
company_id	INT(10) UNSIGNED NOT NULL,
name VARCHAR(100) NOT NULL,
email VARCHAR(100) NOT NULL UNIQUE,
address VARCHAR(255) NOT NULL,
hp CHAR(15) NOT NULL,
PRIMARY KEY (id),
KEY customers_company_id (company_id),
CONSTRAINT fk_customers_to_companies FOREIGN KEY (company_id) REFERENCES companies(id)
);`,
	},
```
	rows, err := tx.QueryContext(
		ctx, 
		qCustomers+" WHERE company_id=?", 
		ctx.Value(api.Ctx("auth")).(User).Company.ID,
	)
	stmt, err := tx.PrepareContext(ctx, `
		INSERT INTO customers (company_id, name, email, address, hp) 
		VALUES (?, ?, ?, ?, ?)
	`)