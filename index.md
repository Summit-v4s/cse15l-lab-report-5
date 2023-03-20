**Lab Report 5**<br>
**Seungki Kim**<br>

The lab activitiy or report that I liked the most was week 4 Lab task, Lab report 2, and Lab report 3. <br>
During week 4 lab session, it introduced briefly about the new technology 'ChatGPT' by OpenAI. <br>
I have only heard about ChatGPTs by this time, not having to try it myself.
After trying out ChatGPT for a few minutes, I immediately realised how advanced AI language model technologies has become.<br> 
At this period of time, I was not familiar with using commands for searching through documents. By utilizing ChatGPT, I was able to go understand better how commands such as 'find', 'grep', 'less' worked, being able to complete lab activity on time.

I also understood how different commands could be used to perform a search within the document.<br>
For Lab report 2, I was able to understand how web servers could be initialized using programming language and being able to see web servers created by myself running on local host was very interesting, as I have always wondered how web pages were created in technological level.

After becoming more familiar with class materials and differenet kinds of command options, this time, I decided to try combining the activities for Lab Report 2 and 3, where I will initialize server with the data from docsearch, using different functions to perform more advanced searching.<br>

I will implement the following functions for DocSearch:
1. case-insensitive search: Modifies the search to be case-insensitive by converting file content and the search query to lowercase.
2. Regular expressions search: Uses regular expresions to perform pattern-based searches, looking for a certain pattern instead of specific word.
3. Fuzzy search: Using Levenshtein distance, it will be able to find files containig words similar, not exact to the search query. I used the Apache Commons Text as a library.
4. Pagination search: Enhances the function to support pagination by returning only a specific number of results per page.

I have used the following prompt in ChatGPT (GPT-4) to learn about new ways of utilizing docsearch in web server:<br>
"For using the web server with docsearch, give me more advanced techniques for searching elements within the files" <br>

With this prompt, I was able to modify the DocSearchServer file to implement case-insensitive search, regular expression search, and fuzzy search (using the Levenshtein distance).<br>


1. Basic server start command:
```
javac Server.java DocSearchServer.java
java DocSearchServer [####]
```
This command initializes the web server for docsearch.

2. Search for keyword "example":
```
http://localhost:8080/search?q=example
```
This command searches directories containing the keyword "example".
