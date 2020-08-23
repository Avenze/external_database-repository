# external_database-repository
this is a project, that allows you to host user data from roblox, on a remote database, or in this case, an external offsite database, currently, this project runs on SQLite3, however, I'm working on implementing support for MariaDB instances.

### Setting up your project
you're able to host this project on multiple services, I personally use a raspberry pi, with an offsite database instance, if you're hosting this on a Raspberry Pi, you'll need to transfer the [node-js](https://github.com/Avenze/external_database-repository/tree/master/node-js) directory into the /home/pi/ directory on the Raspberry Pi.

## Configuring the project
inside of the server.js file, you will see several variables above a comment, do not modify anything below unless you know what you're doing.
Configurations settings:

- **databaseFile**: this is the file where your SQLite3 data is stored. Changing this will reset all data.
- **tables**: this is the tables that you want. Tables allow you to seperate categories of information.
- **ApiToken**: this is the token you will use to access the database. I highly recommend using a password generating and storing the password in your .env file
- **tableKeyLength**: ignore, doesn't do anything.
- **tableValueLength**: ignore, doesn't do anything.
- **getAsyncAllowStar**: allow you to get all data from a table by specifying the key as "\*"; If a key is a "\*" you will not be able to access it with this option on.

# Roblox Api
the roblox API is located in the [/roblox](https://github.com/Avenze/external_database-repository/tree/master/roblox) subdirectory in this repo.
copy the source of sql.lua into a new ModuleScript in roblox studio, after doing so, you can require the module and it'll return a few configuration options.

- **ApiUrl**: set this to either your public IP adress, if you're hosting it on a Raspberry Pi, and the port. EX: IP:PORT, 12.345.678.90:1234
- **Token**: set this to the ApiToken you specified in the server.js file.

###### Roblox Module Callback
if Success: Callback(boolean Success(true), table ServerResponse)

if Failure: Callback(boolean Success(false), string errorMessage, table ServerResponse)

##### ServerResponse table
{

  Success: boolean,
  
  (if not success) Message: string Error Message,
  
  (postAsync) Changes: int
  
  (deleteAsync) KeyDeleted: boolean
  
  (getAsync) ValueExists: boolean
  
  (getAsync) Value: string or null
  
}

## Documentation

##### sql:GetAsync(string Table, string Key, function Callback)
Get a value from the database.

Example: sql:GetAsync("table1", "ROBLOX", function(Success, Value, Response) if (Success) then (Value is the value from key or null if it doesn't exist) end end)

##### sql:PostAsync(string Table, string Key, string Value \[, function Callback]);
This sets (or creates) a value in the database.

Example: sql:PostAsync("table1", "ROBLOX", 'Some Information About Roblox');

##### sql:DeleteAsync(string Table, string Key \[, function Callback]);
This deletes a value in the database.

Example: sql:DeleteAsync("table1", "ROBLOX")
