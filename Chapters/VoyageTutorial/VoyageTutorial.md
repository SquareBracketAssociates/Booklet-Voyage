## A Simple Tutorial with Super Heroes
repository := VOMongoRepository 
		host: 'localhost' 
		database: 'superHeroes'.
repository enableSingleton.
repository := VOMemoryRepository new. 
repository enableSingleton
	| repository |
	repository := VOMongoRepository 
		host: 'localhost' 
		database: 'superHeroes'.
	repository enableSingleton.
   instanceVariableNames: 'name level powers' 
   classVariableNames: ''
   package: 'SuperHeroes'
   ^ name
   
Hero >> name: aString 
   name := aString
   ^ level

Hero >> level: anObject 
   level := anObject
   ^ powers ifNil: [ powers := Set new ]
   self powers add: aPower
   instanceVariableNames: 'name' 
   classVariableNames: '' 
   package: 'SuperHeroes'
   ^ name
   
Power >> name: aString 
   name := aString
   ^ true
   name: 'Spiderman';
   level: #epic;
   addPower: (Power new name: 'Super-strength');
   addPower: (Power new name: 'Wall-climbing');
   addPower: (Power new name: 'Spider instinct');
   save.
Hero new
   name: 'Wolverine';
   level: #epic;
   addPower: (Power new name: 'Regeneration');
   addPower: (Power new name: 'Adamantium claws');
   save.
local        0.078GB
superHeroes  0.078GB

> use superHeroes
switched to db superHeroes

> show collections
Hero
{
	"_id" : ObjectId("d847065c56d0ad09b4000001"),
	"#version" : 688076276,
	"#instanceOf" : "Hero",
	"level" : "epic",
	"name" : "Spiderman",
	"powers" : [
		{
			"#instanceOf" : "Power",
			"name" : "Spider instinct"
		},
		{
			"#instanceOf" : "Power",
			"name" : "Super-strength"
		},
		{
			"#instanceOf" : "Power",
			"name" : "Wall-climbing"
		}
	]
}
> an OrderedCollection(a Hero( Spiderman ) a Hero( Wolverine )
> a Hero( Spiderman )
> an OrderedCollection(a Hero( Spiderman ) a Hero( Wolverine )
> a Hero( Spiderman ) 
> an OrderedCollection(a Hero( Spiderman ) a Hero( Wolverine )
	selectMany: { #level -> #epic } asDictionary 
	sortBy: { #name -> VOOrder ascending } asDictionary
	limit: 10
	offset: 0
> 2
> 1
hero remove.
> a Hero
> Hero class
	^ true
Power new name: 'Super-strength'; save.
	Hero
	Power
fly := Power selectOne: [ :each | each name = 'Fly']. 
superStrength := Power selectOne: [ :each | each name = 'Super-strength'].
Hero new
	name: 'Superman'; level: #epic;
	addPower: fly; 
	addPower: superStrength; 
	save.
{
	"_id" : ObjectId("d8474983421aa909b4000008"),
	"#version" : NumberLong("3874503784"),
	"#instanceOf" : "Hero",
	"level" : "epic",
	"name" : "Superman",
	"powers" : [
		{
			"#collection" : "Power",
			"#instanceOf" : "Power",
			"_id" : ObjectId("d84745dd421aa909b4000005")
		},
		{
			"#collection" : "Power",
			"#instanceOf" : "Power",
			"_id" : ObjectId("d84745dd421aa909b4000006")
		}
	]
}
   instanceVariableNames: 'name level powers equipment' 
   classVariableNames: ''
   package: 'SuperHeroes'
   ^ equipment ifNil: [ equipment := Set new ]
   self equipment add: anEquipment
	instanceVariableNames: '' 
	classVariableNames: '' 
	package: ‘SuperHeroes'
	^ true
	instanceVariableNames: ''
	classVariableNames: ''
	category: 'SuperHeroes'
	instanceVariableNames: ''
	classVariableNames: ''
	category: 'SuperHeroes'
	name: 'Iron-Man'; 
	level: #epic; 
	addEquipment: Armor new; 
	save.
{
	"_id" : ObjectId("d8475734421aa909b4000001"),
	"#instanceOf" : "Hero",
	"#version" : NumberLong("2898020230"),
	"equipment" : [
		{
			"#instanceOf" : "Armor"
		}
	],
	"level" : "epic",
	"name" : "Iron-Man",
	"powers" : null
}
	instanceVariableNames: 'powers' 
	classVariableNames: '' 
	package: 'SuperPowers'
	^ powers ifNil: [ powers := Set new ]
	self powers add: aPower
hero := Hero selectOne: [ :each | each name = 'Iron-Man' ].
fly := Power selectOne: [ :each | each name = 'Fly' ].
superStrength := Power selectOne: [ :each | each name = 'Super-strength' ].
hero addEquipment: (Armor new
        addPower: fly;
        addPower: superStrength;
        yourself);
save.
{
	"_id" : ObjectId("d8475777421aa909b4000003"),
	"#instanceOf" : "Armor",
	"#version" : NumberLong("4204064627")
}