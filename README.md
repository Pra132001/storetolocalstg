<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Expense Tracker</title>
</head>
<body>

  <h1>Expense Tracker</h1>

  <!-- Form to add expense -->
  <form id="expenseForm">
    <label for="expenseDescription">Description:</label>
    <input type="text" id="expenseDescription" required>
    
    <label for="expenseAmount">Amount:</label>
    <input type="number" id="expenseAmount" required>
    
    <button type="button" onclick="addExpense()">Add Expense</button>
  </form>

  <!-- Display expenses -->
  <ul id="expenseList"></ul>

  <script>
    document.addEventListener("DOMContentLoaded", function() {
      loadExpenses();
    });

    function addExpense() {
      var description = document.getElementById("expenseDescription").value;
      var amount = parseFloat(document.getElementById("expenseAmount").value);

      if (!description.trim() || isNaN(amount) || amount <= 0) {
        alert("Please enter valid values for description and amount.");
        return;
      }

      var expense = {
        description: description,
        amount: amount
      };

      var expenses = JSON.parse(localStorage.getItem("expenses")) || [];
      expenses.push(expense);
      localStorage.setItem("expenses", JSON.stringify(expenses));
      
      loadExpenses();
      document.getElementById("expenseForm").reset();
    }

    function loadExpenses() {
      var expenses = JSON.parse(localStorage.getItem("expenses")) || [];
      var expenseList = document.getElementById("expenseList");
      expenseList.innerHTML = "";

      expenses.forEach(function(expense, index) {
        var li = document.createElement("li");
        li.textContent = `${expense.description} - $${expense.amount.toFixed(2)}`;

        var deleteButton = document.createElement("button");
        deleteButton.textContent = "Delete";
        deleteButton.onclick = function() {
          deleteExpense(index);
        };

        var editButton = document.createElement("button");
        editButton.textContent = "Edit";
        editButton.onclick = function() {
          editExpense(index);
        };

        li.appendChild(deleteButton);
        li.appendChild(editButton);

        expenseList.appendChild(li);
      });
    }

    function deleteExpense(index) {
      var expenses = JSON.parse(localStorage.getItem("expenses")) || [];
      expenses.splice(index, 1);
      localStorage.setItem("expenses", JSON.stringify(expenses));
      loadExpenses();
    }

    function editExpense(index) {
      var expenses = JSON.parse(localStorage.getItem("expenses")) || [];
      var selectedExpense = expenses[index];

      var newDescription = prompt("Enter new description:", selectedExpense.description);
      var newAmount = parseFloat(prompt("Enter new amount:", selectedExpense.amount));

      if (newDescription && !isNaN(newAmount) && newAmount > 0) {
        selectedExpense.description = newDescription;
        selectedExpense.amount = newAmount;
        localStorage.setItem("expenses", JSON.stringify(expenses));
        loadExpenses();
      } else {
        alert("Invalid input. Expense not updated.");
      }
    }
  </script>

</body>
</html>
