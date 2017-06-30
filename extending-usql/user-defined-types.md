# User-Defined Types

## Concepts

In order to create a UDT in U-SQL there are two thing that have to be created:
* The UDT type itself
* A formatter that can conver the UDT into a string and can transform a string into an instance of the UDT

# Scenario

We will define a UDT - and its corresponding formatter - for a "Bits" UDT. This UDFT is a simple bitarray with a textual representation that looks like "1100101010".

# UDT and formatter skeletons

First let's look at the fundamental structure of the Bits type. 

```
[SqlUserDefinedType(typeof(BitFormatter))]
public struct Bits
{
}
```

Notice that the only thing the Bits UDT requires is that it identifies its corresponding formatter.

And here is the skeleton of its corresponding formatter.

```
public class BitFormatter : Microsoft.Analytics.Interfaces.IFormatter<Bits>
{
    public BitFormatter()
    { ... }

    public void Serialize(
        Bits instance,
        IColumnWriter writer,
        ISerializationContext context)
    { ... }

    public Bits Deserialize(
         IColumnReader reader,
         ISerializationContext context)
    { ... }
}
```

# Full code for the UDT 

```

namespace MyUDTExamples
{
    [SqlUserDefinedType(typeof(BitFormatter))]
    public struct Bits
    {
        System.Collections.BitArray bitarray;
    
        public Bits(string s)
        {
            this.bitarray = new System.Collections.BitArray(s.Length);
            for (int i = 0; i<s.Length; i++)
            {
                this.bitarray[i] = (s[s.Length-i-1] == '1' ? true : false);
            }
        }
    
        public int ToInteger()
        {
            int value = 0;
            for (int i = 0; i < this.bitarray.Length; i++)
                { if (bitarray[i]) { value += (int)System.Math.Pow(2, i); } }
            return value;
        }
    
        public override string ToString()
        {
            var sb = new System.Text.StringBuilder(this.bitarray.Length);
            for (int i = 0; i < this.bitarray.Length; i++)
            { sb.Append(this.bitarray[i] ? "1" : "0"); }
            return sb.ToString();
        }
    }

}
```

# Full code for the UDT's formatter

```
namespace MyUDTExamples
{

    public class BitFormatter : Microsoft.Analytics.Interfaces.IFormatter<Bits>
    {
        public BitFormatter()
        {
        }
    
        public void Serialize(
            Bits instance,
            IColumnWriter writer,
            ISerializationContext context)
        {
            using (var w = new System.IO.StreamWriter(writer.BaseStream))
            {
                var bitstring = instance.ToString();
                w.Write(bitstring);
                w.Flush();
            }
        }
    
        public Bits Deserialize(
            IColumnReader reader,
            ISerializationContext context)
        {
            using (var w = new System.IO.StreamReader(reader.BaseStream))
            {
                string bitstring = w.ReadToEnd();
                var bits = new Bits(bitstring);
                return bits;
            }
        }
    }

}
```

# Using the UDT

```
@products  = 
    SELECT * FROM 
        (VALUES
            ("Apple", "0000"),
            ("Cherry", "0001"),
            ("Banana", "1001"),
            ("Orange", "0110")
        ) AS 
              D( bitstring );

@products = 
    SELECT 
       ProductCode
       BitString, 
       new MyUDTExamples.Bits(BitString) AS Bits
    FROM @products;
```

