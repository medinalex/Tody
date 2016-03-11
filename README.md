**Tody** is a C# class library to connect to a SQL database and execute stored procedures and/or other sql queries.

## Usage
###Example One

Executing a sql query with no parameter.
```cs

  //get connection string from webconfig
  sqlconnection = ConfigurationManager.ConnectionStrings["MyDataBase"].ToString()

  string query = "Select top 10 * from tblPets";
  
  //timeout value in seconds
  int timeout=15;
  
  //Declare Tody object
  Tody sqldb = new Tody(sqlconnection);
  
  //open database
  sqldb.OpenDb();
  
  //if connection to database was good then execute query. 
  if(sqldb.IsDbOkay())  
     sqldb.ExecQuery(timeout, query);  
     
   sqldb.CloseDb();

```
###Example Two

Executing a stored procedure **getAllDepartments** its has no parameter.
```cs

  //get connection string from webconfig
  sqlconnection = ConfigurationManager.ConnectionStrings["MyDataBase"].ToString()

   // stored procedure name
  string stored_procedure_name = "getAllDepartments";
  
  //Declare Tody object
  Tody sqldb = new Tody();
    
  //open database
  sqldb.OpenDb(sqlconnection);
  
  //if connection to database was good then execute stored procedure. 
  if(sqldb.IsDbOkay())  
      sqldb.ExecStoredProcedure(stored_procedure_name);
      
  sqldb.CloseDb();

```
###Example Three

Executing a stored procedure named **getAllClientsByDepartment** with a parameter named **Department**.
```cs

  //declare string value for the parameter
  string strDepartment = "Accounting"; 
  
  int timeout=30;
  
  //declare an array of sql parameters
  SqlParameter[] arrParameters = { new SqlParameter("@department", strDepartment) };
  
  //Declare Tody object
  Tody sqldb = new Tody(ConfigurationManager.ConnectionStrings["MyDataBase"].ToString());
  
  //open database
  sqldb.OpenDb();
  
  //if connection to database was good then execute stored procedure. 
  if(sqldb.IsDbOkay())  
      sqldb.ExecStoredProcedure(timeout, "getAllClientsByDepartment", arrParameters);  
      
  sqldb.CloseDb();  

```

###Example Four

Executing a stored procedure named **getMovieByTypeYear** with two parameters **Category** and **Year**. Then loop the resulting records.
```cs
  
   //declare an array of sql parameters
  SqlParameter[] arrParameters = { 
      new SqlParameter("@Category", "Comedy"),
      new SqlParameter("@Year", "2015") 
  };
  
  //Declare Tody object
  Tody sqldb = new Tody(ConfigurationManager.ConnectionStrings["MyDataBase"].ToString());
  
  //open database
  sqldb.OpenDb();
  
  if(sqldb.IsDbOkay())
  {  
      movie = sqldb.ExecStoredProcedure(10, "getMovieByTypeYear", arrParameters);   
     
       if(db.IsQuerySuccess())
        { 
          //get data set
          DataSet movie = db.GetDataSet();
          
          //loop each row
          foreach(DataRow dr in movie.Tables[0].Rows)              
              Console.Write(" Movie "+ dr["movie_name_column"]);                        
        }
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
GetDataTables() | | [DataTableCollection](https://msdn.microsoft.com/en-us/library/system.data.datatablecollection%28v=vs.110%29.aspx) | Return the data table collection existing on the dataset created after calling the methods ExecStoredProcedure and ExecQuery 
IsQuerySuccess() | | bool | Verify is there was no error
IsDbOkay() | | bool | Verify if connection to server and database was a success
IsDbOpen() | | bool | Verify if database is open

