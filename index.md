# Assignment 2: The Dining philosophers problem 

Author: Abdulla Alameri
Class: Operating Systems CSC 308
Adminstered By: Dr. Adel Khelifi
--

The problem of the dining philosophers was first stated by the Dutch computer scientist Edsger Dijkstra in 1965 as an examination question for students. Since then, the problem has undergone a number of changes. It appears in several slightly different formats, some of which only change the details of the story, but others put additional limitations on the problem to demonstrate difficult concepts. 
Generally, the dining problem created by Dijkstra is as follows: five philosophers gather around a table.Each philosopher has a plate of some sort of food in front of him that he would like to eat. There is a fork on both sides of each of their plates; a total of five forks. A philosopher needs two forks to eat his dinner dish, one on the right and other one on the left. 

Think of the philosophers as processes and the forks as a shared resource. Each process needs two resources of which one has already acquired and the other is acquired by some other process. Until the other process releases the resource, the process in question cannot continue. Similarly, the other process relies on another process for its appeal, and so on. Since all processes depend on each other, a circular wait forms and the system enters a ‘deadlock’.

> What is a deadlock?
A deadlock situation on a resource when these conditions are meet: 
1.    Mutual exclusion: At least one resource must be kept in a non-shareable mode. Otherwise, the processes would not be prevented from using the resource if necessary. Only one process can use the resource at any given time.
2.    Resource holding: a process currently holds at least one resource and requests additional resources that are held by other processes.
3.    Non-preemption: a resource can only be released voluntarily by the process that owns it.
4.    circular wait: Each process must wait for a resource that is owned by another process, which in turn waits for the first process to release the 


Here, when each philosopher takes the fork from his right, he also ends up taking the left fork of the person sitting next to him, which leaves each philosopher with only one fork and no one can start eating.


The metaphors of the problem are, forks that represent a resource, while a philosopher represents a process in need of resources, thinking is when a process is staying idle and the dinner table and the situation as a whole represents a system. 
Moreover, the two existential problems that the philosophers are facing is a situation where none of them is eating, which can lead to starving and then death. Although this problem might initially feel like it is distant from the computer world and practical physical world for that matter, however it accurately represents how processes toothpick and programs contend for the limited resources in a computer. 
if they all the philosophers act in naive and identical ways, run the risk of finding themselves in a deadlock situation. In fact, everyone can just grab their left fork and then each wait for their right fork to become free so that none of them can eat, and this state can last for eternity. We can also consider that a philosopher who dies (crash of the process) remains in a phase of “thinking” infinitely. 

The challenge of the dining problem comes through its constraints, in other words how can we solve this problem such that the philosophers are equally fed  concurrently and to avoid deadlocks- which happens when for example all the philosophers are holding only one fork, where the whole system is not progressing. Thus, we need to prioritize avoidance of resource starvation and implement mutual exclusion to avoid data corruption.
