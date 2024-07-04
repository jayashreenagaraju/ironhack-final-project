# Global retail analysis


## About Dataset

### Dataset Description:
The dataset represents retail transactional data. It contains information about customers, their purchases, products, and transaction details. The data includes various attributes such as customer ID, name, email, phone, address, city, state, zipcode, country, age, gender, income, customer segment, last purchase date, total purchases, amount spent, product category, product brand, product type, feedback, shipping method, payment method, and order status.

#### Key Points:

##### Customer Information:
Includes customer details like ID, name, email, phone, address, city, state, zipcode, country, age, and gender.
Customer segments are categorized into Premium, Regular, and New.

##### Transaction Details:
Transaction-specific data such as transaction ID, last purchase date, total purchases, amount spent, total purchase amount, feedback, shipping method, payment method, and order status.

##### Product Information:
Contains product-related details such as product category, brand, and type.
Products are categorized into electronics, clothing, grocery, books, and home decor.

##### Geographic Information:
Contains location details including city, state, and country.
Available for various countries including USA, UK, Canada, Australia, and Germany.

##### Temporal Information:
Last purchase date is provided along with separate columns for year, month, date, and time.
Allows analysis based on temporal patterns and trends.

##### Data Quality:
Some rows contain null values, and others are duplicates, which may need to be handled during data preprocessing.
Null values are randomly distributed across rows.
Duplicate rows are available at different parts of the dataset.

### Business Cases

* Customer segmentation analysis based on age, gender, income etc.
* Sales trend analysis over time to identify peak seasons or trends.
* Product performance analysis to determine popular categories, brand and type.
* Geographic analysis to understand regional preferences.

### [SQL Analysis](/sql/README.md)


### [Tableau dashboard](https://public.tableau.com/views/revenuepercategorypergender_17195173267050/CustomerInsightsDashboard?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

