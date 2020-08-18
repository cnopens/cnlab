#devtools-chain.md


##  GO_REST_API_Generator space_invader

https://github.com/Darkmyes/GO_REST_API_Generator

sparkles This is a REST API generator based in GO language that can generate code in NodeJS, PHP and GO.

sparkles Can connect to MySQL and SQLServer Databases and generate a json file with the basic information of the DB and tables Schema.

sparkles Supported Languages with DB Connection:
LANGUAGES 	MySQL 	MSSQL
PHP 	heavy_check_mark 	x
NODEJS 	heavy_check_mark 	heavy_check_mark
GO 	heavy_check_mark 	x
sparkles Flags Examples:

    ✔ go run main.go -mode=gen -folder=public -lang=nodejs
    
    ✔ go run main.go -mode=db_read -db=mysql -server=localhost -user=root -pass=Password -db_name=DB -folder=project_folder   
    
    ✔ go run main.go -mode=db_read_gen -db=mysql -server=localhost -user=root -pass=Password -db_name=DB -folder=project_folder


## NodeJS+golang 实例场景

Webpages on NodeJS server connected to a Golang Api on a Go server.

serverexample.js is the JS code for setting up the node in node.js as a server. page1.html is html page. page2.html is html page.

I've made the button work using JS.

To run :

NodeJS :

    In the terminal run (keep terminal in the NodeJS folder): node serverexample.js
    Open browser : localhost:3000/page.html
    The port can be changed in the last line of serverexample.js

Go :

    terminal : go get github.com/gorilla/handlers
    terminal : go get github.com/gorilla/mux
    goapi2.go returns json objects.
    goapi.go return text response.
    go run goapi.go / go run goapi2.go
    Check using http://localhost: port-number/ name

JS part :

    Parser for text.
    Parser for Json.

Clone the git, unzip the node_modules for it to work.

