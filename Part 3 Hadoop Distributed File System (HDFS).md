# Part 3: Hadoop Distributed File System (HDFS)

HDFS is the main file storage component in Hadoop and is primarily designed to store and manage large datasets typical of Big Data applications.

The file system created and maintained by HDFS is distinct and separate from the local file system of the Linux OS that it is installed on.

Files stored in its local file system will physically reside only on the disks of this machine. Files stored in HDFS on the other hand are broken into blocks and distributed for storage across multiple machines in a server cluster. Modifications made to the local Linux file system do not affect files in HDFS and vice versa. Most Hadoop components and tools will only operate on files stored in HDFS. 

In many cases, the Linux distro with the Hadoop installation will not be natively installed on a machine; rather it is packaged for deployment as a virtual machine (VM) or a Docker container. For VM distributions, the host OS running the VM containing the guest Linux distro is typically Windows. The files and directories will be transferred between the host Windows and guest Linux systems as well as between the guest Linux file system and HDFS.

An important point to note is that files must usually be in HDFS for most Hadoop components to process them. Final results are stored in HDFS and may need to be transferred back to the local file system in order to be archived or used with native applications.

![1557212642435](C:\Users\twens\AppData\Roaming\Typora\typora-user-images\1557212642435.png)

***Basic HDFS commands***

To see the contents of the HDFS root directory:

```
hdfs dfs -ls /
```

To create a hierarchical directory structure in the home directory:

```
hdfs dfs -mkdir -p dir1/dir2
```

Transfer file from Linux home directory to directory in HDFS:

```
hdfs dfs -put sample.txt dir1
```

View the contents of the text file:

```
hdfs dfs -cat dir1/sample.txt
```

Download the file in HDFS to Linux local filesystem:

```
hdfs dfs -get dir1/sample.txt tempdata
```

Remove file in HDFS:

```
hdfs dfs -rm -r dir1
```

Transfer the entire directory to HDFS:

```
hdfs dfs -put tempdata newdata
```

Move the file in HDFS:

```
hdfs dfs -mv tempdata newdata
```

Copy the file in HDFS:

```
hdfs dfs -cp dir1/sample.txt newdata
```

View the content of all directories within the home directory in HDFS of the active user account with a recursive listing:

```
hdfs dfs -ls -R
```

Merge the contents of all the files in the `somedata/newdata` directory into a single file
finalresult.txt that is then downloaded to the home directory of the local file system:

```
hdfs dfs -getmerge somedata/newdata/* finalresult.txt
```

***HUE File Browser***

Hadoop User Experience (HUE) provides a Web interface that simplifies the process of working with the various Hadoop ecosystem components.

One key difference between HDFS commands and the Hue File Browser is that deleting files using Hue File Browser moves them to a temporary .Trash folder in the home directory of the active user account (cloudera), instead of removing them permanently.

