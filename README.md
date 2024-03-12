```js
const fs = require('fs');
const PDFParser = require('pdf-parse');

// Read PDF file
const pdfBuffer = fs.readFileSync('invoice.pdf');

// Parse PDF
PDFParser(pdfBuffer).then(data => {
    const text = data.text;
    const transactions = extractTransactions(text);
    console.log(transactions);
});

// Function to extract transactions from the extracted text
function extractTransactions(text) {
    const lines = text.split('\n');
    const transactions = [];

    for (let i = 0; i < lines.length; i++) {
        // Assuming each transaction occupies one line
        const line = lines[i].trim();

        if (line) {
            const parts = line.split(/\s+/); // Split by whitespace
            const transaction = {
                'App ID': parts[0],
                'Xref': parts[1],
                'Settlement Date': parts[2],
                'Broker': parts[3],
                'Sub Broker': parts[4],
                'Borrower Name': parts[5],
                'Description': parts.slice(6, parts.length - 3).join(' '), // Join remaining parts as description
                'Total Loan Amount': parseFloat(parts[parts.length - 3].replace(',', '')), // Remove commas and parse as float
                'Comm Rate': parseFloat(parts[parts.length - 2]),
                'Upfront': parseFloat(parts[parts.length - 1].replace(',', ''))
            };

            transactions.push(transaction);
        }
    }

    return transactions;
}

```


## Create Database

```
// db.js

const sqlite3 = require('sqlite3').verbose();

// Create a new SQLite database instance
const db = new sqlite3.Database(':memory:'); // Use ':memory:' for in-memory database, or provide a file path for persistent database

// Define schema and create tables
db.serialize(() => {
    db.run(`
        CREATE TABLE IF NOT EXISTS transactions (
            id INTEGER PRIMARY KEY,
            xref TEXT,
            total_loan_amount REAL
        )
    `);
});

// Export the database instance
module.exports = db;

```


## How to insert in sqlite

```js

// app.js

const db = require('./db');

// Function to insert a transaction into the database
function insertTransaction(transaction) {
    db.run("INSERT INTO transactions (xref, total_loan_amount) VALUES (?, ?)", [transaction.xref, transaction.total_loan_amount], (err) => {
        if (err) {
            console.error('Error inserting transaction:', err);
        } else {
            console.log('Transaction inserted successfully.');
        }
    });
}

// Function to retrieve all transactions from the database
function getAllTransactions(callback) {
    db.all("SELECT * FROM transactions", (err, rows) => {
        if (err) {
            console.error('Error retrieving transactions:', err);
            callback(err, null);
        } else {
            callback(null, rows);
        }
    });
}

// Example usage
insertTransaction({ xref: '1001', total_loan_amount: 5000 });
getAllTransactions((err, transactions) => {
    if (err) {
        console.error('Error:', err);
    } else {
        console.log('All transactions:', transactions);
    }
});

```
