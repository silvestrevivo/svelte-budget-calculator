<script>
  import { setContext } from "svelte";
  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  // data
  import expensesData from "./expenses";
  // variables
  let expenses = [...expensesData];
  // set editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;
  // reactive
  $: total = expenses.reduce((acc, curr) => (acc += curr.amount), 0);
  $: isEditing = setId ? true : false;
  // functions
  function addExpense({ name, amount }) {
    let expense = {
      id: Math.random() * Date.now(),
      name,
      amount
    };
    expenses = [expense, ...expenses];
    // console.log("addExpense", expense);
  }

  function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
  }

  function clearExpenses() {
    expenses = [];
  }

  function setModifiedExpense(id) {
    let expense = expenses.find(item => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
  }

  function editExpense({ name, amount }) {
    // console.log("editExpense", { name, amount });
    expenses = expenses.map(item => {
      return item.id === setId ? { ...item, name, amount } : item;
    });
    setId = null;
    setAmount = null;
    setName = "";
    isEditing = false;
  }

  // Context
  setContext("removeExpense", removeExpense);
  setContext("modifyExpense", setModifiedExpense);
</script>

<Navbar />
<main class="content">
  <ExpenseForm
    {addExpense}
    {editExpense}
    name={setName}
    amount={setAmount}
    {isEditing} />
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>
    clear expenses
  </button>
</main>
