
# edxCovid19

  

## node web app created to share edx promotional codes at the Universitat Politènica de Valencia

  ### The [Universitat Politècnica de Valencia](https://www.upv.es/) is proud to contribute with the edX project with this promotional code dispatcher.

> Disclaimer: you can see the web working at
> http://www.upv.es/edxcovid19/ but you need an upv related email to
> request licences
> 
> 
> 
> Disclaimer2: since it has been done in 2-3 days lapse while I relearnt
> node after long hiatus without using it. You prolly can find better
> ways to do some stuff (feel free to teach me the ways).

  

# I don't care, I want to use it for my organization!

  

If you want to use it for your organization you will need a mysql server with 2 tables and a nodejs server, just that.

###  1. For the creation of tables at the mysql server you'll need to run:

    CREATE TABLE `solicitudes` (  
    `id` int(11) NOT NULL AUTO_INCREMENT,  
    `email` varchar(255) NOT NULL,  
    `licencia` varchar(45) DEFAULT NULL,  
    `fecha` date NOT NULL,  
    `enviada` date DEFAULT NULL,  
    PRIMARY KEY (`id`)  
    ) ENGINE=InnoDB AUTO_INCREMENT=211 DEFAULT CHARSET=utf8;
    CREATE TABLE `licencias` (  
    `idlicencias` int(11) NOT NULL AUTO_INCREMENT,  
    `plataforma` varchar(45) DEFAULT NULL,  
    `licencia` varchar(45) NOT NULL,  
    `url` varchar(255) DEFAULT NULL,  
    `entregada` tinyint(4) DEFAULT NULL,  
    PRIMARY KEY (`idlicencias`),  
    UNIQUE KEY `plataforma` (`plataforma`,`licencia`)  
    ) ENGINE=InnoDB AUTO_INCREMENT=7009 DEFAULT CHARSET=utf8;

My apologies for the column names, when I generated the tables did not expect this to go public ^^ feel free to translate them but if you do so you'll need to change the querys as well.

###  2. Configure the connection to the mysql server at config/database_conf.js

    const pool = mysql.createPool({    
	    connectionLimit:  1000,    
	    password:  'yourpassword',    
	    user:  'youruser',    
	    database:'licences',    
	    host:'localhost',    
	    port:'3306'    
    });



### 3. Change the mail filter and messages
Ok, this is prolly not the most elegant way to do things, but you can change the mail configuration at config/email.js.

Here you can change the domain allowed to request licences and the message that the users will receive.

### 4. change the user messages:
Like above, I created at config/lang_conf.js: a file with the popup messages the user will receive, you can change it there.

### 5. change the task_rest urls:

There are 2 tasks that every 15 mins will check if there are new requests and will asign licences (if there are available and the limit is not reached) and send messages with the codes to the users. You will need to set the valid route at config/task_rest.js

And thats the last configuration step.
 
 ### 6. Once you have all configured install de node app with

     npm install

 ### 7. Then launch the webserver  

    npm start

Tthis will launch the server at ./bin/www and start to listen at the port 3000

### 6. I have a server running and an empty database, what now?

OK,OK calm down, edX surelly sent you or your organization a csv file with the licences right? put that file at **/edxlicences/new** and then you have 2 options: wait for a magic creature (crontask) to upload them to the mysql or you can just visit **http://yourhost/licences/load** this will launch a process that loads the csv file into the mysql and then moves the file from new to processed.
 
 

> Disclaimer3: Aha you thought i'm done he,he guess again. If you have
> problems setting this up first of all my apologies and second you can
> contact with me via github and i'll try to help in the way i can.
