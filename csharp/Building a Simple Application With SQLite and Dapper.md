## Getting Started
All you need to get started is two packages: [Dapper](https://github.com/DapperLib/Dapper) and SQLite from [Microsoft.Data.Sqlite](https://learn.microsoft.com/en-us/dotnet/standard/data/sqlite/?tabs=net-cli).
To install those dependencies use the following commands
```
dotnet add package Dapper
dotnet add package Microsoft.Data.Sqlite
```
### Opening a SqliteConnection
To start a SQLite connection you need to create a SQLite database file and pass the path in a connection string.
```C#
string ConnectionString = "Data Source=./path/to/db";
```
Now you can take advantage of the `SqliteConnection` class from Microsoft's SQLite package to open a connection. Now create a new instance of the `SqliteConnection` class.
```C#
SqliteConnection connection = new(ConnectionString);
```
## Example Of Using SqliteConnection
```C#
using SqliteConnection connection = new(ConnectionString); // using so that the connection will close automatically
string sql = @"SELECT * FROM Accounts WHERE HolderID=@HolderID";
var results = await connection.QueryAsync<Account>(sql, new { HolderID = 12});
return [.. results];
```
- The first line opens the SqliteConnection with the `using` keyword so that the connection will close at the end of the code block without having to manually close it. 
- The second line creates the sql string to pass into the query. 
- The third line assigns the Enumerable returned from QueryAsync. Inside the QueryAsync method you pass: the sql statement as the first argument and an anonymous object as the second argument. The second argument is only necessary if we are passing a value to the sql statement. (In this case we are passing the HolderID).
- The fourth line returns the enumerable as a list. The short hand for converting an Enumerable to a List is `[.. enumerable]` but you could also do `enumerable.ToList()`.  