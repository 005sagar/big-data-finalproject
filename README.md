# Big-Data-FinalProject
## About
In this project, I are trying to tell a story with data. I will first download a text file and then process data using spark and bash command only.

### Pre-requisites and references :
1. Spark to be configured - [Click here for repo](https://github.com/denisecase/setup-spark). 
2. Basic coding knowledge of Spark.[Click here for help](https://spark.apache.org/)
3. Basic Powershell and bash command.[Click here for help](https://devblogs.microsoft.com/scripting/use-a-powershell-cmdlet-to-count-files-words-and-lines/)
4. Basic Markdown Syntax [Click here for help](https://www.markdownguide.org/basic-syntax/)

### Getting data and clean data:
Find a intersteing article/play that you need to do the procesing. I choose [seven] movie script.(https://www.dailyscript.com/scripts/seven_unknown.html) 
1. Then to get the HTML to a text file use curl commands ```$  curl "https://www.dailyscript.com/scripts/seven_unknown.html" -o "sevenoriginal.txt"```
2. I didn't have the html tags, but usually these contains html tags. So, if you need to remove html tag use sed command ``` sed -E 's/<[^>]*>/''/g' sevenorignal.txt > seven.txt ```
3. To clean your data more use this command ```sed -E 's/\b(is|IS|am|AM|are|are|he|He|She|she|them|Them|was|Was|Were|i|I|for|For|you|You|her|Her|him|Him|but|But|So|so|Will|will|Would|would|his|do|Do|it|It|my|My|on|as|at|in|to|me|with|of|that|those|a|up|To|may|In|And|and|if|If|no|No|yes|Yes|all|All|Is|A|be|or|Or|now|Now|we|WE|The|the|what|What|When|when|Where|where)\b/''/g' sevenoriginal.txt > seven.txt ```

### Process data
I am going to use Spark for the processing data.

1. On your project right click and run Open powershell window here as administrator and use command ```spark-shell``` to bring up spark with scala.
2. Now we need to create a Resilient Distributed Dataset (RDD) from the text file which we have created using ```val rddSeven = sc.textFile("seven.txt")``` command.It will create RDD named rddSeven
3. After creating, we need to map the words and then apply aggregation using reduceByKey```val rddSevensplit = rddSeven.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey((a, b) => a + b)```
4. We can collect all the dataset using ```rddSevensplit.collect()```. 
5. We can sort the result acoording to our need. I did it in descending order using ``` val rddsorted = rddSevensplit.sortBy(_._2, false)```
6. Then we can take the needed word and do what we need to. I just took the top 15 result using ```rddsorted.take(15)```
7. We can save the result using ```rddsorted.saveAsTextFile("Top10words")``` command
8. We will get a folder named "Top10words" inside there, open partfile and paste the top 10 results in excel.

  

