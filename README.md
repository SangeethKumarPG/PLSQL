# PL/SQL NOTES
Architecture of PL/SQL

PL SQL architecture has 2 parts, physical architecture and logical architecture.

In an oracle database there an SQL engine which executes the SQL statements, When we sent a query to execute there are 3 steps that is happening:

1\. Parsing

2\. Fetching

3\. Executing

Oracle SQL engine also optimizes queries you run based on the optimisation level you selected. The SQL Engine handles all the DML operations (Insert, Update, delete). 

The PLSQL programs are run by the PLSQL engine. If your plsql program have some DML statements it is given to the SQL engine to be executed.

  
1\. PLSQL Engine is closely tied with the SQL engine of the oracle database. PLSQL codes are similar to SQL codes. The process of executing SQL statements from PLSQL engine is called **as context switches.** This operation is very fast. But if your program has too many context switches then you will have performance issues. 

2\. PLSQL enables us to have subprograms which helps us to store complex logics. These programs can be shared with others so that they can use it as a library.

3\. Dynamic Queries: PLSQL let's us create dynamic queries or you can use it create new SQL queries.

4\. PLSQL is case insensitive. 

5\. Code optimizer which is a build in feature of oracle which you can use to make your code performant. The optimizer changes the code that we write automatically based on the level of optimization before the code is executed.

6\. Enables object oriented programming

7\. Helps in webdevelopment. You can use PLSQL gateway and PLSQL webtoolkit for webapp development.

Pluggable database or multitenant architecture. This feature was introduced in oracle 12c. In this there is a big database called container database(Root database). Inside the container database there are pluggable databases. There must be atleast one pluggable databases in the root database. Each pluggable database has the full attributes of a regular database. It has it's own users, objects, applications etc. But the container database is not like pluggable database, it does not have any objects. It stores only metadata such as configuration and files. In previous versions of oracle such as 11g we had a dedicated server for each databases. Even if it is a small database we needed a dedicated server. But the pluggable architecture allows us to have multiple smaller databases in the same database server. This helped in reducing server cost and workload of the DBAs. 

Schema is a user in an oracle database. Each user must have a schema, so we can consider schemas as users. But, technically schemas are the collections of objects for each user in the oracle database. Every user has objects under their schemas and user does not have anything more than their schema has. One user has one schema. A schema has objects like:

* Tables
* Views
* Constraints
* Triggers  
etc.  
Schema design is also known as entity-relationship diagram.

Blocks are executable statements that are between **BEGIN** and **END** keywords. Apart from this other sections in the PLSQL are:

1\. **DECLARE** : Used to define variables, cursors and other user defined exceptions. It is an optional section. If you are not using any variables in your code the DECLARE section is optional.

2\. **BEGIN** : It is a mandatory section. It indicates that you are starting to write the executable code. 

3\. **EXCEPTION** : If there are any issues in the code this section will be executed. It is optional. If there is your code crashes the remaining lines are not executed and all the DML operations that you performed till then will be rolled back, to avoid this we can use exception. The syntax errors in your code are not considered as exceptions. EXCEPTIONS are only for logical errors.

4\. **END** : Indicates that your code is finished. You should put a semi-colon after this. It is mandatory. You cannot write after the end keyword. 

You cannot change the order of these keywords. 

NOTE: We should write something between BEGIN and END keywords. If you don't have anything between begin and end just simply specify NULL; eg:

```javaScript
DECLARE
 
BEGIN
    NULL;
END;
```

This is called an anonymous block.

  
There are 3 types of blocks in PLSQL 

**1\. Anonymous blocks:** If you open an SQL worksheet and write some PLSQL statements it is called an anonymous block. These blocks won't be saved into the database even though the operations you perform inside these blocks will be done. 

**2\. Procedures :** These types of blocks are used to store business logic. Typically used to perform repeating operations. These will be stored in the database. They are called named objects. You must define a name for the procedure to call them. You can get IN some parameters as take OUT some parameters from procedures.

**3\. Function :** Functions and procedures are essentially the same but, functions must return a value. 

