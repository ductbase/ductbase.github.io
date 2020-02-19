# DUCTbase - Welcome

Devices become more powerful as servers were only a few years ago. They support close to a terrabyte of memory and on-device databases that offer impressive database management capabilities. We are serving these devices with back-end data architectures from the client-server days. An endliess cycle of ever more complex queries and data structures demanding backend cloud datbases to routinely handle petabytes of storage in billions of database instances (planet scale). DUCTbase is an atteempt to reverse this trend and it will simplify a number of complexities.

Imagine 

## TCO (total cost of ownership)

## Backend Schema vs On Device Schema

## What to cache 


 it will answer the following fundamental questions in database management:




- Reduce TCO (total cost of ownersip) by making the a by targeting one architecture and take advantage of frameworks written for that architecture - i.e. Mac OSX Server, Swift and Network Framework
- The de

We are serving these devices with architectures from the client-server world. 





DUCTBase Faisal Bhombal Vercom Software, Inc.
ABSTRACT
Devices are now supporting close to a terabyte of memory and processing power equalling those of servers from only a few years ago. These devices are demanding backend and cloud databases to to support ever more complex data structures. 
Cloud databases are routinely handling petabytes of data in billions of database instances on the planet scale.
 Devices are supporting close to a terabyte of storage with the ability to manage multi-threaded databases on the device itself. 
These highly potent devices place demands on these large database for features contributing to database complexities pushing the physical limits of memory and disk drives. 

 where handling device faults is part of managing a large database. 


Applications require fast, structured, reliable and scalable backend databases. A large company may have resources to build and maintain their own custom database architectures, small developers on the other hand are forced to rely on cloud database providers such as Apple’s CloudKit. Business applications need key-value structured storage that can manage relationships and process complex queries similar to relational databases. These cloud databases handle petabytes of data in billions of database instances on a daily basis. Managing database consistency on updates on such large databases is a challenge. Mobile devices, desktop platforms, and web browsers are speedy application development platforms with the ability to cache gigabytes of structured data and process it with ease. DUCTdb is an architecture for back-end databases that forces architects to think of device storage as a central piece in the overall puzzle. DUCTdb stores a chain of operations that record modifications to the data, these include the Delete, Update and Create transactions. These operations are delivered to the device in order and the device is responsible for constructing the database locally structured to suit the application on the device. DUCTdb greatly simplifies the architecture of the database and we will show that it does not complicate the application architecture any more than it already is. No database element is every modified on the server, thus eliminating the complexity related to concurrency control. The device databases are designed to server one user at a time and sans the complexity necessary to server multiple users simultaneously. 
INTRODUCTION
The macro database architecture has not changed since the advent of client-server applications, where a central repository maintains the source of truth which is distributed to clients on a per-need basis. As mobile, desktop and web platforms have become faster, so has the demand for fast, scalable and capable back end databases. Cloud storage such as CloudKit are popular for their low barrier to entry for developers who cannot afford to build their own back end databases. Key-value transactional storage is a requirement for most business applications. Through a RESTful interface, the database provides CRUD capabilities while maintaining consistency, specially as updates can be performed from multiple devices simultaneously on the same data. Developing such datastore present “many challenges” [1] as they are expected to handle billions of database instances with petabytes of data and millions of active users at a time.
The sheer size of the database requires distribution of data distribution using milt-tenancy and architectural improvements such as micro-services. Non-transactional data such as news posts, blog items, photos or videos are not expected to change that often and as such can scale easily. Transactional data that is updated more frequently requires complex solutions to ensure consistency and atomicity across all versions of a document. First, a data value may have an index. Update the data must also be applied to the Index atomically. Fault tolerance should be build in so that a failure does not sacrifice consistency. Second, relationships between records pose a problem. An update or delete must cascade through all relationships if necessary. Third, a database may serve different requirements for analytical data may need to be maintained in a different manner to facilitate reporting, therefore a single transaction may affect multiple apps in the same domain and must be kept in synch. These problems are “notoriously difficult to solve”. Fifth, multiple updates to the same data item must be resolved correctly. A transaction may update the same JSON document where the updates may or may not overlap. The database must be able to ensure that there are no inconsistencies. As it is impossible to determine which is the correct version, the database normally rejects the update that is sent out of sequence. The device sub sitting the request receives additional information, such as a sequence token, the current state of the data and the data that was sent with the query. The resolution on the client-side is still the client’s responsibility. The simplest solution is for the client to update the version on the device and reject the previous update and prompt the user to resubmit a new update. 
Devices have local cache storage that they may also utilize. However, it is almost never considered as part of the back-end database. App developers will usually create a local cache to duplicate the the schema or part of the schema. CloudKit and CoreData enhancements in iOS13 have implemented a solution where CoreData will map directly to the cloud store [5]. 
We believe that there needs to be rethink on how we are managing storage across devices and servers. Almost unlimited amounts of storage and CPU cycles on devices are being wasted by focusing solely on the back-end database for the data architecture. DUCTdb (Delete, Update, Create Transaction Database) eliminates the majority of the issues related to large transactional databases. It eliminates the majority of the transactions that may create conflicts and consistency issue thus eliminating most of the complexity in database systems. The backend database is only required to maintain the chain of transactions that when applied in sequence, will create the truth according to the needs of the client. If a client only needs summary information for analytics or all of the transactions, the central database in DUCTdb does not have to worry about it. A root block indicates the start of a new database. Al clients must possess the rootBlock’s recordName and it may be hardcoded in the app. Each block including the root block has a unique recordName which is a UUID that is globally unique. Each block also contains the recordName of the next block in the update chain. The application may receive a push notification to process pending blocks or may read the next block. Initially the next block is the rootBlock. If the block is read, it’s changes are processed that may be 

