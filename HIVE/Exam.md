1.
SELECt
  A.prod_id
  , A.brand
  , A.name
  , count(1) as sale_cnt
from
  PRODUCTS A
  LEFT OUTER JOIN  ORDER_DETAILS B
  ON (A.prod_id = b.prod_id)
GROUP BY A.PROD_ID, A.BRAND, A.NAME
ORDER BY sale_cnt desc
limit 3;

=>
1274348	Byteweasel	Tablet PC (10 in. display, 64 GB)	119801
1274021	Dualcore	8GB DDR3-1600 (PC3-12800) Dual Channel Desktop Memory Kit (2x4GB)	29053
1273970	Megachango	F Jack Male-to-Male Cable (12 in.)	14566

2.
SELECt
  A.prod_id
  , A.brand
  , A.name
  , sum(A.price) as total_dollar
from
  PRODUCTS A
  LEFT OUTER JOIN  ORDER_DETAILS B
  ON (A.prod_id = b.prod_id)
GROUP BY A.PROD_ID, A.BRAND, A.NAME
ORDER BY total_dollar DESC
limit 10;

=>
1274348	Byteweasel	Tablet PC (10 in. display, 64 GB)	4552318199
1274315	Sparky	Server (1U rackmount, hex-core, 16GB, 8TB)	768481065
1274330	Olde-Gray	Premium Gamer Desktop	617149944
1274503	Foocorp	VPN Appliance (250 Clienti license)	608639166
1274322	Olde-Gray	Home Media Server	363194766
1274499	Duff	VPN Appliance (50 Clienti license)	337645721
1274345	Orion	Ultra-portable Notebook	306690578
1274613	Megachango	173 GB SAS Disk	293179471
1274336	Bitmonkey	Basic Desktop	264143802
1274351	Orion	Tablet PC (10 in. display, 32 GB)	247508898

3.
SELECt
A.brand
  , to_date(C.order_date) as DAT
  , sum(A.price) as revenue
  , sum(A.price) - sum(A.cost) as profit
from
  PRODUCTS A
  JOIN  ORDER_DETAILS B
  ON (A.prod_id = b.prod_id)
  JOIN  ORDERS C
  ON (B.ORDER_ID = C.ORDER_ID)
WHERE
  A.brand like 'Dualcore%'
GROUP BY A.BRAND,DAT
ORDER BY dat
;

=>
결과가 많아서 결과는 생략합니다.

4.

SELECT
    rank() over (partition by year(AAA.DAT), month(AAA.DAT) order by AAA.profit desc)
    , AAA.DAT
FROm (
SELECt
  to_date(C.order_date) as DAT
  , sum(A.price) as revenue
  , sum(A.price) - sum(A.cost) as profit
from
  PRODUCTS A
  JOIN  ORDER_DETAILS B
  ON (A.prod_id = b.prod_id)
  JOIN  ORDERS C
  ON (B.ORDER_ID = C.ORDER_ID)
WHERE
  A.brand like 'Dualcore%'
GROUP BY DAT
) AS AAA;
