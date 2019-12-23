<script>
  import { setContext, onMount, afterUpdate } from "svelte";
  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import Modal from "./Modal.svelte";
  // variables
  let expenses = [];
  // set editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;
  // toggle form variable
  let isFormOpen = false;
  // reactive
  $: total = expenses.reduce((acc, curr) => (acc += curr.amount), 0);
  $: isEditing = setId ? true : false;

  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });
  afterUpdate(() => setLocalStorage());

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
    showForm();
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

  function showForm() {
    isFormOpen = true;
  }

  function hideForm() {
    isFormOpen = false;
    setId = null;
    setAmount = null;
    setName = "";
  }

  // Context
  setContext("removeExpense", removeExpense);
  setContext("modifyExpense", setModifiedExpense);

  // LocalStorage
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }
</script>

<Navbar {showForm} />
<main class="content">
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {addExpense}
        {editExpense}
        {hideForm}
        name={setName}
        amount={setAmount}
        {isEditing} />
    </Modal>
  {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>
    clear expenses
  </button>
</main>
