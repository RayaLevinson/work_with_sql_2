1.
	SELECT c.name FROM jbh_shop.customers AS c
	LEFT JOIN jbh_shop.orders AS o
	ON c.id = o.customer_id
	LEFT JOIN jbh_shop.orders_products AS op
	ON o.id = op.order_id
	LEFT JOIN jbh_shop.products AS p
	ON op.product_id = p.id
	WHERE p.name LIKE "Apple%"

2. 
	SELECT c.id, c.name FROM jbh_shop.customers AS c
  INNER JOIN
		(SELECT o.customer_id FROM jbh_shop.orders AS o
		INNER JOIN jbh_shop.orders_products AS op1
		ON o.id = op1.order_id
		INNER JOIN jbh_shop.orders_products AS op2
		ON o.id = op2.order_id        
		WHERE op1.product_id = 1 AND op2.product_id = 1
    GROUP BY o.customer_id
    ) AS t
	ON c.id = t.customer_id

3.
	SELECT c.id, c.name, Count(op.product_id) FROM jbh_shop.customers AS c
	LEFT JOIN jbh_shop.orders AS o
	ON c.id = o.customer_id
	LEFT JOIN jbh_shop.orders_products AS op
	ON o.id = op.order_id
	GROUP BY order_id

4.
	SELECT 	ROUND(AVG(totalPricePerOrder))
	FROM
	(
		SELECT  SUM(p.price) AS totalPricePerOrder FROM jbh_shop.orders_products AS op
		LEFT JOIN jbh_shop.products AS p
		ON op.product_id = p.id
		LEFT JOIN jbh_shop.orders AS o
		ON o.id =  op.order_id
		LEFT JOIN jbh_shop.customers AS c
		ON c.id = o.customer_id
		GROUP BY op.order_id
	) AS inner_query