PLSQL is not an output language, i.e, it does not have built in output functionality. If you need output you need to explicitly run a code at least once for a session called `**SET SERVEROUTPUT ON**`   
**DBMS\_OUTPUT** is a plsql package to help with output operations. To call a particular procedure in a package we use :

`package_name.procedure_name` syntax. 

To output something we use the **PUT\_LINE()** procedure. this will output to the script output whatever we write in the parenthesis. **We must use single quoted strings inside the parenthesis.** eg:

```javaScript
BEGIN
   dbms_output.put_line('HELLO WORLD!');
END;
```

  
**NESTED BLOCKS:** are blocks inside of other blocks. eg: 

```javaScript
BEGIN
   dbms_output.put_line('HELLO WORLD!');
   BEGIN
        dbms_output.put_line('INSIDE NESTED BLOCK!');
    END;
END;
```

  
Every variable must have a datatype. There are 4 types of variables in PLSQL, 

**1.** **Scalar Data Types**: Holds a single value with one type. It is the basic simple type. 

**2.** **Reference Data Types**: Reference data types holds values which points to a storage location. They can be called as pointers. 

**3.** **Large Objects**: Large objects are also reference types that are stored outside of the table such as images and files.

**4.** **Composite Data Types** : These are also known as collections or records. They can hold more than one value. They can be of a single type or of different type. 

#### Scalar Types:

**1\. CHAR(max\_length)** : Use to store strings and text. The maximum capacity of this type is 32767 bytes. If you don't specify any length the default value is 1 byte. If you specify 30 bytes for a char type 30 bytes is used anyway.
  
  
**2.** **VARCHAR2(max\_length)**: The main difference between the char type and varchar2 data type is that it has variable length. The length that you define is the maximum length it can use. Only the necessary space is allocated for each value.

**3\. NUMBER(precision, scale)** : Also called fixed length number data type with precision and scale. If you write only the number keyword it will take the maximum capacity of the number type. The precision can be between 1 and 38, and the scale can be between -84 and 127\. If we create a number with precision 3 and scale 2 this means that the number will be 3 digits in total and 2 of them will be decimal parts. 

**4\. BINARY\_INTEGER or PLSINTEGER** : both are identical data types after oracle 11g. Their range is between -2 billion and +2 billion. This type is faster and consumes less storage than number datatype. These are faster than numbers in arithmatic operations. 

  
**5\. BINARY\_FLOAT** : Allocates 5 bytes of memory. It ends with f character. eg: 2.07f. 

**6\. BINARY\_DOUBLE** : Required 9 bytes in the memory. It ends with d character eg: 5.098d. It has larger precision than binary float data type. 

**7\. BOOLEAN** : It is used to store True or False values. **NOTE that in PLSQL it can also hold NULL by default the value is stored as NULL.** 

**8\. DATE** : Used to store data in hour, minutes, seconds. To get the current date you should use the current\_date or sysdate functions. 

**9\. TIMESTAMP** : It is an extension of DATETIME. In addition to the date format, it has precision in milliseconds which we explicitly specify. 

**10\. TIMESTAMP(p) WITH TIME ZONE** : This data type extends the Time stamp data type with timezone. Timezone is difference in hours and minutes between local time and co-ordinated universal time. It has similar precision to that of timestamp which ranges between 0 and 9\. The p indicates the precision in milliseconds for the timezone.

  
**11\. TIMESTAMP WITH LOCAL TIME ZONE** : This is similar to the time stamp with the time zone data type. The difference is that when you insert a value it is stored in the data bases timezone wherever your database timezone is set. But when you query the database oracle database will return the local time zone of your session. 

**12\. INTERVAL YEAR(precision) TO MONTH** : This data type manipulates intervals of years and months. We can set the precision for numbers of digits in year section.

**13\. INTERVAL DAY(maxvalue) TO SECONDS(precision) :** This type stores and manipulates intervals in days, hours, minutes and seconds. These precisions specify the number of digits in the specified time value. 

**NOTE : BINARY\_FLOAT and BINARY\_DOUBLE data types don't raise exceptions.**

PLSQL has rules for naming of the variables. They are:

1\. They must start with a letter

