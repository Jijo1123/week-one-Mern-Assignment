Please enter a MongoDB connection string (Default: mongodb://localhost/):

Current Mongosh Log ID: 67c1865cfcef25b3314d7941
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.3.9
Using MongoDB:          8.0.4
Using Mongosh:          2.3.9
mongosh 2.4.0 is available for download: https://www.mongodb.com/try/download/shell

For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/

------
   The server generated these startup warnings when booting
   2025-02-28T12:10:01.270+03:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
------

test> use ecommerce
switched to db ecommerce
ecommerce> show collections
orders
products
users
ecommerce> db.users.insertMany([
...     {
...         userId: 1,
...         name: "John Doe",
...         email: "john@example.com",
...         address: "123 Main St, City, Country"
...     },
...     {
...         userId: 2,
...         name: "Jane Smith",
...         email: "jane@example.com",
...         address: "456 Elm St, City, Country"
...     }
... ])
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('67c18846fcef25b3314d7942'),
    '1': ObjectId('67c18846fcef25b3314d7943')
  }
}
ecommerce> db.products.insertMany([
...     {
...         productId: 101,
...         name: "Laptop",
...         price: 999.99,
...         category: "Electronics"
...     },
...     {
...         productId: 102,
...         name: "Smartphone",
...         price: 499.99,
...         category: "Electronics"
...     },
...     {
...         productId: 103,
...         name: "Headphones",
...         price: 199.99,
...         category: "Accessories"
...     }
... ])
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('67c18850fcef25b3314d7944'),
    '1': ObjectId('67c18850fcef25b3314d7945'),
    '2': ObjectId('67c18850fcef25b3314d7946')
  }
}
ecommerce> db.orders.insertMany([
...     {
...         orderId: 1001,
...         userId: 1, // John Doe
...         products: [101, 103], // Laptop and Headphones
...         totalAmount: 1199.98,
...         orderDate: new Date("2023-10-01T10:00:00Z")
...     },
...     {
...         orderId: 1002,
...         userId: 2, // Jane Smith
...         products: [102], // Smartphone
...         totalAmount: 499.99,
...         orderDate: new Date("2023-10-02T12:00:00Z")
...     }
... ])
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('67c18875fcef25b3314d7947'),
    '1': ObjectId('67c18875fcef25b3314d7948')
  }
}
ecommerce> db.users.find()
[
  {
    _id: ObjectId('67c18846fcef25b3314d7942'),
    userId: 1,
    name: 'John Doe',
    email: 'john@example.com',
    address: '123 Main St, City, Country'
  },
  {
    _id: ObjectId('67c18846fcef25b3314d7943'),
    userId: 2,
    name: 'Jane Smith',
    email: 'jane@example.com',
    address: '456 Elm St, City, Country'
  }
]
ecommerce> db.products.find()
[
  {
    _id: ObjectId('67c18850fcef25b3314d7944'),
    productId: 101,
    name: 'Laptop',
    price: 999.99,
    category: 'Electronics'
  },
  {
    _id: ObjectId('67c18850fcef25b3314d7945'),
    productId: 102,
    name: 'Smartphone',
    price: 499.99,
    category: 'Electronics'
  },
  {
    _id: ObjectId('67c18850fcef25b3314d7946'),
    productId: 103,
    name: 'Headphones',
    price: 199.99,
    category: 'Accessories'
  }
]
ecommerce> db.orders.find()
[
  {
    _id: ObjectId('67c18875fcef25b3314d7947'),
    orderId: 1001,
    userId: 1,
    products: [ 101, 103 ],
    totalAmount: 1199.98,
    orderDate: ISODate('2023-10-01T10:00:00.000Z')
  },
  {
    _id: ObjectId('67c18875fcef25b3314d7948'),
    orderId: 1002,
    userId: 2,
    products: [ 102 ],
    totalAmount: 499.99,
    orderDate: ISODate('2023-10-02T12:00:00.000Z')
  }
]
ecommerce> db.orders.find({ userId: 1 })
[
  {
    _id: ObjectId('67c18875fcef25b3314d7947'),
    orderId: 1001,
    userId: 1,
    products: [ 101, 103 ],
    totalAmount: 1199.98,
    orderDate: ISODate('2023-10-01T10:00:00.000Z')
  }
]
ecommerce> db.orders.aggregate([
...     { $group: { _id: null, totalRevenue: { $sum: "$totalAmount" } } }
... ])
[ { _id: null, totalRevenue: 1699.97 } ]
ecommerce> db.orders.aggregate([
...     { $unwind: "$products" },
...     { $group: { _id: "$products", totalOrders: { $sum: 1 } } },
...     { $sort: { totalOrders: -1 } },
...     { $limit: 1 }
... ])
[ { _id: 101, totalOrders: 1 } ]
ecommerce> db.orders.createIndex({ userId: 1 })
userId_1
ecommerce> db.products.createIndex({ category: 1 })
category_1
ecommerce> use library
switched to db library
library> show collections
books
library> db.books.find()
[
  {
    _id: ObjectId('67bf24d3d991af15904d7944'),
    title: 'To Kill a Mockingbird',
    author: 'Harper Lee',
    publishedYear: 1960,
    genre: 'Fiction',
    ISBN: '9780061120084',
    rating: 4.5
  },
  {
    _id: ObjectId('67bf24d3d991af15904d7945'),
    title: 'The Catcher in the Rye',
    author: 'J.D. Salinger',
    publishedYear: 1951,
    genre: 'Literary Fiction',
    ISBN: '9780316769488',
    rating: 4.5
  },
  {
    _id: ObjectId('67bf24d3d991af15904d7946'),
    title: "Harry Potter and the Sorcerer's Stone",
    author: 'J.K. Rowling',
    publishedYear: 1997,
    genre: 'Fantasy',
    ISBN: '9780590353427',
    rating: 4.5
  }
]
library>

