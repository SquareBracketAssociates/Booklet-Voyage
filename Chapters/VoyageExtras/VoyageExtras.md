## Tips and Tricks
OID value: 16r55CDD2B6E9A87A520F000001. "hex"
		selectMany:
			{('receipts.description' -> aString).
			('person._id' -> aPerson voyageId)} asDictionary
'/{pathToMongoDB}/MongoDB/bin/mongo --eval "db.getSiblingDB(''myDB'').Trips.createIndex({''receipts.description'':1})"'
'/{pathToMongoDB}/MongoDB/bin/mongo --eval "db.getSiblingDB(''myDB'').Trips.dropIndexes()"'
 '/{pathToMongoDB}/MongoDB/bin/mongodump  --out {BackupPath}'
{
	"nIndexesWas" : 2,
	"msg" : "non-_id indexes dropped for collection",
	"ok" : 1
}
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
	^ true
    <voyageContainer>
    ^ VOContainer new
        collectionName: 'myCollection';
        yourself
	<mongoDescription>
	^ VOToOneDescription new
		attributeName: 'item';
		kind: String;
		yourself
	<mongoDescription>
	^ VOToOneDescription new
		attributeName: 'qty';
		kind: Integer;
		yourself
	<mongoDescription>
	^ VOToOneDescription new
		attributeName: 'tags';
		kind: OrderedCollection;
		yourself
repository := VOMongoRepository database: 'databaseName'.
repository selectAll: MyClass