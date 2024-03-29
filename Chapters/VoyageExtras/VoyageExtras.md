## Tips and Tricks


This chapter contains some tips and tricks that people collected over the years. It was written by Sabina Manaa.


### How to query for an object by id?    


If you know the \_id value, you initialize an OID with this and query for it.
```
Person selectOne: {('_id' -> (OID value: 16r55CDD2B6E9A87A520F000001))} asDictionary.
```


Note that both are equivalent:
```
OID value: 26555050698940995562836590593. "dec"
OID value: 16r55CDD2B6E9A87A520F000001. "hex"
```


Or you have an instance \(in this example of `Person`\) which is in a root collection, then you ask it for its `voyageId` and use it in your query. The following assumes that you have a `Trips` root collection and a `Persons` root collection. The trip has an embedded `receipts` collection. Receipts have a `description`.  The query asks for all trips of the given person with at least one receipt with the description `aString`.

```
 Trip
		selectMany:
			{('receipts.description' -> aString).
			('person._id' -> aPerson voyageId)} asDictionary
```


### Not yet supported mongo commands


#### Indexes

It is not yet possible to create and remove indexes from Voyage, but you can use OSProcess.

Assume you have a database named `myDB` with a collection named `Trips`. 
The trips have an embedded collection with receipts. The receipts have an attribute named `description`.
Then you can create an index on the description with:

```
OSProcess command:
'/{pathToMongoDB}/MongoDB/bin/mongo --eval "db.getSiblingDB(''myDB'').Trips.createIndex({''receipts.description'':1})"'
```



Remove all indexes on the Trips collection with:
```
OSProcess command:
'/{pathToMongoDB}/MongoDB/bin/mongo --eval "db.getSiblingDB(''myDB'').Trips.dropIndexes()"'
```


#### Backup

It is not yet possible to create a backup from Voyage, so use 

```
OSProcess command:
 '/{pathToMongoDB}/MongoDB/bin/mongodump  --out {BackupPath}'
```


Please see the mongo documentation for mongo commands, especially the `--eval` command.

### Useful mongo commands


Use “.explain\(\)” in the Mongo console to ensure that your query indeed uses the index.

Example:

Create an index on an embedded attribute \(`description`\):
```
> db.Trips.createIndex({"receipts.description":1})
```


Query for it and call explain. We see, that only 2 documents were scanned.
> db.Trips.find\({"receipts.description":"a"}\).explain\("executionStats"\)
\{
	"cursor" : "BtreeCursor receipts.receiptDescription\_1",
	"isMultiKey" : true,
	"n" : 2,
	"nscannedObjects" : 2,
	"nscanned" : 2,
	"nscannedObjectsAllPlans" : 2,
	"nscannedAllPlans" : 2,
	"scanAndOrder" : false,
	"indexOnly" : false,
	"nYields" : 0,
	"nChunkSkips" : 0,
	"millis" : 0,
	"indexBounds" : \{
		"receipts.receiptDescription" : \[
			\[
				"a",
				"a"
			\]
		\]
	},
	"allPlans" : \[
		\{
			"cursor" : "BtreeCursor receipts.receiptDescription\_1",
			"n" : 2,
			"nscannedObjects" : 2,
			"nscanned" : 2,
			"indexBounds" : \{
				"receipts.receiptDescription" : \[
					\[
						"a",
						"a"
					\]
				\]
			\}
		\}
	\],
	"server" : "MacBook-Pro-Sabine.local:27017"
\}
\]\]\]

Now, remove the index
```
> db.Trips.dropIndexes()
{
	"nIndexesWas" : 2,
	"msg" : "non-_id indexes dropped for collection",
	"ok" : 1
}
```


Query again, all documents were scanned.

```
> db.Trips.find({"receipts.receiptDescription":"a"}).explain("executionStats")
{
	"cursor" : "BasicCursor",
	"isMultiKey" : false,
	"n" : 2,
	"nscannedObjects" : 246,
	"nscanned" : 246,
	"nscannedObjectsAllPlans" : 246,
	"nscannedAllPlans" : 246,
	"scanAndOrder" : false,
	"indexOnly" : false,
	"nYields" : 0,
	"nChunkSkips" : 0,
	"millis" : 1,
	"indexBounds" : {
		
	},
	"allPlans" : [
		{
			"cursor" : "BasicCursor",
			"n" : 2,
			"nscannedObjects" : 246,
			"nscanned" : 246,
			"indexBounds" : {
				
			}
		}
	],
	"server" : "MacBook-Pro-Sabine.local:27017"
}
```




