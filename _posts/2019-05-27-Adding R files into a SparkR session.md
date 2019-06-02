---
title:  "Adding R files into a SparkR session"
date:   2019-01-18 06:48:00
categories: [data]
tags: [R,Spark,SparkR,Zeppelin, HDFS]
---

Is not so easy to find examples with SparkR when Spark (Scala) and PySPark are leading between communities. I'm trying to bring an example about how to load and R file into Spark.

This is helpul when you are not using RStudio as interface, or you don't have any access to your remote machine to upload files and then "source" into the enviorment. In this case, I was using Zeppelin, a Web-based notebook.

First, create a simple R file with a basic function and then store it in HDFS. This function has no arguments and prints "Hello, World" as output.

```r
function() 
{
    print(“Hello, World”)
}
```

In your notebook, check if you already have a spark session available ("spark"), then use the "addFile" method to a specific path in HDFS.


```r
spark.addFile("hdfs:///user/username/hello.R", recursive = FALSE)
```

Check where the file will be stored
```r
spark.getSparkFilesRootDirectory()

[1] "/tmp/spark-ccb98cb6-ccb9-4b30-97dc-6765693e17e9/userFiles-57e5fd3d-6c67-4fe2-bf65-0c4110a88d25"

```
If load add more then 1 file, you can check the aboslute path of each one by using "getSparkFiles", in this case we are looking for "hello.R" file.

```r

spark.getSparkFiles("hello.R")

[1] "/tmp/spark-ccb98cb6-ccb9-4b30-97dc-6765693e17e9/userFiles-57e5fd3d-6c67-4fe2-bf65-0c4110a88d25/hello.R"

```

Use the source function to execute you file. The content is only a function that will be available in global environment.

```r
source(spark.getSparkFiles("hello.R"))
```

```r
hello

function () 
{
    print(“Hello, World”)
}
```

Execute the function, and check the output.

```r
hello()

[1] "Hello, World"

```

That's it, you can centralized you code and add it into a spark session.

## References

SparkR - AddFile [https://spark.apache.org/docs/latest/api/R/spark.addFile.html]