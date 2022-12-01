# scandiweb_jmeter
Load testing script for ScandiPWA using Jmeter (5.4.1)

_Note: Script wasn't tested on Jmeter 5.5+_


---

## Installation
Just download the repo and use jmx file
https://github.com/magento-ecg/scandipwa_jmeter

## Preparation
Before running TL it needs to prepare a list of 
1. Update Domain name variable: base_host
2. Update Search keyword list csv file .scandipwa/search_list.csv
3. Update Category list csv file .scandipwa/category_url.csv
4. Update Product list csv file .scandipwa/product_list.csv

_Note:_
_To prepare category and product list, use url_rewrite table or access.log. Filter our product url's by stock availability_
_To prepare search list we would recommend to use magento search_terms DB table._


---


## Usage
### Run from the interface
Just do it :)

### Run from the command line

running 2 threads 
>jmeter -Jthreads=2 -Jstartup=30 -Jhold=250 -Jshutdown=30 -n -t Scandi.jmx -l result_2.jtl -e -o gg_test2
 
runing 10 threads
>jmeter -Jthreads=10 -Jstartup=30 -Jhold=250 -Jshutdown=30 -n -t Scandi.jmx -l result_10.jtl -e -o gg_test10

###Typical Erros:

- Response Code: 404

- Products out of stock
{"errors":[{"message":"Product that you are trying to add is not available.","extensions":{"category":"graphql-input"},
"locations":[{"line":1,"column":164}],"path":["saveCartItem"]}],"data":{"saveCartItem":null}}`


---

### Specifics
Each GraphQL request has a "hash" attribute as a parameter.
>hash=2263075323

This "hash" is used for the reverse-proxy caching (CDN - cache) mechanism and represent unique mutations without variables.
> console.log(hash("query($url_1:String!){urlResolver(url:$url_1){id,sku,type}}"));

As soon as mutations are not changing often, This "hash" might be identical across all installations.
But worth re-checking in the browser tool before running.
