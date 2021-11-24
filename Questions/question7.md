# Question 07

Design a model for a simple Order Management System.

The system consists of:

* Clients
* Products
* Orders
* Any other tables/entities you may need

You can draw, describe, or list the tables as SQL.
Extras:
* SQL: list ORDERS with number of items
* Which indexes should be created in this model?

# Resolution

First, I designed this Entity-Relationship model, just to help me see more clearly how to create the SQL tables:

![.]('Questions/assets/ER_model.png)

Then, I create the SQL tables:

~~~SQL
CREATE TABLE Client(
    client_id VARCHAR(256) NOT NULL,
    username VARCHAR(256) NOT NULL,
    email VARCHAR(256) NOT NULL,
    PRIMARY key(client_id),
);

CREATE TABLE Delivery_Company(
    delivery_id VARCHAR(256) NOT NULL,
    delivery_name VARCHAR(256) NOT NULL,
    PRIMARY key(delivery_id),
);

CREATE TABLE Company(
    company_id VARCHAR(256) NOT NULL,
    company_name VARCHAR(256) NOT NULL,
    PRIMARY key(company_id),
);

CREATE TABLE Order(
    order_number INTEGER NOT NULL,
    client_id VARCHAR(256) NOT NULL,
    number_of_items INTEGER NOT NULL,
    client_address VARCHAR(256) NOT NULL,
    client_name VARCHAR(256) NOT NULL,
    delivery_id VARCHAR(256) NOT NULL,

    PRIMARY key(order_number),
    FOREIGN key(client_id) REFERENCES Client(client_id),
    FOREIGN key(delivery_id) REFERENCES Delivery_Company(delivery_id)
);

CREATE TABLE Product(
    product_id VARCHAR(256) NOT NULL,
    product_name VARCHAR(256) NOT NULL,
    company_id VARCHAR(256) NOT NULL,
    quantity INTEGER NOT NULL,
    order_number INTEGER NOT NULL,

    PRIMARY key(product_id),
    FOREIGN key(company_id) REFERENCES Company(company_id),
    FOREIGN key(order_number) REFERENCES Order(order_number
);
~~~

Query to list the orders with the number of items:

~~~SQL
SELECT O.order_number, O.number_of_items
    FROM Orders as O
~~~

If I didn't put the attribute number_of_items in Orders, the query would be:

~~~SQL
SELECT O.order_number, COUNT(P)
    FROM Orders as O, Product as P
        WHERE P.order_number = O.order_number
~~~

To create the database, the products would be the last entity to be created (in fact, the order presented in the creation above is the correct order) since it depends of foreign keys of previous tables.


