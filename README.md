# Delete Update Change Transaction Database
Devices are as powerful as servers were only a few years ago. Supporting close to a terrabyte of memory and on-device databases that offer impressive DBMS and multi-core capbilites. However, these devices continue to be served by backend database architectures from the client-server days. We believe there is a better way. 

## Why a central schema?
Architects think of a the device datbase as a reflection of the backend datasource. They like to call it the `source of truth` (which a central authority must control). Thus the state we are in. Ever more complex queries and datastructures demanding backend cloud datbases to routinely handle petabytes of storage in billions of database instances and trying to be everything for every one. Managing database consistency on updates on such large databases is a challenge.

What if the device was to construct its own truth in the manner that could be different for different types of users. An analytical or reporting app may want to keep summary data whereas a transaction app may want to keep more detailed information. 

Think of concurrencly control with millions of devices trying to access and perform updates in a database. The database must keep information consistent. Half baked updates cannot be possible. Imagine trying to manage this on a peta sized databsae with millions of user trying perform updates simultaneously. 

What if the device could construct its own local schema focusing on the apps needs. Only one user will access the database at a time, therefore majority of concurrencly control issues are eliminated. Each application may be trying to solve a different problem. 

The days of a centralized Entity Relationship Diagram (ER) are gone.

## What to cache and what not to cache
Which part of the database should the device cache for the user and when should the refresh happen. Do you update the cache each time the ueser fetches?, do you do it in the background?, do you do it preemptively?. All difficult questions to solve when you are tyring to maintain two schemas, one of the device and one on the back-end. 

## Support for an App driven world
Business applications require fast, structured, reliable and scalable backend databases. A large company may have resources to build and maintain their own custom database architectures, small developers on the other hand are forced to rely on cloud database providers such as Apple’s CloudKit. 

Business applications need key-value structured storage that can manage relationships and process complex queries similar to relational databases. A central database management continues to be a drag when one thinks of developing a new application. 

Business applications must ensure [concurrencly control](https://www.geeksforgeeks.org/concurrency-control-in-dbms/). A difficult problem to solve in large multi-threaded databses. 

We are literally walking around with devices almost as powerful as servers were only a few years ago. Terrabyte of data, multithreading, relational and object oriented databses, all on a pocket device. Yet business applications continue to be designed as browser apps, fetching everything they need from the back end every time. 

## Enter DUCTbase
`Delete`, `Update`, and `Create` are the three of the CRUD transactions that affect the state of the database. DUCT stands for `Delte-Update-Change-Transaction`. 

DUCTdb is the shared-ledger database in ductbase, it is only responsible for delivering changes to the databse in the order in which they occured. Each device (app) may use part of that information or all of it construct a schema that is best for the app to access. 

Similar to the block-chain architecture, transactions are delivered (pushed) to the device in a chain. When someone needs to make an update to the chain it must add a transaction only to the end of the chain. There is some `"proof of work"` that needs to be done to achieve this, but not as complex as what blockchain requires. 

Designed to reduce TCO, targets a single architecture (MacOS), developed in a single language (Swift), and utilizes the frameworks prefected for that one architecture alone. 

___MORE INFORMATON COMING SOON.___
