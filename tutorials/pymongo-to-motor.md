# __pymongo vs Motor__
[Motor](https://motor.readthedocs.io/en/stable/index.html) is an async Python driver for MongoDB.

## __*When should I use Motor?*__
You should use Motor when you're trying to interact with a MongoDB database in an asynchronous context. When you're making something that needs to be asynchronous (like a web server, or most commonly from what I've seen here, Discord bots), you also want all the database calls to be done asynchronously. But pymongo is _synchronous_, i.e it is blocking, and will block the execution of your asynchronous program for the time that it is talking to the database.

## __*Okay, How do I switch now?!*__
Thankfully for us, switching from pymongo to Motor isn't too hard, and won't need you to change much code. This process can be roughly summarized as:
### **Step 1: Install Motor, and import it**
Installing can be done with pip - `pip install motor`
After this it can be imported into your file as
```py
import motor.motor_asyncio as motor
``` 
(NOTE: The `as motor` part is just to avoid having to type `motor.motor_asyncio` everytime)

### **Step 2: Make a Client and get a database**
You must be familiar with pymongo's `MongoClient` - you will switch that out with motor's equivalent class
```py
client = motor.AsyncIOMotorClient(<connection details>)
```
Getting a database and a collection is similar to pymongo, (you can do `client["database name"]` to get the database and you'd do `database["collection name"]` to get the collection. Or you can also use dot-notation)

### **Step 3: Make the queries asynchronous**
Now add an `await` before _every query_ you have (**except** `.find()`, this will be explained later)
```py
# before
something = collection.find_one({"foo": "bar"})
collection.delete_one({"foobar": 1})
collection.update_many({"foo": "bar"}, {"$set": {"foo": "foobar"}})
```
```py
# after
something = await collection.find_one({"foo": "bar"})
await collection.delete_one({"foobar": 1})
await collection.update_many({"foo": "bar"}, {"$set": {"foo": "foobar"}})
```
And you're done! Your queries are no longer blocking the event loop :D

#### Sidenote; Why aren't `.find`s `await`ed?
[`collection.find` only returns a _cursor_ object.](https://motor.readthedocs.io/en/stable/tutorial-asyncio.html#querying-for-more-than-one-document) No real I/O operation takes place until you actually try accessing the data from the cursor.
You can asynchronously loop over the contents of the cursor (with an `async for` loop), or use the cursor's `to_list` method to immedietely get all the data (now _this_ has to be awaited!)
Meaning,
```py
data = collection.find()    # correct
data = await collection.find()    # wrong!

# retrieving data
# option 1:
async for i in data:
  # do stuff 

#option 2
as_list = await data.to_list(length=None)
```
`to_list` takes a `length` kwarg, which specifies how many documents to retrieve - passing None means you retrieve all documents.

[Documentation](https://motor.readthedocs.io/en/stable/tutorial-asyncio.html#tutorial-using-motor-with-asyncio)
[Back to Homepage](https://anand2312.github.io)