### Storing instances of Date in Mongo

A known issue of Mongo is that it does not make a difference between `Date` and `DateAndTime`, so even if you commit a `Date`, you will get back a `DateAndTime`. You have to transform it back to `Date` manually when materializing the object.

### Database design

Often your objects do not form a simple tree but a graph with cycles. For example, you could have persons who are pointing to their trips and each trip knows about its person \(`Person <->>Trip`\). If you create a root Collection with Persons and a Root collection with Trips, you avoid endless loops being generated \(see chapter 1.2\).

This is an example of a `trip` pointing to a `person` which is in another root collection and another root collection, `paymentMethod`. Note that the receipt also points back to the trip, which does not create a loop.

```
Trip
{
 "_id" : ObjectId("55cf2bc73c9b0fe702000008"), 
"#version" : 876079653, 
"person" : { 
	"#collection" : "Persons", 
	"_id" : ObjectId("55cf2bbb3c9b0fe702000007") }, 
"receipts" : [ 	
	{ "currency" : "EUR", 	
	"date" : { "#instanceOf" : "ZTimestamp", "jdn" : 2457249, "secs" : 0 }, 	
	"exchangeRate" : 1, 	
	"paymentMethod" : {
		"#collection" : "PaymentMethods", 	
		"_id" : ObjectId("55cf2bbb3c9b0fe702000003") }, 
	"receiptDescription" : "Taxi zum Hotel", 	
	"receiptNumber" : 1, 	
	"trip" : { 
		"#collection" : "Trips", 	
		"_id" : ObjectId("55cf2bc73c9b0fe702000008") } } ], 
	"startPlace" : "Österreich",    
	"tripName" : "asdf", 
	"tripNumber" : 1 }
```


The corresponding `person` points to all its `trips` and to its `company`.
```
{ "#version" : 714221829, 
"_id" : ObjectId("55cf2bbb3c9b0fe702000007"), 
"bankName" : "",
 "company" : { 
"#collection" : "Companies", 
"_id" : ObjectId("55cf2bbb3c9b0fe702000002") },
"email" : "bb@spesenfuchs.de", 
"firstName" : "Berta",
"lastName" : "Block",  
"roles" : [  "user" ], 
"tableOfAccounts" : "SKR03", 
"translator" : "German", 
"trips" : [ 	
{
"#collection" : "Trips", 	
"_id" : ObjectId("55cf2bc73c9b0fe702000008") } ] }
```


If your domain has strictly delimited areas, e.g. clients, you could think about creating one repository per area \(client\). 

### Retrieving data 


One question is if it is possible to retrieve data from Mongo collection even if the database was not created via Voyage.
Yes, it is possible. Here is the solution.

First we create a class `MyClass` with two class side methods:

```
MyClass class >> isVoyageRoot
	^ true
```


```
MyClass class >> descriptionContainer
    <voyageContainer>
    ^ VOContainer new
        collectionName: 'myCollection';
        yourself
```

	
Also, to properly read the data one should add instance variables depending on what is in the database. 

For example if we have the following information stored in the database: 

```
{ "_id" : ObjectId("5900a0175bc65a2b7973b48a"), "item" : "canvas", "qty" : 100, "tags" : [ "cotton" ] }
```


In this case `MyClass` should have instanceVariables: `item`, `qty` and `tags` and accessors.
Then we define the following description on the class side

```
MyClass class >> mongoItem
	<mongoDescription>
	^ VOToOneDescription new
		attributeName: 'item';
		kind: String;
		yourself
```


```
MyClass class >> mongoQty
	<mongoDescription>
	^ VOToOneDescription new
		attributeName: 'qty';
		kind: Integer;
		yourself
```


```
MyClass class >> mongoTags
	<mongoDescription>
	^ VOToOneDescription new
		attributeName: 'tags';
		kind: OrderedCollection;
		yourself
```

	
After that one can connect to database and get the information.

```
| repository | 
repository := VOMongoRepository database: 'databaseName'.
repository selectAll: MyClass
```





