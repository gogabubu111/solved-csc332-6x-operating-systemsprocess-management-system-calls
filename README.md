Download Link: https://assignmentchef.com/product/solved-csc332-6x-operating-systemsprocess-management-system-calls
<br>
<h2>Process</h2>

<ul>

 <li>A process is basically a single running program</li>

 <li>Each running process has a unique number – a process identifier, pid (an integer value)</li>

 <li>Each process has its own address space</li>

 <li>Processes are organized hierarchically. Each process has a <strong>parent </strong>process which explicitly arranged to create it. The processes created by a given parent are called its <strong>child </strong>processes</li>

 <li>The C function getpid() will return the pid of process</li>

 <li>A child inherits <em>many </em>of its attributes from the parent process</li>

 <li>The UNIX command ps will list all current processes running on your machine and will list the pid</li>

 <li><strong>Remark: </strong>When UNIX is first started, there is only one visible process in the system. This process is called init with pid 1. The only way to create a new process in UNIX is to duplicate an existing process, so init is the ancestor of all subsequent processes</li>

</ul>

<h2>Process Creation</h2>

<ul>

 <li>Each process is named by a process ID number</li>

 <li>A unique process ID is allocated to each process when it is created</li>

 <li>Processes are created with the fork() system call (the operation of creating a new process is sometimes called forking a process)</li>

 <li>The fork() system call does not take an argument</li>

 <li>If the fork() system call fails, it will return -1</li>

 <li>If the fork() system call is successful, the process ID of the child process is returned in the parent process and a 0 is returned in the child process</li>

 <li>When a fork() system call is made, the operating system generates a copy of the parent process which becomes the child process</li>

 <li>Some of the specific attributes of the child process that differ from the parent process are:

  <ul>

   <li>The child process has its own unique process ID</li>

   <li>The parent process ID of the child process is the process ID of its parent process</li>

   <li>The child process gets its own copies of the parent process’s open file descriptors. Subsequently changing attributes of the file descriptors in the parent process won’t affect the file descriptors in the child, and vice versa</li>

  </ul></li>

 <li>When the lifetime of a process ends, its termination is reported to its parent and all the resources including the PID is freed.</li>

</ul>

<h2>Example of fork() system call structure</h2>

Here a process <em>P </em>calls fork(), the operating system takes all the data associated with <em>P</em>, makes a brandnew copy of it in memory, and enters a new process <em>Q </em>into the list of current processes. Now both <em>P </em>and <em>Q </em>are on the list of processes, both about to return from the fork() call, and they continue.

The fork() system call returns <em>Q</em><sup>0</sup><em>s </em>process ID to <em>P </em>and 0 to <em>Q</em>. This gives the two processes a way of doing different things. Generally, the code for a process looks something like the following.

int child = fork(); if(child == 0)

{

//code specifying how the child process Q is to behave

} else {

//code specifying how the parent process P is to behave

}

<h2>Process Identification</h2>

<ul>

 <li>You can get the process ID of a process by calling getpid()</li>

 <li>The function getppid() returns the process ID of the parent of the current process</li>

 <li>Your program should include the header files unistd.h and sys/types.h to use these functions</li>

</ul>

<h2>waitpid() System Call</h2>

<ul>

 <li>A parent process usually needs to synchronize its actions by waiting until the child process has either stopped or terminated its actions</li>

 <li>The waitpid() system call gives a process a way to wait for a process to stop. It’s called as follows. pid = waitpid(child, &amp;status, options);</li>

</ul>

In this case, the operating system will block the calling process until the process with ID child ends

<ul>

 <li>The options parameter gives ways of modifying the behavior to, for example, not block the calling process. We can just use 0 for options here.</li>

 <li>When the process child ends, the operating system changes the int variable status to represent how child passed away (incorporating the exit code, should the calling process want that information), and it unblocks the calling process, returning the process ID of the process that just stopped.</li>

 <li>If the calling process does not have any child associated with it, wait will return immediately with a value of -1</li>

</ul>

<h2>TASK 2</h2>

<strong>Part 1 </strong>Write a program children.c, and let the parent process produce two child processes. One prints out <em>“I am child one, my pid is: ” PID</em>, and the other prints out <em>“I am child two, my pid is: ” PID</em>. Guarantee that the parent terminates after the children terminate (Note, you need to wait for two child processes here). Use the getpid() function to retrieve the PID.

<strong>Part 2 </strong>Consider the parent process as <em>P</em>. The program consists of <em>fork() </em>system call statements placed at different points in the code to create new processes <em>Q </em>and <em>R</em>. The program also shows three variables: <em>a</em>, <em>b</em>, and <em>pid </em>– with the print out of these variables occurring from various processes. Show the values of <em>pid</em>, <em>a</em>, and <em>b </em>printed by the processes <em>P</em>, <em>Q</em>, and <em>R</em>.

//parent P int a=10, b=25, fq=0, fr=0 fq=fork()                // fork a child – call it Process Q if(fq==0)         // Child successfully forked a=a+b print values of a, b, and process_id fr=fork() // fork another child – call it Process R if(fr!=0) b=b+20 print values of a, b, and process_id

else a=(a*b)+30 print values of a, b, and process_id

else b=a+b-5;

print values of a, b, and process_id


