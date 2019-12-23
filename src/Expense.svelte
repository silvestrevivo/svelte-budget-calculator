<script>
  import { getContext } from "svelte";
  import { blur, scale, fly } from "svelte/transition";
  // Props
  export let id;
  export let name = "";
  export let amount = 0;
  //Variables
  let displayAmount = false;

  const removeExpense = getContext("removeExpense");
  const setModifiedExpense = getContext("modifyExpense");
</script>

<article class="single-expense">
  <div class="expense-info">
    <h2>
      {name}
      <button
        class="amount-btn"
        on:click={() => (displayAmount = !displayAmount)}>
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount}
      <h4 in:blur out:scale>amount: ${amount}</h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button
      class="expense-btn edit-btn"
      on:click={() => setModifiedExpense(id)}>
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn" on:click={() => removeExpense(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>
