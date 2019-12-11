<script>
  import Title from "./Title.svelte";
  import Expense from "./Expense.svelte";

  export let expenses = [];
  export let removeExpense;
</script>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>

<section>
  <Title title="expense list" />
  <ul>
    {#each expenses as expense}
      <Expense {...expense} {removeExpense} />
    {:else}
      <h2>currently you have no expenses</h2>
    {/each}
  </ul>
</section>
