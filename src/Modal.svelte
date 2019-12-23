<script>
  import { blur, fade, fly } from "svelte/transition";
</script>

<div class="modal-container" in:blur out:fade={{ duration: 200 }}>
  <div class="modal-content" in:fly={{ y: 1000, delay: 200 }}>
    <div in:fade={{ delay: 400 }}>
      <slot />
    </div>
  </div>
</div>
