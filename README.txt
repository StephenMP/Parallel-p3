Team:       Berimbolo
Members:    Stephen Porter & Ryan Peachey
Updated:    11/19/2014
Assignment: p3 - Better Inverted Index Using Mapreduce
Class:      Parallel Computing

=============
General Info
=============
To Compile: make

To Run:
 - Requirements:
 -- Hadoop 1.2.1
 -- Root access
 -- ssh to localhost without password
 -- A jar of the java code
 
[HADOOP_INSTALL_PATH]/local-scripts/create-hadoop-cluster.sh
[HADOOP_INSTALL_PATH]/hadoop/bin/hadoop fs -put [INPUT] input/
[HADOOP_INSTALL_PATH]/hadoop/bin/hadoop jar [JAR_FILE] input/ output/
[HADOOP_INSTALL_PATH]/hadoop/bin/hadoop fs -get output/ .

======
Files
======
src
  |_
	InvertedIndex.java
README.txt

=====
Data
=====
/* Data will be available once we can run in parallel on Onyx */

===========
Discussion
===========
We began discussing this project through direct messaging and examining the provided code and how it was supposed to work. While this
was going on (and since our schedules conflict so much) we arranged a couple of times to meet via screen sharing in order to work on the
solution together. During one of these sessions we were able to talk through and figure out some of the misunderstandings on how the
original source code was supposed to work. These screen sharing options also made it far easier for us to work on the code than to assign
separate parts and complete them on our own.

With this understanding we came to find that since the mapper was already doing all of the work in finding the different files 
in which the terms are found we simply needed to modify the reducer method in such a way so that, rather than repeatedly
printing out the file name it would reduce the file names and print the wordcount and the file it was found in. Once we had that segment
working we worked on sorting the data.

At first we were planning on using a HashMap/HashSet in order to store the data that we would need. After some discussion though we ruled
this option out thinking about the issues that may occur when attempting to sort the data. Rather we decided to attempt to use
a list with our own custom object that held the filename and the number of counts through a custom object and custom comparator.
Unfortunately, this option did not pan out as we quickly ran into ConcurrenceExceptions where a thread in the reducer was trying to modify
the list while another thread was iterating through it.

Upon some research, we found that when you use a HashMap collection, when you obtain the data it is backed by the map itself. In other words,
modifying the map would also modify the whatever collection you pulled from the map (e.g. map.getKeySet() returns a Set of your keys which
is backed by the map itself). Thus, we could build up our HashMap and send out a copy post-sorting which allowed us to then avoid data loss
when sorting the HashMap by value then by key and avoid ConcurrenceExceptions by iterating over a copy of the map.