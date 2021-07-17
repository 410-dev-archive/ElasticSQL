# ElasticSQL

## Freedom from columns in SQL

All data is stored as JSON format.

### Required Setup - Database

MySQL / MariaDB

Table format:

id                          | data          | indexes
(Unique, INT)        | (TEXT)       | (TEXT)



#### Classes included

- ElasticSQLController
- ElasticSQLException
- ElasticSQLResultSet
- SQLController



## ElasticSQLController

### Declaring Controller

For Localhost MariaDB:
```ElasticSQLController esqlctl = new ElasticSQLController((String)databaseName, (String)tableName, (String)username, (String)password);```

For others databases:
```ElasticSQLController esqlctl = new ElasticSQLController((String)protocol, (String)address, (String)port, (String)databaseName, (String)tableName, (String)username, (String)password, (String)driverName);```

### Methods

#### Add Data - Add a data to the row

```(void) addData(String selectionRowID, String newKey, String newData)``` -> Add data to the row that is specified as RowID.

```(void) addData(String selectionKey, String selectionData, String newKey, String newData)``` -> Add data to the row that has the specified data matching with the specified selection key.

```(void) addData(String selectionKey, String selectionData, String newKey, String newData, int limit)``` -> Same as above but with number of row limits.

#### Pop Data - Remove a data from the row

```(void) popData(String selectionID, String keyToPop)``` -> Remove the key and the data of the key of the row that is selected with ID.

```(void) popData(String selectionKey, String selectionData, String keyToPop)``` -> Remove the key and the data of the key of the row that contains specified key and the data.

```(void) popData(String selectionKey, String selectionData, String keyToPop, int limit)``` -> Same as above, but with number of row limits.

#### Delete - Remove a row

```(void) delete(String selectionKey, String selectionData)``` -> Delete all the rows that has the specified data with matching key.

```(void) delete(String selectionKey, String selectionData, int limit)``` -> Same as above, but with number of row limits.

```(void) delete(String selectionID)``` -> Delete a row that has the specified ID.

#### Insert - Add a row

```(void) insert(String[] keys, String[] data)``` -> Add a new row with the specified keys and data.

#### Select - Select specific row / rows

```(ElasticSQLResultSet) select()``` -> Load all rows from SQL.

```(ElasticSQLResultSet) select(int limit)``` -> Load all rows with number of limits from SQL.

```(ElasticSQLResultSet) select(String key, String data)``` -> Load all rows that have the specified key and data.

```(ElasticSQLResultSet) select(String key, String data, int limit)``` -> Same as above, but with number of row limits.

```(ElasticSQLResultSet) selectAt(String selectionID)``` -> Load only one row that has the specified ID.

```(ElasticSQLResultSet) selectSimilar(String key, String data)``` -> Load all rows that contains the string inside the data of the key.

```(ElasticSQLResultSet) selectSimilar(String key, String data, int limit)```-> Same as above, but with number of row limits.

```(ElasticSQLResultSet) selectWhereKey(String key)``` -> Load all rows that contains the specified key.

```(ElasticSQLResultSet) selectWhereKey(String key, int limit)```-> Same as above, but with number of row limits.

#### Update - Change data inside a row / rows

``` (void) update(String selectionKey, String selectionValue, String updateKey, String newValue)``` -> Update the data of the key of rows that contains the specified data of the key.

```(void) update(String selectionKey, String selectionValue, String updateKey, String newValue, int limit)```-> Same as above, but with number of row limits.



## ElasticSQLResultSet

### Declaring ElasticSQLResultSet

Convert from java.sql.ResultSet:
```ElasticSQLResultSet ers = new ElasticSQLResultSet((ResultSet) resultSet);```

Create from ArrayList:
```ElasticSQLResultSet ers = new ElasticSQLResultSet((ArrayList<String>) id, (ArrayList<String>) data, (ArrayList<String>) index);```



### Methods

#### Select - Select one row from filtered rows

```(boolean) absolute(int index)```-> Go to the specific index of array. Returns true if successful.

```(boolean) select(int index)```-> Same as above.

```(boolean) last()```-> Go to the last index of array.

```(boolean) first()```-> Go to the first index of array.

```(boolean) next()```-> Go to the next index of array.

```(boolean) previous()```-> Go to the previous index of array.

#### Get Data

```(int) getCurrentlySelectedIndex()```-> Returns the current index number of array.

```(int) length()```-> Returns the size of the array.

```(String) getEntireData()```-> Returns the JSON data from current index of array.

```(String) getStringData(String key)```-> Returns the value of the key from current index of array.

```(String) getID()```-> Returns the ID of current index of array.

```(String) getIndex()```-> Returns the index of keys the row contains.

```(boolean) hasKey(String keyToVerify)```-> Returns true if the row contains the key.

```(String) toString()```-> Returns in a string form.

## Usage Example (All exceptions are thrown)

```Insert Row
// Declare new ElasticSQLController
ElasticSQLController esqlctl = new ElasticSQLController("companyX", "users", "admin", "adminpassword");

// Add new row (in this context, a user) to companyX.users
String[] keys = {"name", "age", "email"};
String[] data = {"Alex", "30", "alex.compx@companyx.com"};
esqlctl.insert(keys, data);
```

```Update
// Declare new ElasticSQLController
ElasticSQLController esqlctl = new ElasticSQLController("companyX", "users", "admin", "adminpassword");

// Change age of Alex to 31
esqlctl.update("name", "Alex", "age", "31");

// Add age 1 to Alex
ElasticSQLResultSet ers = esqlctl.select("name", "Alex", 1); // Select only one
ers.next();
esqlctl.update("name", "Alex", "age", Integer.toString(ers.getStringData("age") + 1)); // Get data from DB, then add 1 to the age
```

```Remove Row
// Declare new ElasticSQLController
ElasticSQLController esqlctl = new ElasticSQLController("companyX", "users", "admin", "adminpassword");

// Remove Alex from database
esqlctl.delete("name", "Alex", 1);
```

```Add data
// Declare new ElasticSQLController
ElasticSQLController esqlctl = new ElasticSQLController("companyX", "users", "admin", "adminpassword");

// Add new key to Alex - Birthday
esqlctl.addData("name", "Alex", "birthday", "January 1, 2021", 1);
```

```Pop data
// Declare new ElasticSQLController
ElasticSQLController esqlctl = new ElasticSQLController("companyX", "users", "admin", "adminpassword");

// Pop key from Alex - Birthday
esqlctl.popData("name", "Alex", "birthday");
```

