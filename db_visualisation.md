# Visualising Database Schema:

When creating a new database for any projects, the schema a.k.a the layout of the database will always differ! This is logical ofcours as different data needs to be saved wihin different layouts. First off it's important to note what a database 
actually contains and the official terms corresponding. 

## Every DB has the following; 

+ name-datbase 1x
+ name-schema 1-20x
+ name-table 1-20x
    + name-id             (within the table)
    + name-data           (within the table)
    + created_at          (within the table)


Here is a example database witch has all the standard listed above things:
```bash

saving_mail_templates                                                                       (NAME DATABASE)
│
├── mail_content_template                                                                  (NAME SCHEMA) 
│   └── template_data                                                                      (NAME TABLE)
│       ├── template_id (SERIAL)                                                           (NAME ID)                  ((COLUMN))
│       ├── template_data (JSONB)                                                          (NAME DATA)                ((COLUMN))
│       └── created_at (TIMESTAMP)                                                         (NAME TIME-CREATED)        ((COLUMN))
│                                                                                           

```


## Terms every DB has explained: 

1. "database"
This one I think is very self explanetory, you can simply specify a name when creating a dabatase. 

2. "schema" 
You can view the schema within a database as the second directory within a database. It is helpfull to me to view a entire db as a directory tree. (see above visualisation) 
Within a schema tables reside, to the schema it's self is more like a indicator which tables reside there. 
So a schema is simply a directory that makes the whole structure of a db more clear and logical. Like "/passwords/" would likely contain passwords within that dir... "/passwords/mail_pass.txt" 

3. "table" 
As listed above in the visual a table always resides within a schema. The table within a schema (within a db) acts as the last directory before the actuall relevant information.  Within the table we can find the actuall JSONB object, 
the creation time. The time the object was added to the table. And finally the ID of the entry. 
Simply the table carries the information we actually want to fetch from a database. If thinking of it as a dir, it would look like so ""/passwords/mail/yahoo.txt" The relevant information clearly structured :D 


4, 5, 6. "column"
A calumn is simply a datatype present within a table. You can have verious different type of columns. We always use atleast these 3.
The last 3 terms are very much self explanetory, so I am not even going to explain them. Read ther'e names... 
```
name-id             (within the table within the schema within the database)
name-data           (within the table within the schema within the database)
created_at          (within the table within the schema within the database)
```


# Why visualise a DB? :

When creating a DB it is always very usefull and important to first off visualise the DB before actually creating it. While visualising the DB you will also directly figure out how the flow of you're code will interact with you're database. 

For instance you might first upload all needed web-urls for webscraping to a schema calle "web_scraping_urls" . Then secondly fetch those urls from the table within that schema. And after successfully webscraping those urls we can pass/input them into a schema called "successfully_scraped_urls" . When 100 ID's are present within the table "scrapped_urls" within schema "successfully_scraped_urls" we can stop webscrapping urls. 

What data do you need to save? What data do you need to access? What is most clear or logical layout for you're project? What flow does you're code follow? 


## Example datbase layouts:

```bash
web_scraping_data                                                                       (DATABASE NAME)
│
├── web_scraping_urls                                                                  (SCHEMA NAME) 
│   └── urls_to_scrape                                                                 (TABLE NAME)
│       ├── url_id (SERIAL)                                                           (COLUMN: ID)
│       ├── url (TEXT)                                                                 (COLUMN: URL)
│       ├── status (TEXT)                                                              (COLUMN: STATUS)
│       └── created_at (TIMESTAMP)                                                     (COLUMN: TIME CREATED)
│
└── successfully_scraped_urls                                                          (SCHEMA NAME)
    └── scraped_urls                                                                   (TABLE NAME)
        ├── url_id (SERIAL)                                                           (COLUMN: ID)
        ├── url (TEXT)                                                                 (COLUMN: URL)
        ├── data (JSONB)                                                               (COLUMN: DATA)
        └── scraped_at (TIMESTAMP)                                                     (COLUMN: TIME SCRAPED)

```
corresponding "init-db.psql" file;
```bash
-- Create the database if it does not exist
CREATE DATABASE IF NOT EXISTS web_scraping_data;

-- Connect to the web_scraping_data database
\c web_scraping_data;

-- Create the web_scraping_urls schema
CREATE SCHEMA IF NOT EXISTS web_scraping_urls;

-- Create the successfully_scraped_urls schema
CREATE SCHEMA IF NOT EXISTS successfully_scraped_urls;

-- Create the web_scraping_urls.urls_to_scrape table
CREATE TABLE IF NOT EXISTS web_scraping_urls.urls_to_scrape (
    url_id SERIAL PRIMARY KEY,
    url TEXT,
    status TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create the successfully_scraped_urls.scraped_urls table
CREATE TABLE IF NOT EXISTS successfully_scraped_urls.scraped_urls (
    url_id SERIAL PRIMARY KEY,
    url TEXT,
    data JSONB,
    scraped_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA web_scraping_urls TO <user_name_postgresql>;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA successfully_scraped_urls TO <user_name_postgresql>;

```


