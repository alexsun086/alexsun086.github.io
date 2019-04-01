```
hdfs dfs version

mkdir <path>
hdfs dfs -mkdir /user/dataflair/dir1

ls <path>
hdfs dfs -ls /user/dataflair/dir1
hdfs dfs -ls -R

put <localSrc> <dest>
hdfs dfs -put /home/dataflair/Desktop/sample /user/dataflair/dir1

copyFromLocal <localSrc> <dest>
hdfs dfs -copyFromLocal /home/dataflair/Desktop/sample /user/dataflair/dir1
//This hadoop shell command is similar to put command, but the source is restricted to a local file reference.

get [-crc] <src> <localDest>
hdfs dfs -get /user/dataflair/dir2/sample /home/dataflair/Desktop
hdfs dfs -getmerge /user/dataflair/dir2/sample /home/dataflair/Desktop
//This HDFS basic command retrieves all files that match to the source path entered by the user in HDFS, and creates a copy of them to one single, merged file in the local file system identified by local destination.
hadoop fs -getfacl /user/dataflair/dir1/sample
hadoop fs -getfacl -R /user/dataflair/dir1
//This Apache Hadoop command shows the Access Control Lists (ACLs) of files and directories. If a directory contains a default ACL, then getfacl also displays the default ACL.
hadoop fs -getfattr -d /user/dataflair/dir1/sample
//hadoop fs -getfattr -d /user/dataflair/dir1/sample

copyToLocal <src> <localDest>
hdfs dfs -copyToLocal /user/dataflair/dir1/sample /home/dataflair/Desktop
//Similar to get command, only the difference is that in this the destination is restricted to a local file reference.

cat <file-name>
hdfs dfs -cat /user/dataflair/dir1/sample

mv <src> <dest>
hadoop fs -mv /user/dataflair/dir1/purchases.txt /user/dataflair/dir2

cp <src> <dest>
hadoop fs -cp /user/dataflair/dir2/purchases.txt /user/dataflair/dir1

moveFromLocal <localSrc> <dest> 
hdfs dfs -moveFromLocal /home/dataflair/Desktop/sample /user/dataflair/dir1 
moveToLocal <src> <localDest> 
hdfs dfs -moveToLocal /user/dataflair/dir2/sample /user/dataflair/Desktop 

hdfs dfs -tail [-f] <filename> 
hdfs dfs -tail /user/dataflair/dir2/purchases.txt
hdfs dfs -tail -f /user/dataflair/dir2/purchases.txt

rm <path> 
hdfs dfs -rm /user/dataflair/dir2/sample 
hdfs dfs -rm -r /user/dataflair/dir2 

hdfs dfs -expunge
//This Hadoop shell commmand is used to empty the trash.

hdfs dfs -chown [-R] [OWNER][:[GROUP]] URI [URI ]
hdfs dfs -chown -R dataflair /opt/hadoop/logs

hdfs dfs -chgrp [-R] <NewGroupName> <file or directory name> 

hdfs dfs -setrep -w 3 /user/dataflair/dir1 

hdfs dfs -du /user/dataflair/dir1/sample 
hdfs dfs -du -s /user/dataflair/dir1/sample

hdfs dfs -df -h

hdfs dfs -touchz /user/dataflair/dir2 

hdfs dfs -test -[ezd] URI 
Options:
-d: if the path given by the user is a directory, then it gives 0 output.
-e: if the path given by the user exists, then it gives 0 output.
-f: if the path given by the user is a file, then it gives 0 output.
-s: if the path given by the user is not empty, then it gives 0 output.
-z: if the file is zero length, then it gives 0 output.

hdfs dfs -text /user/dataflair/dir1/sample 

hdfs dfs -stat /user/dataflair/dir1 
This HDFS File system command prints information about the path.
%b: If the format is a string which accepts file size in blocks.
%n: Filename
%o: Block size
%r: replication
%y, %Y: modification date.

hdfs dfs -tail [-f] <filename2> 

hdfs dfs -chown -R dataflair /opt/hadoop/logs
hdfs dfs -chmod 777 /user/dataflair/dir1/sample

hadoop fs -checksum /user/dataflair/dir1/sample 
 
hadoop fs -appendToFile <localsource> ... <dst> 
hadoop fs -appendToFile /home/dataflair/Desktop/sample /user/dataflair/dir1 

hdfs dfs -count /user/dataflair 

hadoop fs -find <path> ... <expression> ...
hadoop fs -find /user/dataflair/dir1/ -name sample -print

hadoop fs -truncate [-w] <length> <paths>
hadoop fs -truncate 55 /user/dataflair/dir2/purchases.txt /user/dataflair/dir1/purchases.txt
hadoop fs -truncate -w 127 /user/dataflair/dir2/purchases.txt

hadoop fs -help 
hadoop fs -usage command 
```