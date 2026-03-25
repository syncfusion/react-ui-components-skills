# Connecting to Databases

## Table of Contents
- [Overview](#overview)
- [Database Support](#database-support)
- [Architecture Pattern](#architecture-pattern)
- [Web API Setup](#web-api-setup)
- [React Configuration](#react-configuration)
- [Database Connection Examples](#database-connection-examples)

## Overview

Connect PivotView to enterprise databases (SQL Server, MySQL, PostgreSQL, MongoDB, etc.) using a Web API intermediary. This pattern securely retrieves and serializes database records for pivot analysis.

## Database Support

The PivotView integrates with these databases via ASP.NET Core Web API:

| Database | Library | Nuget Package |
|----------|---------|---|
| **Microsoft SQL Server** | SqlClient | `System.Data.SqlClient` |
| **MySQL** | MySql.Data | `MySql.Data` |
| **PostgreSQL** | Npgsql | `Npgsql.EntityFrameworkCore.PostgreSQL` |
| **MongoDB** | MongoDB.Driver | `MongoDB.Driver`, `MongoDB.Bson` |
| **Oracle** | OracleClient | `Oracle.ManagedDataAccess.Core` |
| **Elasticsearch** | NEST | `Nest` |
| **Snowflake** | Snowflake.Data | `Snowflake.Data` |

## Architecture Pattern

### Step 1: ASP.NET Core Web API Service
Create a PivotController that:
1. Connects to the database using appropriate NuGet library
2. Executes SQL query to fetch data
3. Serializes result to JSON using Newtonsoft.Json
4. Returns JSON to React frontend

### Step 2: React Pivot Table Configuration
Configure the `url` property in `dataSourceSettings` to point to the Web API endpoint.

## Web API Setup

### Generic Controller Pattern

```csharp
using Microsoft.AspNetCore.Mvc;
using Newtonsoft.Json;
using System.Data;
using YourDatabaseLibrary;

namespace MyWebService.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class PivotController : ControllerBase
    {
        [HttpGet(Name = "GetDatabaseResult")]
        public object Get()
        {
            return JsonConvert.SerializeObject(FetchDatabaseData());
        }

        private static DataTable FetchDatabaseData()
        {
            // 1. Create database-specific connection
            var connection = new YourDatabaseConnection(
                "<Enter your valid connection string here>"
            );
            connection.Open();

            // 2. Execute query
            var command = new YourDatabaseCommand("SELECT * FROM TableName", connection);
            var dataAdapter = new YourDatabaseDataAdapter(command);

            // 3. Populate DataTable
            DataTable dataTable = new DataTable();
            dataAdapter.Fill(dataTable);
            connection.Close();

            return dataTable;
        }
    }
}
```

### Required NuGet Packages
- `Newtonsoft.Json` - JSON serialization
- Database-specific library (above table)
- `Microsoft.AspNetCore.Mvc` - ASP.NET Core framework

## React Configuration

### Step 1: Configure Web API URL

```typescript
import { PivotViewComponent, FieldList, Inject } from '@syncfusion/ej2-react-pivotview';

function App() {
  const dataSourceSettings = {
    url: 'https://localhost:7139/Pivot'  // Web API endpoint
  };

  return (
    <PivotViewComponent
      id='PivotView'
      height={350}
      dataSourceSettings={dataSourceSettings}
      showFieldList={true}
    >
      <Inject services={[FieldList]} />
    </PivotViewComponent>
  );
}
```

### Step 2: Define Report Structure

```typescript
const dataSourceSettings = {
  url: 'https://localhost:7139/Pivot',
  expandAll: false,
  enableSorting: true,
  columns: [{ name: 'Product' }],
  rows: [
    { name: 'Country' },
    { name: 'State' }
  ],
  values: [
    { name: 'Quantity' },
    { name: 'Amount', caption: 'Sold Amount' }
  ],
  filters: [],
  formatSettings: [
    { name: 'Amount', format: 'C0' }
  ]
};
```

## Database Connection Examples

### Microsoft SQL Server

```csharp
private static DataTable FetchSQLResult()
{
    string connStr = @"Server=localhost;Database=MyDb;User Id=sa;Password=YourPassword;";
    SqlConnection connection = new SqlConnection(connStr);
    connection.Open();
    SqlCommand cmd = new SqlCommand("SELECT * FROM Orders", connection);
    SqlDataAdapter adapter = new SqlDataAdapter(cmd);
    DataTable dt = new DataTable();
    adapter.Fill(dt);
    connection.Close();
    return dt;
}
```

**Port**: Default 1433

### MySQL

```csharp
private static DataTable FetchMySQLResult()
{
    string connStr = "Server=localhost;Database=mydb;Uid=myuser;Pwd=password;";
    MySqlConnection connection = new MySqlConnection(connStr);
    connection.Open();
    MySqlCommand cmd = new MySqlCommand("SELECT * FROM orders", connection);
    MySqlDataAdapter adapter = new MySqlDataAdapter(cmd);
    DataTable dt = new DataTable();
    adapter.Fill(dt);
    connection.Close();
    return dt;
}
```

**Port**: Default 3306

### PostgreSQL

```csharp
private static DataTable FetchPostgreSQLResult()
{
    string connStr = "Server=localhost;Database=mydb;User Id=myuser;Password=password;";
    NpgsqlConnection connection = new NpgsqlConnection(connStr);
    connection.Open();
    NpgsqlCommand cmd = new NpgsqlCommand("SELECT * FROM tablename", connection);
    NpgsqlDataAdapter adapter = new NpgsqlDataAdapter(cmd);
    DataTable dt = new DataTable();
    adapter.Fill(dt);
    connection.Close();
    return dt;
}
```

**Port**: Default 5432

### MongoDB

```csharp
private static List<BsonDocument> FetchMongoDbResult()
{
    string connStr = "mongodb://localhost:27017";
    MongoClient client = new MongoClient(connStr);
    IMongoDatabase db = client.GetDatabase("sample_training");
    var collection = db.GetCollection<BsonDocument>("ProductDetails");
    return collection.Find(new BsonDocument()).ToList();
}
```

**Port**: Default 27017

### Oracle

```csharp
private static DataTable FetchOracleResult()
{
    string connStr = "Data Source=localhost;User Id=myuser;Password=password;";
    OracleConnection connection = new OracleConnection(connStr);
    connection.Open();
    OracleCommand cmd = new OracleCommand("SELECT * FROM EMPLOYEES", connection);
    OracleDataAdapter adapter = new OracleDataAdapter(cmd);
    DataTable dt = new DataTable();
    adapter.Fill(dt);
    connection.Close();
    return dt;
}
```

**Port**: Default 1521

### Elasticsearch

```csharp
private static object FetchElasticsearchData()
{
    var uri = new Uri("http://localhost:9200");
    var settings = new ConnectionSettings(uri);
    var client = new ElasticClient(settings);
    var response = client.Search<object>(s => s
        .Index("product")
        .Size(1000)
    );
    return response.Documents;
}
```

**Port**: Default 9200

### Snowflake

```csharp
private static DataTable FetchSnowflakeResult()
{
    using (var connection = new SnowflakeDbConnection())
    {
        connection.ConnectionString = 
            "account=myaccount;user=myuser;password=password;db=mydb;schema=public;";
        connection.Open();
        var adapter = new SnowflakeDbDataAdapter("SELECT * FROM MY_TABLE", connection);
        DataTable dt = new DataTable();
        adapter.Fill(dt);
        return dt;
    }
}
```

## Connection String Anatomy

**Format**: `Server=host;Port=port;Database=dbname;User Id=user;Password=pwd;`

**Common Parameters**:
- `Server`/`Host`: Database server address
- `Port`: Port number (varies by database)
- `Database`: Database name
- `User Id`/`Uid`: Username
- `Password`/`Pwd`: Password

## Testing the Connection

1. Run Web API application
2. Access endpoint in browser: `https://localhost:PORT/Pivot`
3. Verify JSON data displays correctly
4. Update React `url` to match running port

## Error Handling

Add error handling to PivotController:

```csharp
[HttpGet]
public object Get()
{
    try
    {
        return JsonConvert.SerializeObject(FetchDatabaseData());
    }
    catch (Exception ex)
    {
        return new { error = ex.Message };
    }
}
```

## CORS Configuration

Enable CORS in Startup.cs for React frontend:

```csharp
services.AddCors(options =>
{
    options.AddPolicy("AllowReact",
        builder => builder.AllowAnyOrigin().AllowAnyMethod().AllowAnyHeader()
    );
});

app.UseCors("AllowReact");
```
