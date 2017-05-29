Launch Visual Studio. Go to **File &gt; New &gt; Project &gt; Installed &gt; Templates &gt; Azure Data Lake &gt; U-SQL \(ADLA\)**

At this point you should notice that an empty U-SQL script has been created called "Script.usql".

![](/assets/empty_usql_script_buttons.png)

The **Submit** button will run the script. The dropdown next to the **Submit** button says `(Local)`. This means that Submit will run the script in Local Execution mode. In future chapters, we'll pick an Azure Data Lake Analytics account and run the script in Cloud Execution mode - but for now we will continue to locally execute all our U-SQL script.

You can ignore the dropdowns that say `master` and `dbo`. They will not impact the tutorial in any way.

Make sure the Solution Explorer is visible. If it isn't go to **View &gt; Solution Explorer**.

![](/assets/solution_explorer_new_usql_proj.png)

The solution contains single U-SQL project called `USQLApplicationX`. Inside that project is the empty `Script.usql`file.

Notice that you can expand the .usql file. It contains C\# file. This is called the code-behind file. You will find this code-behind file very useful in later chapters.

