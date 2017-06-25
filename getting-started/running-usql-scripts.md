# Running U-SQL scripts

There are two ways to run U-SQL scripts:

* You can run U-SQL scripts on your own machine. The data read and written by this script will be on you own machine. You aren't using Azure resources to do this so there is no additional cost. This method of running U-SQL scripts is called **"U-SQL Local Execution"**
* You can run U-SQL scripts in Azure in the context of a Data Lake Analytics account. The data read or written by the script will also be in Azure - typically in an Azure Data Lake Store account. You pay for any compute and storage used by the script. This is called **"U-SQL Cloud Execution"**

For most of the tutorial we will work with **U-SQL Local Execution** because we want you to learn the language first. Later on we can explore the very rich topic that is **U-SQL Cloud Execution**.

