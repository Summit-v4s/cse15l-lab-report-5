**Lab Report 5**<br>
=====
The lab activitiy or report that I liked the most was week 4 Lab task, Lab report 2, and Lab report 3. <br>
During week 4 lab session, it introduced briefly about the new technology 'ChatGPT' by OpenAI. <br>
I have only heard about ChatGPTs by this time, not having to try it myself.
After trying out ChatGPT for a few minutes, I got impressed on how advanced AI language model technologies has become.<br> 
At this period of time, I was not familiar with using commands in terminal for opening web servers or searching through documents. By utilizing ChatGPT, I was able to understand better how commands such as 'find', 'grep', 'less' worked, being able to complete lab activity on time.

I also understood how different commands could be used to perform a search within the document.<br>
For example, in Lab report 2, I was able to learn how web servers could be initialized using programming language and being able to see web servers created by myself running on local host was very interesting, as I have always wondered how web pages that we interact were created.

After becoming more familiar with class materials and differenet kinds of command options, this time, I decided to try combining the activities for Lab Report 2 and 3 and push it further beyond, where I will initialize server with the data from docsearch, implementing different advanced functions to create a simple yet detailed search engine.<br>

I will implement the following functions for DocSearch:
>1. Case-insensitive search: Modifies the search to be case-insensitive by converting file content and the search query to lowercase.
>2. Regular expressions search: Uses regular expresions to perform pattern-based searches, looking for a certain pattern instead of specific word.
>3. Fuzzy search: Using Levenshtein distance, it will be able to find files containig words similar, not exact to the search query. I used the Apache Commons Text as a library.
>4. Pagination: Enhances the function to support pagination by returning only a specific number of results per page.

I have used the following prompt in ChatGPT (GPT-4) to learn about new ways of utilizing docsearch in web server:<br>
"For using the web server with docsearch, give me some examples of a more advanced techniques for searching through documents" <br>

With this prompt, I got suggested with different functions as mentioned above, and I was able to research them and modify the DocSearchServer file to implement case-insensitive search, regular expression search, fuzzy search, and pagination.<br>

Implementations and Examples
-------

### **1. Compile and Server start command:**
```
javac -cp ".;lib/commons-text-1.10.0.jar" *.java
java -cp ".;lib/commons-text-1.10.0.jar" DocSearchServer [####]
```
This command initializes the web server for docsearch. [####] is for port number.
* * *
### **2. Search for keyword "hello":**
```
http://localhost:8080/search?q=hello
```
This command searches directories containing the exact keyword "hello", which shows 2 path results. <br>
![](https://cdn.discordapp.com/attachments/890102969536753746/1087448157295755390/image.png)
* * *
### **3. Search for keyword "hello" with case-insensitive search:**
```
http://localhost:8080/search?q=hello&ci=true
```
This command adds case-insensitive search condition, showing 4 results instead of 2. <br>
![](https://cdn.discordapp.com/attachments/890102969536753746/1087276268623495229/image.png)
* * *

### **4. Search for keyword "aple" with fuzzy search:**
```
http://localhost:8080/search?q=aple&fuzzy=true&max_dist=1
```
Since "aple" is supposed to act as a typo of "apple", it is showing less than 40 results with regular search. <br>
![](https://cdn.discordapp.com/attachments/890102969536753746/1087450106032312380/image.png)<br>
<br>
When fuzzy search is enabled with max_dist=1, Lavenshtein distance measures how far the two strings are from being an exact match, showing many more results of 12 pages.<br>
However, this command will search for any words that are 1 string different than the keyword "aple", and this does not only apply to "apple".<br>
![](https://cdn.discordapp.com/attachments/890102969536753746/1087451211042664508/image.png)<br>
* * *
### **5. Search for pattern using regex:**
```
http://localhost:8080/search?q=app.e&regex=true
```
This example command searches for a keyword "app+[any single letter]+e". For example, it would look for "apple", "appee", "appnl", etc. In this case, it's matching for word "apple", although the keyword was not exactly "apple".<br>
![](https://cdn.discordapp.com/attachments/890102969536753746/1087512256234926160/image.png)
* * *
### **6. Go through different pages using pagination:**
```
http://localhost:8080/search?q=[keyword]&page=[#]
```
This command changes result pages from 1 (default) to any other number. In this example, search keyword was "apple", and page number is set to 2, which shows second page of the results.<br>
![](https://cdn.discordapp.com/attachments/890102969536753746/1087454368804110346/image.png)
* * *

### **7. Change the number of elements shown in a single page:**
```
http://localhost:8080/search?q=[keyword]&page=[#]&page_size=[#]
```
This command changes the number of elements shown in a single page by using the command "page_size=[number]". In this example, number of results for a single page has been increased to 15.<br>
![](https://cdn.discordapp.com/attachments/890102969536753746/1087456540279189544/image.png)

**Sources**
--------
ChatGPT Language model by OpenAI (GPT-4)<br>
Apache Commons Text Library 1.10.0 (https://commons.apache.org/proper/commons-text/)<br>
Regular Expression Syntax Cheat Sheet (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet)<br>
