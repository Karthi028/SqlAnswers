![customer table](image.png) 
![orders table](image.png) 
![products table](image.png)


1: Retrieve all customers who have placed an order in the last 30 days.

          =>  select c.name from customers as c join orders as o on c.id = o.customer_id where order_date > "2025-03-10" - interval 30 day;

          were are using inner join & relational operators to retrive the data that we want...which is past 30days customers who have placed orders ....  
          the inner join helps to join the two table data where the foreign key matches gives a table of combined data only for the matches...now logical operator will help us to get the data ...this case considered current day to be "2025-03-10" and past 30days data is obtained by reducing interval 30 day from the current day...if dates are realistic we can use (curdate() or now() instead of "2025-03-10");



2: Get the total amount of all orders placed by each customer.

          => select c.name,sum(o.total_amount) as TotalAmount from customers as c join orders as o on c.id = o.customer_id group by c.name;

          we need to join two tables so that we can get the total amount placed by each customers ....for this we can use inner join once the table are joined ....the total_amount column from orders can be made sum with respect to the customer name with help  of sum(orders.total_amount) inorder to get all the customers name with respect to sum of toal_amount we need group by ...

    
3: Update the price of Product C to 45.00.

          =>  update products set price = 45 where id =3;

          to update we can use update command and set the price to the table that we need and to the column we want by using relational operators and for specific we can use where and corresponding unique id;



4: Add a new column discount to the products table.
        
          =>  alter table products add column discount decimal(10,2);

          new column can be added using alter table tables name add column columns name and its type;

5: Retrieve the top 3 products with the highest price.

          => select name,price from products order by price desc limit 3;

          to list the price column in descending order we can use order by and then for top3 we can use limit ;


6: Join the orders and customers tables to retrieve the customer's name and order date for each order. 

          =>  select o.id as OrderId,c.name,o.order_date from customers as c join orders as o on c.id = o.customer_id order by OrderId;

          we can use the same inner join for this and the required fields we want can be listed out..



7: Retrieve the orders with a total amount greater than 150.00.
  
          => select * from orders where total_amount > 150;

          we use relational operator to solve this which is > 150 on total_amount column will give us results.

8 : Normalize the database by creating a separate table for order items and updating the orders table to reference the order_items table.

          =>  create table order_items (id int primary key auto_increment,order_id int,product_id int,quantity int,price decimal(10,2),foreign key (order_id) references orders(id),foreign key (product_id) references products(id));

          sample data insertion into Order_items table: 
              => insert into order_items (order_id,product_id,quantity,price) values (1,2,1,200),(1,1,1,100),(3,4,1,100),(2,3,2,90),(2,4,2,200);

              now we can remove the total_amount from the orders table using => alter table orders drop column total_amount; because the total amount can now be calculated using order_items table...

              what we have done here is created a order_items table where order_id is foreign key of orders id products_id is foreign key of products id...and the price will be the total price that is price * quantity...

![current orderstable](image.png)
![order_items Table](image.png)

9: Get the names of customers who have ordered Product A.

         => select c.name from customers as c join orders as o on c.id=o.customer_id join order_items as oi on o.id = oi.order_id join products as p on oi.product_id = p.id where p.name = "shirt";

         we need to join 3 times so we can achive the results because the tables are interconnected in that way ...say (customers and orders) then (orders and order_items) and then (order_items and products)..

         the customer name that we need in in the first join and the product A which is in this case shirt is in the last join ......once the tables are joined using inner joint we can use where clause to find the particular product ordered by particular individual or few...

10: Retrieve the average total of all orders.

          => select avg(total_amount) from orders;

          using summirisation with help of avg of particular price column from particular table gives us the result...

