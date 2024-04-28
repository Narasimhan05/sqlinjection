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
![Screenshot 2024-04-28 204119](https://github.com/Narasimhan05/sqlinjection/assets/132819871/1d5dacde-778f-40a1-9747-b8f3a236ea15)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![Screenshot 2024-04-28 204137](https://github.com/Narasimhan05/sqlinjection/assets/132819871/657b25be-4323-4cd7-81dd-c2e0fd8c7b83)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![Screenshot 2024-04-28 204150](https://github.com/Narasimhan05/sqlinjection/assets/132819871/f98ae854-7e84-401d-8536-b9e5ec8bbbab)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![Screenshot 2024-04-28 204231](https://github.com/Narasimhan05/sqlinjection/assets/132819871/fdb1c2f2-3666-4c20-bbf0-0bfc7d65eacb)

Click on the link “Please register here”
![Screenshot 2024-04-28 204250](https://github.com/Narasimhan05/sqlinjection/assets/132819871/b95b6b02-cd8d-48b8-a63b-d14adb6b6f85)

Click on “Create Account” to display the following page:
![Screenshot 2024-04-28 204324](https://github.com/Narasimhan05/sqlinjection/assets/132819871/48673350-3d02-4ce2-8379-005da8bc0f23)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
![Screenshot 2024-04-28 204334](https://github.com/Narasimhan05/sqlinjection/assets/132819871/6b448556-2674-41c8-b669-338cabcb6456)

Click “Login”. The logged in page will show as below:
![Screenshot 2024-04-28 204348](https://github.com/Narasimhan05/sqlinjection/assets/132819871/5e4e88c8-d16b-4ce7-9273-9c7946a7762b)

### Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![Screenshot 2024-04-28 204413](https://github.com/Narasimhan05/sqlinjection/assets/132819871/b4fe7604-1f16-4087-9577-dbe4457ed098)

Click the login button and you will see it enter into the administrator page.
![Screenshot 2024-04-28 204431](https://github.com/Narasimhan05/sqlinjection/assets/132819871/c72bff28-af1f-4aba-861e-6fa92be7f5d4)

Union-based SQL injection UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: img
![Screenshot 2024-04-28 204455](https://github.com/Narasimhan05/sqlinjection/assets/132819871/2f565b0a-b58f-4928-94e0-47700487786e)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below: http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

After adding the order by 6 into the existing url , the following error statement will be obtained:

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below: http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

After adding the order by 6 into the existing url , the following error statement will be obtained:

As it is having 5 columns the query worked fine and it provides the correct result

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
