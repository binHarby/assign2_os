# Assignment 2: The Dining philosophers problem 
---
Author: Abdulla Alameri   
Class: Operating Systems CSC 308    
Adminstered By: Dr. Adel Khelifi    

---
![The Bois](https://upload.wikimedia.org/wikipedia/commons/7/7b/An_illustration_of_the_dining_philosophers_problem.png)

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

## Solutions and analysis:
![boi](https://www.cs.fsu.edu/~baker/opsys/notes/graphics/deadlockedphilos.gif)

One of the main solutions to this problem is that through the use of semaphores- variables that can be accessed through different processes and threads- we can have an ordering hierarchy of resources. Another solution is to give resources randomly to any process that can use them to do so(this solution is actually incorrect). Both these solutions among other suggested solutions will be discussed. 

### Solution 1- Atomic Arbitrator:

A solution can be to make the fork picking an atomic action- i.e pick both forks or none. This solution is straightforward and simple, if a philosopher can’t eat because he can’t get both forks at a relatively similar time, then he shouldn’t and he should also forfeit any fork at hand so that other philosophers may eat. This solution sometimes adds the concept of a ‘waiter’ that the philosophers ask of whether they could eat or not. This can be implemented with mutex or semaphores as shown below.

Some of this solution’s negative connotations however, are the possible unequal distribution of time between the philosophers. 

#### Example code:    

```c
#include<unistd.h>
#include<pthread.h>
#include<semaphore.h>





void * phil(void * num)
{
	int p=*(int *)num;

	sem_wait(&dine);
	printf("new  philosopher #%d",p);
	sem_wait(&forks[p]);
	sem_wait(&forks[(p+1)%5]);

	dine(p);
	sleep(5);
	printf("# %d  finined",p);

	sem_post(&forks[(p+1)%5]);
	sem_post(&forks[p]);
	sem_post(&table);
}

void dine(int philNum)
{
	printf("# %d is dining pizza ",philNum);
}

sem_t table;
sem_t forks[5];
int main()
{
	int i,a[5];
	pthread_t tid[5];
	
	sem_init(&table,0,4);
	
	for(i=0;i<5;i++)
		sem_init(&forks[i],0,1);
		
	for(i=0;i<5;i++){
		a[i]=i;
		pthread_create(&tid[i],NULL,phil,(void *)&a[i]);
	}
	for(i=0;i<5;i++)
		pthread_join(tid[i],NULL);
}
```

### Solution 2-Even/odd case solution:
In the even case a simple solution exists. The philosophers are numbered according to their place at the table. And we decide that the philosophers having an even number take their left fork first, then their right and the reverse with the philosophers having an odd number.


### Solution 3 -Resource hierarchy: 

Dijkstra solution was to give partial orders to each resource, and thus when a resource is requested by a process (philosopher) , and that a philosopher should start with request a fork of a lower order then higher order fork. This solution does avoid deadlocks, whoever, it is not practical as resources have to be known and numbered in advance. Also, if a process needs access to lower order or higher order resources it has to forfeit all its current resources in order to try to acquire that resource.  Thus, this solution is not very practical 

### Solution 4- Chandy/Misra solution: 

This solution takes a more distributed approach, and introduces the concepts of dirty and clean forks. There is communication between the philosophers in the form of messages, allowing an arbitrary number n of agents identified by any name to use a number m of resources.
To explain, the solution is as follows:
1.    There is 5 forks, each assigned to a philosopher.
2.    when a philosopher needs resources to eat, it should request it from a higher order philosopher.
3.    a philosopher that already has a fork, after finishing eating with it, it becomes dirty.
4.    a philosopher that holds a fork and was requested to hand it over, should keep it if it is still clean.
5.    a philosopher should handover a fork only when it is dirty, and should clean it before doing so. 
6.    if all the philosophers ate, then all the forks are dirty.


## Conclusion: 

In conclusion, I prefer the Chandy/Misra solution Solution, it is a distributed solution that has no intermediate elements or agents. It uses communication which makes it more flexible. It is also secure since if one process crashes, the system is distributed so it can function normally under worst case scenarios. It promotes most optimal resources sharing to neighboring processes after a process has finished utilizing the resources. Moreover, it is more fault tolerant and could be rather transparent to the end user. 
The Chandy/Misra solution may seem generic, yet it is elegant and it focuses on the time dedicated to each process and the equal sharing of processes in a system( or philosophers). Furthermore, having no intermediate between the philosophers/processes is quite an effective solution since there is no need for an arbitrator yet can still be more effective without the unnecessary back and forth messaging to another dedicated party. 
