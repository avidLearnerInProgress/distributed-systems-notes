LeCloud Posts

Cloning :=

* First golden rule for scalability: every server contains exactly the same codebase and does not store any user-related data, like sessions or profile pictures, on local disc or memory. 

=> public servers of scalable web service are hidden behind load balancers. These load balancers distribute load(requests) from the users to different servers in a group/cluster.

=> users should always get the same results from the web service even if it is served by different servers at different instances.

	=> sessions need to be stored in a centralised data store which has access to all the app servers. (can be external database or a persistent storage like Redis/memcache) [if it’s on external, it has better performance] (why??)
	
	=> thing to ensure that your application servers don’t keep state. State should be externalized into a central data store.

	=> need to ensure that the code change within sessions has to be outsourced to all the servers serving the requests - to ensure consistency.

Database :=

* Even if servers can horizontally scale and we are ready to server thousands of concurrent requests, the app gets slower and slower and breaks down. - because of DB

=> 2 ways to tackle this issue
	
A. ) Keep the DB as it is.
	Use master-slave replication and upgrade master by adding more RAM. Use something like “sharding”, “denormalization” and “sql tuning” > but every such action will be more and more expensive and time consuming process. 

B.) Denormalize data right from the beginning and include more joins in DB query. Keep using the same DB or any form of NoSQL DB. Use a lot more joins as early as possible. However, all such efforts will also lead to DB requests being slower. This in turn will cause a need to introduce Cache.

	Cache :=

* Even if you have found a near-good solution to store TB’s of information on your server end; the users are still suffering from slow response because of DB overhead. Hence. There is a need for “Cache”

	=> Cache - in-memory caches like Memcached or Redis.  File-based caching is not preferred because it creates unnecessary pain while scaling and cloning of servers.
	
=> A cache is a simple key-value store and it should reside as a buffering layer between your application and your data storage. Whenever your application has to read data it should at first try to retrieve the data from your cache. Only if it’s not in the cache should it then try to get the data from the main data source

=> 2 patterns of caching data

A.) Cached DB queries 
	Query to DB -> store the returned results in cache. Cache is moreover a form of key-value pair Data structure. So, the hashed version of query is the cache key. So next time, whenever we fire the same query on app server; the first check is made whether the hashed version of query exists in cache; if it does, it is returned, else, we reach out to the main DB from where the results are returned and stored in cache and then served to the user. 
	Issue - 1. Expiration. Hard to delete the cached result if we cache a complex query. [ When one piece of data changes (for example a table cell) you need to delete all cached queries who may include that table cell.]

	B.) Cached Objects
	See data as objects. Let class assemble dataset from the DB and then store complete instances of class / dataset in cache. When the class has finished the “assembling” of the data array, directly store the data array, or better yet the complete instance of the class, in the cache! This results in easily getting rid of the object whenever something does change and makes the overall operation of code faster and more logical. It is an async operation. App consumes the latest cached object and never touches the DB anymore.
		
Async :=
	
(Bakery example…)

=> 2 Async paradigms 
A.) Doing time-consuming work in advance and serving the completed work with low request time. Example - turn dynamic content to static.

B.) Handling “new” tasks asynchronously. 
A user comes to your website and starts a very computing intensive task which would take several minutes to finish. So the frontend of your website sends a job onto a job queue and immediately signals back to the user: your job is in work, please continue to browse the page. The job queue is constantly checked by a bunch of workers for new jobs. If there is a new job then the worker does the job and after some minutes sends a signal that the job was done. The frontend, which constantly checks for new “job is done” - signals, sees that the job was done and informs the user about it. 
