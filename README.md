##### Current repo has 4 jar files which are compiled from my following MapReduce project repositories:
<ol>
<li> MapReduce_WordCount </li>
<li> MapReduce_TotalWordsCount </li>
<li> MapReduce_FirstLetterCount </li>
<li> MapReduce_WordLengthCount </li>
</ol>

--------------------------------------------------------------------------------------------------------------

The following steps show how to use WordCount_2.jar to count the words by groups from text file which will be stored in HDFS.

--------------------------------------------------------------------------------------------------------------
Note: </br>
This example is demonstrated on `hduser_1` username account on my local Linux machine where my Hadoop is installed. </br>

1 - 
Before running jar file in Hadoop, we should create and move input text file into HDFS. </br>
For that, you should first switch to `hduser_1` user in terminal and create HDFS home directory `hdfsHomeDir`: </br>

	hadoop fs -mkdir -p /user/hdfsHomeDir

2 - 
Then run the following command to create `textFile.txt` file in `hduser_1` user which you may find it in `/home/hduser_1`: </br>

	cat>textFile.txt

Type something and then press CTRL+d to save it. </br>


3 - 
Copy the file to HDFS directory: </br>

	hadoop fs -copyFromLocal /home/hduser_1/textFile.txt /user/hdfsHomeDir/textFile.txt

and to check if your `textFile.txt` is copied to hdfs directory: </br>

	hadoop fs -ls /user/hdfsHomeDir

and if you want to delete that text file by any reason: </br>

	hdfs dfs -rm -r hdfs:/user/hdfsHomeDir/textFile.txt

4 - 
Then, you can paste `WordCount_2.jar` file into Desktop folder of `hduser_1` and run it on `textFile.txt`: </br>

	hadoop jar /home/hduser_1/Desktop/WordCount_2.jar /user/hdfsHomeDir/textFile.txt WordCountOutput

job will create 'WordCountOutput' folder in 'user' and will put the results there. </br>

Note: Your jar may fail with such a following error: </br>

	Exception in thread "main" java.io.IOException: Error opening job jar: /home/hduser_1/wordcount.jar
		at org.apache.hadoop.util.RunJar.run(RunJar.java:160)
		at org.apache.hadoop.util.RunJar.main(RunJar.java:136)
	Caused by: java.io.FileNotFoundException: /home/hduser_1/wordcount.jar (Permission denied)
		at java.util.zip.ZipFile.open(Native Method)
		at java.util.zip.ZipFile.<init>(ZipFile.java:215)
		at java.util.zip.ZipFile.<init>(ZipFile.java:145)
		at java.util.jar.JarFile.<init>(JarFile.java:154)
		at java.util.jar.JarFile.<init>(JarFile.java:91)
		at org.apache.hadoop.util.RunJar.run(RunJar.java:158)

That may be becuase jar file has default `rw-r--r--` permission, and you can try to change it to `rwx-rwx-rwx` by: </br>

	chmod 777 /home/hduser_1/Desktop/WordCount_2.jar

and then run your jar for text file again. </br>
e.g.  </br>

	hadoop jar /home/altay/Desktop/WordCount_2.jar /user/hdfsHomeDir WordCountOutput


5 - 
After job is done, we can look up if there is any result file in 'WordCountOutput' folder: </br>

	hadoop fs -ls /user/hduser_1/WordCountOutput

you should see something similar to the following: </br>

	15/10/08 20:03:27 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
	Found 2 items
	-rw-r--r--   1 hduser_1 supergroup          0 2015-10-08 19:57 /user/hduser_1/WordCountOutput/_SUCCESS
	-rw-r--r--   1 hduser_1 supergroup         31 2015-10-08 19:57 /user/hduser_1/WordCountOutput/part-r-00000

it means you have 'part-r-00000' as output result file.


6 -
To look up the results of your job saved in that file: </br>

	hadoop fs -cat /user/hduser_1/WordCountOutput/part-r-00000

7 - 
To delete output folder or file: </br>

	hdfs dfs -rm -r /user/hduser_1/WordCountOutput