2\. Can contain special characters like \_ and $

3\. Must be maximum 30 characters.

4\. Cannot have oracle's reserve words like select, varchar2 etc.

Apart from this there are some naming conventions in PLSQL:

1\. v\_variable\_name for variables eg: v\_max\_salary.

2\. cur\_cursor\_name for cursors eg: cur\_employees.

3\. e\_exception\_name for exceptions. eg: e\_invalid\_salary

4\. p\_procedure\_name for procedures and functions. eg: p\_calculate\_salary

5.b\_bind\_name for bind variables eg: b\_emp\_id.

  
To declare variable we use the general syntax of:

`**Name [CONSTANT] datatype [NOT NULL] [:= DEFAULT value | expression];**`If you don't want the variable value not to be changed we use mark them as constant. If you declare a variable constant it cannot be changed until the program is finished. 

Any sql datatype can be used. If you don't want the variables to have null value we mark them as not null after specifying the datatype. 

:= is used for assigning the value.

DEFAULT keyword is used to assign a default value to your variable. If you assign any value to that variable the default value will not work. For example even if assign a NULL value to the variable it will be NULL and not the default value. 

expressions can be a calculation or a value coming from a function. If you want to assign a value to the variable we must do them between BEGIN and END block.

**In PL/SQL we concatenate string using the || operator.** 

eg:

```javaScript
DECLARE
    v_test VARCHAR2(50) NOT NULL DEFAULT 'HELLO';
BEGIN
    v_test := 'PL/SQL' || ' COURSE';
    dbms_output.put_line(v_test || ' BEGINNER TO ADVANCED');
END;
```

Unlike the VARCHAR2 datatype we don't need to define a fixed size for the NUMBER datatype. eg:

```javaScript
DECLARE
    v_test NUMBER NOT NULL := 50;
BEGIN
    dbms_output.put_line(v_test || ' BEGINNER TO ADVANCED');
END;
```

For example:

```javaScript
DECLARE
    v_number NUMBER(2) NOT NULL := 50.42;
BEGIN
    dbms_output.put_line('The number is : '||v_number);
END;
```

The output of the above program will be :

The number is : 50

because NUMBER(2) is indicating that the number only has 2 digits. even if we change to NUMBER(4) it will still print 50\. because we are only specifying the precision( whole numbers). To display the digits after the decimal point we need to specify the scale.

**NOTE: The precision must be greater than or equal to the total digits of numbers.**

```javaScript
DECLARE
    v_number NUMBER(4,2) NOT NULL := 50.42;
BEGIN
    dbms_output.put_line('The number is : '||v_number);
END;
```

This will be the correct way to display the number correctly.

Similarly if we reduce the scale to 1 we can only see 50.4 which is only 1 digit after the decimal point.

**_This means that the precision and scale must be greater than or equal to the number._**

If we are dealing with a complex code we should use PLS\_INTEGER or BINARY\_INTEGER. 

We don't need to specify the precision and scale for the BINARY\_FLOAT and BINARY\_DOUBLE. But we must indicate it with the characters f and d.

eg:

```javaScript
DECLARE
    v_number BINARY_FLOAT NOT NULL := 50.42f;
BEGIN
    dbms_output.put_line('The number is : '||v_number);
END;
```

This will output The number is : 5.04199982E+001.

We must be careful with these datatypes because it is mostly used for scientific calculations where accuracy is important.

```javaScript
DECLARE
    v_date DATE NOT NULL := SYSDATE;
BEGIN
    dbms_output.put_line('The date is : '||v_date);
END;
```

This will display the current date. To create a date with time information we can create a timestamp string and use it like:

```javaScript
DECLARE
    v_date TIMESTAMP NOT NULL := '22-MAY-25 14:02:22';
BEGIN
    dbms_output.put_line('The date is : '||v_date);
END;
```

Instead of a timestamp string we can use the SYSTIMESTAMP keyword to get the current time stamp. so the output will be like The date is : 28/05/25 01:55:11.670777

```javaScript
DECLARE
    v_date TIMESTAMP WITH TIME ZONE NOT NULL := SYSTIMESTAMP;
BEGIN
    dbms_output.put_line('The date is : '||v_date);
END;
```

