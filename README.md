# Iii
H
To convert the code you provided into a sales order report format using Bootstrap, you can make several modifications. Here's an example of how you can structure your code:

```jsx
import React, { useEffect, useState } from 'react';
import { Table, TableContainer, TableHead, TableBody, TableCell, TableRow, Paper, Button } from 'react-bootstrap';
import 'bootstrap/dist/css/bootstrap.min.css';

// Assuming you have a function to fetch sales order data
async function fetchSalesOrders() {
  try {
    // Fetch your sales order data here
    const response = await fetch('your-api-endpoint-for-sales-orders');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error fetching sales orders:', error);
    return [];
  }
}

function SalesOrderReport() {
  const [salesOrders, setSalesOrders] = useState([]);

  useEffect(() => {
    fetchSalesOrders().then((data) => {
      setSalesOrders(data);
    });
  }, []);

  return (
    <div>
      <h1>Sales Order Report</h1>
      <TableContainer component={Paper} style={{ marginBottom: '16px' }}>
        <Table striped bordered hover>
          <thead>
            <tr>
              <th>Order Number</th>
              <th>Customer Name</th>
              <th>Total Amount</th>
              <th>Status</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            {salesOrders.map((order) => (
              <tr key={order.orderNumber}>
                <td>{order.orderNumber}</td>
                <td>{order.customerName}</td>
                <td>{order.totalAmount}</td>
                <td>{order.status}</td>
                <td>
                  <Button variant="info">View Details</Button>
                </td>
              </tr>
            ))}
          </tbody>
        </Table>
      </TableContainer>
    </div>
  );
}

export default SalesOrderReport;
```

In this example, we've used the `react-bootstrap` library to style the table and buttons. You should replace `'your-api-endpoint-for-sales-orders'` with the actual API endpoint to fetch your sales order data.

This code creates a simple sales order report with columns for Order Number, Customer Name, Total Amount, Status, and an "Actions" column with a "View Details" button for each order. You can further customize the styling and functionality as needed for your specific requirements.
