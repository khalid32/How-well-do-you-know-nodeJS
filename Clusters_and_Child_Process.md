# Clusters and Child Process

## Scaling Node.js Applications
While single-threaded, non-blocking performance is quite good, eventually, one process in one CPU is not going to be enough to handle an increasing workload of an application. 

The fact that node runs in a single thread does not mean that we can't take advantage of multiple processes, and of course, multiple machines as well. Using multiple processes is the only way to scale a Node.js application.

`Workload` is the most popular reason we scale our applications, but it's not the only reason. We also scale our applications to increase their `availability` and `tolerance to failure`.

It's important to understand the different strategies of scalability. There are mainly 3 things we can do to scale an application. 

![Scalability Strategies](https://user-images.githubusercontent.com/8571179/60584983-74bf2680-9db0-11e9-8f4a-c7a4c018278e.png)

- The easiest thing to do to scale a big application is to clone it multiple times and have each cloned instance handle part of the workload. This does not cost a lot in term of development time and it's highly effective.
- We can scale an application by decomposing it based on functionalities and services. This means having multiple different applications with different code bases and sometimes with their own dedicated databases and User Interfaces. </br>This strategy is commonly associated with the term microservice, where micro indicates that those services should be as small as possible, but in reality the size of the service is not what's important, but rather the enforcement of loose coupling and high cohesion between services. </br>The implementation of this strategy is often not easy and could result in long-term unexpected problems, but when done right the advantages are great.
- The third scaling strategy is to split the application into multiple instances where each instance is responsible for only a portion of the application's data. This strategy is often named `horizontal partitioning`, or `sharding`, in databases. </br>`Data partitioning` requires a lookup step before each operation to determine which instance of the application to use.


## Child Processes Events and Standard IO
We can easily spin a child process using the `child_process` node modules, and those child processes can easily communicate with each other with a messaging system.

The `child_process` module enables us to access Operating system functionalities by running any system command inside a child process, control its input stream, and listen in on its output stream.
We can control the arguments to pass to the command and we can do whatever we want with its output. We can , for example, pipe the output of one command to another command, as all inputs and outputs of those commands can be presented to us using `Node streams`.

There are 4 different ways we can use to create a child process in Node.
- `spawn()`
- `fork()`
- `exec()`
- and `execFile()`


The `spawn()` method launches a command in a new process and we can use it to pass that command any arguments.
