## Testing for Membership with the IN Operator {#testing-for-membership-with-the-in-operator}

You can use the**IN**operator as shown below to test for membership in a set of values.

```
@rs =
 SELECT FirstName, LastName, JobTitle
 FROM People
 WHERE JobTitle IN ("Design Engineer", "Tool Designer", "Marketing Assistant");

```

Keep in mind that the U-SQL**IN**operator does not offer all the features of the SQL**IN**operator.

