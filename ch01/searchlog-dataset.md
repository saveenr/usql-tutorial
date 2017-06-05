# The SearchLog sample datset

You previously downloaded the SearchLog data set from this location:

[https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/SearchLog.tsv](https://www.gitbook.com/book/saveenr/usql-tutorial/edit#)

The SearchLog is a small dataset that represents "sessions" that a user has when performing a search on a web search engine. Each row represents a session for a specific search query,

The `SearchLog.tsv` file doesn't contain a header row so we'll have to document the columns below:

* **UserId** - this is an integer representing an anonymized user
* **Start** - when started a session with the search engine
* **Region** - What geographical region the user is searching from
* **Query** - What the user searched for
* **Duration** - How long their search session lasted
* **Urls** - A semicolon-separated list All the URLs that were shown to the user in the session
* **ClickedUrls** - A subset of **Urls** that the user actually clicked on \(also a semicolon-separated list\)



