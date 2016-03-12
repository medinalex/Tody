**Tody** is a C# class library to connect to a SQL database and execute stored procedures and/or other sql queries.

## Usage
###Open Data Base

```cs
  //get connection string from webconfig
  sqlconnection = ConfigurationManager.ConnectionStrings["MyDataBase"].ToString();

  Tody sqldb = new Tody(sqlconnection);
  
  //open database
  sqldb.OpenDb();

```
###Also Open Data Base

```cs
  //get connection string from webconfig
  sqlconnection = ConfigurationManager.ConnectionStrings["MyDataBase"].ToString();

  Tody sqldb = new Tody();
  
  //open database
  sqldb.OpenDb(sqlconnection);

```

###And close Data Base

```cs
   sqldb.CloseDb();
```

### Use IsDbOkay() method

Returns true if connection to database was good. 
```cs
  //get connection string from webconfig
  sqlconnection = ConfigurationManager.ConnectionStrings["MyDataBase"].ToString();

  Tody sqldb = new Tody();
  
  //open database
  sqldb.OpenDb(sqlconnection);  
  
  if(sqldb.IsDbOkay())
  {
    //do something
  }     

```
###Executing a sql query

Executing a sql query with no parameter.
```cs

  //get connection string from webconfig
  sqlconnection = ConfigurationManager.ConnectionStrings["MyDataBase"].ToString();

  string query = "Select top 10 * from tblPets";
  
  //timeout value in seconds
  int timeout=5;
  
  //Declare Tody object
  Tody sqldb = new Tody();
  
  //if connection to database was good then execute query
  if(sqldb.OpenDb(sqlconnection).IsDbOkay())  
     sqldb.ExecQuery(timeout, query);  
     
   sqldb.CloseDb();
```
###Executing a stored procedure no parameters

Executing a stored procedure named **getAllDepartments** but without parameters.
```cs

  //get connection string from webconfig
  sqlconnection = ConfigurationManager.ConnectionStrings["MyDataBase"].ToString();

   // stored procedure name
  string stored_procedure_name = "getAllDepartments";
  
  Tody sqldb = new Tody();
  
  //if connection to database was good then execute query
  if(sqldb.OpenDb(sqlconnection).IsDbOkay())  
      sqldb.ExecStoredProcedure(stored_procedure_name);
      
  sqldb.CloseDb();
```
###Executing a stored procedure with one parameter

Executing a stored procedure named **getAllClientsByDepartment** with a parameter named **Department**.
```cs
  //get connection string from webconfig
  sqlconnection = ConfigurationManager.ConnectionStrings["MyDataBase"].ToString();
  
  //declare string value for the parameter
  string strDepartment = "Accounting"; 
  
  int timeout=30;
  
  //declare an array of sql parameters
  SqlParameter[] arrParameters = { new SqlParameter("@department", strDepartment) };
  
  Tody sqldb = new Tody();
    
  //if connection to database was good then execute query
  if(sqldb.OpenDb(sqlconnection).IsDbOkay())
      sqldb.ExecStoredProcedure(timeout, "getAllClientsByDepartment", arrParameters);  
      
  sqldb.CloseDb(); 
```

###Executing a stored procedure with two parameters

Executing a stored procedure named **getMovieByTypeYear** with two parameters **Category** and **Year**.
```cs
    //get connection string from webconfig
  sqlconnection = ConfigurationManager.ConnectionStrings["MyDataBase"].ToString();
  
   //declare an array of sql parameters
  SqlParameter[] arrParameters = { 
      new SqlParameter("@Category", "Comedy"),
      new SqlParameter("@Year", "2015") 
  };
  
  Tody sqldb = new Tody();
  
  //if connection to database was good then execute query
  if(sqldb.OpenDb(sqlconnection).IsDbOkay())  
     sqldb.ExecStoredProcedure(10, "getMovieByTypeYear", arrParameters);    
       
   sqldb.CloseDb();          
```
###Executing stored procedure & loop the resulting records.

Executing a stored procedure named **getMovieByTypeYear** with two parameters and then loop the resulting records.
```cs
    //get connection string from webconfig
  sqlconnection = ConfigurationManager.ConnectionStrings["MyDataBase"].ToString();
  
   //declare an array of sql parameters
  SqlParameter[] arrParameters = { 
      new SqlParameter("@Category", "Comedy"),
      new SqlParameter("@Year", "2015") 
  };
  
  Tody sqldb = new Tody();
  
  //if connection to database was good then execute query
  if(sqldb.OpenDb(sqlconnection).IsDbOkay())  
     sqldb.ExecStoredProcedure(10, "getMovieByTypeYear", arrParameters);    
  
  //if query executed with no errors
  if(sqldb.IsQuerySuccess())
     { 
          //get first data table
          DataTable movieTable = sqldb.GetFirstDataTable();
          
          //loop each row
          foreach(DataRow dr in movieTable.Rows)              
              Console.Write(dr["column_name"]);                        
    }   
   
   sqldb.CloseDb();            
  
```

## Installation

Download [Tody.dll](http://medinalex.github.io/Tody/Tody.dll) and then create the reference on the Visual Studio project.

## Methods

Methods      | Parameters    | Return        | Description
------------ | ------------- | ------------- | -------------
ExecQuery | int timeout, string query, SqlParameter[] parameters | | Use to execute query
ExecNonQuery | int timeout, string query, SqlParameter[] parameters | | Use to execute none query command
ExecStoredProcedure | int timeout, string name, SqlParameter[] parameters | | Use to execute a stored procedure
OpenDb() | | | Open database
CloseDb() | | | Close database
GetMessage() | | string | Get latest error message
GetDataSet() | | [DataSet](https://msdn.microsoft.com/en-us/library/system.data.dataset%28v=vs.110%29.aspx) | Return the dataset created after calling the methods ExecStoredProcedure and ExecQuery 
GetAllDataTables() | | [DataTableCollection](https://msdn.microsoft.com/en-us/library/system.data.datatablecollection%28v=vs.110%29.aspx) | Return the data table collection existing on the dataset created after calling the methods ExecStoredProcedure and ExecQuery 
IsQuerySuccess() | | bool | Verify is there was no error
IsDbOkay() | | bool | Verify if connection to server and database was a success
IsDbOpen() | | bool | Verify if database is open
GetDataTableByIndex() | int index | DataTable | Return a datatable created after calling the methods ExecStoredProcedure and ExecQuery but using index number of the table in case the stored procedure retun more than one.
GetFirstDataTable() |  | DataTable | Return the firts datatable created after calling the methods ExecStoredProcedure and ExecQuery.

