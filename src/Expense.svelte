<script>
  //   export let id;
  export let name = "";
  export let amount = 0;

  let displayAmount = false;
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
      <h4>amount: ${amount}</h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button class="expense-btn edit-btn">
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn">
      <i class="fas fa-trash" />
    </button>
  </div>
</article>