This will display the timestamp with the timezone. If we specify the precision with the timestamp, it will only display milli second digits with the specified precision. eg:

```javaScript
    v_date TIMESTAMP(3) WITH TIME ZONE NOT NULL := SYSTIMESTAMP;
```

This will display The date is : 28/05/25 01:58:55.787 +00:00

The interval data types are used to calculate how many days and seconds passed. We can use interval days like:

```javaScript
DECLARE
    v_date INTERVAL DAY(4) TO SECOND(2) := '24 02:05:21.012';
BEGIN
    dbms_output.put_line('The date is : '||v_date);
END;
```

This will provide an output like: 

The date is : +0024 02:05:21.01

We can specify 1 to 9 values days and seconds. The default value is 2 for days and 6 for milli seconds.

To use interval year to month we use like: 

```javaScript
DECLARE
    v_date INTERVAL YEAR TO MONTH := '12-3';
BEGIN
    dbms_output.put_line('The date is : '||v_date);
END;
```

This will provide The date is : +12-03\. We cannot specify a precision for month here by default it is 2.

If we use boolean variable like this :

```javaScript
DECLARE
    v_bool BOOLEAN := TRUE;
BEGIN
    dbms_output.put_line('The value is : '||v_bool);
END;
```

This will give us an error because **we cannot use boolean as printable**. We can only use them with IF statements.

%TYPE is used to get the datatype of the referenced column. This is useful when we don't exactly know the precisions, scales and types of the column, if the datatypes of the columns do not match we will get an error. If you alter the precision or scale or type of a column in a database we also need to change them in all our procedures if we are manually assigning types for each variable. To avoid this we can use the %TYPE. You should try to avoid hardcoding the types for the column. **If you are working with a datatype column with the %TYPE column.** For example:

```javaScript
DECLARE
    v_type employees.job_id%TYPE;
BEGIN
    v_type:= 'IT_PROG';
    dbms_output.put_line(v_type);
END;
```

This will work because the job\_id column is of varchar2 type and of precision 10\. but when we change the value of v\_type into 'ADMINISTRATOR'. We will get an error like the character string buffer too small. 

We can also reference the type of a variable to another variable. for example:

```javaScript
DECLARE
    v_type employees.job_id%TYPE;
    v_type2 v_type%TYPE;
BEGIN
    v_type:= 'IT_PROG';
    v_type2 := 'TEST_PROG';
    dbms_output.put_line(v_type);
    dbms_output.put_line(v_type2);
END;
```

**NOTE: Even if your column is a NOT NULL type the referenced type value can be null** for example:

```javaScript
DECLARE
    v_type employees.job_id%TYPE;
    v_type2 v_type%TYPE;
    v_type3 employees.job_id%type;
BEGIN
    v_type:= 'IT_PROG';
    v_type2 := 'TEST_PROG';
    dbms_output.put_line(v_type);
    dbms_output.put_line(v_type2);
    dbms_output.put_line(v_type3 || ' ' || 'HELLO');
END;
```

This will not throw an error because of that. It will also not print the null in the script output. If we want to make sure the values are null we need to handle it in another way.

Delimiters are compound symbols that have special meaning in Pl/SQL. We know that ; is to terminate a statement and := is used to assign a value. 

**\+ Addition**

**\- Subtraction**

**\* Multiplication**

**/ Division**

**\= Equality**

**@ Remote Access** : To access a remote database we use DB\_LINKS. and we use @ symbol when we are using db\_links. 

**; Statement terminator**

**<> Inequality**

**!= Inequality**

**|| Concatenation**

**:= Assignment**

**\-- Single line comment**

**/\* \*/ Multi line comment**

The lines marked as comments will not be treated as code. Comments are useful when creating packages, procedures, functions, triggers etc because they are reused many times by different developers. 

Scopes are bounds that a variable is valid. We use indentation to nest blocks of PL/SQL code. The scoping of variables is similar to scoping in other programming languages. I.e, the outer variables can be reached by the inner block but the inner variables cannot be accessed by the outer blocks.

