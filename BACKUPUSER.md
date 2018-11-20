# Backing up and recovering user data in a simple way

## Backup
Open the terminal and connect to the MongoDB database using admin user with the command:
mongo -u admin -p [PASSWORD] admin
The password can be found in Secret.

Use the database wiki with the command:
> use wiki

Get all the user documents from the collection user with the command:
> db.user.find()

Results of all user documents should return. Copy all the user to somewhere safe.

## Recovering
After you have reinstalled wiki.js, the database wiki should be overwritten (or deleted and reinitialised) and thus user documents are removed.

Now we start the process of recovering user documents.

Connect to the MongoDB database using admin user again and use the database wiki.
mongo -u admin -p [PASSWORD] admin
> use wiki

Use the following command to insert all the users you want to insert:
> try { db.user.insertMany( [ {USER 1}, {USER 2}, {USER 3} ]); } catch (e) { print(e); }
It is recommanded that you first write this command somewhere and add the users, then copy and paste to the command line.

You can also using the following command to insert user one by one:
> try { db.user.insertOne( {USER 1} ); } catch (e) { print(e); }

Example:
Suppose we have two users to insert:
{ "_id" : ObjectId("123"), "updatedAt" : ISODate("2018-11-19T17:26:43.406Z"), "createdAt" : ISODate("2018-11-19T17:26:43.406Z"), "email" : "abc@e.com", "provider" : "local", "password" : "$1412asd", "name" : "abc", "rights" : [ { "role" : "read", "path" : "/", "exact" : false, "deny" : false, "_id" : ObjectId("a") } ], "__v" : 0 }
{ "_id" : ObjectId("567"), "updatedAt" : ISODate("2018-11-19T11:35:32.418Z"), "createdAt" : ISODate("2018-11-19T11:33:08.129Z"), "email" : "def@e.com", "provider" : "local", "password" : "$12312aa", "name" : "def", "rights" : [ { "role" : "write", "path" : "/", "exact" : false, "deny" : false, "_id" : ObjectId("z") } ], "__v" : 1 }

We insert both the users at the same using the command:
> try { db.user.insertMany( [ { "_id" : ObjectId("123"), "updatedAt" : ISODate("2018-11-19T17:26:43.406Z"), "createdAt" : ISODate("2018-11-19T17:26:43.406Z"), "email" : "abc@e.com", "provider" : "local", "password" : "$1412asd", "name" : "abc", "rights" : [ { "role" : "read", "path" : "/", "exact" : false, "deny" : false, "_id" : ObjectId("a") } ], "__v" : 0 }, { "_id" : ObjectId("123"), "updatedAt" : ISODate("2018-11-19T17:26:43.406Z"), "createdAt" : ISODate("2018-11-19T17:26:43.406Z"), "email" : "abc@e.com", "provider" : "local", "password" : "$1412asd", "name" : "abc", "rights" : [ { "role" : "read", "path" : "/", "exact" : false, "deny" : false, "_id" : ObjectId("a") } ], "__v" : 0 }
{ "_id" : ObjectId("567"), "updatedAt" : ISODate("2018-11-19T11:35:32.418Z"), "createdAt" : ISODate("2018-11-19T11:33:08.129Z"), "email" : "def@e.com", "provider" : "local", "password" : "$12312aa", "name" : "def", "rights" : [ { "role" : "write", "path" : "/", "exact" : false, "deny" : false, "_id" : ObjectId("z") } ], "__v" : 1 } ]); } catch (e) { print(e); }

