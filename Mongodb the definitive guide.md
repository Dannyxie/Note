##Chapter 2

basic concepts:

  - A document is the basic unit of data for MongdoDB, roughly equivalent to a row in a relational database management system (but must more expressive).
  - Similarly, a collection can be thought of as the schema-free equivalent of a table.
  - A single instance of MongoDB can host multiple independent databases, each of which can have its own collections and permissions.
  - MongoDB comes with a simple but powerful JavaScipt shell, which is useful for the administration of MongoDB instances and data manipulation.
  - Every document has a special key, "\_id", that is unique across the document;s collection.

###Documents

- Keys must not contain the charater \0 (the null  character). This character is used to signify the end of a key.
- The . and $ characters have some special properties and should be used only in certain circumstances, as described in later chapters. In general, they should be considered reserved, and drivers will complain if they are used inappropriately.
- Keys starting with \_ should be considered reserved; although this is not strictly enforced.

- MongoDB is type-sensitive and case-sensitive.
- Documents in MongoDB cannot contain duplicate keys.

###Collections
- Collections are schema-free. This means that the documents within a single collection can have any number of different "shapes".

###Naming

A collection is identified by its name. Collection names can be any UTF-8 string, with a few restrictions:

	- The empty string("") is not a valid collection name.
	- Collection names may not contain the character \0 (the null character) because this delineates the end of a collection name.
	- You should not create any collections that start with **system.**, a prefix reserved for system collections.
	- User-created collections should not contain the reserved character $ in the name.


###Databases

- The empry string ("") is not a valid database name.
- Database names should be all lowercase.
- Database names are limited to a maximum of 64 bytes.

- Reserved database :  `admin`, `local`, `config`

