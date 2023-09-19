I understand your request. Here's the full modified code with the sorting functionality added:

```javascript
import React, { useState, useEffect } from "react";
import { Table, TableBody, TableCell, TableContainer, TableHead, TableRow, Paper, Button } from "@mui/material";
import ViewAllPopUp from "./ViewAllPopUp";
import { getExpenseByEmployee } from "../../service/ApiService";

const viewAllButtonStyle = {
  float: "right",
  marginBottom: "12px",
  fontSize: "11px",
  backgroundColor: "#e7e7e7",
};

function ExpenseHistory() {
  const [isViewAllOpen, setIsViewAllOpen] = useState(false);
  const [data, setData] = useState([]);
  const [run, setRun] = useState(false);
  const [sortOrder, setSortOrder] = useState("asc"); // Default sorting order is ascending

  useEffect(() => {
    getExpenseByEmployee()
      .then((response) => {
        setData(response.data);
        console.log(response.data);
      })
      .catch((error) => {
        console.log(error);
      });

    setRun(true);
  }, [run]);

  const openViewAll = () => {
    setIsViewAllOpen(true);
  };

  const closeViewAll = () => {
    setIsViewAllOpen(false);
  };

  const handleSort = () => {
    // Toggle sorting order between "asc" and "desc"
    const newSortOrder = sortOrder === "asc" ? "desc" : "asc";

    // Sort the data based on the current sorting order
    const sortedData = [...data].sort((a, b) => {
      if (newSortOrder === "asc") {
        return a.amount - b.amount;
      } else {
        return b.amount - a.amount;
      }
    });

    // Update the sorted data and sorting order state
    setData(sortedData);
    setSortOrder(newSortOrder);
  };

  return (
    <div>
      <div style={viewAllButtonStyle}>
        <Button
          variant="contained"
          class="btn btn-light"
          onClick={openViewAll}
          style={{ fontSize: "14px", fontWeight: "bold", color: "black" }}
        >
          All Expenses
        </Button>
      </div>

      <TableContainer component={Paper} style={{ marginBottom: "16px" }}>
        <Table aria-label="Expense History Table">
          <TableHead>
            <TableRow>
              <TableCell style={{ fontSize: "18px", fontWeight: "bold", backgroundColor: "#cfd7e8" }}>ID</TableCell>
              <TableCell style={{ fontSize: "18px", fontWeight: "bold", backgroundColor: "#cfd7e8" }}>Category</TableCell>
              <TableCell style={{ fontSize: "18px", fontWeight: "bold", backgroundColor: "#cfd7e8" }}>
                Amount{" "}
                <Button variant="text" onClick={handleSort}>
                  {sortOrder === "asc" ? "↑" : "↓"}
                </Button>
              </TableCell>
              <TableCell style={{ fontSize: "18px", fontWeight: "bold", backgroundColor: "#cfd7e8" }}>Status</TableCell>
              <TableCell style={{ fontSize: "18px", fontWeight: "bold", backgroundColor: "#cfd7e8" }}>Receipt</TableCell>
            </TableRow>
          </TableHead>
          <TableBody>
            {data.map((expense) => (
              <TableRow key={expense.expense_id}>
                <TableCell>{expense.expense_id}</TableCell>
                <TableCell>{expense.category}</TableCell>
                <TableCell>{expense.amount.toFixed(2)}</TableCell>
                <TableCell>{expense.status}</TableCell>
                <TableCell>
                  <Button variant="light" color="primary" style={{ backgroundColor: "#e7e7e7", color: "black" }}>
                    View
                  </Button>
                </TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </TableContainer>
      <ViewAllPopUp open={isViewAllOpen} handleClose={closeViewAll} />
    </div>
  );
}

export default ExpenseHistory;
```

Now, you have the sorting functionality for the "Amount" column in your Expense History table. It will toggle between ascending and descending order when you click the sorting button.
