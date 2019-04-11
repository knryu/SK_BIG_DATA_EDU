select * from customers where fname like 'Bridge%' and city = 'Kansas City';
=>
Bridget Burch


select * from products order by price desc limit 3;
=>
Byteweasel	Hadoop Cluster, Economy (4-node)
Gigabux	Server (2U rackmount, eight-core, 64GB, 12TB)
Krustybitz	Server (2U rackmount, eight-core, 64GB, 12TB)


SELECT o.order_id, fname, lname, o.order_date, e.name
FROM customers c
JOIN orders o
   ON (c.cust_id = o.cust_id)
JOIN order_details d
   ON (o.order_id = d.order_id)
JOIN products e
   ON (d.prod_id = e.prod_id)
WHERE d.prod_id=1274348
  AND c.cust_id=1139477
=>  5월에 주문했습니다.


select * from products where brand = 'Gigabux' and price < 1000;