eg:

```javaScript
DECLARE
    v_outer VARCHAR2(50):='outer!';
BEGIN
    DECLARE
        v_inner VARCHAR(50) := 'inner!';
    BEGIN
        dbms_output.put_line('v_inner -> '||v_inner);
        dbms_output.put_line('v_outer -> '||v_outer);
    END;
END;
```

We can declare same variables inside and outside of the nested blocks but if you are using inner variable inside the nested block the value of the nested block will be used. for example:

```javaScript
DECLARE
    v_text VARCHAR2(50):='inner text!';
BEGIN
    DECLARE
        v_text VARCHAR(50) := 'inner!';
    BEGIN
        dbms_output.put_line('inside -> '||v_text);
    END;
    dbms_output.put_line('outside -> '||v_text);
 
END;
```

We cannot define same named variables in the same scope. There is also another way to access the outer variable inside the inner block. This is by using labels for the blocks. eg:

```javaScript
BEGIN <<outer>>
DECLARE
    v_text VARCHAR2(50):='inner text!';
BEGIN
    DECLARE
        v_text VARCHAR(50) := 'inner!';
    BEGIN
        dbms_output.put_line('inside -> '||v_text);
        dbms_output.put_line('accessing using label ->'||outer.v_text);
    END;
    dbms_output.put_line('outside -> '||v_text);
 
END;
END outer;
```

This is by using a label for the outer block. We can do that by adding a label with the begin keyword wrapped in <<>>. At the end of the block we need to end it by specifying the label name.

Syntax: 

```javaScript
BEGIN <<label>>
    statements;
END label;
```

To access the value we use :

`**label.variable_name;**`   

Bind variables increases performance, They are created in the host environment. As they are created in an environment they can be used or modified by multiple subprograms. It's scope is your whole worksheet. You can use bind variables like any other variable. To create a bind variable we use the syntax :

`variable variable_name type;`   
After this we choose the run script option. This will not show any output or errors which is normal.

To access the variable in the program we create a PLSQL block and use the : symbol before the variable name. We can assign the value of bind variable to another variable inside the block or directly access the value. eg:

```javaScript
DECLARE
    v_text VARCHAR(30);
BEGIN
    :var_text := 'Hello PLSQL';
    v_text := :var_text;
    dbms_output.put_line('Bind variable value is : '||v_text);
    dbms_output.put_line('The original variable is : '||:var_text);
END;
```

**To print the value of the variable outside of a block we can use the print statement.** for example:

```javaScript
print var_text;
```

  
We don't need to specify the : symbol when accessing the bind variable outside of the scope. 

**NOTE : We can only access the bind variables in the same worksheet.** 

Another way to access the bind variables is using the set autoprint on command.

syntax: `set autoprint on;` 

This will print the value of the bind variables wherever it sees that bind variable. 

**NOTE : We can assign a maximum value for varchar2 when using bind variables but for numbers we cannot define the scale or precision.** 

**Also note that we cannot create bind variables for all types. We can create bind variables for types varchar, number, char, clob, nclob, refcursor, binary\_float, binary\_double.**

**Also we cannot directly assign a value to the bind variable outside of the begin end blocks.** 

We can use bind variables with SQL queries. 

For example:

```javaScript
variable var_sql NUMBER;
 
BEGIN
    :var_sql := 100;
END;
 
SELECT * FROM employees e WHERE e.EMPLOYEE_ID =:var_sql;
```

  
**IF,** **THEN** and **END IF** statements are mandatory for an if statement. The syntax is :

```javaScript
IF condition THEN statements;
[ELSIF condition THEN statements;]
[ELSIF condition THEN statements;]
[ELSE statements;]
END IF;
```

The condition must be a boolean value or an expression which returns a boolean value. statements are any SQL or PLSQL statements. If the first condition is satisfied other conditions are not checked. The END IF statement is mandatory to terminate the IF statement. 

**NOTE : We use ELSIF for the else if in PLSQL**

