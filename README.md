# Lab6: Customer Gift Package Reward Manager (Nested Tables + Packages + CASE)
In this lab, we aim to work on the Customer-Order database that we installed in lab2. The schema of this database is provided in this assignment. You will need to use this schema in order to complete the lab. 


### Schema Diagram

```mermaid
---
title: CO schema
config:
  layout: elk
---
erDiagram
  CUSTOMERS ||--o{  ORDERS      : have
  CUSTOMERS ||--o{  SHIPMENTS   : have
  STORES    ||--o{  ORDERS      : have
  STORES    ||--o{  SHIPMENTS   : have
  STORES    ||--o{  INVENTORY   : have
  ORDERS    ||--|{  ORDER_ITEMS : have
  SHIPMENTS ||--|{  ORDER_ITEMS : have
  PRODUCTS  ||--o{  ORDER_ITEMS : have
  PRODUCTS  ||--o{  INVENTORY   : have

  CUSTOMERS {
    interger       customer_id    PK
    varchar2(255)  email_address  UK "NN"
    varchar2(255)  full_name         "NN"
  }

  STORES {
    integer        store_id            PK
    varchar2(255)  store_name          UK "NN"
    varchar2(100)  web_address
    varchar2(512)  physical_address
    number(9)      latitude
    number(9)      longitude
    blob           logo
    varchar2(512)  logo_mime_type
    varchar2(512)  logo_filename
    varchar2(512)  logo_charset
    date           logo_last_updated
  }

  PRODUCTS {
    interger       product_id         PK
    varchar2(255)  product_name          "NN"
    number(10)     unit_price
    blob           product_details
    blob           product_image
    varchar2(512)  image_mime_type
    varchar2(512)  image_filename
    varchar2(512)  image_charset
    date           image_last_updated
  }

  ORDERS {
    integer      order_id       PK
    timestamp    order_tms         "NN"
    interger     customer_id    FK "NN"
    varchar2(10) order_status      "NN"
    integer      store_id       FK "NN"
  }

  SHIPMENTS {
    integer        shipment_id       PK
    integer        store_id          FK "NN"
    integer        customer_id       FK "NN"
    varchar2(512)  delivery_address     "NN"
    varchar2(100)  shipment_status      "NN"
  }

  ORDER_ITEMS {
    integer   order_id      PK "FK, NN"
    integer   line_item_id  PK "NN"
    integer   product_id    FK "NN"
    integer   unit_price    "NN"
    integer   quantity      "NN"
    integer   shipment_id   FK
  }

  INVENTORY {
    integer    inventory_id       PK
    integer    store_id           FK "NN"
    integer    product_id         FK "NN"
  integer      product_inventory     "NN"
}
```
## Part A — Create Gift Types and GIFT_CATALOG Table (Nested Tables)
(8 marks)
1.	Create a nested table type to store multiple gift items (e.g., 'Teddy Bear', 'Chocolate Box').
2.	Create a table GIFT_CATALOG with the following columns:
     - GIFT_ID — PRIMARY KEY
     - PURCHASE_AMOUNT — the purchase amount to qualify for the gift package
     - a nested table of gift items (use the type created above)
3. Configure nested table storage:
```
NESTED TABLE gifts STORE AS <your_storage_table>;
```
4. Insert at least three gift packages, each containing multiple gift items.
  	
