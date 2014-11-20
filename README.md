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
/*It is required to have hadoop installed and set up on your machine. You also need to be root.*/


======
Files
======


=====
Data
=====

===========
Discussion
===========
We began discussing this project through direct messaging and examining the provided code and how it was supposed to work.
While this was going on we arranged a couple of times to meet via screen sharing in order to work on the solution together.
During one of these sessions we were able to talk through and figure out some of the misunderstandings on how the original
source code was supposed to work. 
With this understanding we came to find that since the mapper was already doing all of the work in finding the different files 
in which the terms are found we simply needed to modify the reducer method in such a way that rather than repeatedly
printing out the file name it would only print out the number of times it appeared and then the respective file name for those
occurrences. Once we had that segment working we would then work on sorting the data so that it would be output with the files 
receiving the most hits printing out first and then sorting through in descending order.
At first we were planning on using a hashmap in order to store the data that we would need. After some discussion though we ruled
this option out thinking about the issues that may occur when attempting to sort the data. Rather we decided to attempt to use
a list with our own custom object that held the filename and the number of counts. We did this knowing that we could use the 
comparator method and set it to compare the values in the way we desired.
For this solution we would have the iterator run through the data that was passed to the reduce method. When the filename was returned
if it was the first occurrence of the filename we would setup a new object, set the count on that object to 1 and move on to the next
iterated item. Once all of the items had been put into the list we would then use a similar method as provided by the initial file, 
to append our data onto the output string.
Feeling that our logic was sound we went on to the testing phase following the instructions on how to use hadoop.
When we tried to run the jar file through hadoop it would send out a concurrency exception. This is due to the fact that
hadoop uses multithreads to do the tasks assigned to it. Since lists are not threadsafe because they need to 