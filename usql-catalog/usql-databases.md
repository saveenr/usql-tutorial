# Creating a database

To create a database is simple.

```
CREATE DATABASE MyDB;
```

This command will fail if the database already exists. Often, it will be the case that you want to create a database only if it does not exist. In this case the following command is used.

```
CREATE DATABASE IF NOT EXISTS MyDB;
```


## Deleting a Database


```
DROP DATABASE MyDB;
```

DROP DATABASE IF EXISTS MyDB;