```bash
ecommerce_inventory                                                                    (DATABASE NAME)
│
├── product_catalog                                                                    (SCHEMA NAME) 
│   └── products                                                                       (TABLE NAME)
│       ├── product_id (SERIAL)                                                       (COLUMN: ID)
│       ├── name (TEXT)                                                               (COLUMN: NAME)
│       ├── description (TEXT)                                                        (COLUMN: DESCRIPTION)
│       ├── price (NUMERIC)                                                           (COLUMN: PRICE)
│       ├── quantity_available (INTEGER)                                               (COLUMN: QUANTITY AVAILABLE)
│       └── created_at (TIMESTAMP)                                                    (COLUMN: TIME CREATED)
│
└── order_management                                                                   (SCHEMA NAME)
    ├── orders                                                                         (TABLE NAME)
    │   ├── order_id (SERIAL)                                                         (COLUMN: ID)
    │   ├── customer_name (TEXT)                                                      (COLUMN: CUSTOMER NAME)
    │   ├── order_date (DATE)                                                         (COLUMN: ORDER DATE)
    │   ├── total_amount (NUMERIC)                                                    (COLUMN: TOTAL AMOUNT)
    │   └── status (TEXT)                                                              (COLUMN: STATUS)
    │
    └── order_items                                                                    (TABLE NAME)
        ├── order_item_id (SERIAL)                                                    (COLUMN: ID)
        ├── order_id (FOREIGN KEY: orders.order_id)                                    (COLUMN: ORDER ID)
        ├── product_id (FOREIGN KEY: products.product_id)                              (COLUMN: PRODUCT ID)
        ├── quantity_ordered (INTEGER)                                                 (COLUMN: QUANTITY ORDERED)
        ├── unit_price (NUMERIC)                                                       (COLUMN: UNIT PRICE)
        └── subtotal (NUMERIC)                                                         (COLUMN: SUBTOTAL)

```
corresponding "init-db.psql" file;
```bash
-- Create the database if it does not exist
CREATE DATABASE IF NOT EXISTS ecommerce_inventory;

-- Connect to the ecommerce_inventory database
\c ecommerce_inventory;

-- Create the product_catalog schema
CREATE SCHEMA IF NOT EXISTS product_catalog;

-- Create the order_management schema
CREATE SCHEMA IF NOT EXISTS order_management;

-- Create the product_catalog.products table
CREATE TABLE IF NOT EXISTS product_catalog.products (
    product_id SERIAL PRIMARY KEY,
    name TEXT,
    description TEXT,
    price NUMERIC,
    quantity_available INTEGER,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create the order_management.orders table
CREATE TABLE IF NOT EXISTS order_management.orders (
    order_id SERIAL PRIMARY KEY,
    customer_name TEXT,
    order_date DATE,
    total_amount NUMERIC,
    status TEXT
);

-- Create the order_management.order_items table
CREATE TABLE IF NOT EXISTS order_management.order_items (
    order_item_id SERIAL PRIMARY KEY,
    order_id INTEGER REFERENCES order_management.orders(order_id),
    product_id INTEGER REFERENCES product_catalog.products(product_id),
    quantity_ordered INTEGER,
    unit_price NUMERIC,
    subtotal NUMERIC
);

GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA product_catalog TO <user_name_postgresql>;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA order_management TO <user_name_postgresql>;

```











