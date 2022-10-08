1. Display the VendorID and VendorName for all vendors.

```sql
SELECT vendorid, vendorname FROM vendor
```

2. Display the ProductID, ProductName, and ProductPrice for products with a ProductPrice of $100 or higher.

```sql
SELECT productid, productname, productprice FROM product
WHERE productprice >= 100
```

3. Display the ProductID, ProductName, ProductPrice, and VendorName for all products. Sort the results by ProductID.

```sql
SELECT product.productid, product.productname, product.productprice, vendor.vendorname FROM product
LEFT JOIN vendor
ON vendor.vendorid = product.vendorid
ORDER BY productid
```

4. Display the ProductID, ProductName, and ProductPrice for products in the category whose CategoryName value is Camping. Sort the results by ProductID.

```sql
SELECT productid, productname, productprice FROM product
LEFT JOIN category
ON product.categoryid = category.categoryid
WHERE category.categoryname = 'camping'
ORDER BY productid
```

5. Display the TID, CustomerName, and TDate for sales transactions involving a customer buying a product whose ProductName is Dura Boot.

```sql
SELECT salestransaction.TID, customer.customername, salestransaction.tdate FROM salestransaction
LEFT JOIN customer
ON customer.customerid = salestransaction.customerid
LEFT JOIN includes
ON includes.tid = salestransaction.tid
LEFT JOIN product
ON product.productid = includes.productid
Where product.productname = 'Dura Boot'
```

6. Display the RegionID, RegionName, and number of stores in the region for all regions.

```sql
SELECT region.regionid, region.regionname, COUNT(region.regionid) FROM region
LEFT JOIN store
ON region.regionid = store.regionid
GROUP BY region.regionname
```

7. For each product category, display the CategoryID, CategoryName, and average price of a product in the category

```sql
SELECT category.categoryid, category.categoryname, AVG(product.productprice) FROM category
LEFT JOIN product
ON product.categoryid = category.categoryid
GROUP BY category.categoryid
```

8. Display the ProductID and ProductName of the cheapest product.

```sql
-- Give One of the cheapest product
SELECT productid, productname FROM product
ORDER BY product.productprice ASC
LIMIT 1
```
or

```sql
-- Give all products that has cheapest price
SELECT productid, productname FROM product
WHERE productprice = (SELECT MIN(productprice) FROM product)
```

9. Display the ProductID, ProductName, and VendorName for products whose price is below the average price of all products.

```sql
SELECT productid, productname, vendorname FROM product
LEFT JOIN vendor
ON vendor.vendorid = product.vendorid
WHERE product.productprice < (SELECT AVG(productprice) FROM product)
```

10. Display the ProductID for the product that has been sold the most (i.e., that hs been sold in the highest quantity).

```sql
-- Give One product with highest sold quantity
SELECT productid FROM includes
GROUP BY productid
ORDER BY SUM(quantity) DESC
LIMIT 1
```

or

```sql
-- Give all products with highest sold quantity
SELECT productid FROM includes
GROUP BY productid
HAVING SUM(quantity) = (SELECT SUM(quantity) FROM includes GROUP BY productid ORDER BY SUM(quantity) DESC LIMIT 1)
```