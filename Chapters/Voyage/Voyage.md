## Persisting Objects with Voyage
	host: 'mongo.db.url'
	database: 'databaseName'.
	instanceVariableNames: 'oldRepository'
	classVariableNames: ''
	package: 'MyVoyageTests'
	oldRepository := VORepository current.
	VORepository setRepository: VOMemoryRepository new.
	VORepository setRepository: oldRepository
	^ true
	^ 'Associations'
anAssociation save.
	"_id" : ObjectId("a05feb630000000000000000"),
	"#instanceOf" : "Association",
	"#version" : NumberLong("3515916499"),
	"key" : 'answer',
	"value" : 42
}
	^ true
aRectangle save.
	"_id" : ObjectId("ef72b5810000000000000000"),
	"#instanceOf" : "Rectangle",
	"#version" : NumberLong("2460645040"),
	"origin" : {
		"#instanceOf" : "Point",
		"x" : 42,
		"y" : 1
	},
	"corner" : {
		"#instanceOf" : "Point",
		"x" : 10,
		"y" : 20
	}
}
	"_id" : ObjectId("7c5e772b0000000000000000"),
	"#instanceOf" : "Rectangle",
	"#version" : 423858205,
	"origin" : {
		"#collection" : "point",
		"#instanceOf" : "Point",
		"_id" : ObjectId("7804c56c0000000000000000")
	},
	"corner" : {
		"#collection" : "point",
		"#instanceOf" : "Point",
		"_id" : ObjectId("2a731f310000000000000000")
	}
}
	"_id" : ObjectId("7804c56c0000000000000000"),
	"#version" : NumberLong("4212049275"),
	"#instanceOf" : "Point",
	"x" : 42,
	"y" : 1
}

{
	"_id" : ObjectId("2a731f310000000000000000"),
	"#version" : 821387165,
	"#instanceOf" : "Point",
	"x" : 10,
	"y" : 20
}
	<mongoContainer>

	^ VOMongoContainer new
		collectionName: 'rectanglesForTest';
		kind: Rectangle;
		yourself
	<mongoDescription>

	^ VOMongoToOneDescription new
		attributeName: 'origin';
		kind: Point;
		yourself
	<mongoDescription>

	^ VOMongoToOneDescription new
		attributeName: 'corner';
		kind: Point;
		yourself
	"_id" : ObjectId("ef72b5810000000000000000"),
	"#version" : NumberLong("2460645040"),
	"origin" : {
		"x" : 42,
		"y" : 1
	},
	"corner" : {
		"x" : 10,
		"y" : 20
	}
}
	<mongoDescription>

	^ VOMongoToOneDescription new
		attributeName: 'currency';
		accessor: (MAPluggableAccessor
			read: [ :amount | amount currency abbreviation ]
			write: [ :amount :value | amount currency: (Currency fromAbbreviation: value) ]);
		yourself
	<mongoDescription>
	
	^VOTransientDescription new
		attributeName: 'currencyMetaData';
		yourself
Fri Aug 28 14:21:10.815 Error: invalid object id: length
	"origin" : {
		"x" : 42,
		"y" : 1
	},
	"corner" : {
		"x" : 10,
		"y" : 20
	}
}
	'name' -> 'John'.
	'orders' -> { '$gt' : 10 } asDictionary
} asDictionary
OID value: 16r55CDD2B6E9A87A520F000001. "hex"
	'origin.x' -> {'$eq' : 42} asDictionary
} asDictionary
	{
		{
			'name' -> 'John'.
			'orders' -> { '$gt': 10 } asDictionary
		} asDictionary.
		{
			'name' -> { '$ne': 'John'} asDictionary.
			'orders' -> { '$lte': 10 } asDictionary
		} asDictionary.
	}.
} asDictionary.
	'fullname.lastName' -> {
		'$regexp': '^D.*'.
		'$options': 'i'.
	} asDictionary.
} asDictionary.
	instanceVariableNames: 'collectionClass where pageCount'
	classVariableNames: ''
	package: 'DemoPaginator'

Paginator class>>on: aClass where: aCondition
	^ self basicNew
		initializeOn: aClass where: aCondition

Paginator>>initializeOn: aClass where: aCondition
	self initialize.
	collectionClass := aClass.
	where := aCondition
	^ 25

Paginator>>pageCount
	^ pageCount ifNil: [ pageCount := self calculatePageCount ]

Paginator>>calculatePageCount
	| count pages |
	count := self collectionClass count: self where.
	pages := count / self pageSize.
	count \\ self pageSize > 0
		ifTrue: [ pages := pages + 1].
	^ count
	^ self collectionClass
		selectMany: self where
		limit: self pageSize
		offset: (aNumber - 1) * self pageSize
	'/{pathToMongoDB}/MongoDB/bin/mongo --eval ',
	'"db.getSiblingDB(''myDB'').Trips.',
	'createIndex({''receipts.description'':1})"'
	'/{pathToMongoDB}/MongoDB/bin/mongo --eval ',
	'"db.getSiblingDB(''myDB'').Trips.dropIndexes()"'
						 .explain("executionStats")
{
	"cursor" : "BtreeCursor receipts.receiptDescription_1",
	"isMultiKey" : true,
	"n" : 2,
	"nscannedObjects" : 2,
	"nscanned" : 2,
	"nscannedObjectsAllPlans" : 2,
	"nscannedAllPlans" : 2,

  [...]
}
					  ..explain("executionStats")
{
	"cursor" : "BasicCursor",
	"isMultiKey" : false,
	"n" : 2,
	"nscannedObjects" : 246,
	"nscanned" : 246,
	"nscannedObjectsAllPlans" : 246,
	"nscannedAllPlans" : 246,

  [...]
}