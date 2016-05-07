#### Ready-to-use JAR files which are compiled from my project repositories:
<ol>
<li> 1 - MapReduce_WordCount </li>
<li> 2 - MapReduce_TotalWordsCount </li>
<li> 3 - MapReduce_FirstLetterCount </li>
<li> 4 - MapReduce_WordLengthCount </li>
</ol>

--------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------

THIS GUIDE SHOWS HOW TO CALCULATE AMOUNT OF EACH WORD USING PREPARED JAR FILE WordCount_2.jar

--------------------------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------------------------

1 - 
Before running job in Hadoop, we should copy or move input file into HDFS. 
For that we should create HDFS home directory first:

	hadoop fs -mkdir -p /user/hdfsHomeDir


2 - 
Then create any file with text in 'hduser_1' user which is /home/hduser_1:

	cat>textFile.txt

Then type something, and then press CTRL+d to save it.


3 - 
Copy the file to hdfs directory use:

	hadoop fs -copyFromLocal /home/hduser_1/textFile.txt /user/hdfsHomeDir/textInput.txt

and to check if there is any files in that hdfs directory use:

	hadoop fs -ls /user/hdfsHomeDir

and if you want to delete that text file later:

	hdfs dfs -rm -r hdfs:/user/hdfsHomeDir/textFile.txt

4 - 
Then, you can run wordcount jar file on textFile:

	hadoop jar /home/altay/Desktop/WordCount_2.jar /user/hdfsHomeDir WordCountOutput

job will create 'WordCountOutput' folder in 'user' and will put the results there.

Note: Your jar may fail with such a following error:

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

That may be becuase jar file has default rw-r--r-- permission, and you can try to change it to rwx-rwx-rwx by:

	chmod 777 <your jar file>

and then run your jar for text file again.
e.g. 

	hadoop jar /home/altay/Desktop/WordCount_2.jar /user/hdfsHomeDir WordCountOutput


5 - 
After job is done, we can look up if there is any result file in 'WordCountOutput' folder:

	hadoop fs -ls /user/hduser_1/WordCountOutput

you should see something similar to the following:

	15/10/08 20:03:27 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
	Found 2 items
	-rw-r--r--   1 hduser_1 supergroup          0 2015-10-08 19:57 /user/hduser_1/WordCountOutput/_SUCCESS
	-rw-r--r--   1 hduser_1 supergroup         31 2015-10-08 19:57 /user/hduser_1/WordCountOutput/part-r-00000

it means you have 'part-r-00000' as output result file.


6 -
To look up the results of your job saved in that file:

	hadoop fs -cat /user/hduser_1/WordCountOutput/part-r-00000

7 - 
To delete output folder or file:

	hdfs dfs -rm -r /user/hduser_1/WordCountOutput

