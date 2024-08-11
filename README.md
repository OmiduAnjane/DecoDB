DecoDB Documentation
====================

Welcome to the DecoDB documentation. DecoDB is a lightweight, JSON-based database designed for simplicity and performance. It provides a powerful set of features, including multi-field indexing, asynchronous operations, and schema validation. DecoDB is ideal for projects needing a simple, effective database solution without the overhead of more complex systems.

Features
--------

*   **Multi-Level Indexing:** Optimize query performance with indexes on multiple fields.
*   **Asynchronous Operations:** Improve performance with non-blocking file and database interactions.
*   **Rich Query Language:** Perform complex queries with operators like `$gt`, `$lt`, `$in`, and `$exists`.
*   **Improved Schema Management:** Ensure data integrity with advanced schema validation, including type checking and required fields.
*   **Data Backup and Restore:** Safeguard data with easy backup and restore features.

Installation
------------

To get started with DecoDB, install it via npm:

    npm install decodb

Make sure Node.js and npm are installed on your machine. You can download them from [nodejs.org](https://nodejs.org/).

Usage
-----

Here’s a basic example to get you started:

```js    const DecoDB = require('decodb');
    const path = require('path');
    const db = new DecoDB(path.join(__dirname, 'db.json'));
    
    // Set Schema with Advanced Validation
    db.setSchema('users', {
        _id: { type: 'string' },
        name: { type: 'string', required: true },
        age: { type: 'number', required: true }
    });
    
    // Example Documents
    const doc1 = { _id: '1', name: 'Alice', age: 30 };
    const doc2 = { _id: '2', name: 'Bob', age: 25 };
    
    // Upsert Documents
    db.upsert('users', doc1).then(() => {
        return db.upsert('users', doc2);
    }).then(() => {
        // Find Documents
        return db.find('users', { age: { $gte: 25 }, name: { $in: ['Alice', 'Bob'] } });
    }).then(results => {
        console.log('Find Result:', results);
    });
   ``` 

API Reference
-------------

### Constructor

`new DecoDB(filePath)` - Initializes a new DecoDB instance. The `filePath` is the path to the JSON file where the database is stored.

### Methods

`upsert(collection, document)`

Creates or updates a document with schema validation. If the document does not have an `_id`, one will be generated.

`find(collection, query)`

Finds documents based on the query. Supports operators like `$gt`, `$lt`, `$in`, and `$exists`.

`delete(collection, id)`

Deletes a document by its ID.

`createIndex(collection, fields)`

Creates an index on the specified fields. The `fields` parameter is an array of field names.

`dropIndex(collection, fields)`

Deletes an index from the specified fields.

`setSchema(collection, schema)`

Sets the schema for a collection. The `schema` parameter defines the structure and validation rules for documents in the collection.

`backup(backupPath)`

Creates a backup of the database at the specified `backupPath`.

`restore(backupPath)`

Restores the database from a backup file at the specified `backupPath`.

Examples
--------

### Basic Example

Initialize DecoDB, set a schema, upsert documents, and perform a query:
```js
    const DecoDB = require('decodb');
    const path = require('path');
    const db = new DecoDB(path.join(__dirname, 'db.json'));
    
    // Set Schema
    db.setSchema('users', {
        _id: { type: 'string' },
        name: { type: 'string', required: true },
        age: { type: 'number', required: true }
    });
    
    // Insert Data
    const user = { _id: '1', name: 'Alice', age: 30 };
    db.upsert('users', user).then(() => {
        return db.find('users', { name: 'Alice' });
    }).then(results => {
        console.log('User:', results);
    });
    
```
### Advanced Example

Features such as indexing and complex queries:
```js
    const DecoDB = require('decodb');
    const path = require('path');
    const db = new DecoDB(path.join(__dirname, 'db.json'));
    
    // Set Schema
    db.setSchema('products', {
        _id: { type: 'string' },
        name: { type: 'string', required: true },
        price: { type: 'number', required: true },
        category: { type: 'string' }
    });
    
    // Create Index
    db.createIndex('products', ['price', 'category']).then(() => {
        // Insert Data
        const product1 = { _id: '1', name: 'Laptop', price: 1200, category: 'Electronics' };
        const product2 = { _id: '2', name: 'Phone', price: 800, category: 'Electronics' };
        
        return db.upsert('products', product1);
    }).then(() => {
        return db.upsert('products', product2);
    }).then(() => {
        // Find Documents with Advanced Query
        return db.find('products', { price: { $gte: 800 }, category: 'Electronics' });
    }).then(results => {
        console.log('Products:', results);
    }).catch(err => {
        console.error('Error:', err);
    });
    ```