The update chain consists of a chain of blocks with a recordName for the block and the recordName for the next block. 

struct Block {
	let recordName: String
	let transactions: [Transaction]
	let nextRecordName: String
}
struct Transaction {
	let keyPath = “/“
	let opcode: String
	let documentName: String
	let documentType: String
	let payload: String 
}
struct Payload {
	// attributes depend on the document type
}


Instead of maintaining one source of truth DUCTdb only maintains the chain of transactions that when applied in sequence will recreate the truth. The only conflict that can occur is when two clients try to record an update block. If that block has already been created, the update will be rejected with appropriate information returned to the client to resolve the conflict. The client will fetch all of the updates it has missed and send the update again. Not any more complex than what clients need to do to resolve conflicts now. 

A block written to the database looks as follows:


The client app is responsible for creating the schema per its needs. The central database does not need to cater to all 

When a client needs to record an update to the database is places the transaction or the batch of transactions 

The back-end database only maintains the transactions that modify the data and in sequence. Any c
 Instead of creating a central source of truth, each application accessing the data maintains the truth based on its needs. An analytics app in the same domain may create and schema only with summary information, whereas the transactional version of the app will maintain all of the details. Both derived from the same source. 
 
targeted withs summary information whereas 
keep summary data whereas the transactional version of 
 A DUCTdb delivers Deletes, Updates and Create transactions in order. 
The device app is responsible for creating the database schema locally. 

The only conflict that can occur is that two devices may attempt to write the next update to the database. We have implemented a solution that solves this problem by taking guidance from block-chain. 

Database on the device such as CoreData provide all of the capabilities that large distributed databases provide. With one advantage and that the database services a single user. There may be multiple simultaneous database accesses due to multi-threading but implementing threadsafety locally is much more achievable for a known domain than implementing for billions of unknown implementations. 








greatly simplifying the architecture of the central database. As in a block-chain, the database store is expected to keep a chain of transactions in the order that when applied to a database, will produce current state of the database. Data is never over-written but rather a new record is created in the chain. We believe that an entire suite of problems related to very large databases (VLDB) will disappear. We also do not believe that moving some of the database logic on to the device will not complicate the end-to-end architecture any more than what is expected these days. CloudKit is the reference architecture that we are trying to simplify.
Introduction

