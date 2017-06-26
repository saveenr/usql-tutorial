# Text Encoding

By default all the extractors default to UTF-8 as the encoding.

All three built-in extractors allow you to control the encoding through the **encoding** parameter as shown below.

```
@trips =
EXTRACT
date DateTime,
driver_id int,
vehicle_id int,
trips string
FROM "/Samples/Data/AmbulanceData/DriverShiftTrips.csv"
USING Extractors.Csv( encoding: Encoding.[ASCII] );
```

These are the supported text encodings:

```
Encoding.[ASCII]
Encoding.BigEndianUnicode
Encoding.Unicode
Encoding.UTF7
Encoding.UTF8
Encoding.UTF32
```
