# Introduction

The purpose of this project was to implement a multi-threaded client and a multi-threaded server that interacted with each other. Like in our first project, the client must interact with the user to receive commands and process these requests by a multithreaded implementation where all the requests are sent to the server as soon as they are ready. The server handles the requests concurrently, executes the specified tasks, and returns the requested data to the client. This paper will discuss the design and implementation of both the client and the server. Each client request was performed for 1, 5, 10, 15, 20, 25, and 100 times and the elapsed time for the data to be sent from the client to the server and to return was recorded. This paper covers the data collection and analysis.

# Client-Server Setup and Configuration

After the client and server begin running, the user will be prompted to input the IP address and the port number by the client and to input just the port number on the server. Then, the client prompts the user with a menu to choose from the functions it can perform, including Date and Time, Uptime, Memory Use, Netstat, Current Users, and Running Processes. After the choice is made, the client will prompt the user with how many requests are to be sent to the server. 
Our multithreaded client had no changes from our first project. As we discussed in our last report, the multithreading of the client is created in a class called “ClientConnection.” A new ClientConnection object is created for each request. Within ClientConnection, the socket that connects the server and the client is created, meaning a new socket is created for each request and is closed once the request is completed. 
Our server has changed significantly from the first project so that it runs concurrently instead of iteratively. The server listens for requests from the client and when they are connected, the server immediately creates a new thread to handle the client request. This new thread is implemented in a new ServerThread object. It is within this object that the server processes the client’s request and returns the data to the client before closing the connection. The client then prints the elapsed time for each request and then the total elapsed time of all the requests and the average elapsed time per request. The client and server will repeat these steps until the user exits the menu. 
As expected for a multithreaded client and server, every ClientConnection object has a corresponding ServerThread object. The design of the server is far simpler for the concurrent server than it was for the past iterative server. The server’s processing of the data to and from the client is exactly like the iterative server. It reads input from the Linux commands and appends all the data into one long string and sends that string to the client. The client reads the data from the server in a do…while loop using the DataInputStream function “.available()”. To successfully keep listening for the client, creating a connection, and starting a new thread to process the client’s request, the server only needed one while(true) loop that kept running until the client chose to exit. This is far simpler than the iterative server, which required us checking whether the main thread listening for the client was alive or not.

# Testing, Data Collection, and Analysis

To test the total turn-around time of the Concurrent Socket Server, we asked the client to generate 1, 5, 10, 15, 20, 25, and 100 requests to the server for each available command. The total and average elapsed times of each request is recorded in the tables below and for clarity we have graphed the total elapsed time by number of requests and the average elapsed time by number of requests. We have kept these same graphs from the Iterative Socket Server for comparison. 
Increasing the number of clients did not have a large impact on the total time until more than 25 requests were made. When we look at the graph covering the average time in microseconds depending on the request, we see that under 25 requests, Uptime, Memory Use, and Date and Time all remain fairly constant while Netstat, Current Users, and Running Processes slowly increase in time. The Date and Time request remains the most constant, with its highest average time being 29,311 microseconds at 100 requests, around only 25% of the time it took the Current Users and Running Processes to perform 100 requests. The graph seems to indicate that amounts larger than 100 requests might yield an exponential growth in average time for the Current Users and Running Processes while Date and Time grows more incrementally. 
When comparing the average turn-around times of the Iterative and the Concurrent Socket Servers, we see that for less requests, the Iterative Socket Server runs faster. When one request is made, 5/6 of the types of requests perform in under 7500 microseconds on the Iterative Socket Server. On the Concurrent Socket Server, only 1/6 of the request types completes in under 7500 microseconds when one request is made. On the largest comparable amount of client requests, 25, the two servers perform comparatively. However, if we tested larger amounts on both servers, I would predict the concurrent server would perform better, as it does not need to wait for the server to finish processing the current request before starting the next one. The concurrent server is preferred in scenarios where there are more requests, but also where the requests have a larger average turn-around time. An iterative server can get away with waiting for each request to be processed before starting another if the requests do not take long. From this data, we can gather that iterative servers are preferable in situations that only require a small number of requests.

# Concurrent Socket Server Graphs and Data

![image1](/images/Picture1.png)

![image2](/images/Picture2.png)

![image3](/images/Picture3.JPG)

![image4](/images/Picture4.JPG)

# Iterative Socket Server Graphs and Data

![image5](/images/Picture5.png)

![image6](/images/Picture6.png)

![image7](/images/Picture7.JPG)

![image8](/images/Picture8.JPG)

# Conclusion and Lessons Learned

From the data we can glean that for these commands, the iterative server is preferable when only small numbers of requests are made. Whether this can be generalized to all iterative servers is unclear. If each request takes seconds—or even minutes—to complete, is an iterative server still preferable when we need to make 5 requests? This would require further testing. We anticipated seeing a drastic reduction in average turn-around time for the concurrent server, and frankly we did not see it. Conceptually, it seems intuitive that a client and a server both running in a multithreaded fashion would run faster because no request would ever be waiting. However, conclusions drawn on this data are hardly full or complete as each thread per request was only performed once. Because each was only tested once, it is hard to know if our data was full of outliers that would be weeded out if we had tested each of the two variables (function call and amount of threads) fifty times. 
The lessons we learned during this assignment were far fewer than the lessons learned with the Iterative Socket Server. The Iterative Socket Server took many productive hours and more futile ones as we tried to solve bugs in our code. For this assignment, our entire client was already built and did not need changing. The knowledge we had from implementing the Iterative Socket Server allowed us to create the multithreaded server very quickly. 