Single user
The reference architecture we are simplifying is the CloudKit’s Record Store [1], which is like a relational database, is implemented on top of FoundationDB [2] and offers “an ordered, transactional key-value store. Each application creates a container with a unique name, within which there are three types of databases. The public database is accessible to all users who install the application and can be used to distribute information to information that everyone needs, such as a news feed. The private database is used to store user-specific information such as settings but supports more complex key-value storage that can have relationships with other entities. A third type of database is shared database, which is simply some records from a user’s private database that he or she shares with other users. The shared database is a unique feature of CloudKit that is not found in other database. Theoretically, it can allow developers to create an application distribute it to millions or billions of users, where each users can create their own group of users to share the data with, in.a company for example. Thus allowing developers to create business applications without the need to manage any back-end infrastructure on their own. However, there are a number of limitations to the CloudKit datastore. Firstly, the immense size of the database that maintains billions of containers require’s multi-tenancy to be implemented. Records are therefore not immediately updated in some cases as one would expect. Second, shared database require users to share individual records that may have dependent records, but there is a limit of approximately 5,000 dependent records that may be a children to a shared parent record. 
With billions of individual containers handling petabytes of data daily, one can imagine the complexity of the implementation. The complexity is derived from :
1. 
The complexity of a central database is derived from multiple factors, including size, relationship complexity, concurrency and atomicity requirements, diversity, and availability demands. 
Mobile devices are becoming as powerful and complex as servers were just a few years ago. Multi-core architectures with a terabyte of memory and 5G internet will soon be common. The back-end servers to process and deliver petabytes of data to mobile devices on a daily basis is a “challenge” [1] to manage for database management server. Multi-tenancy and micro-services are architectural enhancements in an effort to keep up with demand. However, the fundamental storage architecture where a database or a cluster of databases manage the entities, entity-relationships, and records has not changed since the days of client-server computing. 
Database stores such as CloudKit provide binary storage store

Users also expect their data to be shared between devices and with other users

Mobile devices are expected to grow in processing speed, on board storage and complexity. So 
 than servers were only a few years ago, the need to share data between a user’s devices and between users is ever increasing and posts gargantuan challenges. Databases are expected to manage petabytes of storage for billions of devices on a daily basis. Architectural enhancements such as Multi-tenancy, micro-services architectures are trying to keep up with demand. Expecting a common storage architecture for billions of devices, each with close to a terabyte of storage is unmanageable if not now, will be in the very near future. Concurrency Control, Query Management, Transaction Atomicity, ACID guarantees are what make DBMS’s complex.  
A single application may have diverse needs. A transactional database that supports live data as well as a reporting and analytic module that has a completely different requirements for data summarization and accessibility. 
Data Consistency while updating a structured JSON document is another issue due to the stateless nature of today’s transaction. When there is a mismatch detected due to a versioning token or a optimistic scheme of ensuring that none of the original data has changed, the device (client) is required to resolve the mismatch. Either by merging the new with what has changed or having the user resolve it or simply by completely rejecting the update that has been received after a mismatch. Either way, the app on the device needs to get involved.
Updating a piece of data on the server with a billion of users is another issue. Partitioning techniques such as sharing is implemented. Multiple writes to the same file must also be controlled in order not to corrupt the data. Data corruption is a regular occurrence when dealing with such large database that they must be accounted for in the management system.
Another capability Cloud databases need to support for devices is the ability to create virtual container where a users’s data can be securely kept. The data owner has the option to share the container with other users by invitation only. Only Apple’s CloudKit offers this capability among the large databases. The Data sharing capability is severely limited for structured data. In the absence of data sharing, app developers developing business applications are required to implement the mechanism for data sharing themselves. This could involve creating a different end point for each shared group or a less secure virtual container that can be created off the a single database instance for each group of users that need to share.
Relationships, cascading deletes, etc.
On-device databases have all the complexity that is needed. They manage relationships, data consistency, and atomicity. The task of maintain the structure on the device is made exponentially simpler as the on-device database is not expected to server multiple users simultaneously. One user with possible multiple threaded transactions, but no where near service billions of users and petabyte of data. 
Apps are architected to implement on-device caching of at least the most commonly used data. The data schema is somewhat distributed on the device as well as the central store. 
The pressure on maintaining a multi-peta byte data store with containers and the ability to be able to share data by uses with other users, is enormous. We propose a better solution that is simple and based on the following objectives:
The central data store maintains only the changes occurring to transactions, namely the deletes, updates, and create statements for the structured data. Second, no data on the server is ever updated, only new ones created. This avoids all concurrency control issues and reduces the complexity of managing the physical storage. Third, the structure of the data that the device needs is maintained only on the device. Different modules (apps) consuming the same data stream may arrange the data per their needs, eg. Analytics vs transactions.
DUCTdb is primarily solves the database issues related with structured data such as key-value pairs where relationships between records (documents) need to be maintained. Data such as photos and atomic documents normally have no such relationship’s and much easier to handle except the sheer size of them. 


The architecture to manage the 

