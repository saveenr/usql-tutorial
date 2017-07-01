# Complex Types

## Nullability

Complex types are reference types. You MUST check if they are null first before using them.

## Maximum size of a complex value

There is no specific limit inherent to Complex Types

However, rowsets must still fit within the limits defined by U-SQL (currently no row can be bigger than 4MB)

## Persistence

#### U-SQL Tables
Complex Types can be stored in and read from U-SQL Tables without any additional effort.

#### Files
To read/write Complex Types with files, you must use your own Custom Outputter & Extractor.
