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