Developers can potentially distribute their app to billions of customers without the need to manage much of the infrastructure that is managed by the platform’s app-store. In addition to mobile platforms, desktops are moving towards the same App store distribution model. While making it easier to create a consumer application, there are a number of challenges for developers of business applications. The primary hurdle in creating a secure 

Consider a developer wanting to develop an ERP solo

However if a developer wants to create a system 
however what is missing is the ability for a company to develop a common system for example an ERP that may be distributed to multiple customers where each customer may share their data with specific users and no one else. 

First is the need to share data. Consider a developer who wants to develop an ERP system that any user may be able to do
the management of a back-end data store to allow users at a company
need to maintain a backend data store for a group of users who may securely share data with each other but no one else. 
Cloud storages such as iCloud maintain petabytes of data 

Primarily due to the need for a backend datastore that must allow users to securely share data with other users.

More and more apps are distributed through an “app store” reducing the need for developers to manage distribution and device management, including mobile and desktop platforms. This model provides tremendous cost-savings to developers. Developers can d

As Microsoft and Apple are moving to the same type of distribution model as iOS Apps, the same challenges will be faced by developers of desktop applications. First, the closed eco system from development to delivery is designed to ease the delivery of apps potentially billions of users. The developer cannot access individual installations, nor is it feasible to expect it. Specific functionality can only be controlled through configuration options. This model affords 


is controlled by the platform. Developers do not have access to individual installation nor is is viable to expect it I

Once a developer releases an app to the store, it is r

 that is why B2B applications on devices are limited. 
most important of which is the structure of the backend database to support shared data. 

B2B applications on mobile devices are limited due to the unique challenges that mobile devices introduce for developers, specially iOS devices. 
First, Apps are distributed through the App Store  to all the devices, customizations are handled via configuration options. 
All business applications require a backend database for data that is shared with other users in the company. 

One of those challenges is the distribution of a single version to all 


Masjidbook implementation
We have implemented a version of DUCTdb that delivers updates to the database to the application and the application is responsible for creating and managing the structure on the device. This we have implemented on the MasjidBook app available on the App store. Masjidbook provides users information on the location of masjid around the world. The data needs to be corrected frequently as new locations are added and information changes. All data resides on the device and is read from the devices Core Data database. 


[1]	Christos Chrysafis, Ben Collins, Scott Dugas, Jay Dunkelberger, Moussa Ehsan, Scott Gray, Alec Grieser, Ori Herrnstadt, Kfir Lev- Ari, Tao Lin, Mike McMahon, Nicholas Schiefer, and Alexander Shraer. 2019. FoundationDB Record Layer: A Multi-Tenant Structured Data- store. In 2019 International Conference on Management of Data (SIG- MOD ’19), June 30-July 5, 2019, Amsterdam, Netherlands. ACM, New York, NY, USA 
[2]
A. Shraer, A. Aybes, B. Davis, C. Chrysafis, D. Browning, E. Krugler, E. Stone, H. Chandler, J. Farkas, J. Quinn, J. Ruben, M. Ford, M. McMahon, N. Williams, N. Favre-Felix, N. Sharma, O. Herrnstadt, P. Seligman, R. Pisolkar, S. Dugas, S. Gray, S. Lu, S. Harkema, V. Kravtsov, V. Hong, Y. Tian, and W. L. Yih. Cloudkit: Structured Storage for Mobile Applications. Proc. VLDB Endow., 11(5):540–552, Jan. 2018. 
[3]
D. DeWitt and J. Gray. Parallel Database Systems: The Future of High Performance Database Systems. Communication ACM, 35(6), June 1992.
[4] FoundationDB. https://www.foundationdb.org, 2018.
[5] FoundationDB source. https://github.com/apple/foundationdb, 2018.

[6] WWDC enhancements on CloudKit & core data

DUCTbase is a simple, shared-ledger database designed for an app driven world. DUCTdb is the database engine at its core. 


# The Justification
Business applications on mobile devices and apps in general do not take advantage of the close to a terrabyte of storage and the power of the multi-threaded device that most of us carry in our pockets now. 

Architects continue to design database architectures from the days of client-server computing. Where the business rules are still implemented on the back-end server. Supporting petabytes of storage on backend servers is becoming the norm. 

DUCTdb is a shared ledger database that is simple. Its purpose is to deliver Delete, Update and Change transaction to devices in order. 

The databse is designed to be implemented on Mac OSX only and is written completely in Swift using the latest Apple Network Framework. Now that we have Mac OSX servers, virtualization on Macs cannot be very far away. 