```javaScript
DECLARE
    v_number NUMBER := 30;
BEGIN
    IF v_number < 10 THEN 
        dbms_output.put_line('The number is less than 10');
    ELSIF v_number < 20 THEN 
        dbms_output.put_line('The number is less than 20');
    ELSIF v_number < 30 THEN 
        dbms_output.put_line('The number is less than 30');
    ELSE
        dbms_output.put_line('The number is greater than or equal to 30');
    END IF;
END;
```

This will print The number is greater than or equal to 30

We can nest IF statements within an if or else statement. We can also add multiple conditions with the IF statements using **NOT**, **AND** and **OR** operators. For example:

```javaScript
DECLARE 
    v_number NUMBER := 10;
    v_name VARCHAR2(30) := 'Carol';
BEGIN
    IF v_number = 10 AND v_name = 'Carol' THEN
        dbms_output.put_line('The number is 10 and the name is Carol');
    END IF;
END;
```

  
CASE statements or CASE expressions are similar to IF statements. There are 3 types of CASE statements in PLSQL 

**1\. CASE Expressions :** The syntax of the case expression looks like this:

```javaScript
CASE [expression or condition]
    [WHEN condition1 THEN result1]
    [WHEN condition2 THEN result2]
    [WHEN conditionN THEN resultN]
    [ELSE       result]
END;
```

When one condition is matched then other cases are not checked. **The expressions and all the conditions must be of same datatype. If we are omitting the expression part and check a condition we can have multiple types of comparisons because we are using boolean results to match the case.** 

for example:

```javaScript
DECLARE
    v_job_code VARCHAR2(10) := 'SA_MAN';
    v_salary_increase NUMBER;
BEGIN
    v_salary_increase := CASE v_job_code
        WHEN 'SA_MAN' THEN 0.2
        WHEN 'SA_REP' THEN 0.3
        ELSE 0
        END;
    dbms_output.put_line('The salary increase percentage is '||v_salary_increase);
END;
```

note that expressions are of same data type. 

The results are also of the same datatype. 

**2\. searched CASE statements :** In this we omit the expression and there will be condition. Our condition must be something that returns a boolean value. So the above code can be written as :

```javaScript
DECLARE
    v_job_code VARCHAR2(30) := 'SA_MAN';
    v_salary_increase NUMBER;
BEGIN
    v_salary_increase := CASE 
        WHEN v_job_code = 'SA_MAN' THEN 0.2
        WHEN v_job_code = 'SA_REP' THEN 0.3
        ELSE 0
    END;
    dbms_output.put_line('The salary increase percentage is '||v_salary_increase);
END;
```

The above example is not useful but if we have multiple conditions we can use this. for example:

```javaScript
DECLARE
    v_job_code VARCHAR2(30) := 'IT_PROG';
    v_salary_increase NUMBER;
BEGIN
    v_salary_increase := CASE 
        WHEN v_job_code = 'SA_MAN' THEN 0.2
        WHEN v_job_code IN('SA_REP', 'IT_PROG') THEN 0.3
        ELSE 0
    END;
    dbms_output.put_line('The salary increase percentage is '||v_salary_increase);
END;
```

  
**NOTE: There must be condition which returns a boolean value between WHEN and THEN.**

**3\. CASE Statements :** In the above examples we assigned the results of case statements into a variable. We can use case statements like an IF statement. So the above code can be rewritten as:

```javaScript
DECLARE
    v_job_code VARCHAR2(30) := 'IT_PROG';
    v_salary_increase NUMBER;
BEGIN
    CASE 
        WHEN v_job_code = 'SA_MAN' THEN 
            v_salary_increase := 0.2;
            dbms_output.put_line('The salary increase percentage for Sales Man is '||v_salary_increase);
        WHEN v_job_code IN('SA_REP', 'IT_PROG') THEN 
            v_salary_increase := 0.3;
            dbms_output.put_line('The salary increase percentage for SA_REP/ IT_PROG is '||v_salary_increase);
        ELSE 
            v_salary_increase := 0;
            dbms_output.put_line('The salary increase percentage is '||v_salary_increase);
    END CASE;
END;
```

**NOTE: We must use semi-colons to terminate the statements. also use END CASE**

This let's us write whole code blocks inside of a CASE statement like an IF statement.
